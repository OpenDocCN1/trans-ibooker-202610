
| 服务类别 | 服务名称 | 描述 |
|---|---|---|
| 基础设施保护 | AWS Shield | AWS Shield 是一种分布式拒绝服务保护服务，通过始终开启的检测和自动内联缓解来保护在 AWS 上运行的应用程序，从而最大限度地减少应用程序停机时间和延迟。 |
|  | AWS Web Application Firewall (WAF) | AWS Web Application Firewall 用于保护你的 Web 应用程序或 API 免受常见的 Web 攻击，从而提高 Web 应用程序的整体可用性。 |
|  | AWS Firewall Manager | AWS Firewall Manager 是一项安全管理服务，允许你在 AWS Organizations 中的账户和应用程序之间集中配置和管理防火墙规则。 |
| 数据保护 | Amazon Macie | Amazon Macie 是一项由机器学习驱动的安全服务，可帮助发现、分类和保护 AWS 中的敏感数据，如 PII 信息或知识产权；它为客户提供仪表板和警报，以了解这些数据被访问或移动的方式。 |
|  | AWS Key Management Service | AWS KMS 使加密密钥的创建、管理和控制更加简单，并且可以与 CloudTrail 集成，为你的监管和合规需求提供日志。 |
|  | AWS CloudHSM | AWS CloudHSM 是一种基于云端的硬件安全模块（HSM），使你能够轻松地在 AWS 云中生成和使用自己的加密密钥。 |
|  | AWS Certificate Manager | AWS Certificate Manager 是一项服务，允许你配置、管理和部署公有和私有 SSL/TLS 证书，用于 AWS 服务和你的内部连接资源。 |
|  | AWS Secrets Manager | AWS Secrets Manager 用于轮换、管理和检索机密，如数据库凭证、API 密钥等；它提供与 Amazon Redshift、Amazon DocumentDB 和 Amazon RDS 的内置集成。 |

主节点的默认托管安全组之一称为 ElasticMapReduce-master，它预先配置了一条允许来自所有来源（IPv4 0.0.0.0/0）的入站 SSH 流量（端口 22）的规则。作为最佳实践，你应该仅将入站流量限制为受信任的来源，因此应指定自定义安全组来实现这一点。

Amazon EMR 为主实例和任务实例提供不同的托管安全组。当你在私有子网中创建集群时，将额外创建一个用于服务访问的托管安全组。Amazon EMR 将使用默认托管安全组，并在需要时创建它们。

默认托管安全组如表 6.4 所述。

| 安全组名称 | 安全组描述 |
|---|---|
| ElasticMapReduce-master | 公有子网中主实例的默认安全组 |
| ElasticMapReduce-slave | 公有子网中核心实例和任务实例的默认安全组 |
| ElasticMapReduce-Master-Private | 私有子网中主实例的默认安全组 |
| ElasticMapReduce-Slave-Private | 私有子网中核心实例和任务实例的默认安全组 |
| ElasticMapReduce-ServiceAccess | 私有子网中服务访问的默认安全组。它没有入站规则，仅允许通过 HTTPS（8443）的出站流量。这些规则允许集群管理器与主节点以及核心节点和任务节点通信。 |

## 附加安全组（可选）

附加安全组是可选的，在需要定义由你提供和管理的自定义访问规则时定义。Amazon EMR 不会在附加安全组中添加或删除规则。

## 私有子网

私有子网与公有子网在以下方面有所不同：

- 提供 VPC 端点的 AWS 服务可以通过该端点访问；但是，不提供端点的服务必须使用 NAT 实例或 Internet 网关。例如，如果你正在使用 EMRFS 功能，则需要具有 Amazon S3 VPC 端点；如果你正在使用 DynamoDB，则需要提供从私有子网到 DynamoDB 的路由。
- 你应该提供到 Amazon EMR 服务日志桶和 S3 中 Amazon Linux 存储库的路由。
- 为了调试，请提供从私有子网到 Amazon SQS 端点的路由。

图 6.3 描述了在私有子网中运行的 Amazon EMR 的示例架构图。

图 6.3 私有子网中的 Amazon EMR

![](img/fc6a7083d65b0716921d7bd4a5298460_331_0.png)

当你打开 EMR 控制台，甚至在创建 EMR 集群之前，你看到的三个与安全相关的关键内容是安全配置、VPC 子网和阻止公共访问（见图 6.4）。

**图 6.4** Amazon EMR 控制台内的安全选项

![](img/fc6a7083d65b0716921d7bd4a5298460_332_0.png)

在我们查看 Amazon EMR 集群内的其他安全选项之前，先来看看这些内容。

## 安全配置

Amazon EMR 允许你创建可应用于多个集群的安全配置。配置设置可分为四大类：

- 加密
- 身份验证
- EMRFS 的 IAM 角色
- AWS Lake Formation 集成

让我们更详细地了解每一项。

### 加密

数据加密是必要的，以确保数据仅对经过身份验证和授权的用户可见。EMR 可以通过 EMRFS 处理 S3 上的数据，以及存储在 HDFS 中的数据，也称为*静态数据*。EMR 还需要通过线路传输数据，也称为*传输中数据*。为了保护你的数据免遭窥探，你需要加密静态数据和传输中数据。Amazon EMR 的安全配置选项提供三种数据加密选项：S3 加密、本地磁盘加密和传输中数据加密。

**S3 加密：为 Amazon S3 中的数据启用静态加密**

Amazon S3 的加密模式适用于从 Amazon S3 读取和写入的 EMR 文件系统（EMRFS）对象。你可以在此配置选项中定义服务端加密（SSE）或客户端加密。可用选项如下：

- SSE-S3
- SSE-KMS（你需要提供你的 KMS 客户主密钥以在服务端加密数据。）
- CSE-KMS（你需要提供你的 KMS 客户主密钥以在客户端加密数据。）
- CSE-Custom（你必须提供自定义密钥提供程序位置 [S3 存储桶] 和密钥提供程序类。）

Amazon EMR 的配置选项还允许你为特定存储桶选择加密覆盖。你可以通过指定存储桶名称和要应用于该存储桶的加密模式，为每个存储桶指定不同的加密模式和加密材料。

**本地磁盘加密**

Amazon EMR 有三种类型的本地存储，它们使用各种机制进行加密，并协同工作为你的静态数据提供最佳安全性。

**HDFS 加密**

HDFS 不仅用于间歇性存储，还用于从实例存储卷和附加到实例的 EBS 卷读取和写入数据。当你启用本地磁盘加密时，会激活许多 Hadoop 加密选项：

**安全 Hadoop RPC（远程过程调用）** 此选项控制 `core-site.xml` 中的 Hadoop `.rpc.protection` 设置以激活数据保护，并用于在 Hadoop 服务和客户端之间加密数据。

**HDFS 块数据传输上的数据加密** 此选项允许我们在数据节点上配置数据加密。在 `hdfs-site.xml` 中，`dfs.encrypt.data.transfer` 设置为“true”，并配置为使用 AES-256 加密。

**Amazon EMR 上的透明数据加密** 虽然此配置中的设置不会在 Amazon 中激活透明数据加密，但你可以手动配置。透明加密通过使用 HDFS 加密区域来实现，加密区域是你定义的 HDFS 路径。每个加密区域都有自己的密钥，该密钥存储在通过“hdfs-site”配置分类指定的密钥服务器中。Amazon EMR 默认使用 KMS，但只要你使用实现了 KeyProvider API 操作的不同密钥管理服务，也可以使用它。

