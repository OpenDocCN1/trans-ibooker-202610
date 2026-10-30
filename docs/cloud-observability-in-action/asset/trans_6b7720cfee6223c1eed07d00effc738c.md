
## 5.3 Metrics backends

* *Netdata* ([https://github.com/netdata/netdata](https://github.com/netdata/netdata))—一个开源的高保真基础设施监控和故障排除工具（代理和 UI）。
* *Nightingale* ([https://github.com/ccfos/nightingale/blob/main/README_en.md](https://github.com/ccfos/nightingale/blob/main/README_en.md))—一个云原生监控系统，可用于封装和增强 Prometheus、Alertmanager 和 Grafana 组合。
* *OpenTSDB* ([https://github.com/OpenTSDB/opentsdb](https://github.com/OpenTSDB/opentsdb))—一个基于 HBase 构建的分布式、可扩展 TSDB。其鼎盛时期在 2010 年代中期，当时 HBase 被广泛使用。
* *Promscale* ([https://www.timescale.com/promscale](https://www.timescale.com/promscale))—一个用于 Prometheus 指标和 OpenTelemetry 追踪的统一可观测性后端，构建于 PostgreSQL 以及名为 TimescaleDB ([https://www.timescale.com/](https://www.timescale.com/)) 的 TSDB 扩展之上。
* *QuestDB* ([https://github.com/bluestreak01/questdb](https://github.com/bluestreak01/questdb))—一个高性能的开源 SQL 数据库，适用于从金融服务到可观测性的各种应用。它包括 PostgreSQL 线路协议的端点；使用 InfluxDB 行协议的高吞吐量、与模式无关的摄取；以及用于查询、批量导入和导出的 REST API。
* *Zabbix* ([https://github.com/zabbix/zabbix](https://github.com/zabbix/zabbix))—一个开源的分布式监控解决方案。与本列表中的其他产品（如 Nagios、Icinga 和 OpenTSDB）一样，它在当今的云原生环境中并不广泛使用，但你应该了解它。

其中一些开源项目被打包并作为服务提供。让我们现在转向这个话题。

### 5.3.3 Commercial offerings for metrics backends

有一些专门针对指标的解决方案，包括以下内容：

* *Chronosphere* ([https://chronosphere.io/](https://chronosphere.io/))—本质上是“M3DB 即服务”。它们在功能和 OpenTelemetry 兼容性方面取得了良好进展。
* *VictoriaMetrics* ([https://github.com/VictoriaMetrics/VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics))—一个集成了 TSDB 的监控解决方案，可独立使用或作为服务使用。虽然 Prometheus 兼容性 ([https://github.com/prometheus/compliance](https://github.com/prometheus/compliance)) 在撰写本文时是 VictoriaMetrics 的一个增长领域，但整体方案是云原生且功能强大的。

在大多数，如果不是全部的情况下（Kubernetes、ECS），你需要指标，并且需要相应的后端。选择后端时应基于 Prometheus 兼容性以及你的特定使用模式（例如，摄取、存储或查询分发）。

## 5.4 Backend destinations for traces

现在，我们来看一下用于摄取和查询分布式追踪的各种选项。在这个领域，你通常会发现对 OpenTelemetry 的广泛支持，尽管在某些地方（如 PHP）仍然需要 Zipkin 支持。

### 5.4.1 Cloud providers

就像日志和指标一样，如果你想摄取和使用追踪，你选择的云提供商也有相应的产品：

* *AWS X-Ray* ([https://aws.amazon.com/xray/](https://aws.amazon.com/xray/))—Amazon 的托管分布式追踪服务，可用于分析和调试应用程序。它过去是一个独立服务，现在正被集成到 CloudWatch 中。X-Ray 已标准化为 OpenTelemetry，并可捕获来自多个计算和数据库 AWS 服务以及用 Java、Node.js 和 .NET 编写的应用程序的追踪数据。
* *Distributed tracing in Azure* ([http://mng.bz/zXWw](http://mng.bz/zXWw))—Application Insights 的一部分，提供 .NET、Java 和 Node.js/JavaScript 的 SDK。Application Insights 现在也通过 OpenTelemetry 支持分布式追踪。
* *Cloud Trace* ([https://cloud.google.com/trace](https://cloud.google.com/trace))—Google 的分布式追踪服务，通过 OpenTelemetry 支持 Go、Java、Node.js 和 Python。

图 5.4 提供了 Google Cloud Trace 的示例输出，Google Cloud Trace 是一个复杂的分布式追踪后端。它展示了追踪的分析，特别是延迟分布。接下来，我们将介绍开源追踪解决方案。

![](OEBPS/Images/05-04.png)

图 5.4 在 Google Cloud Trace 中查看追踪（Google Cloud 文档；[https://cloud.google.com/trace/docs/trace-overview](https://cloud.google.com/trace/docs/trace-overview)）

### 5.4.2 Open source traces backends

开源追踪解决方案提供的选项不如指标后端多，但仍有一些选择：

* *CNCF Jaeger and Zipkin*—Jaeger ([https://github.com/jaegertracing/jaeger](https://github.com/jaegertracing/jaeger)) 和 Zipkin ([https://github.com/openzipkin/zipkin](https://github.com/openzipkin/zipkin)) 都是由 CNCF 主办的分布式追踪系统。如今，Jaeger 似乎是更积极开发的项目，但 Zipkin 仍然被广泛使用和支持。我建议你查看各自的社区（Slack 和 GitHub 问题）并自行判断。我们将在第 6 章介绍两者。
* *Elasticsearch and OpenSearch*—这些选项已从日志扩展到支持处理追踪。两者都有出色的前端（Kibana 和 Dashboards），我们将在第 6 章再次介绍。
* *Grafana Tempo* ([https://grafana.com/oss/tempo/](https://grafana.com/oss/tempo/))—一个在 AGPLv3 许可证下提供的分布式追踪后端。它在持久层使用对象存储（例如 S3），并与 Grafana、Prometheus 和 Loki 集成。它支持 Jaeger、Zipkin 和 OpenTelemetry 摄取。

现在，让我们转向商业方案，与开源方案相比，它们配备非常丰富。这从关联功能开始，一直到采样和集成。

### 5.4.3 Commercial offerings for trace backends

最初被称为 *application performance monitoring*（APM）厂商，如今，追踪方面（或 *observability solution*）经常被突出强调。我们将在第 6 章详细介绍这些方案。

虽然分布式追踪尚未达到与指标和日志相同的采用水平，但对于云原生环境来说越来越重要。即使你没有短期策略，也应考虑在范围较小的项目或产品中进行测试。现在，我们将转换话题，讨论一类有助于解决上述基数爆炸挑战的后端：列式数据存储和格式。

## 5.5 Columnar data stores

有许多所谓的基于列式存储的引擎或简单的数据存储，可用作遥测数据的后端。*Columnar* 指的是数据在内存或磁盘上的布局方式，从而提升查询性能。

行式数据库，如 MySQL 和 PostgreSQL，会将记录相关的数据在内存（或磁盘）中紧密存储在一起。这意味着，一般来说，行式数据库非常适合在线事务处理（OLTP）用例，例如电子商务，其中需要对记录进行就地更新。它们针对读取和写入行进行了优化，这在存在大量单个事务且数据量可能较低的情况下非常适用，例如在在线 Web 应用程序中插入或更新单个记录时（例如，将产品放入购物车、在社交媒体网站上关注某人等）。
