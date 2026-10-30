
* 对于 OpenSearch Dashboards，我建议查阅项目文档，其中提供了 OpenSearch Dashboards 101 快速入门（[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)），如果你想了解 OpenSearch Dashboards Notebooks（类似 Jupyter Notebook；[http://mng.bz/2D80](http://mng.bz/2D80)），项目博客已为你涵盖。

顺带一提，有趣的是 Grafana 最初是 Kibana 的分支，极大地简化了 UI，并强烈聚焦于时间序列数据，尤其是指标。这导致 Grafana 常常与 Prometheus 一起使用，今天仍然如此。过去几年中，Grafana 扩大了其范围，现在也旨在涵盖日志（和追踪）可视化。另一方面，Kibana 和 OpenSearch Dashboards 最初专注于日志，现在正逐步增加对指标和追踪的支持。确实是令人有趣的时代！

与 Grafana 一样，你可以将 Kibana 和 OpenSearch Dashboards 的运维外包给 SaaS 服务：

* Elastic 提供托管 Kibana 作为其可观测性（[https://www.elastic.co/observability/](https://www.elastic.co/observability/)）解决方案的一部分。
* Amazon OpenSearch Service（[https://aws.amazon.com/opensearch-service/](https://aws.amazon.com/opensearch-service/)）提供由 OpenSearch Dashboards 驱动的可视化功能，同时也支持 Kibana（版本 1.5 至 7.10）。
* Aiven for OpenSearch（[https://aiven.io/opensearch/](https://aiven.io/opensearch/)）支持 OpenSearch Dashboards，你可以选择部署在哪个云平台（AWS、Azure、Google 等）。
* Logz.io 拥有基于 OpenSearch 和 OpenSearch Dashboards 构建的日志管理（[https://logz.io/platform/log-management/](https://logz.io/platform/log-management/)）服务，包含集成和人工指导的 AI/ML。

接下来，让我们看看在可观测性设置中使用的另外两个开源前端。

### 6.1.3 其他开源前端

除了 Grafana 和 Kibana/OpenSearch Dashboards，在评估选项时你可能还想查看另外两个开源前端：

* Apache Superset（[https://superset.apache.org/](https://superset.apache.org/)），在 Apache License Version 2.0 下可用，定位为数据探索和可视化平台，支持多种数据库（[http://mng.bz/1qWj](http://mng.bz/1qWj)），包括 Amazon Athena、Google BigQuery、ClickHouse、Elasticsearch、MySQL 和 TimescaleDB；不过截至撰写时，Prometheus 支持不可用。它于 2021 年初成为 Apache 软件基金会（ASF）的顶级项目，拥有众多贡献者，包括 Airbnb（其发源地）、Dropbox 和 Lyft。
* Metabase（[https://github.com/metabase/metabase](https://github.com/metabase/metabase)）承诺让公司里的每个人都能“提问并从数据中学习”，这非常适合可观测性用例。它在 AGPL 以及商业许可证下发布，具有强大的 SQL  focus。你可以将（[http://mng.bz/Pzpn](http://mng.bz/Pzpn)）Metabase 连接到多种数据库（包括 Google BigQuery、PostgreSQL 和 Snowflake），更多选项可通过合作伙伴和社区驱动程序获得。除了开源 DIY 模式，你还可以使用同名商业托管服务（[https://www.metabase.com/pricing/](https://www.metabase.com/pricing/)），可在本地或云中使用。现在，让我们转向云服务提供商和 ISV 提供的服务。

### 6.1.4 云服务提供商和商业前端

在这个领域，你经常会听到或读到 `business intelligence`（BI）这一术语——可将其视为业务流程的可观测性。事实上，该领域许多产品都源于此，并已适配或在可观测性上下文中作为特定用例领域使用。让我们更仔细地看看：

* `Amazon QuickSight`（[https://aws.amazon.com/quicksight/](https://aws.amazon.com/quicksight/)）——AWS 的云 BI 服务，可用于仪表盘和报告。它还支持许多流行的后端，包括 Amazon Athena、S3、OpenSearch 和 Salesforce。
* `Looker Studio`（[https://lookerstudio.google.com/overview](https://lookerstudio.google.com/overview)）——Google 的 BI 托管服务，支持多种可视化和连接到数百个后端的连接器。
* `Microsoft Power BI`（[https://powerbi.microsoft.com/en-us/](https://powerbi.microsoft.com/en-us/)）——Microsoft 的 BI 云服务，支持数据集市和应用，并与 Microsoft Office 套件深度集成。或者，你也可以使用 Azure 仪表盘（[https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards](https://learn.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards)），它附带了与 Azure Monitoring 的多个开箱即用集成。
* `Tableau`（[https://www.tableau.com/](https://www.tableau.com/)）——一种可视分析平台，可在多种环境中使用，从桌面到本地再到云，支持大量数据源和可视化。

本节提到的商业产品被广泛使用，主要是由企业业务部门而非工程团队使用。然而，在可观测性上下文中，我们确实需要一种全面的方法，使尽可能多的岗位族能够参与并从系统信号中受益。

## 6.2 一体化方案

从技术上讲，所有云服务提供商的服务，包括 CloudWatch、X-Ray、Azure Monitor 和 Google Cloud 的运维套件，都属于一体化类别。我们已在第 5 章中介绍过它们，因此这里重点关注该领域的其他产品。

有一些开源产品，部分由云原生计算基金会（CNCF）或 Apache 软件基金会（ASF）托管，提供两种或更多信号类型的支持。它们具备摄取和查询能力，通常允许插入不同的后端或使用内存（非持久化）存储。

### 6.2.1 CNCF Jaeger

CNCF Jaeger（[https://github.com/jaegertracing/jaeger](https://github.com/jaegertracing/jaeger)）项目起源于 Uber，并受 Google 的 Dapper 项目和 Zipkin 启发。如今，它是一个毕业的 CNCF 项目，定位为分布式追踪平台，内置对多种后端的支持，包括 Apache Cassandra；ES/OS；以及通过第三方存储插件框架的自定义后端，如 ClickHouse。

Jaeger 的整体架构是一组协同工作的微服务，用 Go 编写。核心 Jaeger 组件包括

* `Agent (optional)`——守护进程通过 UDP 将追踪发送到收集器，设计用于部署到运行应用的所有主机。代理正被 OpenTelemetry 取代。
* `Collector`——接收来自 OpenTelemetry（SDK 或收集器）或 Jaeger 代理的追踪，并对其进行验证、丰富并存储到存储后端。
* `Query service`——公开从存储中检索追踪的 API 并托管 UI。
* `UI`——用户交互的基于 Web 的 GUI，用于搜索和分析追踪和/或查看系统架构（微服务的图表示）。
* `Ingester`——收集器的简化版本，从 Apache Kafka（流行的开源消息代理）读取追踪并将其写入存储后端。

对于实验（开发-测试用例），Jaeger 还支持纯内存存储，可通过“一体化”发行版轻松使用，该发行版将核心 Jaeger 组件组合在一个包中。

让我们看看 Jaeger 的实际运行：我们再次使用 Docker Compose 设置，并将 Jaeger 配置为使用 ClickHouse 作为 span 存储（列表 6.2）。这次我们使用了一个小型微服务设置，其中三个 `ho11y` 实例相互连接，以模拟一个调用两个后端服务的前端服务。
