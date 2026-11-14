

## L

- 在其中运行受信任代码，使用 `AWS Signer`，[问题](#ch05.html#ix_Lmbdatrstcode)–[问题](#ch05.html#idm46022856948768)
- 900 秒后超时，[讨论](#ch05.html#idm46022856808320)
- 使用其转换数据，[讨论](#ch07.html#idm46022855864848)
- 对于长时间运行的作业，改用 EventBridge 而非它，[讨论](#ch06.html#idm46022856098784)
- 登陆区，[讨论](#ch09.html#idm46022854751520)
- 最小权限访问
    - 基于访问模式实施，[问题](#ch01.html#ix_LPAgen)–[挑战](#ch01.html#idm46022860474128)
    - 最小权限原则，[讨论](#ch01.html#idm46022860561088)
- 库，使用 Lambda Layers 打包，[问题](#ch05.html#ix_libpkg)–[挑战](#ch05.html#idm46022857158112)
- 生命周期策略 (S3)，使用它们来降低存储成本，[问题](#ch03.html#ix_lifeS3)–[挑战 2](#ch03.html#idm46022858813904)
- Lightsail，使用它部署容器，[问题](#ch06.html#ix_Ltsldep)–[挑战](#ch06.html#idm46022856385152)
- Linux，在其上安装 `Docker`，[Linux](#ch06.html#idm46022856602464)
- 负载均衡器
    - 使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS，[问题](#ch02.html#ix_ldbal)–[挑战](#ch02.html#idm46022859202352)
    - AWS 提供的 `ALB` 之外的类型，[讨论](#ch02.html#idm46022859215632)
- 日志记录
    - 捕获在 ECS 上运行的容器的日志，[问题](#ch06.html#ix_logctnrECS)–[挑战](#ch06.html#idm46022856013424)
    - CloudTrail，为您的账户启用它，[先决条件](#ch01.html#idm46022860513664)
    - 删除所有匹配文本模式的日志组，[快速修复](#app01.html#idm46022854706048)
    - 为 AWS 账户启用 CloudTrail 日志记录，[问题](#ch09.html#ix_logAWSacct)–[挑战](#ch09.html#idm46022854968352)
    - Glue 爬网程序自动将信息记录到 CloudWatch Logs，[验证检查](#ch07.html#idm46022855790032)
- 登录配置文件，为用户创建，[步骤](#ch01.html#idm46022860425824)
- 低代码开发平台 (LCDP)，[讨论](#ch07.html#idm46022855636864)

## M

- 机器学习 (ML)（请参阅 AI/ML）
- MacOS，在其上安装 `Docke Desktop`，[MacOS](#ch06.html#idm46022856615104)
- 管理账户，[讨论](#ch09.html#idm46022854764400)
- 元数据，[讨论](#ch07.html#idm46022855700336)
    - 使用 AWS Glue 爬网程序自动发现，[问题](#ch07.html#ix_metaautoGlcr)–[挑战](#ch07.html#idm46022855772304)
    - EC2 实例，从中检索公有 IP，[验证检查](#ch02.html#idm46022859567376)
    - EC2 实例，[步骤](#ch01.html#idm46022860130528)
- 指标
    - CloudWatch 上的自动扩缩指标，[讨论](#ch06.html#idm46022856175536)
    - AWS 控制台上的 ECS 服务指标，[验证检查](#ch06.html#idm46022856184864)
    - 使用 Storage Lens 观察 S3 存储指标，[讨论](#ch03.html#idm46022858598416)
- 数据库迁移
    - 使用 DMS 将数据库迁移到 Amazon RDS，[问题](#ch04.html#ix_migrDB)–[挑战](#ch04.html#idm46022857411520)
    - RDS 上的预置容量类型迁移到 Aurora Serverless，[讨论](#ch04.html#idm46022858039040)
- 挂载和卸载 EBS 卷，[验证检查](#ch03.html#idm46022858279072)
- 基于 MP3 的音频，转录为文本，[问题](#ch08.html#ix_MP3aud)–[挑战](#ch08.html#idm46022855544784)
- 多因素认证 (MFA)，[讨论](#ch01.html#idm46022860545840)，[讨论](#ch01.html#idm46022860400432)
    - 在您的 AWS 账户中为根用户设置，[问题](#ch09.html#ix_MFAroot)–[挑战](#ch09.html#idm46022854830432)
- MySQL
    - RDS 实例，加密其存储，[解决方案](#ch04.html#ix_MySQLRDSenc)–[挑战](#ch04.html#idm46022857699904)
    - RDS 实例，利用 RDS Proxy 从 Lambda 进行数据库连接，[问题](#ch04.html#ix_MySQLRDSProxy)
    - RDS 实例，将 IAM 认证与其配合使用，[问题](#ch04.html#ix_MySQLRDS)–[挑战](#ch04.html#idm46022857912752)

## N

- NAT 网关
    - 由 Transit Gateway 启用的共享，[步骤](#ch02.html#idm46022859003920)
    - 用于从私有子网进行出站互联网访问，[问题](#ch02.html#ix_NATgtwy)–[挑战](#ch02.html#idm46022859475408)
- `Ncat` 实用程序，[验证检查](#ch02.html#idm46022859338912)
- 网络洞察路径，为 EC2 实例创建，[步骤](#ch02.html#idm46022859365824)
- 网络负载均衡器，[讨论](#ch02.html#idm46022859210880)
- 网络，[简介](#ch02.html#ix_ntwk)–[挑战](#ch02.html#idm46022858883712)
    - 提供网络的 AWS 服务，[简介](#ch06.html#idm46022856646224)
    - 使用互联网网关将 VPC 连接到互联网，[问题](#ch02.html#ix_ntwkconnint)–[挑战](#ch02.html#idm46022859551872)
    - 使用 VPC 终端节点控制从 VPC 对 S3 的网络访问，[问题](#ch02.html#ix_ntwkS3acc)–[挑战](#ch02.html#idm46022859050656)
    - 在 VPC 中使用子网和路由表创建网络层级，[问题](#ch02.html#ix_ntwksubrt)–[挑战](#ch02.html#idm46022859625696)
    - 使用 Amazon VPC 在云中定义私有虚拟网络，[问题](#ch02.html#ix_netwkAVPC)–[挑战](#ch02.html#idm46022859712880)
    - 删除与安全组关联的网络接口，[快速修复](#app01.html#idm46022854680160)
    - 使用 Transit Gateway 启用跨 VPC 的传递性连接，[问题](#ch02.html#ix_ntwkXVPC)–[挑战](#ch02.html#idm46022858955984)
    - 通过引用安全组授予动态访问权限，[问题](#ch02.html#ix_ntwkdynacc)–[挑战](#ch02.html#idm46022859390256)
    - AWS 的创新，相关资源，[简介](#ch02.html#idm46022859787712)
    - 对等连接 VPC 以实现 VPC 间网络通信，[问题](#ch02.html#ix_ntwkpeerVPC)–[挑战](#ch02.html#idm46022858887920)
    - 使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS，[问题](#ch02.html#ix_ntwkredHTTP)–[挑战](#ch02.html#idm46022859206192)
    - 使用前缀列表简化安全组中 CIDR 的管理，[问题](#ch02.html#ix_ntwkCIDR)–[挑战](#ch02.html#idm46022859127344)
    - 使用 NAT 网关从私有子网进行出站互联网访问，[问题](#ch02.html#ix_ntwkNATgtwy)–[挑战](#ch02.html#idm46022859470912)
    - 使用 VPC 可达性分析器验证和排查网络路径，[问题](#ch02.html#ix_ntwkVPCRA)–[挑战](#ch02.html#idm46022859319680)
- NGINX 容器
    - 使用 Amazon Lightsail 部署，[解决方案](#ch06.html#ix_NGINXdep)–[挑战](#ch06.html#idm46022856387584)
    - `nginx:latest` 镜像，[步骤](#ch06.html#idm46022856563280)
- 通知 (S3)，[步骤](#ch05.html#idm46022856827072)

## O

- OpenSearch，[讨论](#ch09.html#idm46022854981312)
- `OpenSSL` CLI，使用它生成自签名证书，[步骤](#ch02.html#idm46022859274352)
- 编排器 (容器)，[简介](#ch06.html#idm46022856662752)
- Organizations (AWS)，设置，[简介](#ch09.html#idm46022855151376)，[问题](#ch09.html#ix_OrgAWS)–[挑战 2](#ch09.html#idm46022854727088)
- 来源访问身份 (OAI)
    - 配置它以要求 S3 存储桶仅可从 CloudFront 访问，[解决方案](#ch01.html#idm46022859859776)
    - 为 CloudFront 创建它以引用 S3 存储桶策略，[步骤](#ch01.html#idm46022859846688)
- `OS X CLI`，在其上收听 MP3 文件，[步骤](#ch08.html#idm46022855519888)

## P

- 密码
    - 为 RDS 数据库自动轮换，[问题](#ch04.html#ix_pwdrota)–[挑战](#ch04.html#idm46022857575936)
    - 使用 Secrets Manager 生成复杂密码，[步骤](#ch04.html#idm46022858129424)

