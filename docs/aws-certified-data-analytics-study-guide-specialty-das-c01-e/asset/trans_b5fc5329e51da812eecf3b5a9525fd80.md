
您还可以使用免注册的联合查询，这对于快速原型开发非常有用。您只需将前缀 `"lambda:<function_name>"` 添加为目录名称，即可访问数据源。

例如，`SELECT * from "lambda:cmdb".ec2.ec2_instances`。

在撰写本书时，可用的连接器如下：

- HBase
- DocumentDB
- DynamoDB
- JDBC
- Redis/ElastiCache
- Amazon CloudWatch Logs
- Amazon CloudWatch Metrics
- TDPS Data Generator

您还可以使用 Amazon Athena Query Federation SDK 编写自己的数据源连接器。一个示例连接器可从 bit.ly/2YEm0og 获取。Amazon Athena Query Federation SDK 包含以下主要功能：

- S3 溢出
- 分区裁剪
- 并行扫描
- 可移植的列式内存格式（Apache Arrow）
- 授权
- 拥塞控制/避免

## Amazon Athena 自定义 UDF

Amazon Athena 现在增加了构建自定义 *用户定义函数（UDF）* 的能力，这些函数能够在数据访问期间对数据进行预处理或后处理、提供访问控制并添加自定义逻辑。您可以在 Amazon Athena 查询的 select 或 filter 阶段调用 UDF。Amazon Athena 自定义 UDF 提供以下功能：

- UDF 由 AWS Lambda 提供支持。
- UDF 支持网络调用。
- UDF 调用可用于查询的 select 和/或 filter 阶段。
- Athena 会优化性能，从而让您专注于业务逻辑。

## 在 Amazon Athena 中使用机器学习

Amazon Athena 允许您直接从 SQL 查询调用 ML 模型进行推理。您可以在 SQL 分析中使用机器学习进行异常检测、客户群组分析，

Presto、Apache Pig 以及其他一些项目。我们将在本章后面详细讨论其中一些项目。

## Apache Hadoop 概述

Apache Hadoop 是一个开源项目，网址为 www.hadoop.apache.org，它允许使用简单的编程模型跨计算机集群对大型数据集进行分布式处理。它旨在实现线性可扩展性、高可用性和容错能力。Apache Hadoop 是最受欢迎的开源项目之一，也是 Amazon EMR 的关键组成部分。

Apache Hadoop 的联合创始人 Doug Cutting 和 Mike Cafarella 将 Hadoop 的创建归功于 Google 发表的两篇论文。“Google 文件系统”发表于 2003 年，而“MapReduce：大型集群上的简化数据处理”发表于 2004 年。Hadoop 0.1 于 2006 年 4 月发布，此后彻底改变了大范围数据处理领域。

Apache Hadoop 包含以下主要模块：

- **Hadoop Common**：这些是 Apache Hadoop 项目中支持其他 Hadoop 模块的通用工具。
- **Hadoop 分布式文件系统（HDFS）**：HDFS 是一个分布式文件系统，可提供对数据的高吞吐量访问，并具有内在的数据复制能力（默认为 3 倍）。
- **Hadoop MapReduce**：MapReduce 是 Hadoop 最初的默认处理框架，后来分离为 YARN（资源处理）和 MapReduce（处理）。
- **YARN**：YARN 代表 *另一个资源协调器*，是一个用于在 Hadoop 集群中调度作业和管理资源的框架。

Apache Hadoop 成为一个流行的数据处理框架，有望取代传统的大规模数据处理系统，这些系统不仅效率低下，而且成本高昂。尽管该技术在规模和性能方面承诺良多，但其易用性仍有很多不足之处。希望取代传统内部数据处理系统的传统企业当时运行着许多不同的工作负载，而通过 MapReduce 将它们改造到 Hadoop 上，即使并非不可能，也显得极具挑战性。

例如，传统数据处理系统最常见的用户是那些更熟悉数据流语言和 SQL 的分析师，而用 Java 编程语言编写 MapReduce 并不是他们的强项。这促使了 Apache Hive 和 Apache Pig 等项目的诞生。

此外，数据科学家希望利用这个可扩展平台进行机器学习，但在此平台上开发、训练和部署模型变得困难重重，由此催生了 Apache Mahout 和 Spark ML 等项目。

由于开源项目如此丰富，生态变得异常拥挤，要部署一个包含必要项目的 Hadoop 平台变得困难重重，因为每个项目团队都在按照自己的发布周期独立工作。这造成了一个

Macie，具有使用 IAM 控制访问和权限以及使用 Kerberos 进行身份验证的能力。

Amazon EMR 使 Hadoop 集群的部署变得简单，让您能够在几分钟内启动 Hadoop 或 Spark 集群。您无需担心资源预置、集群设置、Hadoop 配置或集群调优。它使您能够在几分钟内预置数百或数千个计算实例。Amazon EMR 可以使用自动扩展功能自动扩展您的 Hadoop 或 Spark 集群，以处理任意大小的数据，并在作业完成时将其缩减。这有助于避免为未使用的容量付费，从而节省成本。

您还可以通过承诺使用预留实例来节省长期运行的 Hadoop 集群的成本，最高可节省 75% 的费用；对于间歇性工作负载，您可以利用备用计算容量，通过 Spot 实例最高可节省 90% 的费用。使用 Amazon EMR，成本节省来源于多种因素：

- 管理和支持 Hadoop 集群的管理开销更少
- 无需为硬件采购或软件安装支付前期成本
- 通过节省数据中心空间、电力和散热费用来降低运营支出
- 由于减少了延迟成本、能够使用最新最好的软件而具有更强的竞争能力，从而更快实现业务价值；同时，由于最注重安全的组织的需求，云中的安全性也得到了改进

## Amazon EMR 上的 Apache Hadoop

我在本章前面简要讨论了 Apache Hadoop。现在让我们来看看 Hadoop 的架构、主要用例，以及 Hive、Tez、Pig、Mahout 等生态系统项目，以及对 Apache Spark 的需求。

Hadoop 采用主从架构进行分布式存储和计算，主节点称为 *名称节点*，从节点称为 *数据节点*。该架构源于大规模数据处理领域的 *大规模并行处理（MPP）* 世界，一些数据库（如 Teradata）已经在类似的架构框架上运行。MPP 架构的目标是将存储和计算分散到多台机器（数据节点）上，并通过单个主节点（名称节点）进行管理。

名称节点（主节点）负责管理命名空间，例如整个文件系统元数据，以及节点的位置和特定文件的块位置。这些信息在系统启动时从数据节点重建。由于名称节点的重要性，考虑到它拥有文件和块的位置映射，使其能够从故障中恢复非常重要。这通常通过备份文件系统元数据或运行辅助名称节点进程（通常在不同的机器上）来实现。然而，辅助名称节点在发生故障时并不提供高可用性。您需要启动一个新的名称节点来恢复失败的名称节点，并使用元数据副本重建名称节点，这一过程可能需要 30 分钟到 1 小时。
