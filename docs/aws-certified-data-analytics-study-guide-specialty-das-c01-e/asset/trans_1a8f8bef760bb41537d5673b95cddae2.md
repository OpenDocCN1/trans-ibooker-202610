

## Task Runner

任务运行是一个持续轮询新任务然后执行它们的流程。*Task Runner* 会安装在由你的管道定义创建的资源上并自动运行。当 Task Runner 安装并配置好后，它会轮询已激活的 AWS 管道以获取任务。一旦任务被分配，Task Runner 就会执行该任务并将状态报告回 AWS Data Pipeline。需要记住的是，虽然一个管道可能被评估为多个任务，但单个任务是一个独立的工作单元。Task Runner 的总体流程接下来会进行说明。

AWS Data Pipeline 可以与以下服务配合进行数据转换：

- Amazon EC2
- Amazon EMR

AWS Data Pipeline 与以下服务配合存储数据：

- Amazon DynamoDB
- Amazon RDS
- Amazon Redshift
- Amazon S3

AWS Data Pipeline 可以通过以下接口访问：

- AWS Management Console
- AWS CLI
- AWS SDK
- Query API：提供使用 HTTPS 请求调用的低级 API。这是访问 AWS Data Pipeline 的最直接方式之一，但它要求你的应用程序处理低级细节。这可能很繁琐，但 AWS Data Pipeline 提供了关于此的良好文档。

AWS Data Pipeline 还有一些其他概念，也在用户指南中进行了介绍：

- **Data nodes：** 数据节点定义了管道用作输入或输出的数据的位置和类型。支持以下数据节点：
  - DynamoDBDataNode
  - SqlDataNode
  - RedshiftDataNode
  - S3DataNode
- **Databases：** AWS Data Pipeline 支持以下数据库：
  - JdbcDatabase – 任何符合 JDBC 的数据库
  - RdsDatabase – 任何 RDS 数据库
  - RedshiftDatabase
- **Activities：** 在 AWS Data Pipeline 中，活动实际上是执行的工作。AWS Data Pipeline 提供了多个预打包的活动，以适应客户现场常见的场景：
  - CopyActivity – 将数据从一个位置复制到另一个位置
  - EmrActivity – 运行 Amazon EMR 集群
  - HiveActivity – 在 Amazon EMR 集群上运行 Hive 查询
  - HiveCopyActivity – 在 Amazon EMR 集群上运行 Hive 查询，并支持高级过滤和 S3 数据节点支持
  - PigActivity – 在 Amazon EMR 集群上运行 Pig 脚本
  - RedshiftCopyActivity – 将数据复制到和从 Amazon Redshift 表中复制
  - ShellCommandActivity – 运行自定义 Unix/Linux shell 命令活动
  - SqlActivity – 在数据库上运行 SQL 查询
- **Preconditions：** 在开始活动之前，经常需要检查某些条件。例如，你可能想要检查某个 DynamoDB 表是否存在，或者在运行下游处理之前检查某个特定日期的某个特定 S3 Key 是否存在。AWS Data Pipeline 支持两种主要类型的先决条件：
  - 系统管理的先决条件由 AWS Data Pipeline 代表你运行，并且不需要计算资源。以下是系统管理先决条件的一些示例：
    - DynamoDBDataExists
    - DynamoDBTableExists
    - S3KeyExists
    - S3PrefixNotEmpty
  - 用户管理的先决条件：
    - Exists – 检查数据节点是否存在
    - ShellCommandPrecondition – 作为先决条件运行自定义 Unix/Linux 命令
- **Resources：** 活动需要资源来执行。例如，像 PigActivity 或 EmrActivity 这样的活动需要像 EMR 这样的计算资源可用。AWS Data Pipeline 支持以下类型的资源：
  - Ec2Resource
  - EmrCluster
- **Actions：** 在管道执行期间，步骤可能会成功、失败或延迟。当此类事件发生时，你需要采取适当的行动。AWS Data Pipeline 满足了管道构建的这一需求，并支持以下操作：
  - SnsAlarm – 此操作根据 onSuccess、onFail 和 onLateAction 事件向主题发送 SNS 通知。
  - Terminate – 此操作触发挂起或未完成的活动、资源或数据节点的取消。

## Large-Scale Data Transfer Solutions

我们已经看到了许多不同的技术，可以在数据量较低到中等时从不同的数据源收集数据。然而，当你遇到需要迁移大量数据的情况时，例如数据大小超过多个 TB 或 PB，通过 Internet 连接传输数据可能需要极其大量的时间。也有一些场景需要便携式数据传输，但需要传输多个 TB 的数据。这些就是你需要便携式解决方案如 AWS Snowcone 或大规模数据传输选项如 AWS Snowball 和 AWS Snowmobile 以及连接选项如 AWS Direct Connect 的场景。让我们更详细地了解一下这些选项。

### AWS Snowcone

AWS Snowcone 是 AWS Snow 家族中最小的成员，重量轻至 2.1 kg，并为你提供 8 TB 的可用存储。其小巧的尺寸使其非常适合狭小空间或便携性是关键的情况。它在许多场景中都很有用，包括组织希望将多个 TB 的数据迁移到云端但缺乏足够的网络带宽的情况，或在数据所在位置几乎没有或完全没有连接的情况下。

此外，AWS Snowcone 在医疗物联网中可能很有用，可用于在医疗机构或偏远医疗区域进行数据收集和数据处理。它可用于实时从急救车辆传输关键数据。

### AWS Direct Connect

AWS Direct Connect 让你能够在你的网络与 AWS Direct Connect 位置之一之间建立专用网络连接。使用工业标准 802.1Q VLAN，此专用连接可以划分为多个虚拟接口。这使你能够使用同一连接通过公共 IP 地址空间访问公共资源，例如存储在 Amazon S3 中的对象，并通过私有 IP 空间访问私有资源，例如在 Amazon Virtual Private Cloud (VPC) 中运行的 Amazon EC2 实例，同时在公共和私有环境之间保持网络隔离。虚拟接口可以随时重新配置以满足你不断变化的需求。

AWS Direct Connect 的主要优势如下：

- 降低你的带宽成本
- 一致的网络性能
- 与所有 AWS 服务兼容
- 与你的 Amazon VPC 的私有连接
- 在 1 Gbps 到 10 Gbps 之间弹性扩展

> **Exam Tip**
>
> AWS Direct Connect 从考试角度来看是一个重要主题，因此你对 AWS Direct Connect 的理解将有助于考试准备。

## Summary

在本章中，我们讨论了在将数据摄取到 AWS 云时数据管道的各个组件。我们查看了使用 AWS IoT 从物联网设备摄取数据、使用 Amazon Kinesis 流式传输数据、使用 AWS Glue 从数据库和其他数据源传输数据，以及使用 AWS Database Migration Service 从数据库迁移大规模数据并捕获增量更改。我们还查看了用于事件的 Amazon SQS、用于过大而无法通过标准 Internet 连接传输的数据传输的 Amazon Snowball 和 Snowmobile，以及用于在你的数据中心和 AWS 之间建立直接连接的 AWS Direct Connect。

现在数据已在 AWS 上可用，下一章将介绍使用我们可用的各种工具处理数据。
