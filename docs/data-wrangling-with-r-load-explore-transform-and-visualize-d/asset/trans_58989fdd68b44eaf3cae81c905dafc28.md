
- Amazon S3 服务器日志用于以细粒度细节捕获日志（生命周期转换事件、身份验证失败等）。这些日志具有对象大小、HTTP 引用者等额外字段。除日志存储成本外没有额外费用。

- AWS CloudTrail 事件（S3 API 调用）通过 CloudTrail 捕获对象和存储桶的 API 操作。日志可以交付到多个位置并转发到 Amazon CloudWatch 以进行进一步分析。除日志存储成本外，CloudTrail 事件捕获还有额外费用，但它与其他系统具有更好的集成能力。

请参考以下文档页面了解 AWS CloudTrail 与 Amazon S3 服务器日志之间的区别：https://docs.aws.amazon.com/AmazonS3/latest/userguide/logging-with-S3.html。

- Amazon S3 Inventory 可帮助您定期导出包含 S3 存储桶或前缀中对象列表的 CSV/ORC/Parquet 格式文件。该报告可用于分析对象的复制和加密状态以确保数据安全。清单文件可使用 Amazon Athena 通过 SQL 查询来识别 S3 存储桶中的监管或合规问题。

## 监控、发现和识别安全问题

以下服务可以帮助您监控、发现和识别数据湖中的安全问题：

- AWS Config 帮助用户持续监控 AWS 资源并审计配置更改。用户可以在 AWS Config 上配置规则以监控并标记不符合配置规则的资源。您可以设置 AWS Config 以验证组织内所有存储桶的公共读/写配置。请参考以下博客文章了解如何使用 AWS Config 识别和更新账户中 S3 存储桶的公共设置：https://aws.amazon.com/blogs/security/how-to-use-aws-config-to-monitor-for-and-respond-to-amazon-s3-buckets-allowing-public-access/。

- AWS Trusted Advisor 扫描您账户上的 AWS 基础设施并提供遵循最佳实践的建议。它可用于识别数据湖配置中的任何问题并适当修复。这适用于 AWS 企业级支持客户。Trusted Advisor 提供优化的建议，帮助以下方面：
  - 成本优化：识别未分配、未充分利用和空闲的 AWS 资源以帮助用户优化成本。
  - 性能：通过分析 AWS 资源的吞吐量、延迟和计算使用情况提供改进性能的建议。
  - 安全：识别并提供有关暴露的凭据、安全组漏洞、S3 存储桶权限更开放等问题的建议。
  - 容错：识别并突出显示 EC2 自动扩展、健康检查、数据备份等。
  - 服务限制：AWS 对不同服务有服务限制。它们会在用户达到服务限制的 80% 时发出提示，以便更好地做好准备。

- Amazon Macie 通过在 S3 存储桶中使用机器学习模式匹配来识别敏感数据。Macie 还支持对 S3 存储桶进行自动敏感数据发现。请参考以下帖子了解可使用 Amazon Macie 在多个账户中使用的自动化敏感数据发现框架：https://aws.amazon.com/blogs/security/use-amazon-macie-for-automatic-continual-and-cost-effective-discovery-of-sensitive-data-in-s3/。

- AWS GuardDuty 是一种威胁检测服务，可持续监控您的 AWS 账户中的可疑活动。您可以使用 GuardDuty 监控 S3 管理事件和数据事件以识别安全问题。将生成报告以便用户采取行动。一些常见用例如下：
  - 识别存储在 EBS 存储卷中的文件的问题
  - 通过分析 EKS 审计日志识别容器部署的问题
  - 识别受损凭据、可疑登录和访问模式等

## AWS Lake Formation 集成

AWS Lake Formation 为数据湖提供细粒度访问控制，允许您向 Data Catalog 资源、Amazon S3 位置和底层数据上的各个主体授予权限。这实现了对数据访问和用户操作的确切控制。

在 Lake Formation 中执行细粒度访问控制有两种方法：

