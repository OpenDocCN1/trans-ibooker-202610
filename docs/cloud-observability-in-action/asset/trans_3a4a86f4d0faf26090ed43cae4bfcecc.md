
```
a_counter 42.0 # {traceID="a6427ef2021cce03"}
```

现在，我们将继续介绍我们的示例关联设置。

### 11.2.1 指标–追踪关联示例设置

我们将使用 Prometheus 和 Jaeger 作为后端，Grafana 作为前端。本示例中的示例服务基于我们在第 2 章中使用的 `echo` 服务。它会发出跨度，这些跨度由 OpenTelemetry collector 路由到 Jaeger，同时，collector 会抓取服务的 `/metrics` 端点以获取带有嵌入式 trace ID 的指标，并使其可供 Prometheus 后端抓取。你应该已经了解所有这些工具以及如何配置它们，因此我在这里会简要说明。

所有展示的源代码都在第 11 章，请切换到该目录。首先，用 Go 编写的 `echo` 服务会发出跨度并暴露指标。此外，它现在还支持 OpenMetrics exemplars（[http://mng.bz/9DZ0](http://mng.bz/9DZ0)）。源代码位于 `echo/main.go` 文件中；我们只会关注与如何发出 exemplars 相关的新增内容，并且只涵盖 happy path（错误情况大致相同，但它在 40% 的情况下返回 `500` 代码，而不是 `200`）：

```
invoke, _ := invokes.GetMetricWith(prometheus.Labels{
        "http_status_code": strconv.Itoa(http.StatusOK),
    })
    invoke.(prometheus.ExemplarAdder).AddWithExemplar(    ❶
        1,
        prometheus.Labels{"traceID": traceID},
    )
    invokes.WithLabelValues(                              ❷
        strconv.Itoa(http.StatusOK),
    ).Inc()
    span.SetAttributes(                                   ❸
```

❶ 将 `traceID` 作为 exemplar 嵌入到 `invoke` 计数器中
❷ 使用 `http_status_code=200` 标签增加 `invoke` 计数器
❸ 发出跨度，其元数据与指标中使用的相同（HTTP 状态码）

除了发出带有嵌入式 exemplars 的跨度和指标外，在 `main.go` 中还有一件需要注意的事情：Prometheus 指标暴露的配置。我们需要确保使用 OpenMetrics 格式，做法如下：

```
http.Handle("/metrics", promhttp.HandlerFor(
        registry,
        promhttp.HandlerOpts{
            EnableOpenMetrics: true,
        },
    ))
```

现在，我们需要引入所有依赖项，因此（假设你已安装 Go 1.20 或更高版本）执行以下操作：

```
go mod vendor
```

你现在应该会看到一个子目录 `vendor/`，其中包含所有必要的本地可用包。

接下来，让我们回到主目录 `ch11/metrics-traces`，并启动在 `docker-compose.yaml` 中定义的 Docker Compose 栈：

```
docker-compose -f docker-compose.yaml up
```

以下列表显示了我们用于指标–追踪示例的 Docker Compose 文件的相关片段。

列表 11.1 用于关联设置的 Docker Compose 文件的相关片段

```
prometheus:
    image: prom/prometheus:v2.33.4
    ports:
    - "9090:9090"
    volumes:
    - ./prometheus.yml:/etc/prometheus/prometheus.yml
    command:
    - "--config.file=/etc/prometheus/prometheus.yml"
    - "--storage.tsdb.path=/prometheus"
    - "--enable-feature=exemplar-storage"   ❶
  grafana:
    image: grafana/grafana:latest
    ports:
    - "3000:3000"
    volumes:                                ❷
    - ./grafana-storage:/var/lib/grafana
    - ./grafana:/etc/grafana/provisioning
    - ./grafana/dashboards:/var/lib/grafana/dashboards
```

❶ 需要配置 Prometheus 以支持 exemplars（内存中）
❷ 用于自动配置 Prometheus 和 Jaeger 数据源以及仪表板

让我们继续查看以下列表中显示的 OpenTelemetry collector 配置——`otel-config.yaml` 文件。

列表 11.2 用于关联设置的 OpenTelemetry collector 配置

```
exporters:
  prometheus:
    endpoint: "0.0.0.0:1234"
    metric_expiration: 180m
    enable_open_metrics: true     ❶
  jaeger:
    endpoint: jaeger:14250
    tls:
      insecure: true
service:
  pipelines:                      ❷
    metrics:
      receivers: [ prometheus ]
      processors: [ batch ]
      exporters: [ logging, prometheus ]
    traces:
      receivers: [ otlp ]
      processors: [ batch ]
      exporters: [ logging, jaeger ]
```

❶ 在 Prometheus exporter 中启用 exemplar 支持
❷ 指标和追踪管道

如果你想查看原始 exemplars，可以在你的机器上执行以下操作（假设 Docker 正在运行；http://127.0.0.1:8888/ 是 `echo-svc` 提供服务的地址）：

```
curl -H "Accept: application/openmetrics-text" \   ❶
        "http://127.0.0.1:8888/metrics"            ❷
```

❶ 发送此 HTTP 头是获取 exemplar 的关键（请求 OpenMetrics）。
❷ echo 服务端点，以 OpenMetrics 格式提供指标

当你执行上述 `curl` 命令时，你应该会看到类似以下输出的内容：

```
# HELP echo Total invocations of the echo service endpoint.
# TYPE echo counter
echo_total 930.0 # {http_status_code="200",                      ❶
                   traceID="f2a9e19d5c34427cf2f50a430de9fa3c"}   ❷
                   1.0 1.6801173123788908e+09
# EOF
```

❶ 一个简单的维度，通过 HTTP 状态码的标签表示
❷ 通过 Prometheus 标签 `traceID` 表示的 exemplar

有了这些设置，我们就可以使用信号关联了。现在就开始吧。

### 11.2.2 使用指标–追踪关联

是时候进行关联了！打开你的浏览器并输入 `localhost:3000`，即可打开 Grafana（无需凭据）。现在，打开名为 Echo Service Dashboard With Exemplars 的仪表板，并等待几分钟以收集足够的数据。然后你应该会看到类似图 11.2 的内容。第一行包含 `echo_total` 指标，具有 `200` 和 `500` 状态码，仔细观察，你会在图表底部看到一些小点。这些点允许 Grafana 时间序列可视化渲染 exemplars。将鼠标悬停在其上，你会看到跨度信息；如果点击它们，你将进入图 11.3 所示的 Jaeger 追踪视图。

![](OEBPS/Images/11-02.png)

图 11.2 `echo` 服务的 Grafana 仪表板截图，其中包含 exemplars（底部的微小点）

图 11.2 的下半部分显示了错误追踪，你也可以使用它们导航到 Jaeger 追踪视图。

![](OEBPS/Images/11-03.png)

图 11.3 `echo` 服务的 Jaeger 视图截图，聚焦于表示 500 响应的错误跨度

花点时间探索视图以及如何使用 exemplars 从指标到追踪的路径。内容很多，可能需要一些时间。接下来，我们将快速回顾信号关联领域的商业产品。

## 11.3 商业产品中的信号关联支持

虽然目前还处于早期阶段，并非所有关联路径都得到每个提供商的支持，但商业领域中已经有对多种关联的核心支持。通常，这些与追踪–日志和指标–追踪支持相关。

让我们先看看云提供商及其原生的监控和可观测性解决方案：

*   AWS 具有基于 OpenTelemetry 的全面关联策略（[http://mng.bz/jPqV](http://mng.bz/jPqV)），通过 AWS X-Ray 和 Amazon CloudWatch（[http://mng.bz/WzWd](http://mng.bz/WzWd)）支持追踪–日志关联。
*   与 AWS 类似，Azure 正在标准化 OpenTelemetry。例如，参见基于 Application Insights 的关联支持（[http://mng.bz/8rAW](http://mng.bz/8rAW)）。
*   Google Cloud 在其运营套件中提供关联日志条目的支持（[https://cloud.google.com/logging/docs/view/correlate-logs](https://cloud.google.com/logging/docs/view/correlate-logs)）。

现在，我们将转向可观测性 ISV 的商业产品。在可观测性领域，几乎所有厂商都在其解决方案中提供一种或另一种形式的关联支持：

