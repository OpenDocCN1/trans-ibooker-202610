

## 安全配置

你还可以定义安全配置，以配置数据加密、Kerberos 认证以及用于 EMRFS 的 Amazon S3 授权。该安全配置可用于多个 EMR 集群。安全配置可以通过 AWS 控制台、AWS CLI 和 AWS SDK 创建，也可以通过 CloudFormation 模板创建。

对于数据加密，你可以指定以下内容：

-   静态数据加密
-   动态/传输中数据加密

Amazon EMR 的静态数据存储在不同位置，包括本地磁盘和通过 EMRFS 的 S3。

你还可以通过为特定应用程序选择适用于传输中数据的开源加密功能选项来选择传输中加密。可用的传输中加密选项可能因 EMR 发行版而异。

对于认证，你可以使用 Kerberos 主体为集群上某些应用程序组件之间的交互启用 Kerberos 认证。对于大多数开发和测试工作负载以及一些生产工作负载，你可以选择让 EMR 在集群的主节点上安装 KDC 服务器，或者配置外部 KDC。对于生产工作负载，客户通常更喜欢使用外部 KDC，这需要你与 EMR 集群共享 KDC 详细信息。

你还可以配置 Lake Formation 集成，并使用企业凭证和 Lake Formation 权限来控制对数据目录和底层数据存储的访问。您必须创建数据湖并在 Lake Formation 中配置相关权限。

图 4.6 和图 4.7 展示了如何在 EMR 中创建安全配置。

## EMR 笔记本

Jupyter 笔记本已成为数据工程师和数据科学家处理数据的最常见方式之一。虽然你可以在 EMR 集群内配置 Jupyter 笔记本，但 AWS 宣布了 EMR 笔记本功能，这意味着你可以使用基于 Jupyter 的 EMR 笔记本，通过实时代码、叙述文本、可视化等交互式分析数据（见图 4.8）。你可以创建笔记本并将其附加到运行 Hadoop、Spark 和 Livy 的 Amazon EMR 集群。笔记本免费运行，并独立于集群保存在 Amazon S3 中。

在 Amazon EMR 中创建笔记本时，你可以选择连接到现有集群或创建新集群。从笔记本界面创建新集群通常是在开发/测试场景中进行的。

你还可以将笔记本链接到 Git 仓库。

**图 4.6 EMR 中的安全配置（加密）**

![](img/fc6a7083d65b0716921d7bd4a5298460_209_0.png)

## Amazon EMR 上的 Apache Hive 和 Apache Pig

如前面各节所述，Apache Hadoop 已迅速成为数据中心领域数据处理的事实标准；然而，MapReduce 作为一种编程模型的复杂性意味着人们希望使用传统编程语言（如 SQL 和数据流语言），这导致了 Apache Hive（起源于 Facebook）和 Apache Pig（起源于 Yahoo）等项目的兴起。

### Apache Hive 概述

我们在本章前面的“Amazon Athena”一节中已经提到了 Apache Hive。Apache Hive 是一个运行在 Hadoop 上的开源类 SQL 数据仓库解决方案，专为希望使用 Hadoop 框架但又希望受益于类 SQL 语言的强大性和简单性的 SQL 分析师、SQL 开发人员和数据科学家而构建。

Apache Hive 相对于 MapReduce 编程范式有一些优点和缺点，其中部分内容见表 4.4，这将帮助你回答考试问题。

**表 4.4 Apache Hive 的优缺点**

|  | Hive | MapReduce |
|---|---|---|
| 技能 | 分析师可以继续使用他们的 SQL 技能，无需学习新语言。 | 对于不熟悉 MapReduce 范式和 Java 语言的人来说，学习和用 MapReduce 表达新问题可能具有挑战性。 |
| BI 集成 | 您可以使用 JDBC/ODBC 驱动程序将 BI 工具连接到 Hive。 | 现有工具无法连接。 |
| S3 集成 | 与 S3 原生集成，包括使用分区 | 支持 S3 集成 |
| UDF | 您可以使用和移植现有的 UDF。 | 重写函数，但使用现有的 Java 函数。 |
| 数据转换能力 | 仅限于 SQL | Java 语言的完整转换能力 |

**图 4.7 EMR 中的安全配置（EMRFS 的认证和 IAM 角色）**

![](img/fc6a7083d65b0716921d7bd4a5298460_210_0.png)