- **IAM 访问控制**：推荐的方法涉及向 Data Catalog 资源、S3 位置和底层数据上的各个主体授予 IAM 权限。此方法提供对数据访问的最大控制。
- **IAM 和 Lake Formation 权限**：此方法将 IAM 权限与 Lake Formation 权限相结合。它可用于强制执行额外的安全控制，例如行级访问控制。

配置安全规则时，请考虑以下因素：

- **数据安全要求**：确定数据所需的安全级别，包括防止未经授权的访问、修改或删除
- **数据访问要求**：确定谁需要访问数据以及他们应能够执行的操作
- **数据治理要求**：考虑需要强制执行的任何数据治理策略

要有效配置安全规则，请遵循以下提示：

- 从最小权限原则开始，仅向需要的人员授予权限以减少攻击面
- 通过授予对特定资源（如表或列）的访问权限来使用细粒度权限，以获得对数据访问的更多控制
- 通过将权限分配给角色，然后将这些角色分配给用户或组来实现 **基于角色的访问控制 (RBAC)**，以简化安全管理
- 启用审计日志记录以跟踪数据访问和用户采取的操作，帮助排查安全事件

通过考虑这些因素并遵循安全规则配置的最佳实践，您可以有效控制对数据湖的访问，降低安全风险并强制执行数据治理策略。

## 数据保护

数据必须在必要时通过安全和加密进行保护。在适用情况下对 S3 存储桶和数据库中的数据进行加密。敏感数据可在必要时进行标记化以增加另一层安全性。标记化数据除非您有权访问标记化服务器，否则无法逆向工程为原始数据项。请参考以下 Protegrity/Athena 博客了解在 AWS 中实施标记化解决方案的更多详细信息：https://aws.amazon.com/blogs/apn/data-tokenization-with-amazon-athena-and-protegrity/。

为实现强大的安全性，我们需要启用额外的 S3 数据保护选项以防止恶意或意外删除：

- **启用对象版本控制**：对象版本控制在每次更改时在存储桶中保留对象的多个版本。当对象被删除时，会插入删除标记，对象不会永久从存储桶中移除。这有助于在出现问题时回滚到对象的先前版本。
- **启用 MFA 删除以防止意外删除**：此功能通过要求删除或更改对象状态时进行两种形式的身份验证来帮助防止意外文件删除。
- **阻止公共访问**：为数据湖存储桶启用此功能以避免每个人都能访问存储桶。这在存储桶策略和其他访问控制机制之外增加了另一层安全性，以保护数据湖对象。
- **对象锁定**：对象锁定可防止对象在一定时间内被删除或覆盖。此功能有助于为监管合规性存储对象（**一次写入多次读取 – WORM**）
- **强制使用 TLS 连接**：您可以通过使用存储桶策略和加密数据存储来实现这一点。

请参考以下 Amazon S3 博客了解有关保护数据的最佳实践的更多信息：https://aws.amazon.com/blogs/security/top-10-security-best-practices-for-securing-data-in-amazon-s3/。

对于 Redshift/RDS 数据库，最佳实践是使用 IAM 身份验证以增强安全性。或者，您可以使用 AWS Secrets Manager 安全地存储数据库用户名/密码并自动轮换密钥以提高安全态势。Redshift 原生支持行级和列级过滤器以提供细粒度访问控制。对于其他自定义数据库，您可以利用视图来强制执行安全：https://aws.amazon.com/blogs/big-data/achieve-fine-grained-data-security-with-row-level-access-control-in-amazon-redshift/。

关于 Redshift，请参考以下文档了解有关安全的更多信息：https://docs.aws.amazon.com/redshift/latest/dg/c_security-overview.html。

## 在 AWS Glue 中保护您的数据

在上一章中，我们学习了如何使用 AWS Glue，它提供了一种简便高效的方式来准备和转换数据以进行分析。在本节中，我们将探讨如何使用 AWS Glue 来保护数据：

- **使用静态数据加密**：保护数据最关键的步骤之一是对其进行加密。AWS Glue 为静态数据提供多种加密选项。您可以使用 Amazon S3 服务器端加密在将数据存储到 S3 之前对其进行加密。您
