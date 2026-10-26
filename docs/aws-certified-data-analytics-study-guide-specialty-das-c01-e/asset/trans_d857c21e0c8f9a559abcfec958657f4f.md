

例如，本章我们介绍了 Amazon EMR，第 2 章“数据收集”中介绍了 AWS Glue。这两项服务都提供了 Apache Spark，这是目前最受欢迎的数据处理框架之一。同样，我们介绍了数据编排服务，而在 AWS 平台上也有许多编排服务。作为一名解决方案架构师，你应该能够根据使用场景指导客户和用户选择合适的技术。

完成后，请在开始下一章“数据可视化”前稍作休息。

## Exam Essentials

**了解数据处理和数据分析。** 数据处理和数据分析是考试中超过三分之一问题的主题，这使得本章非常重要。之所以将这两个主要领域放在同一章中，是因为我认为用于数据处理的许多技术也用于数据分析。

**熟悉相关的 Amazon 服务产品。** 从服务角度来看，我们查看了 Amazon Athena、Apache Presto、Amazon EMR、Apache Hive、Amazon Elasticsearch、Amazon Redshift 和 Kinesis Data Analytics。本章涵盖的内容很多，但仅此是不够的。强烈建议你查阅本章参考部分中建议的博客、文档和研讨会。

## Exercises

完成以下练习时，请参考相关服务的用户指南。

### EXERCISE 4.1: 创建 EMR 集群

请按照以下步骤创建 EMR 集群。

1. 转到 Service ➤ Analytics ➤ EMR。
2. 选择 Create Cluster。
3. 你有两种创建集群的选项：Quick Create 或 Advanced Options（提供更多控制和选择）。选择 Quick Create。
4. 为你的集群命名，例如 wiley-demo-chapter4。
5. 你可以勾选 Logging 选项。这将把所有集群日志记录到 Amazon S3。
6. 选择启动模式。

你有两种启动集群的模式：

- Cluster – 你创建一个长期运行的集群，并从下一步的应用程序列表中选择一组应用程序。
- Step Execution – 使用此选项，EMR 将创建集群，执行添加的步骤，并在步骤完成后终止。

7. 为本练习的目的，我们将选择 Cluster 模式运行。
8. 选择软件配置。选择 EMR Release。截至撰写本文时，最新版本为 emr-5.28.0。

你可以从列表中选择要在启动的集群上配置的应用程序：

- Core Hadoop: Hadoop 2.8.5 with Ganglia 3.7.2, Hive 2.3.6, Hue 4.4.0, Mahout 0.13.0, Pig 0.17.0, and Tez 0.9.2
- HBase: HBase 1.4.10 with Ganglia 3.7.2, Hadoop 2.8.5, Hive 2.3.6, Hue 4.4.0, Phoenix 4.14.3, and ZooKeeper 3.4.14
- Presto: Presto 0.227 with Hadoop 2.8.5 HDFS and Hive 2.3.6 Metastore
- Spark: Spark 2.4.4 on Hadoop 2.8.5 YARN with Ganglia 3.7.2 and Zeppelin 0.8.2

9. 为本练习，我们将选择 Core Hadoop。

你也可以选择 AWS Glue Data Catalog 用于表元数据。这将提供使用外部 Hive metastore 的选项，可与这些应用程序一起使用。

10. 硬件配置：

你必须为集群选择硬件配置，其中包括以下设置：

- Type of an instance
- Number of instances (One of the instances will be a master node, and the remaining will act as core nodes.)

11. 安全与访问配置：

- 你可以选择一个 EC2 密钥对。如果不选择 EC2 密钥对，你将无法通过 SSH 进入主节点。
- 你可以从两个权限级别中选择：
  - Default Permission – 这将使用默认 IAM 角色。如果角色不存在，将自动为你创建，并带有托管策略以实现自动策略更新。
    1. EMR role – EMR_DefaultRole
    2. EC2 instance profile – EMR_EC2_DefaultRole
  - Custom Permission – 你可以选择自定义角色以定制集群的权限。
    1. 选择一个 EMR 角色。
    2. 选择一个 EC2 实例配置文件。

12. 选择 Create Cluster 选项。启动集群大约需要 5–7 分钟。

### EXERCISE 4.2: 入门 - 配置和测试 Elasticsearch 域

请完成以下教程以使用 Amazon Elasticsearch Service 创建和配置测试域：

```
amzn.to/39lh9gt
```

