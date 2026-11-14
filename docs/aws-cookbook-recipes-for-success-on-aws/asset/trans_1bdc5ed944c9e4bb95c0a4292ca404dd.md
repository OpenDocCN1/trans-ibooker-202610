

## D

*   **Docker Linux Engine**，[Docker 安装与验证](#ch06.html#idm46022856621520)
*   **Docker Swarm**，[简介](#ch06.html#idm46022856665232)
*   **DynamoDB**
    *   使用 Lambda 自动从 S3 导入 CSV，[问题](#ch05.html#ix_DynDBimp) – [挑战 3](#ch05.html#idm46022856792832)
    *   自动扩缩 DynamoDB 表预置容量，[问题](#ch04.html#ix_DynDBscale) – [挑战](#ch04.html#idm46022857504752)
    *   向表中填充数据，[快速修复](#app01.html#idm46022854686976)
    *   使用 Glue 爬网程序扫描表，[讨论](#ch07.html#idm46022855779456)

## E

*   **EBS 快照**，[讨论](#ch03.html#idm46022858339408)
    *   从中恢复文件，[问题](#ch03.html#ix_EBSsnp) – [挑战](#ch03.html#idm46022858251712)
*   **EBS 卷**
    *   默认对区域中的新卷启用加密，[快速修复](#app01.html#idm46022854672752)
    *   使用 KMS 密钥进行加密，[问题](#ch01.html#ix_EBSvol) – [挑战 2](#ch01.html#idm46022860024208)
*   **`ebs-encryption-by-default` 选项**，[讨论](#ch01.html#idm46022860041008)
*   **EC2**，[简介](#ch06.html#idm46022856656576)
    *   使用 AWS SSM Session Manager 连接到实例，[问题](#ch01.html#ix_EC2conn) – [挑战](#ch01.html#idm46022860085552)
    *   使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，[问题](#ch03.html#ix_EC2bkup) – [挑战](#ch03.html#idm46022858322672)
    *   授予实例访问密钥的权限，[步骤](#ch01.html#idm46022859969344)
    *   VPC 子网中的实例，连接到互联网，[问题](#ch02.html#ix_EC2connint)
    *   从 Secrets Manager 检索密钥，[验证检查](#ch01.html#idm46022859962416)
    *   使用 Secrets Manager 存储、加密和访问密码，[问题](#ch01.html#ix_EC2storpwd) – [挑战](#ch01.html#idm46022859935984)
    *   使用 EC2 Global View 进行账户资源分析，[问题](#ch09.html#ix_EC2GlVi) – [挑战](#ch09.html#idm46022855107824)
*   **ECR (Elastic Container Registry)**，[简介](#ch06.html#idm46022856647760)
    *   支持的容器镜像，[讨论](#ch06.html#idm46022856523072)
    *   删除包含镜像的存储库，[清理](#ch06.html#idm46022856469984)
    *   **`Docker Credential Helper`**，[步骤](#ch06.html#idm46022856550240)
    *   将容器镜像推送到其中，[解决方案](#ch05.html#ix_ECRpushCIto) – [挑战](#ch05.html#idm46022856879920)，[问题](#ch06.html#ix_ECRpushCI_2) – [挑战](#ch06.html#idm46022856513552)
    *   推送到 ECR 时扫描容器镜像漏洞，[问题](#ch06.html#ix_ECRscan) – [挑战 2](#ch06.html#idm46022856451376)
    *   容器镜像在其上的存储，[讨论](#ch05.html#idm46022856885584)
*   **ECS (Elastic Container Service)**，[简介](#ch06.html#idm46022856655184)
    *   在其上自动扩缩容器工作负载，[问题](#ch06.html#ix_ECSautoscl) – [挑战](#ch06.html#idm46022856164112)
    *   捕获在其上运行的容器的日志，[问题](#ch06.html#ix_ECScntnrlogs) – [挑战](#ch06.html#idm46022856011808)
    *   **`Copilot` 对 ECS 服务相关角色的要求**，[步骤](#ch06.html#idm46022856357872)
    *   使用 EventBridge 触发在 Fargate 上启动容器任务，[解决方案](#ch06.html#ix_ECSEvntBr) – [挑战](#ch06.html#idm46022856090368)
*   **`ECSRunTaskPermissionsForEvents` 策略**，[步骤](#ch06.html#idm46022856127872)
*   **EFS**
    *   配置 Lambda 函数以访问它，[问题](#ch05.html#ix_EFScfgLmda) – [另请参阅](#ch05.html#idm46022857021152)
    *   使用 DataSync 在 EFS 和 S3 之间复制数据，[问题](#ch03.html#ix_EFSrepl) – [挑战 2](#ch03.html#idm46022858178496)
*   **EIP (弹性 IP 地址)**，[步骤](#ch02.html#idm46022859587472)
    *   与 NAT 网关关联，[讨论](#ch02.html#idm46022859488528)
    *   创建并关联到 EC2 实例，[解决方案](#ch02.html#idm46022859610752)
    *   创建以供 NAT 网关使用，[步骤](#ch02.html#idm46022859513632)
*   **Elastic Cloud Compute** (参见 EC2)
*   **Elastic Container Registry** (参见 ECR)
*   **Elastic Container Service (Amazon ECS)** (参见 ECS)
*   **Elastic Load Balancing (ELB) 服务**，[简介](#ch06.html#idm46022856639104)
    *   授予调用 Lambda 函数的权限，[解决方案](#ch05.html#idm46022857271600)
*   **弹性网络接口** (参见 ENIs)
*   **ElastiCache**，访问在 VPC 上具有终端节点的集群，[问题](#ch05.html#ix_EliCache) – [挑战 1](#ch05.html#idm46022856677792)
*   **根用户登录的电子邮件警报**，设置，[问题](#ch09.html#ix_emailalrt) – [挑战](#ch09.html#idm46022854905856)
*   **加密**
    *   默认对区域中的新 EBS 卷启用，[快速修复](#app01.html#idm46022854676256)
    *   使用 KMS 密钥加密 EBS 卷，[问题](#ch01.html#ix_encryptEBSvol) – [挑战 2](#ch01.html#idm46022860021776)
    *   加密现有 RDS for MySQL 数据库的存储，[问题](#ch04.html#ix_encryptstore) – [挑战](#ch04.html#idm46022857698288)
    *   传输中的 SSL 加密，由 RDS Proxy 支持，[讨论](#ch04.html#idm46022857799696)
    *   使用 DataSync 通过 TLS 对传输中和静态流量进行加密，[讨论](#ch03.html#idm46022858188208)
    *   将 S3 存储桶密钥与 KMS 配合使用以加密对象，[问题](#ch03.html#ix_encryptS3) – [挑战](#ch03.html#idm46022858423952)
*   **终端节点策略 (S3 VPC)**，[步骤](#ch02.html#idm46022859090608)
*   **无服务器的 `engine-mode`**，[步骤](#ch04.html#idm46022858109136)
*   **ENIs (弹性网络接口)**
    *   数据库子网组简化 RDS ENI 的放置，[步骤](#ch04.html#idm46022858120704)
    *   两个 EC2 实例的，与其关联的安全组，[问题](#ch02.html#idm46022859459376)
    *   安全组充当其有状态虚拟防火墙，[讨论](#ch02.html#idm46022859405792)
    *   用于其放置的子网，[讨论](#ch02.html#idm46022859664128)
*   **`EvalDecision`**，[验证检查](#ch01.html#idm46022860360144)
*   **事件驱动应用程序**，[讨论](#ch05.html#idm46022856805712)，[讨论](#ch06.html#idm46022856098000)
*   **EventBridge**
    *   按计划调用 Lambda 函数，[解决方案](#ch05.html#ix_EvBrinv) – [挑战](#ch05.html#idm46022857090160)
    *   搜索根用户登录并触发 SNS 主题的规则，[解决方案](#ch09.html#ix_EvBrruleroot) – [挑战](#ch09.html#idm46022854912336)
    *   使用它触发在 Fargate 上启动 ECS 容器任务，[解决方案](#ch06.html#ix_EvntBr) – [挑战](#ch06.html#idm46022856093856)

## F

*   **Fargate**，[先决条件](#ch02.html#idm46022859290944)，[简介](#ch06.html#idm46022856649824)
    *   响应事件启动 Fargate 容器任务，[问题](#ch06.html#ix_Fargt) – [挑战](#ch06.html#idm46022856087168)
*   **快速修复**，[快速修复](#app01.html#ix_fastfix) – [快速修复](#app01.html#idm46022854653920)
*   **联合身份**
    *   利用联合身份的 AWS 账户，[讨论](#ch01.html#idm46022860557056)
    *   身份联合，[解决方案](#ch01.html#idm46022860451984)
*   **表单数据**，计算机视觉分析，[问题](#ch08.html#ix_formCVA) – [挑战](#ch08.html#idm46022855424640)
*   **频繁访问存储层**，[讨论](#ch03.html#idm46022858764848)
*   **`full-load-and-cdc`**，[讨论](#ch04.html#idm46022857433664)

## G

*   **VPC 中的网关终端节点**，创建并关联到路由表，[步骤](#ch02.html#idm46022859094656)
*   **网关负载均衡器**，[讨论](#ch02.html#idm46022859212720)
*   **网关 VPC 终端节点**，[讨论](#ch02.html#idm46022859054176)
*   **`get-secret-value` API 调用**，[讨论](#ch01.html#idm46022859945664)
*   **Glacier 归档 (S3)**，自动将 S3 对象归档到其中，[解决方案](#ch03.html#idm46022858794912)

