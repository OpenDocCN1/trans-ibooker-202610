
```
func handleEcho(w http.ResponseWriter, r *http.Request) {
    message := r.URL.Query().Get("message")
    log.WithFields(log.Fields{
        "service": "echo",
    }).Info("Got input ", message)
    if rand.Intn(100) > 90 {
        log.WithFields(log.Fields{
            "service": "echo",
        }).Error("Something really bad happened :(")
        invokes.WithLabelValues(strconv.Itoa(http.StatusInternalServerError)).Inc()❶
        w.WriteHeader(500)
        return
    }
    invokes.WithLabelValues(strconv.Itoa(http.StatusOK)).Inc()                     ❶
    fmt.Fprintf(w, message)
}
var (
    registry = prometheus.NewRegistry()                                            ❷
    invokes  = promauto.NewCounterVec(                                             ❸
        prometheus.CounterOpts{
            Name: "echo_total",
            Help: "Total invocations of the echo service endpoint.",
        },
        []string{"http_status_code"},
    )
)
func main() {
    rand.Seed(time.Now().UnixNano())
    registry.MustRegister(invokes)                                                 ❹
    log.SetFormatter(&log.JSONFormatter{})
    http.HandleFunc("/echo", handleEcho)
    http.Handle("/metrics", promhttp.HandlerFor(                                   ❺
        registry,
        promhttp.HandlerOpts{
            EnableOpenMetrics: true,
        },
    ))
    log.Fatal(http.ListenAndServe(":4242", nil))
}
```

❶ 使用携带 HTTP 状态码的上下文特定标签递增我们的指标
❷ 设置 Prometheus 注册表
❸ 定义我们的指标
❹ 注册我们的指标
❺ 定义 `/metrics` 端点处理器

此时，你已经知道如何为 `echo` 服务添加 instrumentation 以发出指标，但这些指标如何到达可以消费它们的终点？让我们在下一节中查看。

### 2.4.2 遥测

我们现在可以本地执行 `echo` 服务并公开 Prometheus 指标：

```
$ ./echosvc
```

我们可以使用 `Prometheus` ([http://prometheus.io/](http://prometheus.io/)) 来抓取我们的 `echo` 服务的指标，其本身可以作为 `Grafana` 中的数据源作为目的地。要设置目的地，请使用下一列表中所示的 Docker Compose 清单文件（引用列表 2.9）来设置 `Prometheus` 和 `Grafana`。

列表 2.8 使用 `docker-compose.yaml` 启动 `Prometheus` 和 `Grafana`

```
version: "3.5"
networks:
  prom:
services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prom-config.yaml:/etc/prometheus/prometheus.yml
    networks:
      - prom
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    networks:
      - prom
```

正如我们在日志中所见，我们还需要为指标配置遥测代理。在这种情况下，我们通过定义 `Prometheus` 抓取配置来实现，如以下列表中 `prom-config.yaml` 文件所示。

列表 2.9 `Prometheus` 配置文件 `prom-config.yaml`

```
global:
  scrape_interval: 5s
  evaluation_interval: 5s
  external_labels:
      monitor: 'lab'
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
    - targets: ['localhost:9090']
  - job_name: 'lab'
    static_configs:
    - targets: ['host.docker.internal:4242']
```

接下来，如图 2.3 所示，使用 `http://prometheus:9090/` 作为 URL 在 `Grafana` 中添加 `Prometheus` 作为数据源。

![](OEBPS/Images/02-03.png)

图 2.3 在 `Grafana` 中添加和配置 `Prometheus` 数据源

现在，你可以再次使用 Explore 选项卡来消费 `echo` 服务公开的指标。选择 `Prometheus` 作为数据源，如果你输入以下 `PromQL` ([http://mng.bz/qrGw](http://mng.bz/qrGw)) 查询，表示我们服务的错误率，你应该会看到与图 2.4 所示类似的内容：

```
sum (rate(echo_total{http_status_code="500"}[5m]))
/
sum (rate(echo_total[5m])) * 100
```

![](OEBPS/Images/02-04.png)

图 2.4 使用 `Prometheus` 探索指标

前述 `PromQL` 查询是捕获错误率的规范方法。通过将错误（HTTP 状态码 500）除以所有数据点，你有一种灵活而完整的方式来捕获错误。在这种情况下，错误率是一个低的个位数，并且平均应为 10%，因为我们在列表 2.7 中就是这样配置服务的。接下来，让我们讨论指标的优缺点。

### 2.4.3 成本与收益

我们现在更详细地了解指标信号类型的成本和收益。表 2.2 显示了与指标相关的成本概述。

表 2.2 指标成本概述

|   || Overhead || Considerations |
| --- | --- | --- |
| **插桩** || 如果可能，自动插桩 || 在编程语言中得到日益支持 |
| --- | --- | --- |
| **代理** || `Prometheus`、`OpenTelemetry` collector 和 `OpenMetrics` 作为新兴标准 || 要收集的内容和自动修剪 |
| **目的地** || 基数、长期存储和联邦 || 要摄取的内容和保留的数据温度 |

### 指标的插桩成本

关于使用指标对代码进行插桩的成本，我们可以区分两种情况：
* 使用自动插桩通常需要很少的精力，例如包含一个库或配置它（例如，使用 `Prometheus` [[http://mng.bz/7Dr9](http://mng.bz/7Dr9)]；`OpenTelemetry` [[https://opentelemetry.io/docs/instrumentation/](https://opentelemetry.io/docs/instrumentation/)]；以及日益基于 `eBPF`）。
* 手动插桩比自动插桩需要更多的精力，但也提供了更多控制，可以以何种方式公开哪些指标。通常，这些插桩应该是一次性操作，对于一类指标（业务级指标），这种插桩是唯一可行的。

代码的可读性，以及随之而来的可维护性，可能会因手动插桩而受损。

让我们现在从开发者的角度具体看看插桩成本：列表 2.1 中的原始代码有 24 行代码，而列表 2.3 中使用指标插桩的代码总共有 56 行代码。为了将指标引起的插桩成本与总成本（日志 + 指标）插桩成本分离，我们需要减去日志引起的插桩的 8 行代码。然后我们得到 48 行代码的值。

将这两个 `LOC` 数字放在一起，我们可以计算 `B2I` 比率，并得出指标的值为 2。这个 `B2I` 值略高于我们使用日志时的值（1.33），但仍然不令人担忧。

如果你想了解有关指标插桩良好实践的更多信息，我建议你查阅以下资源：
* `CNCF SIG Instrumentation` 关于为 `Kubernetes` 插桩的指导 ([http://mng.bz/mVer](http://mng.bz/mVer))。
* `CNCF TAG observability` 白皮书输入，其中包含开发人员如何实现新指标的指南 ([https://github.com/cncf/tag-observability/issues/18](https://github.com/cncf/tag-observability/issues/18))。

### 指标的遥测成本

指标的代理具有与我们对日志讨论的类似考虑。此外，还有基数方面的挑战——即指标维度中可能的不同值的数量，这可能导致成本（急剧）增加。

指标代理的示例包括以下内容：
* `CNCF Prometheus`，尤其是在代理模式下 ([https://prometheus.io/blog/2021/11/16/agent/](https://prometheus.io/blog/2021/11/16/agent/))
* `CNCF OpenTelemetry` collector ([https://opentelemetry.io/docs/collector/](https://opentelemetry.io/docs/collector/))，更具体地说，来自 `OpenTelemetry` 发行版的 collector，例如 `AWS Distro for OpenTelemetry Collector` ([http://mng.bz/5wBB](http://mng.bz/5wBB))

### 指标的目的地成本