### EXERCISE 4.3: 入门 - 托管 Elasticsearch 索引

在以下实验中，你将使用 Amazon Elasticsearch Service 创建一个域并在 Amazon Elasticsearch 中托管两个索引。然后你将使用 AWS 平台内的各种工具，如 ElastiCache、Logstash 和 Kibana，并了解一个真实世界的示例。

```
search-sa-log-solutions.s3-us-east-2.amazonaws.com/logstash/html/Lab_Guide_ABD326.html
```

```
bit.ly/ElasticSearchLab
```

### EXERCISE 4.4: 构建日志分析解决方案

在本实验中，你将使用 Kinesis Agent、Kinesis Firehose、Kinesis Analytics、Amazon Elasticsearch 和 Kibana 构建日志分析解决方案。完成此练习应需 60 分钟。

```
aws.amazon.com/getting-started/projects/build-log-analytics-solution
```

## Review Questions

1. 你作为一家总部位于西班牙马德里的大型时尚零售商的企业架构师工作。团队希望构建 ETL，并且有需要转换的大型数据集。数据来自多个来源，因此去重也是一个重要因素。在 AWS 上处理数据的最简单方法是什么？

- A. 将数据加载到 Amazon Redshift，并使用 SQL 构建转换。构建自定义去重脚本。
- B. 使用 AWS Glue 使用内置 FindMatches ML Transform 转换数据
- C. 将数据加载到 Amazon EMR，构建 Spark SQL 脚本，并使用自定义去重脚本。
- D. 使用 Amazon Athena 进行转换和去重。

2. 以下哪一项是在 Apache Hadoop 上的分布式数据处理选项，并且直到 Hadoop 2.0 之前一直是主要的处理引擎？

- A. MapReduce
- B. YARN
- C. Hive
- D. ZooKeeper

3. 你作为一家电信公司的顾问工作。数据科学家要求直接访问数据以深入研究数据结构并构建模型。他们对 SQL 有很好的了解。以下哪种工具将为他们提供直接访问数据并减少基础设施和维护开销，同时确保可以提供对 Amazon S3 上数据的访问？你会向他们推荐哪一种？（单选。）

- A. Amazon S3 Select
- B. Amazon Athena
- C. Amazon Redshift
- D. Apache Presto on Amazon EMR

4. 以下哪些文件格式受 Amazon Athena 支持？（三选一。）

- A. Apache Parquet
- B. CSV
- C. DAT
- D. Apache ORC
- E. Apache Avro
- F. TIFF

- C. Apache Pig on Amazon EMR
- D. Amazon Elasticsearch

9. 你希望构建一个数据仓库解决方案，能够灵活地在数据湖和数据仓库之间传输数据。以下哪种方式最具成本效益以满足你的需求？（单选。）

- A. 使用 S3 作为你的数据湖和 Amazon EMR 作为你的数据仓库。
- B. 使用 HDFS 作为你的数据湖和 Amazon Redshift 作为你的数据仓库。
- C. 使用 S3 作为你的数据湖和 Amazon Redshift 作为你的数据仓库。
- D. 使用 HDFS 作为你的数据湖和 Amazon EMR 作为你的数据仓库。

10. 以下哪些关于 Redshift Leader 节点的陈述是正确的？（二选一。）

- A. Redshift 集群可以有一个 Leader 节点。
- B. Redshift 集群可以有多个 Leader 节点。
- C. Redshift Leader 节点应比计算节点具有更多内存。
- D. Redshift Leader 节点与计算节点的规格完全相同。
- E. 你可以自行选择 Leader 节点大小，并且单独计费。
- F. Redshift Leader 节点是自动选择的，并且对用户免费。

## References

### Recommended Workshops

- Data Ingestion and Processing workshop: dataprocessing.wildrydes.com
- Incremental Data Processing on Amazon EMR (Apache Hudi): incremental-data-processing-on-amazonemr.workshop.aws/en
- Serverless Data Lake workshop: incremental-data-processing-on-amazonemr.workshop.aws/en
- Data Engineering 2.0: aws-dataengineering-day.workshop.aws/en
- Amazon Athena workshop: athena-in-action.workshop.aws
- Amazon EMR with Service Catalog: s3.amazonaws.com/kenwalsh testad/cfn/public/sc/bootcamp/emrloft.html
- Realtime Analytics and Serverless DataLake Demos: demostore.cloud
- Streaming Analytics workshop: streaming-analytics.workshop.aws/en

