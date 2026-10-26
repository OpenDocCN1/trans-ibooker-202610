

- 聚类：K-means，潜在狄利克雷分配（LDA），高斯混合模型（GMM）
- 分解：奇异值分解（SVD），主成分分析

## Spark GraphX/Graph Frames

图计算对于图问题变得越来越常见。图是描述关系的一种自然方式，我们倾向于用关系数据库中的自连接来解决的许多问题。

许多应用程序都在使用图算法，与传统的集合处理相比，图算法不仅更容易实现，而且更适合此类关系分析。例如，搜索引擎使用 PageRank 算法对搜索结果进行排名，电信和社交媒体平台使用社交网络分析来更好地理解行为并提供准确的推荐，银行使用图分析来了解洗钱，安全机构使用图来打击恐怖网络。

GraphX 是 Spark 中用于这些目的的重要组件。GraphX 引擎扩展了 Spark RDD 接口并引入了新的 Graph 扩展，它基本上是一个带有属性的定向多图，每个顶点（图的节点）和边（节点/顶点之间的连接）都附加了属性。图 4.12 显示了一个无向图，包含七个顶点和九条边。

图 4.12 包含七个顶点/节点和九条边的无向图

![](img/fc6a7083d65b0716921d7bd4a5298460_220_0.png)

GraphFrames 目前作为 Spark 外部包提供，您需要在启动 Spark 应用程序时加载它。虽然 GraphX 自诞生以来一直很受欢迎，但 DataFrame API 随着时间的推移变得更受欢迎，因此基于 DataFrame 的 API GraphFrames 在 Spark 中引入。GraphFrames 提供了更好的用户体验，尽管以极其微小的性能影响为代价。

## Hive on Spark

虽然 Spark SQL 是在 Spark 上运行 SQL 的标准，但许多组织一直在使用 Apache Hive，并且其代码库使用 HiveQL，但希望将脚本从 Hadoop 迁移到 Spark 平台以获得更好的性能。Hive on Spark 为这些组织提供了在 Apache Spark 上运行 Hive 程序的选项，并且提供了一个清晰的迁移路径，而无需承担大量的维护和迁移开销。

## 何时使用 Apache Spark

从考试角度来看，了解何时 Apache Spark 比其他平台更合适非常重要。Apache Spark 因表 4.8 中列出的原因而被推荐。

### 表 4.8 Apache Spark 优势

| Tenant | Description |
|---|---|
| Performance | Apache Spark 的性能优于 MapReduce。 |
| Deployment | Apache Spark 可以通过 Apache Mesos、通过 YARN 的 Apache Hadoop 或 Apache Spark 的独立集群管理器进行部署。 |
| Application types | Apache Spark 支持批处理、交互式和流式应用程序。 |
| Data sources | 支持文件、对象、关系和非关系源 |
| Connectors | 提供标准的 JDBC/ODBC 连接器 |
| API | 提供 Java、Scala、Python、R 的 API 并允许使用 SQL |
| Resource handling | 需要精细的资源处理。内存密集型作业需要大量资源。 |
| Streaming | Apache Spark 通过 DStreams 和 Structured Streaming 提供流处理；然而，它采用微批处理方法而不是纯流处理。延迟不能以毫秒为单位，您需要考虑无序流处理。 |

## 在 Amazon EMR 上运行 Spark 的优势

您可以将 Apache Spark 作为独立应用程序运行，也可以作为现有 Hadoop 安装的一部分运行。您还有各种选项可以将 Apache Spark 作为各种云提供商提供的各种发行版的一部分运行，其中一个选项是使用 Amazon EMR 运行它。表 4.9 列出了将其作为 Amazon EMR 的一部分运行可能为您带来的一些原因。

### 表 4.9 在 Amazon EMR 上运行 Apache Spark

| Tenant | Description |
|---|---|
| Storage options | 在 Amazon EMR 上运行 Apache Spark 可让您受益于整个平台，例如使用 EMR-DynamoDB 连接器连接到 Amazon DynamoDB，使用 JDBC 连接到 Amazon RDS，或使用 Elasticsearch 连接器连接到 Elasticsearch，或使用 Spark-Redshift 连接器连接到 Amazon Redshift，或连接到流源如 Amazon Kinesis 和 Apache Kafka（或 MSK）或连接到 Amazon S3。 |
| Data catalog | 在 Amazon EMR 上运行 Apache Spark 可让您受益于 AWS Glue 目录/Lake Formation 目录（如果启用）。 |
| Failover/recovery | Amazon EMR 会为您恢复失败的节点。 |
| Logging | Amazon EMR 聚合集群日志并将其写入 S3，这使您可以轻松调试并确定问题。 |
| Cost | Amazon EMR 可让您通过运行临时集群或运行小型初始集群并通过使用任务实例添加处理能力来降低成本。 |
| API | 提供 Java、Scala、Python、R 的 API 并允许使用 SQL。 |
| Amazon EMRFS | Amazon EMR 与 S3 的集成可让您从 Amazon S3 中的数据创建 RDD 和 Data Frames。 |
| Spark metrics and Cloud-Watch | 您可以使用 Amazon CloudWatch 监控 Spark 集群的指标，在 CPU/内存指标上设置警报，并在阈值达到/超出时通过电子邮件、SNS 或 HTTP API 调用接收通知，从而允许您采取手动或配置自动响应。 |
| Variety of Spark versions | Amazon EMR 可让您使用各种 Spark 版本，从而允许您运行组织根据合规标准内部商定的版本。 |

