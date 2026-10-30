
- *指标* ([http://mng.bz/e1XZ](http://mng.bz/e1XZ)) — HTTP 客户端和服务器指标的语义约定；RPC 客户端和服务器指标；标准系统指标，如 CPU 使用率或磁盘 I/O；硬件；以及大量其他指标。

- *日志* ([http://mng.bz/pP00](http://mng.bz/pP00)) — 属性语义约定在出版时仍具有实验性质。仅定义了两个：一个用于日志媒体（描述日志的来源，例如文件名或 I/O 流）和一个事件属性，可例如描述业务上下文（如浏览器或设备）。

一个资源和信号属性的具体示例会是什么样的？假设我们运行一个由某些服务组成的应用程序。假设其中一个是 `svc-wow`，位于名为 `boom` 的命名空间中，在名为 `prod-42` 的 EKS 集群中，位于 `eu-west-1` 区域，并且该服务公开 HTTP 指标。

该场景在 OpenTelemetry 中将以 Pod 作为信号源表示，因此我们将使用以下资源属性：

- 对于服务所在的 EKS 集群，我们将使用 `cloud.region` ([http://mng.bz/OxDO](http://mng.bz/OxDO)) 属性，其值为 `eu-west-1`，以及 AWS 特定的 `aws.eks.cluster.arn` ([http://mng.bz/Y12Q](http://mng.bz/Y12Q)) 属性，其值例如为 `arn:aws:ecs:eu-west-1:123456789123:cluster/prod-42`。

- 对于服务所在的 Kubernetes 命名空间，我们将使用通用资源属性 ([http://mng.bz/GyQ8](http://mng.bz/GyQ8)) `service.namespace`，其值为 `boom`，以及 `service.name`，其值为 `svc-wow`。

- 我们这里关注的信号类型是指标，这意味着我们使用信号属性为发出的指标提供元数据。假设 `svc-wow` 服务公开了一个 HTTP API，我们想报告入站请求持续时间。我们将使用 `http.server.duration` ([http://mng.bz/zXYg](http://mng.bz/zXYg)) 指标，并使用诸如 `http.method`、`http.status_code` 和 `net.host.name` 之类的属性。巧合的是，`http_status_code` 属性是一个名称和值都标准化的属性的示例——即 RFC 7231 中定义的 HTTP 响应状态码的官方列表 ([https://www.rfc-editor.org/rfc/rfc7231#section-6](https://www.rfc-editor.org/rfc/rfc7231#section-6))。

表 11.1 提供了信号类型之间可能组合和转换的概述，其中 traces 表示跨服务的执行，metrics 表示数值聚合（例如错误百分比），logs 作为信号类型表示请求路径上单个服务。请注意，这些转换并非特定于某种格式，例如 OpenTelemetry。

表 11.1 信号关联概述

| 来自/至 | 跟踪 | 指标 | 日志 |
| --- | --- | --- | --- |
| **跟踪** | 相关 spans | 跟踪到代表性指标 | 跟踪到服务的日志行 |
| **指标** | 指标到代表性 spans | 相关指标 | 指标到服务的日志 |
| **日志** | 日志到服务的 spans | 日志到相关指标 | 相关日志 |

以下小节中的 *关联路径* 一词指的是从一种信号类型转换到另一种相关信号类型。这可能包括一个以上的转换。现在让我们剖析表 11.1，讨论按信号类型的具体关联。

### 11.1.2 关联 traces

在这种第一种情况下，关联路径的起点是 traces，表示跨服务执行，涵盖请求路径上的同步服务，以及潜在的异步服务，例如消息代理。从这里，我们将过渡到具体的关联。

To traces

*Traces–traces 关联* 指的是按环境或服务查找相关 spans。例如，您可以在 OpenTelemetry 中使用 Span Links 功能 ([http://mng.bz/0KNW](http://mng.bz/0KNW))，它允许您将一个 span 与一个或多个其他 span 关联，为异步的 fire-and-forget 工作负载定义因果关系，其中父 span 在其子 span 关闭之前不会等待其关闭。这对于诸如消息总线和消息队列之类的异步服务特别有用，包括 Apache Kafka ([https://kafka.apache.org/](https://kafka.apache.org/)) 和 Amazon EventBridge ([https://aws.amazon.com/eventbridge/](https://aws.amazon.com/eventbridge/))。

To metrics

*Traces–metrics 关联* 意味着输入是一个表示跨服务执行的 trace，输出由对输入 trace 的 spans 有贡献的服务的相关指标组成。

To logs

*Traces–logs 关联* 意味着输入是一个表示跨服务执行的 trace，输出由对输入 trace 的 spans 有贡献的服务的日志行组成。AWS X-Ray SDK for Java ([http://mng.bz/KeaE](http://mng.bz/KeaE)) 中的关联支持是一个示例。

### 11.1.3 关联 metrics

在这种情况下，关联路径的起点是指标的时间序列——数值聚合值。从这里我们可以过渡到具体的关联。

To traces

*Metrics–traces 关联* 意味着您有一个指标作为输入（例如在仪表板上绘制的图形或来自警报），预期的输出由对该时间序列有贡献的服务的代表性 spans 组成。我们将在下一小节中对这种关联案例进行具体研究，在那里我们将使用 Prometheus、Jaeger 和 Grafana 来实现指标与 spans 之间基于 exemplars 的关联。

To metrics

*Metrics–metrics 关联* 意味着相关指标（按类型或服务）（例如客户端和服务器端错误）。

To logs

*Metrics–logs 关联* 意味着输入是一个指标（例如来自仪表板），输出由对聚合值有贡献的服务的日志行组成。

### 11.1.4 关联 logs

在这种情况下，关联路径的起点是来自一个或多个服务的日志。从这里我们可以过渡到具体的关联。

To traces

*Logs–traces 关联* 意味着我们将服务的日志行作为输入，并寻求从写入这些日志行的服务发出的相应 spans。

To metrics

*Logs–metrics 关联* 使用服务的日志行作为输入，并输出来自该服务和相关服务（例如沿请求路径）的相关指标。

To logs

*Logs–logs 关联* 尝试按服务、环境或严重性查找相关日志。例如，您可以请求请求路径上所有服务的 `ERROR` 日志。

### 11.1.5 关联 profiles

我们在第 9 章讨论了 profiles 的性质和用例；它们与日志行有些可比，因为其范围是一个服务。Profiles 可以自动（通过 eBPF 或特定语言的方式）或手动发出。与日志相比，profiles 是数值性的。因此，虽然前述的 to–from 关联观察中的许多内容适用，但请注意 profiles 最终是数值值（因此从关联的角度来看，它们在某种意义上表现得像指标）。

## 11.2 使用 Prometheus、Jaeger 和 Grafana 进行关联

现在您在概念层面上了解了关联是什么，让我们研究如何实现它。为此，我们将使用一个具体示例，展示如何基于 exemplars 进行 metrics–traces 关联。在 metrics–traces 关联的情况下，这意味着从指标表示嵌入到 trace 的链接。这是通过插入 trace ID 来完成的，这样当指标在时间序列图中呈现时，您可以点击 exemplars 并直接跳转到代表性 trace 并浏览其 spans。从概念上讲，这种“将 trace ID 嵌入到指标中”看起来会像以下这样（使用 Prometheus 暴露格式，或更准确地说 OpenMetrics，因为该功能仅存在于那里）：