-   Pig 执行环境——分析师/程序员开发的 Pig 脚本在执行环境中执行。你有两个选项：
    -   在 JVM 中本地运行脚本。
    -   在 Hadoop 集群上的分布式执行环境中运行脚本。

Pig 是一种用于大型数据集的可扩展语言，使数据科学家、工程师和分析师的数据分析过程更加简单。Pig 支持表 4.5 和表 4.6 中所示的数据类型。

**表 4.5** Apache Pig – 简单数据类型

| 数据类型 | 描述 |
|---|---|
| int | 有符号 32 位整数（示例：10） |
| long | 有符号 64 位整数（示例：10L） |
| float | 32 位浮点数（示例：10.5F） |
| double | 64 位浮点数（示例：10.5） |
| chararray | UTF-8 格式的字符数组字符串（“Hello World”） |
| bytearray | 字节数组（二进制长对象） |
| boolean | 布尔值（true/false） |
| datetime | 日期时间（1970-01-01T00:00:00+00:00） |
| biginteger | Java 大整数（20000000000） |
| bigdecimal | Java 大十进制数（33.4567812345） |

来源：bit.ly/2sG6zQs

**表 4.6** Apache Pig – 复杂数据类型

| 数据类型 | 描述 |
|---|---|
| tuple | 有序字段集（19,2） |
| bag | 元组集合 {（19,2），（23,1）} |
| map | 一组键/值对 |

来源：bit.ly/2sG6zQs

下面的代码示例演示了加载“student”数据，然后打印他们的姓名和年龄：

```
dataset = LOAD 'students' USING PigStorage() as (name:chararray, age:int, gpa:float);
studentsage = FOREACH dataset GENERATE name, age;
DUMP studentsage;
(Roger,25)
(Tom,22)
(Cristina,29)
(Steffen,26)
(Vincent,29)
```

从考试的角度来看，你需要了解与传统的 MapReduce 程序相比，Apache Pig 和 Apache Hive 在哪些场景下更有用。

## Amazon EMR 上的 Apache Presto 概述

我们已经在“Amazon Athena”一节中讨论了 Apache Presto，你也许记得，Apache Presto 是 Amazon Athena 背后的技术。你也可以将 Apache Presto 作为 EMR 安装的一部分来运行。数据科学家和数据分析师使用 Apache Presto 来寻求快速的交互式查询响应。其优缺点列于表 4.7 中，这将帮助你回答有关该技术的考试问题。

**表 4.7 Apache Presto 的优缺点**

| 优点 | 缺点 |
|---|---|
| 基于 SQL，因此你可以使用自己的 SQL 技能 | 在大型表的连接规划期间需要优化 |
| 低延迟响应 | 不适合批处理 |
| 符合 JDBC/ODBC | 容错性有限（重新运行失败的作业） |
| BI 集成 | 内存要求高 |
| 支持 UDF |  |
| 联合查询（Hive/Cassandra/关系数据库/对象存储） |  |

-   执行器
-   任务

**图 4.10 Apache Spark 架构**

![](img/fc6a7083d65b0716921d7bd4a5298460_215_0.png)

### 驱动程序

所有 Spark 应用程序都由一个驱动程序组成，该程序运行程序的 main() 函数，通常位于集群的某个节点上。驱动程序“驱动”Spark 应用程序，基本上是应用程序的大脑，维护着关于程序的所有相关和关键信息。它有三个主要职责：

-   维护有关应用程序的所有信息。
-   响应用户的输入。
-   分析、分发和调度跨执行器的工作。

### 集群管理器

集群管理器通常是一种服务，用于代表应用程序获取集群上的资源。Apache Spark 支持多种不同的集群管理器：

-   Standalone——Apache Spark 发行版附带一个简单的集群管理器，包含在 Spark 中，使集群的设置变得容易。
-   Apache Mesos——Apache Mesos 是一个通用集群管理器，在开源界非常流行，允许在计算机集群上运行的应用程序之间进行有效的资源共享。Apache Mesos 还可以运行 Hadoop MapReduce 应用程序。
-   Hadoop YARN——Hadoop YARN 是 Hadoop 集群的默认资源管理器，支持运行 Spark。
-   Kubernetes——Kubernetes 是一个开源系统，用于自动化、扩展和管理容器化应用的部署。