## Amazon EMR 上的 Apache HBase

*HBase* 是一个分布式列式存储，建立在 HDFS 之上，用于在需要对极大数据集进行实时读写随机访问时使用，这通常是 MapReduce 和 Apache Spark 无法实现的。

HBase 是为线性可扩展而构建的，是最受欢迎的 NoSQL 数据库之一。它开箱即用不支持 SQL；然而，您可以使用 Apache Phoenix 等项目在 HBase 上运行 SQL。

HBase 由 Chad Walters 和 Jim Keller 参照 Google 的 Bigtable 进行建模，并于 2007 年 10 月作为 Hadoop 0.15 的一部分发布，后来于 2010 年 5 月分离为一个单独的子项目。

虽然 HBase 在各行业中用于各种用例，但其中最受欢迎的是 Facebook 的消息基础设施在 HBase 上运行，以及 Twitter 在其整个 Hadoop 集群中使用 HBase。

## HBase 架构

HBase 通常遵循主从架构，通常包含四个主要组件：

- Master
- Region server
- ZooKeeper cluster
- Storage (/HDFS/S3)

## 使用 Amazon EMR 设置 HBase

您可以在快速选项和高级模式下设置 HBase（见图 4.13）。

图 4.13 使用快速选项创建模式设置 HBase

![](img/fc6a7083d65b0716921d7bd4a5298460_223_0.png)

在高级选项中，当您在软件配置选项中选择 HBase 时，您可以选择 HBase 数据的存储层，可以是 HDFS 或 S3。HDFS 选项使用默认位置，而对于 S3，您可以选择目标存储桶（见图 4.14）。

### 图 4.14 使用高级选项设置 HBase

![](img/fc6a7083d65b0716921d7bd4a5298460_224_0.png)

## Apache Flink、Apache Mahout 和 Apache MXNet

如果您在设置 Amazon EMR 集群时打开高级配置，您将看到可以作为 EMR 配置的一部分安装的某些其他项目。本书仅在高层次上介绍这些项目是什么，但我不会详细介绍，因为它超出了本书的目的。

## Apache MXNet

*Apache MXNet* 是一个开源深度学习框架，目前处于孵化状态。它适用于多种用例，例如计算机视觉、语言处理、预测和推荐引擎。MXNet 是为企业开发者构建的，在 Amazon.com 上使用非常广泛。它因以下关键因素而受欢迎：

- *原生分布式训练支持：* Apache MXNet 支持在多个 CPU/GPU 机器上进行分布式训练，以利用云平台提供的计算规模。
- *灵活的编程模型：* Apache MXNet 支持命令式和符号式编程，以最大化效率和生产力。
- *从云到客户端的可移植性：* Apache MXNet 可在各种设备上运行，从 CPU 到 GPU 以及集群、服务器、台式机或手机。
- *多语言：* 您可以使用多种编程语言编程 Apache MXNet，从 Python、R、Scala、Julia、C++、Matlab 或 JavaScript。
- *性能优化：* Apache MXNet 具有优化的 C++ 后端引擎，可并行化计算和 I/O，而不论源语言如何。

Amazon EMR 允许您在 EMR 集群上配置 Apache MXNet。

## Apache Mahout

Mahout 是驾驶、控制和照顾工作大象的人。该项目因与 Hadoop（大象）和 mahout（骑大象）相关而得名。

*Apache Mahout* 是一个开源 ML 库，主要用于一些主要的 ML 案例，如分类、降维、推荐引擎和聚类。

了解和学习 Apache Mahout 超出了本书的范围，但就考试而言，它是一个可与 EMR 提供的分析库。

## Apache Flink

*Apache Flink* 是用于有界（有明确开始和结束）和无界（有明确开始但无结束）数据流的有状态计算的分布式流处理引擎。Flink 已设计为在集群环境中运行，并在低延迟和大容量下执行计算。

Apache Flink 因多种原因而受欢迎，但主要原因如下：

- *事件时间和处理时间语义：* 许多流处理引擎使用处理时间，但使用事件时间允许您处理顺序很重要时的乱序事件。处理时间通常用于极低延迟的应用程序，其中事件需要在到达的瞬间被处理。

