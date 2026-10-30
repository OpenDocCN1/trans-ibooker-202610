
### 2.5.1 仪器化

清单 2.10 显示了我们在进行追踪仪器化时 `echo` 服务的相关代码行。注意，这里我们使用 `OpenTelemetry` 来发出 spans，相关内容将在第 4 章详细介绍。

清单 2.10 已为发出追踪而进行仪器化的示例 Go 程序

```
func handleEcho(w http.ResponseWriter, r *http.Request) {
    message := r.URL.Query().Get("message")
    log.WithFields(log.Fields{
        "service": "echo",
    }).Info("Got input ", message)
    ctx, cancel := context.WithCancel(context.Background())
    defer cancel()
    tr := otel.Tracer("http-api")
    _, span := tr.Start(ctx, "echo")                                ❶
    defer span.End()
    if rand.Intn(100) > 90 {
        log.WithFields(log.Fields{
            "service": "echo",
        }).Error("Something really bad happened :(")
        invokes.WithLabelValues(
            strconv.Itoa(http.StatusInternalServerError)).Inc()
        span.SetAttributes(
            attribute.Key("http-status-code").String(
                strconv.Itoa(http.StatusInternalServerError)))      ❷
        w.WriteHeader(500)
        return
    }
    invokes.WithLabelValues(
        strconv.Itoa(http.StatusOK)).Inc()
    span.SetAttributes(
        attribute.Key("http-status-code").String(
            strconv.Itoa(http.StatusOK)))                           ❸
    fmt.Fprintf(w, message)
}
 
func main() {
    rand.Seed(time.Now().UnixNano())
    registry.MustRegister(invokes)
    log.SetFormatter(&log.JSONFormatter{})
    http.HandleFunc("/echo", handleEcho)
    http.Handle("/metrics", promhttp.HandlerFor(
        registry,
        promhttp.HandlerOpts{
            EnableOpenMetrics: true,
        },
    ))
    tp, err := tracerProvider("http://localhost:14268/api/traces")  ❹
    if err != nil {
        log.Fatal(err)
    }
    otel.SetTracerProvider(tp)
    log.Fatal(http.ListenAndServe(":4242", nil))
}
```

❶ 调用时启动一个新的 span
❷ 将错误代码作为 span 的属性（信号元数据）设置
❸ 将成功作为 span 的属性（信号元数据）设置
❹ 设置 OpenTelemetry 配置，将 Jaeger 作为追踪提供者

现在，让我们启动服务并生成一些 spans。

### 2.5.2 遥测

我们现在可以本地执行 `echo` 服务并像这样摄取追踪：

```
$ ./echosvc
```

我们可以使用 Jaeger ([https://www.jaegertracing.io/](https://www.jaegertracing.io/)) 摄取 `echo` 服务的追踪，然后将其用作 Grafana 的数据源。

要设置目标，请使用以下清单中所示的 Docker Compose 清单文件来设置 Jaeger 和 Grafana。

清单 2.11 使用 `docker-compose.yaml` 启动 Jaeger 和 Grafana

```
version: "3.5"
networks:
  jaeger:
services:
  jaeger:
    image: jaegertracing/all-in-one:latest
    ports:
    - "6831:6831/udp"
    - "16686:16686"
    - "14268:14268"
    networks:
    - jaeger
  grafana:
    image: grafana/grafana:latest
    ports:
    - "3000:3000"
    networks:
    - jaeger
```

接下来，如图 2.5 所示，在 Grafana 中添加 Jaeger 作为数据源，使用 `http://jaeger:16686` 作为 URL。

![](OEBPS/Images/02-05.png)

图 2.5 在 Grafana 中添加和配置 Jaeger 数据源

现在，你可以再次使用 Explore 选项卡。选择 Jaeger 作为数据源；切换到 Search 选项卡（在 Query type 字段中）；选择 `echosvc`；然后如果点击 Trace ID 选择其中一个追踪，你应该会看到与图 2.6 所示类似的内容（在 Trace View，右侧）。在下一小节中，我们将查看追踪的成本和收益。

![](OEBPS/Images/02-06.png)

图 2.6 使用 Jaeger 探索追踪：详细追踪视图

### 2.5.3 成本与收益

让我们更仔细地看看 traces 信号类型的成本和收益。表 2.3 提供了追踪的成本概览。

表 2.3 追踪成本概览

|   || 开销 || 考虑因素 |
| --- | --- | --- |
| `Instrumentation` || 一定程度的投入和自动仪器化 || `OpenTelemetry` 采用情况和遗留系统 |
| --- | --- | --- |
| `Agent` || 采样策略和延迟开销 || 使用 `OpenTelemetry` collector |
| `Destination` || 专用存储层以及查询和分析 || 众多商业方案和部分开源软件（例如 `Tempo` 和 `Jaeger`） |

追踪的仪器化成本

从开发者的角度来看，追踪的仪器化成本如下：清单 2.1 中的原始代码有 24 行代码，而清单 2.10 中带有追踪仪器化的代码总共有 99 行。

现在，为了将追踪引起的仪器化成本从总计（`logs` + `metrics` + `traces`）仪器化成本中分离出来，我们需要减去日志引起的仪器化中的 8 LOC，以及指标仪器化贡献的 24 LOC。我们得到仅追踪仪器化为 67 LOC。将这两个 LOC 数字放在一起，我们可以计算 B2I 比率，得到 2.79 的值，这几乎是仅日志的两倍（1.33），略高于仅指标仪器化（2）。

追踪的遥测成本

一个主要考虑因素（这个主题也可以放在之前的仪器化部分，但我选择在这里讨论）是追踪的采样。我们将在第 8 章更详细地讨论这个主题。

采样，在分布式追踪的上下文中，意味着捕获或丢弃源发出的某个追踪。我们通常将其区分为
* `Head-based (sometimes called up-front) sampling`—代理会丢弃固定数量的追踪。它是确定性的，有助于关注网络成本；然而，你可能会错过有趣的追踪（异常、错误等）。
* `Tail-based sampling`—其思想是将采样决策推迟到追踪完成时。但如何知道追踪何时完成（滞后者）？它通常要求代理保留更多状态，并且可能导致更复杂的设置以及更高的资源使用（网络 + 存储成本），相比更简单的预先采样决策。

除了我们已在日志和指标中看到的运行时开销（资源消耗）之外，在追踪的情况下，通信路径中还存在微小的延迟开销。这是因为上下文传播在源中以及在网络上传输时都会消耗时间和带宽。

追踪的目标成本

分布式追踪解决方案通常使用允许处理稀疏、带时间戳数据的存储。在一般情况下，追踪需要被索引，以便你可以搜索或过滤特定服务，或按持续时间排序。这通常意味着存储（后端）需要能够索引某些 span 特性，例如服务名称、标签和其他元数据（例如 HTTP 方法）。

在实际使用中，有不同的实现追踪存储和检索的方法，可分为以下几类：
* 专用数据存储，例如 `ClickHouse` ([https://clickhouse.com/](https://clickhouse.com/))
* 通用 NoSQL 数据存储，例如 `Cassandra`、`HBase` 或 `Elasticsearch/OpenSearch`，它们使用特定领域的模式和逻辑来满足需求
* 基于对象存储的后端（例如使用 `S3` 存储桶），带有查询/摄取逻辑，例如 Grafana `Tempo`

追踪中后端（如指标）的成本通常分为三类之一或多类：摄取（插入了多少追踪以及负载大小）、处理（有时称为 `scanning` 或 `traces retrieved`）以及长期存储成本（保留策略）。我们将在第 5 章详细讨论后端。
