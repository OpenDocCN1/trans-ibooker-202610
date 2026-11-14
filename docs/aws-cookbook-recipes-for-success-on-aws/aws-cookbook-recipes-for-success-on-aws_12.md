# 索引

## A

*   **Access Analyzer**
    *   支持的服务列表，讨论
    *   用于根据 CloudTrail 活动生成 IAM 策略，解决方案
*   **访问点 (S3)**，使用其配置特定于应用程序的存储桶访问权限，问题\-挑战
*   **常见工作职能的访问策略**，步骤
    *   创建用于密钥访问的策略，步骤
*   **账户管理**，简介\-挑战 2
    *   为 AWS 账户启用 CloudTrail 日志记录，问题\-挑战



# 索引

*   使用 `Tag Editor` 一次性修改多个资源的标签，问题\-挑战
*   将 `AWS ACCOUNT ID` 设置为 `bash` 变量，快速修复
*   设置 `AWS Organization` 和 `AWS Single Sign-On`，问题\-挑战 2
*   设置根用户登录的电子邮件警报，问题\-挑战
*   为根用户设置多因素认证，问题\-挑战
*   使用 `EC2 Global View` 进行账户资源分析，问题\-挑战

*   用于日常开发任务的管理员访问权限，不推荐，讨论
*   管理功能，使用权限边界为 `IAM` 委派，问题\-挑战
*   管理员 `IAM` 用户，讨论

*   `AI/ML`，简介\-挑战
    *   表单数据的计算机视觉分析，问题\-挑战
    *   将文本转换为语音，问题\-挑战
    *   检测视频中的文本，问题\-挑战
    *   确定图像中文本的位置，问题\-挑战
    *   医生口述分析，问题\-讨论
    *   使用 `Comprehend` 从文本中编辑 `PII`，问题\-挑战
    *   转录播客，问题\-挑战

*   `ALBs`（请参阅 `Application Load Balancers`）

*   根用户登录的警报（电子邮件），设置，问题\-挑战
*   `Amazon Certificate Manager` (`ACM`)，讨论
*   `Amazon Cloud Watch`（请参阅 `Cloud Watch`）
*   `Amazon Comprehend`，用于从文本中编辑 `PII`，问题
*   `Amazon ECS`（请参阅 `ECS`）
*   `Amazon Elastic Cloud Compute`（请参阅 `EC2`）
*   `Amazon Elastic Kubernetes Service` (`Amazon EKS`)，简介
    *   （另请参阅 `EKS`）
*   `Amazon Lightsail`（请参阅 `Lightsail`，使用其部署容器）
*   `Amazon Machine Image` (`AMI`)，`AWS Backup` 处理构建过程，讨论
*   `Amazon Managed Service for Prometheus`，简介
*   `Amazon OpenSearch`，讨论
*   `Amazon Polly`，将文本转换为语音，问题\-挑战
*   `Amazon Rekognition Video`，用于检测视频中的文本，问题\-挑战
*   `Amazon Textract` 和 `textractor` 工具
    *   确定图像中文本的位置，问题\-挑战
    *   用于表单数据的计算机视觉分析，解决方案\-挑战
*   `Amazon Transcribe Medical` 和 `Amazon Comprehend Medical`，用于分析医生口述，问题\-讨论
*   `Amazon Transcribe`，配合 `MP3` 文件使用，解决方案\-挑战
*   `AmazonDynamoDBFullAccess` 策略，验证检查，步骤
*   `AmazonEC2ReadOnlyAccess` 策略，步骤
*   `AmazonS3ReadOnlyAccess` 策略，步骤
*   `AmazonSSMManagedInstanceCore` 策略，解决方案，步骤

*   `Application Load Balancers` (`ALBs`)
    *   配置 `ALB` 以调用 `Lambda` 函数，问题\-挑战 2
    *   创建新的 `ALB` 目标组以配合 `CodeDeploy` 作为绿色目标，步骤
    *   使用它将 `HTTP` 流量重定向到 `HTTPS`，问题\-挑战

*   `Archive Access` 存储层，讨论

*   `ARN` (Amazon Resource Names)
    *   查找 `ECS` 任务的 ARN，验证检查
    *   检索用户的 `ARN`，步骤

*   人工智能（请参阅 `AI/ML`）

*   `AssumeRole API`，验证检查
*   `AssumeRole` 策略，验证检查
*   代入角色，验证检查

*   `Athena` 服务
    *   使用它查询 `S3` 上的文件，问题\-挑战
    *   使用它就地查询 `S3` 上的日志，讨论

*   音频，转录为文本，问题\-挑战
*   审核会话活动，讨论

*   `Aurora Serverless`
    *   创建 `PostgreSQL` 数据库，问题\-挑战
    *   使用 `RDS Data API` 启用对其的 `REST` 访问，问题\-挑战

*   认证
    *   `IAM`，配合 `RDS` 数据库使用，问题\-挑战
    *   多因素，讨论
        *   在 `AssumeRole` 策略中要求，讨论
        *   为根用户设置，问题\-挑战

*   授权令牌，步骤
*   `AutoPause`，步骤

*   `Availability Zones` (`AZs`)，讨论
    *   中的 `ENI`，讨论
    *   查找跨账户一致的区域 `AZ` ID，快速修复
    *   中的 `NAT` 网关，讨论
    *   `VPC` 中的子网，跨 `AZs` 分布，讨论

*   `VPC` 的可用状态，验证，验证检查

*   `AWS ACCOUNT ID`，设置为 `bash` 变量，快速修复
*   `AWS` 账户管理（请参阅账户管理）
*   `AWS Backup`，讨论
    *   使用它在另一个区域创建和恢复 `EC2` 备份，问题\-挑战
*   `AWS Backup Managed Policy`，步骤

*   `AWS CLI`
    *   自动化令牌检索过程，讨论
    *   为其配置凭证，验证检查
    *   确定发出调用的用户，快速修复
    *   为命令生成 `Yaml` 输入，快速修复
    *   为其安装 `Lightsail Control` 插件，先决条件
    *   启动 `Transcribe` 转录作业，步骤

*   `AWS Cloud Map`，简介




### A

*   AWS CodeDeploy，用于编排应用程序部署到 ECS，问题 - 挑战
*   AWS Config，讨论
*   AWS Control Tower，讨论
*   AWS Copilot，使用它部署容器，问题 - 挑战
*   AWS Database Migration Service (`AWS DMS`)，讨论
*   AWS Glue 爬网程序，使用它自动发现元数据，问题 - 挑战
*   AWS KMS CMK，配置 S3 存储桶以使用引用的密钥，解决方案
*   AWS Lake Formation，讨论
*   AWS Organizations，简介
*   AWS Organizations 和单点登录，设置，问题 - 挑战 2
*   AWS Policy Generator，讨论
*   AWS Schema Conversion Tool，讨论
*   AWS SDK，讨论
    *   配合特定访问点使用，步骤
*   AWS Security Token Service（请参阅 Security Token Service）
*   AWS 安全主题，相关资源，简介
*   AWS 服务，列出所有，快速修复
*   AWS Signer，用于在 Lambda 函数中运行受信任代码，问题 - 问题
*   AWS Single Sign-On，解决方案，简介，讨论
    *   设置 AWS Organizations 和它，问题 - 挑战 2
*   AWS SSM Session Manager
    *   通过它连接到 VPC 子网中的 EC2 实例，验证检查
    *   使用它连接到 EC2 实例，问题 - 挑战
*   AWS Systems Manager (`SSM`) API，讨论
*   AWS 托管 (`aws/ebs`) KMS 密钥，讨论
*   `AWSLambdaBasicExecutionRole` 策略，Lambda 函数执行的 IAM 角色，步骤
*   `AWSLambdaVPCAccess` IAM 策略，步骤

### B

*   备份
    *   在 Aurora Serverless 中自动化，讨论
    *   使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，问题 - 挑战
*   大数据，简介 - 挑战
    *   使用 AWS Glue 爬网程序自动发现元数据，问题 - 挑战
    *   使用 Amazon Athena 查询 S3 上的文件，问题 - 挑战
    *   使用 Kinesis Data Firehose 将数据流式传输到 S3，问题 - 挑战
    *   使用 AWS Glue DataBrew 转换数据，问题 - 挑战
    *   使用 Kinesis Stream 摄取流式数据，问题 - 挑战
    *   工作站配置，工作站配置
*   `Block Public Access` 功能 (S3)，解决方案
*   蓝/绿部署，使用它更新容器，问题 - 挑战
*   边界网关协议 (`BGP`)，讨论
*   存储桶策略模板以强制对所有对象加密，步骤

### C

*   数据库容量，步骤
    *   Aurora Serverless 扩展以容量单位衡量，讨论
    *   自动扩展容量目标，步骤
    *   自动扩展 DynamoDB 表预置容量，问题 - 挑战
    *   检查 RDS 集群，验证检查
    *   DynamoDB 预置容量，容量单位的使用，步骤
    *   缩减至 0，步骤
    *   设置为上限，讨论
    *   当数据库活动恢复时，步骤
*   CDN（内容分发网络），讨论
*   证书颁发机构 (`CA`)，RDS 根 CA，验证检查
*   证书
    *   CloudFront 默认主机名上的 `HTTPS` 证书，讨论
    *   为 `HTTPS` 创建，步骤
    *   `SSL`，讨论
*   `CI/CD` 流水线
    *   在其中嵌入 AWS Copilot 命令，讨论
    *   蓝/绿部署模式，讨论
*   `CIDR`（无类别域间路由）块，解决方案
    *   为您的 VPC 仔细选择，讨论
    *   创建具有 `IPv6` `CIDR` 块的 VPC，讨论
    *   连接 VPC 时使用不重叠的范围，讨论
    *   `IPv4` 和 `IPv6` 的配额，讨论
    *   使用前缀列表简化安全组中 `CIDR` 的管理，问题 - 挑战
    *   为授权指定 `CIDR` 表示法，讨论
*   CLI（请参阅 AWS CLI）
*   Cloud Map，简介
*   CloudFront，讨论
    *   使用它从 S3 安全地提供 Web 内容，问题 - 挑战
*   CloudTrail 日志记录，先决条件
    *   配置以记录 S3 存储桶上的事件，步骤
    *   启用，步骤
    *   为您的 AWS 账户启用，问题 - 挑战
*   CloudWatch，步骤
    *   为 ECS 服务配置警报和扩展策略，解决方案
    *   获取最近创建的日志组名称，快速修复
    *   Glue 爬网程序将信息记录到其中，验证检查
    *   指标服务，讨论
    *   将容器日志流式传输到其中，解决方案 - 挑战
    *   尾随日志组以观察 Lambda 函数调用，验证检查
    *   尾随 CloudWatch 组的日志，快速修复
*   CloudWatch Events（请参阅 EventBridge）
*   `CloudWatchFullAccess` 策略，验证检查
*   集群
    *   创建引擎模式为无服务器的数据库集群，步骤
    *   数据库，检查其容量，验证检查
    *   删除 RDS 集群，清理





### C

*   `CMK`（请参阅 `customer-managed KMS key`）
*   `code signing configuration`，`步骤`
*   `CodeDeploy`，用于编排应用程序部署到 `ECS`，`解决方案`\-`挑战`
*   `CodeDeployRoleForECS policy`，`步骤`
*   `CodeSigningPolicies`，更改，`挑战 2`
*   `Common Vulnerabilities Scoring System (CVSS)`，`讨论`
*   `Comprehend`，用于从文本中编辑 `PII`，`问题`\-`挑战`
*   `computer vision analysis of form data`，`问题`\-`挑战`
*   `concurrency (provisioned)`，使用它减少 `Lambda` 启动时间，`问题`\-`挑战 2`
*   `connection pooling`，利用 `RDS Proxy` 处理来自 `Lambda` 的数据库连接，`问题`\-`挑战`
*   `consumers and producers (streaming data)`，`讨论`
*   `containers`，`简介`\-`挑战`
    *   `autoscaling container workloads on ECS`，`问题`\-`挑战`
    *   `building, tagging and pushing container image to Amazon ECR`，`问题`\-`挑战`
    *   `capturing logs from containers running on ECS`，`问题`\-`挑战`
    *   `deploying using Amazon Lightsail`，`问题`\-`挑战`
    *   `deploying using AWS Copilot`，`问题`\-`挑战`
    *   `launching Fargate container task in response to an event`，`问题`\-`挑战`
    *   `networked together`，`简介`
    *   `orchestrators`，`简介`
    *   `packaging Lambda code in container image`，`问题`
    *   `prerequisite, installing Docker`，`Docker 安装和验证`
    *   `scanning images for security vulnerabilities on push to ECR`，`问题`\-`挑战 2`
    *   `updating with blue/green deployments`，`问题`\-`挑战`
*   `Control Tower`，`讨论`
*   `Copilot`，使用它部署容器，`问题`\-`挑战`
*   `cost allocation tags`，`讨论`
*   `CreateInternetGateway action (EC2)`，测试，`验证检查`
*   `credentials`
    *   `temporary, for a role, returned by AssumeRole API`，`验证检查`
    *   `temporary, from AWS STS instead of IAM user`，`讨论`
*   `cross-account access to AWS resources`，`讨论`
*   `Cross-Region Replication (CRR)`，`讨论`
*   `CSV files`
    *   `generating and exporting CSV of resources with EC2 Global View`，`问题`\-`挑战`
    *   `transforming data in, using Glue DataBrew`，`问题`\-`挑战`
*   `customer-managed KMS key`，`解决方案`，`问题`，`讨论`
*   `CVSS (Common Vulnerability Scoring System)`，`讨论`

### D

*   `dashboard`，为 `S3 Storage Lens` 创建，`步骤`\-`讨论`
*   `data`，`简介`
    *   (另请参阅 `big data`)
*   `Data Catalog`
    *   creating in `Athena`，`步骤`
    *   using `Glue Data Catalog` in `Athena` service，`讨论`
*   `data lakes`，`讨论`
*   `Data Lifecycle Manager`，`讨论`
    *   automating `EBS snapshots` with，`讨论`
*   `database engines`，determining current supported versions for，`快速修复`
*   `Database Migration Service` (see `DMS`)
*   `databases`，`简介`\-`挑战`，`讨论`
    *   automating password rotation for `RDS databases`，`问题`\-`挑战`
    *   autoscaling `DynamoDB table provisioned capacity`，`问题`\-`挑战`
    *   `AWS managed database services`，`简介`
    *   creating `Amazon Aurora serverless PostgreSQL database`，`问题`\-`挑战`
    *   enabling `REST access to Aurora Serverless using RDS Data API`，`问题`\-`挑战`
    *   encrypting storage of existing `RDS for MySQL database`，`问题`\-`挑战`
    *   leveraging `RDS Proxy` for database connections from `Lambda`，`问题`\-`挑战`
    *   migrating to `Amazon RDS using DMS`，`问题`\-`挑战`
    *   using `IAM authentication with RDS database`，`问题`\-`挑战`
*   `DataSync`，replicating data between `EFS and S3` with，`问题`\-`挑战 2`
*   `Days in Transition action`，`验证检查`
*   `db-connect action`，`讨论`
*   `DBAppFunction Lambda function’s role`，`步骤`
*   `DBInstanceStatus`，`步骤`
*   `DBProxyTargets`，`步骤`
*   `Deep Archive Access storage tier`，`讨论`
*   `deploying containers`
    *   updating containers with `blue/green deployments`，`问题`\-`挑战`
    *   using `Amazon Lightsail`，`问题`\-`挑战`
    *   using `AWS Copilot`，`问题`\-`挑战`
*   `DescribeInstances action (EC2)`，testing，`验证检查`
*   `Destination S3 bucket`，`步骤`
*   `DHCP server (AWS-managed)`，options set，`讨论`
*   `DMS (Database Migration Service)`，`讨论`
    *   migrating databases to `Amazon RDS using DMS`，`问题`\-`挑战`
*   `dms.t2.medium replication instance size`，`步骤`
*   `Docker`
    *   creating a `Dockerfile`，`步骤`
    *   installation and validation，`Docker 安装和验证`
*   `Docker container image`，pushing to `ECR repository`，`问题`\-`挑战`
*   `Docker Desktop`，`Docker 安装和验证`





## D

*   **Docker Linux Engine**，Docker 安装与验证
*   **Docker Swarm**，简介
*   **DynamoDB**
    *   使用 Lambda 自动从 S3 导入 CSV，问题 – 挑战 3
    *   自动扩缩 DynamoDB 表预置容量，问题 – 挑战
    *   向表中填充数据，快速修复
    *   使用 Glue 爬网程序扫描表，讨论

## E

*   **EBS 快照**，讨论
    *   从中恢复文件，问题 – 挑战
*   **EBS 卷**
    *   默认对区域中的新卷启用加密，快速修复
    *   使用 KMS 密钥进行加密，问题 – 挑战 2
*   **`ebs-encryption-by-default` 选项**，讨论
*   **EC2**，简介
    *   使用 AWS SSM Session Manager 连接到实例，问题 – 挑战
    *   使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，问题 – 挑战
    *   授予实例访问密钥的权限，步骤
    *   VPC 子网中的实例，连接到互联网，问题
    *   从 Secrets Manager 检索密钥，验证检查
    *   使用 Secrets Manager 存储、加密和访问密码，问题 – 挑战
    *   使用 EC2 Global View 进行账户资源分析，问题 – 挑战
*   **ECR (Elastic Container Registry)**，简介
    *   支持的容器镜像，讨论
    *   删除包含镜像的存储库，清理
    *   **`Docker Credential Helper`**，步骤
    *   将容器镜像推送到其中，解决方案 – 挑战，问题 – 挑战
    *   推送到 ECR 时扫描容器镜像漏洞，问题 – 挑战 2
    *   容器镜像在其上的存储，讨论
*   **ECS (Elastic Container Service)**，简介
    *   在其上自动扩缩容器工作负载，问题 – 挑战
    *   捕获在其上运行的容器的日志，问题 – 挑战
    *   **`Copilot` 对 ECS 服务相关角色的要求**，步骤
    *   使用 EventBridge 触发在 Fargate 上启动容器任务，解决方案 – 挑战
*   **`ECSRunTaskPermissionsForEvents` 策略**，步骤
*   **EFS**
    *   配置 Lambda 函数以访问它，问题 – 另请参阅
    *   使用 DataSync 在 EFS 和 S3 之间复制数据，问题 – 挑战 2
*   **EIP (弹性 IP 地址)**，步骤
    *   与 NAT 网关关联，讨论
    *   创建并关联到 EC2 实例，解决方案
    *   创建以供 NAT 网关使用，步骤
*   **Elastic Cloud Compute** (参见 EC2)
*   **Elastic Container Registry** (参见 ECR)
*   **Elastic Container Service (Amazon ECS)** (参见 ECS)
*   **Elastic Load Balancing (ELB) 服务**，简介
    *   授予调用 Lambda 函数的权限，解决方案
*   **弹性网络接口** (参见 ENIs)
*   **ElastiCache**，访问在 VPC 上具有终端节点的集群，问题 – 挑战 1
*   **根用户登录的电子邮件警报**，设置，问题 – 挑战
*   **加密**
    *   默认对区域中的新 EBS 卷启用，快速修复
    *   使用 KMS 密钥加密 EBS 卷，问题 – 挑战 2
    *   加密现有 RDS for MySQL 数据库的存储，问题 – 挑战
    *   传输中的 SSL 加密，由 RDS Proxy 支持，讨论
    *   使用 DataSync 通过 TLS 对传输中和静态流量进行加密，讨论
    *   将 S3 存储桶密钥与 KMS 配合使用以加密对象，问题 – 挑战
*   **终端节点策略 (S3 VPC)**，步骤
*   **无服务器的 `engine-mode`**，步骤
*   **ENIs (弹性网络接口)**
    *   数据库子网组简化 RDS ENI 的放置，步骤
    *   两个 EC2 实例的，与其关联的安全组，问题
    *   安全组充当其有状态虚拟防火墙，讨论
    *   用于其放置的子网，讨论
*   **`EvalDecision`**，验证检查
*   **事件驱动应用程序**，讨论，讨论
*   **EventBridge**
    *   按计划调用 Lambda 函数，解决方案 – 挑战
    *   搜索根用户登录并触发 SNS 主题的规则，解决方案 – 挑战
    *   使用它触发在 Fargate 上启动 ECS 容器任务，解决方案 – 挑战

## F

*   **Fargate**，先决条件，简介
    *   响应事件启动 Fargate 容器任务，问题 – 挑战
*   **快速修复**，快速修复 – 快速修复
*   **联合身份**
    *   利用联合身份的 AWS 账户，讨论
    *   身份联合，解决方案
*   **表单数据**，计算机视觉分析，问题 – 挑战
*   **频繁访问存储层**，讨论
*   **`full-load-and-cdc`**，讨论

## G

*   **VPC 中的网关终端节点**，创建并关联到路由表，步骤
*   **网关负载均衡器**，讨论
*   **网关 VPC 终端节点**，讨论
*   **`get-secret-value` API 调用**，讨论
*   **Glacier 归档 (S3)**，自动将 S3 对象归档到其中，解决方案





### G

*   `global condition context keys` (`IAM`), 步骤
*   `Glue 服务`
    *   `使用 Glue 爬网程序自动发现元数据`, 问题\-挑战
        *   `爬网程序配置摘要`, 步骤
        *   `创建 Glue Data Catalog 数据库`, 步骤
    *   `使用 Glue DataBrew 转换数据`, 问题\-挑战
*   `用户组` (`IAM`), 步骤

### H

*   针对 `PHI` 的 `HIPAA` 合规性, 讨论
*   `HTTP`
    *   `HTTP 301` 响应, 步骤
    *   允许 `HTTP` 流量的 `安全组规则`, 步骤
*   `HTTPS`
    *   `使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS`, 问题\-挑战
    *   `Session Manager`, `通过 AWS Systems Manager (SSM) 进行通信`, 讨论

### I

*   `IAM` (`Identity and Access Management`, 身份与访问管理)
    *   `访问密钥`, `AWS SSO` 和, 讨论
    *   `访问点策略`, 步骤, 讨论
    *   `创建并代入用于开发人员访问的角色`, 问题\-挑战
    *   `创建用于密钥访问的策略`, 步骤, 步骤
    *   `创建用于 Kinesis Data Firehose 的角色`, 步骤
    *   `创建用于 RDS Proxy 的角色`, 步骤
    *   `使用权限边界委派管理功能`, 问题\-挑战
    *   `启用 IAM 用户和角色对账单控制台的访问`, 讨论
    *   `基于访问模式生成最小权限 IAM 策略`, 问题\-挑战
    *   `允许 S3 将对象从源存储桶复制到目标存储桶的角色`, 步骤
    *   `用于 Lambda 函数执行的角色`, `IAM role for Lambda function execution`
    *   `服务相关角色`, 步骤
    *   `在 AWS 账户中为根用户设置多因素认证`, 步骤
    *   `使用 IAM Policy Simulator 测试策略`, 问题\-挑战
    *   `用户密码策略`, `在 AWS 账户中强制执行`, `Enforcing IAM User Password Policies in Your AWS Account`\-挑战
    *   `将 IAM 身份验证与 RDS 数据库配合使用`, 问题\-挑战
*   `IAM Access Analyzer` (请参阅 `Access Analyzer`)
*   `身份联合`, `用于访问 AWS 账户`, 解决方案
*   `IGW` (请参阅 `internet gateway`)
*   `Infrequent Access` 存储类, 解决方案, 讨论, 讨论
*   `实例元数据` (`EC2`), 步骤
*   `实例配置文件` (`EC2`), 步骤
    *   `创建并关联允许访问密钥的配置文件`, 解决方案
*   `Intelligent-Tiering` 归档策略, `用于自动归档 S3 对象`, 问题\-挑战 2
*   `互联网网关`
    *   `仅出站`, `用于具有 IPv6 功能的 VPC 上的私有子网`, 讨论
    *   `使用互联网网关将 VPC 连接到互联网`, 问题\-挑战
*   `互联网`, `使用 NAT 网关从私有子网进行出站访问`, 问题\-挑战
*   `IP 地址`
    *   `AWS 提供的 IP 范围列表`, 解决方案
    *   `IPv4` 中的 `Elastic IP address` (弹性 IP 地址), 步骤
    *   `用于 ENI`, 讨论
    *   `在子网中新启动的 EC2 实例上自动分配的选项`, 讨论
    *   `从 EC2 实例的元数据中检索公有 IP`, 验证检查
*   `IPv4 CIDR 块`, 步骤
    *   `额外的`, `与您的 VPC 关联`, 讨论
    *   `配额`, 讨论
*   `IPv6 CIDR 块`
    *   `为 Amazon VPC 配置`, 讨论
    *   `配额`, 讨论

### J

*   `Java Database Connectivity (JDBC)` 数据存储, `使用 Glue 爬网程序扫描`, 讨论
*   `jq` 实用程序, 步骤, 先决条件

### K

*   `Key Management Service (KMS)` (密钥管理服务), 简介
    *   `创建用于加密数据库快照的 KMS 密钥`, 步骤
    *   `使用 KMS 密钥加密 EBS 卷`, 问题\-挑战 2
    *   `指定用于加密 RDS 数据库快照的密钥`, 步骤
    *   `结合 S3 存储桶密钥使用 KMS 以加密对象`, 问题\-挑战
*   `密钥轮换`, `自动`, `在 KMS 服务上`, 讨论
*   `Kinesis Client Library (KCL)`, 讨论
*   `Kinesis Data Analytics`, 讨论
*   `Kinesis Data Firehose`, `使用其将流数据传输到 S3`, 问题\-挑战
*   `Kinesis Producer Library (KPL)`, 讨论
*   `Kinesis Stream`, `用于摄取流数据`, 问题\-挑战
*   `KMS` (请参阅 `Key Management Service`)
*   `KMS.NotFoundException` 错误, 验证检查
*   `Kubernetes`, 简介

### L

*   `Lambda 函数`
    *   `使用其访问 VPC 资源`, 问题\-挑战 1
    *   `配置应用程序负载均衡器以调用`, 问题\-挑战 2
    *   `配置以访问 EFS 文件系统`, 问题\-另请参阅
    *   `与 RDS 数据库的连接`, `利用 RDS Proxy 实现`, 问题\-挑战
    *   `用于执行的 IAM 角色`, `IAM role for Lambda function execution`
    *   `将函数与 Secrets Manager 集成以轮换 RDS 数据库密码`, 问题\-挑战
    *   `按计划调用`, 问题\-挑战
    *   `将 Lambda 代码打包在容器镜像中`, 问题\-挑战
    *   `使用 Lambda Layers 打包库`, 问题\-挑战
    *   `使用预置并发减少启动时间`, 问题





## L

- 在其中运行受信任代码，使用 `AWS Signer`，问题–问题
- 900 秒后超时，讨论
- 使用其转换数据，讨论
- 对于长时间运行的作业，改用 EventBridge 而非它，讨论
- 登陆区，讨论
- 最小权限访问
    - 基于访问模式实施，问题–挑战
    - 最小权限原则，讨论
- 库，使用 Lambda Layers 打包，问题–挑战
- 生命周期策略 (S3)，使用它们来降低存储成本，问题–挑战 2
- Lightsail，使用它部署容器，问题–挑战
- Linux，在其上安装 `Docker`，Linux
- 负载均衡器
    - 使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS，问题–挑战
    - AWS 提供的 `ALB` 之外的类型，讨论
- 日志记录
    - 捕获在 ECS 上运行的容器的日志，问题–挑战
    - CloudTrail，为您的账户启用它，先决条件
    - 删除所有匹配文本模式的日志组，快速修复
    - 为 AWS 账户启用 CloudTrail 日志记录，问题–挑战
    - Glue 爬网程序自动将信息记录到 CloudWatch Logs，验证检查
- 登录配置文件，为用户创建，步骤
- 低代码开发平台 (LCDP)，讨论

## M

- 机器学习 (ML)（请参阅 AI/ML）
- MacOS，在其上安装 `Docke Desktop`，MacOS
- 管理账户，讨论
- 元数据，讨论
    - 使用 AWS Glue 爬网程序自动发现，问题–挑战
    - EC2 实例，从中检索公有 IP，验证检查
    - EC2 实例，步骤
- 指标
    - CloudWatch 上的自动扩缩指标，讨论
    - AWS 控制台上的 ECS 服务指标，验证检查
    - 使用 Storage Lens 观察 S3 存储指标，讨论
- 数据库迁移
    - 使用 DMS 将数据库迁移到 Amazon RDS，问题–挑战
    - RDS 上的预置容量类型迁移到 Aurora Serverless，讨论
- 挂载和卸载 EBS 卷，验证检查
- 基于 MP3 的音频，转录为文本，问题–挑战
- 多因素认证 (MFA)，讨论，讨论
    - 在您的 AWS 账户中为根用户设置，问题–挑战
- MySQL
    - RDS 实例，加密其存储，解决方案–挑战
    - RDS 实例，利用 RDS Proxy 从 Lambda 进行数据库连接，问题
    - RDS 实例，将 IAM 认证与其配合使用，问题–挑战

## N

- NAT 网关
    - 由 Transit Gateway 启用的共享，步骤
    - 用于从私有子网进行出站互联网访问，问题–挑战
- `Ncat` 实用程序，验证检查
- 网络洞察路径，为 EC2 实例创建，步骤
- 网络负载均衡器，讨论
- 网络，简介–挑战
    - 提供网络的 AWS 服务，简介
    - 使用互联网网关将 VPC 连接到互联网，问题–挑战
    - 使用 VPC 终端节点控制从 VPC 对 S3 的网络访问，问题–挑战
    - 在 VPC 中使用子网和路由表创建网络层级，问题–挑战
    - 使用 Amazon VPC 在云中定义私有虚拟网络，问题–挑战
    - 删除与安全组关联的网络接口，快速修复
    - 使用 Transit Gateway 启用跨 VPC 的传递性连接，问题–挑战
    - 通过引用安全组授予动态访问权限，问题–挑战
    - AWS 的创新，相关资源，简介
    - 对等连接 VPC 以实现 VPC 间网络通信，问题–挑战
    - 使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS，问题–挑战
    - 使用前缀列表简化安全组中 CIDR 的管理，问题–挑战
    - 使用 NAT 网关从私有子网进行出站互联网访问，问题–挑战
    - 使用 VPC 可达性分析器验证和排查网络路径，问题–挑战
- NGINX 容器
    - 使用 Amazon Lightsail 部署，解决方案–挑战
    - `nginx:latest` 镜像，步骤
- 通知 (S3)，步骤

## O

- OpenSearch，讨论
- `OpenSSL` CLI，使用它生成自签名证书，步骤
- 编排器 (容器)，简介
- Organizations (AWS)，设置，简介，问题–挑战 2
- 来源访问身份 (OAI)
    - 配置它以要求 S3 存储桶仅可从 CloudFront 访问，解决方案
    - 为 CloudFront 创建它以引用 S3 存储桶策略，步骤
- `OS X CLI`，在其上收听 MP3 文件，步骤

## P

- 密码
    - 为 RDS 数据库自动轮换，问题–挑战
    - 使用 Secrets Manager 生成复杂密码，步骤





## P

*   在 AWS 账户中强制执行 IAM 用户密码策略，在您的 AWS 账户中强制执行 IAM 用户密码策略\-挑战
*   使用 Secrets Manager 存储、加密和访问，问题\-挑战
*   对等连接 VPC 以实现 VPC 间网络通信，问题\-挑战
*   权限
    *   限制 SSM 用户的权限，讨论
    *   权限集，讨论
*   权限边界，使用权限边界委托 IAM 管理功能，问题\-挑战
*   PHI（受保护的健康信息），分类以便进一步分析，问题\-讨论
*   使用 Transcribe Medical 和 Comprehend Medical 进行医生口述分析，问题\-讨论
*   PII（个人身份信息），使用 Comprehend 从文本中编辑，问题\-挑战
*   策略模拟器 (IAM)，使用策略模拟器测试 IAM 策略，问题\-挑战
*   Polly 服务，将文本转换为语音，问题\-挑战
*   PostgreSQL
    *   Aurora Serverless 数据库，使用 RDS Data API 允许 REST 访问，问题\-挑战
    *   创建 Amazon Aurora Serverless PostgreSQL 数据库，问题\-挑战
*   `PostgreSQL` 包，安装，验证检查
*   `PowerUserAccess` IAM 策略，解决方案，讨论
    *   附加到角色，步骤
*   安全组中的前缀列表，使用前缀列表管理 CIDR，问题\-挑战
*   最小权限原则，讨论，讨论
    *   (另见最小权限访问)
*   权限提升，IAM 服务降低风险，步骤
*   生产者和消费者（流式数据），讨论
*   Prometheus，Amazon Managed Service for Prometheus，简介
*   受保护的健康信息 (PHI)，分类以便进一步分析，问题\-讨论
*   DynamoDB 表的预置容量，自动扩展，问题\-挑战
*   预置并发，使用预置并发减少 Lambda 启动时间，问题\-挑战 2
*   `Proxy ID`，步骤
*   公共访问，阻止 S3 存储桶的公共访问，问题\-挑战

## Q

*   RDS 控制台中的查询编辑器，步骤，讨论
*   配额和限制 (Kinesis 服务)，讨论

## R

*   RDS Data API
    *   短期数据库连接，讨论
    *   使用 RDS Data API 启用对 Aurora Serverless 的 REST 访问，问题\-挑战
*   `rds-data:CommitTransaction` 权限，讨论
*   `rds-data:RollbackTransaction` 权限，讨论
*   可达性分析器 (VPC)，使用可达性分析器验证和排查网络路径，问题\-挑战
*   `ReadCapacityUnits` 扩展目标，步骤
*   恢复点 (备份)，步骤
*   恢复点目标，复制 S3 存储桶以满足恢复点目标，问题\-挑战 2
*   恢复时间目标 (RTO)，使用 AWS Backup 减少 EC2 实例的恢复时间目标，讨论
*   将所有 HTTP 流量重定向到 HTTPS 的重定向响应，步骤
*   `Redis` Python 包，安装，步骤
*   `Redis`，带有 `Redis` 客户端的 Lambda 函数，访问 VPC 资源，解决方案\-挑战 1
*   区域
    *   使用 AWS Backup 将 EC2 备份创建并恢复到另一个区域，问题\-挑战
    *   在区域中创建 VPC，跨可用区分布子网，讨论
    *   列出所有区域，快速修复
    *   列出拥有特定服务的所有区域，快速修复
    *   在表格中列出区域名称和端点，快速修复
    *   通过从 EC2 实例元数据获取值来设置区域，快速修复
    *   停止当前工作区域中所有正在运行的实例，快速修复
    *   VPC 的区域，讨论
*   Rekognition Video，使用 Rekognition Video 检测视频中的文本，问题
*   关系数据库服务 (RDS)，简介
    *   自动执行 RDS 数据库的密码轮换，问题\-挑战
    *   删除 RDS 集群，清理
    *   利用 RDS Proxy 从 Lambda 进行数据库连接，问题\-挑战
    *   使用 DMS 将数据库迁移到 RDS，问题\-挑战
    *   命名约束，步骤
    *   对 RDS 数据库使用 IAM 身份验证，问题\-挑战
*   复制
    *   Aurora Serverless 数据库，讨论
    *   DMS 复制任务，解决方案\-挑战
    *   使用 DataSync 在 EFS 和 S3 之间复制数据，问题\-挑战 2
    *   复制 S3 存储桶以满足恢复点目标，问题\-挑战 2
*   `ReplicationStatus`，验证检查
*   资源访问管理器，讨论
*   REST
    *   使用 RDS Data API 启用对 Aurora Serverless 的 REST 访问，问题\-挑战
    *   RDS 暴露的 REST API，讨论
*   恢复
    *   从 EBS 快照恢复文件，问题
    *   使用 AWS Backup 将 EC2 备份恢复到另一个区域，步骤
*   角色
    *   创建角色，步骤
    *   检索用户的角色，步骤
*   根用户
    *   根用户登录的电子邮件警报，设置，问题\-挑战
    *   为根用户设置多因素认证，问题\-挑战





### R

- `rotate-secret` command (Secrets Manager), 步骤
- Route 53 DNS records, 讨论
- route tables
    - associated with each subnet, route to direct traffic for peered VPCs to peering connection, 步骤
    - associating gateway endpoint in VPC with, 步骤
    - creating network tier with subnets and route table in a VPC, 问题\-挑战
    - prefix lists associated wtih, 讨论
    - priority given to most specific route, 讨论
- routing, 解决方案
    - (see also CIDR blocks)
    - for Transit Gateway, 步骤
- runtime interface client, 讨论

### S

- S3
    - automatically archiving S3 objects using Intelligent-Tiering, 问题\-挑战 1
    - AWS support for S3 interface endpoints, 讨论
    - blocking public access for a bucket, 问题\-挑战
    - configuring application-specific access to buckets with S3 access points, 问题\-挑战
    - controlling network access to S3 from VPC using VPC endpoints, 问题\-挑战
    - creating presigned URL for object that expires in a week, 快速修复
    - CSV import into DynamoDB from S3 with Lambda, 问题\-挑战 3
    - lifecycle policies, using to reduce storage costs, 问题\-挑战 2
    - observing storage and access metrics using Storage Lens, 问题\-挑战 2
    - querying files using Amazon Athena, 问题\-挑战
    - replicating buckets to meet recovery point objectives, 问题\-挑战 2
    - replicating files from S3 to EFS using DataSync, 问题\-挑战 2
    - serving web content securely from using CloudFront, 问题\-挑战
    - streaming data to, using Kinesis Data Firehose, 问题\-挑战
    - using bucket keys with KMS to encrypt objects, 问题\-挑战
- `S3:GetObject` action, 解决方案
- `S3:PutObject` action, 解决方案
- Same-Region Replication (`SRR`), 讨论
- scaling
    - autoscaling by Kinesis Data Firehose, 讨论
    - autoscaling capacity for database, 步骤
    - autoscaling container workloads on ECS, 问题\-挑战
    - autoscaling DynamoDB table provisioned capacity, 问题\-挑战
- Schema Conversion Tool (`SCT`), 讨论
- schemas, 讨论
- secrets
    - creating using AWS CLI, 步骤
    - `SECRET_ARN` role, replacing, 步骤
    - storing, encrypting, and accessing passwords with Secrets Manager, 问题\-挑战
- Secrets Manager
    - creating and storing passwork in, 解决方案
    - generating passwords with, 步骤
    - using to generate complex password, 步骤
    - using with Lambda function to automatically rotate RDS database passwords, 解决方案\-挑战
- `SecretsManagerReadWrite` policy, 步骤, 步骤
- security
    - administrative access, 讨论
    - blocking public access for S3 bucket, 问题\-挑战
    - encryption at rest, 讨论
    - endpoint policy to restrict access to S3 buckets, 讨论
    - fine-grained security capabilities on AWS, 简介
    - IAM role, creating and assuming for developer access, 问题\-挑战
    - scanning container images for vulnerabilities on push to ECR, 问题\-挑战 2
    - security topics on AWS, 简介
    - serving web content securely from S3 with CloudFront, 问题\-挑战
    - trusted code, running in Lambda, 讨论
    - workstation configuration, Workstation Configuration
- security groups
    - CIDR management using prefix lists, 问题\-挑战
    - creating VPC security group for database, 步骤
    - deleting network interfaces associated with, 快速修复
    - EC2 instances's security group allowing ingress traffic from ALB, 步骤
    - granting dynamic access to EC2 instances by referencing, 问题\-挑战
    - for RDS Proxy, 步骤
        - rule allowing access on TCP port 3306 for Lambda APP function security group, 步骤
    - RDS MySQL database instance, ingress rule allowing access on TCP port 3306, 步骤
    - referencing in peered VPCs, 讨论
    - referencing other security groups, 步骤
    - rules to allow HTTP and HTTPS traffic, 步骤
    - updating rule to allow access SSH access between instances, 步骤
- Security Token Service (`STS`), 验证检查, 讨论
- self-referencing rule (security group), 步骤
- serverless
    - accessing VPC resources with Lambda, 问题\-挑战 1
    - automating CSV import into DynamoDB from S3 with Lambda, 问题\-挑战 3
    - benefits of, on AWS, 简介
    - configuring ALB to invoke Lambda function, 问题\-挑战 2





### S

- 将 Lambda 函数配置为访问 EFS，问题 – 另请参阅
- 将 Lambda 代码打包在容器镜像中，问题 – 挑战
- 使用 Lambda 层打包库，问题 – 挑战
- 先决条件，Lambda 函数执行的 IAM 角色，Lambda 函数执行的 IAM 角色
- 使用预置并发减少 Lambda 启动时间，问题 – 挑战 2
- 使用 AWS Signer 在 Lambda 中运行受信任代码，问题 – 问题
- AWS 上可用的服务，简介
- Session Manager（请参阅 SSM Session Manager）
- 分片，步骤
- Lambda 函数代码的签名过程，解决方案
- 单点登录 (SSO)，解决方案，简介，讨论
  - 设置 AWS Organizations 和 SSO，问题 – 挑战 2
- 快照
  - 使用快照加密 RDS 数据库的存储，解决方案 – 挑战
  - 删除 RDS 集群时跳过最终快照，清理
- SNS 主题，创建并订阅它，解决方案 – 挑战
- 源 S3 存储桶，为其配置复制策略，步骤
- SQL 查询，在存储于 S3 中的文件上运行，问题 – 挑战
- `SSH`，允许 EC2 实例之间连接，问题 – 挑战
- SSL
  - 证书，创建，步骤
  - `OpenSSL`，先决条件
- SSM Session Manager
  - 通过它连接到 VPC 子网中的 EC2 实例，验证检查
  - 使用它连接到 EC2 实例，问题 – 挑战
- SSML (Speech Synthesis Markup Language) 标签，步骤
- SSO（请参阅单点登录）
- 标准存储类，讨论
- 存储，简介 – 挑战 2
  - 使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，问题 – 挑战
  - ElastiCache 服务实现 `redis` 或 `memcached` 用于存储，讨论
  - 加密现有用于 `MySQL` 的 RDS 数据库的存储，问题 – 挑战
  - 从 EBS 快照恢复文件，问题 – 挑战
  - 使用智能分层归档策略自动归档 S3 对象，问题 – 挑战 2
  - 使用 S3 生命周期策略降低存储成本，问题 – 挑战 2
- Storage Lens，使用它观察 S3 存储和访问指标，问题 – 挑战 2
- 流式数据
  - 使用 Kinesis Data Firehose 流式传输到 S3，问题 – 挑战
  - 使用 Kinesis Stream 摄取流式数据，问题 – 挑战
- 子网
  - 创建数据库子网组，步骤
  - 在 VPC 中使用子网和路由表创建网络层，问题 – 挑战
  - 私有子网中 EC2 实例的仅出站互联网访问，问题 – 挑战
- 切换角色功能，验证检查

### T

- 表，讨论
- 标签编辑器，使用它一次性修改多个资源的标签，问题 – 挑战
- 标记容器，步骤，讨论
- AWS 资源的标签，讨论
- 负载均衡器的目标组，步骤
- 图像中的文本，确定其位置，问题 – 挑战
- 文本转语音转换，问题 – 挑战
- 文本，在视频中检测，问题
- `Textractor` 工具
  - 分析 Amazon Textract 的输出以确定图像中文本的位置，问题 – 挑战
  - 用于表单数据的计算机视觉分析，问题 – 挑战
- 令牌 (认证)，讨论
  - 允许 Lambda 函数生成令牌的 IAM 策略，步骤
- 令牌 (授权)，步骤
- Transcribe Medical 和 Comprehend Medical，用于医生口述分析，问题 – 讨论
- 转录播客，问题 – 挑战
- 转换
  - 用于转换的 Lambda 函数，讨论
  - 使用 AWS Glue DataBrew 转换数据，问题 – 挑战
- Transit Gateway，启用跨 VPC 的传递性连接，问题 – 挑战

### U

- 通用唯一标识符 (`UUID`)，验证检查
- 用户密码策略，在 AWS 账户中强制执行，在您的 AWS 账户中强制执行 IAM 用户密码策略 – 挑战

### V

- 视频，检测其中的文本，问题 – 挑战
- VPC (Virtual Private Cloud)，简介
  - 使用 Lambda 访问 VPC 资源，问题 – 挑战 1
  - 使用互联网网关连接到互联网，问题 – 挑战
  - 使用 VPC 端点控制从 VPC 对 S3 的网络访问，问题 – 挑战
  - 创建 VPC，问题 – 挑战
  - 在 VPC 中使用子网和路由表创建网络层，问题
  - 默认 VPC，查找某个区域的，快速修复
  - 使用 Transit Gateway 启用跨 VPC 的传递性连接，问题 – 挑战
  - 查找当前区域的接口 VPC 端点，快速修复





### P
*   将两个 VPC 对等连接以实现 VPC 间网络通信，问题\-挑战

### R
*   可达性分析器，用于验证和排查网络路径，问题\-挑战

### N
*   使用 NAT 网关实现从私有子网访问互联网，问题\-挑战

### V
*   Session Manager 的 VPN 终端节点，讨论

### W
*   Windows，在其上安装 Docker Desktop，Windows
*   WorkSpaces 网关，其 CIDR 范围列表，解决方案

### Y
*   CLI 命令的 Yaml 输入，快速修复

# 作者简介

**John Culkin** 是 AWS 的高级解决方案架构师。他持有所有当前的 AWS 认证。此前，他是 Cloudreach 的首席云架构师负责人，领导交付符合组织需求的云解决方案。作为一名终身技术学习者，他现在专注于创造利用云服务的变革性业务解决方案。

**Mike Zazon** 是 AWS 的高级云架构师，专注于帮助企业客户实现业务现代化。他曾担任云软件开发人员、软件工程师、软件架构师、IT 经理和数据中心架构师。他在工程研究大学环境中担任其中一些角色时，对技术教育的热情油然而生。

# 版权页

*《AWS Cookbook》（AWS 实战指南）* 封面上的动物是**苍鹰**（*Accipiter gentilis*）。作为一种强大的掠食者，苍鹰属于鹰科（隶属于“隼亚科”），分布于北半球的温带地区。苍鹰是 *Accipiter* 属中唯一一种同时生活在欧亚大陆和北美洲的针叶林及混交林中的物种，通常局限于相对开阔的林区或森林边缘。它是一种候鸟，冬季会向南迁徙。

苍鹰拥有相对较短、宽阔的翅膀和长尾巴，以便在其森林栖息地中灵活机动。就其物种而言，它拥有相对较大的喙、强壮且相对较短的腿以及粗壮的爪子。它的背部呈蓝灰色，腹部呈褐灰色或白色并带有深色横纹。这些鸟类倾向于表现出渐变的颜色变异，这意味着生活在更北方的苍鹰比生活在较暖和地区的苍鹰颜色更浅。

苍鹰的飞行速度惊人，它通过短距离爆发式飞行捕捉猎物，常在树枝间穿梭、冲撞灌木丛，凶猛无比。它是一种安静的掠食者，通过栖息在中等高度的位置进行狩猎，一旦发现猎物便迅速发起攻击。其食物通常包括中型鸟类、松鼠和兔子等小型哺乳动物、小型啮齿动物、蛇类和昆虫。

这种鹰每窝产 2–4 枚蓝白色的蛋，非常有领地意识且极力护巢，会俯冲攻击入侵者（包括人类），有时甚至抓破皮流血。幼鹰通常由雌鹰照料，雌鹰大部分时间都陪伴在幼鹰身边，而雄鹰的主要职责是给巢穴带食物。苍鹰通常终身配对，因此它们在大多数事情上都作为伴侣共同行动。

苍鹰拥有悠久而高贵的历史。在中世纪，只有贵族才被允许驾驭苍鹰进行猎鹰活动。古老的欧洲猎鹰文献称苍鹰为“平民的鸟”或“厨师的鸟”，因为它们作为捕猎食用猎物的狩猎伙伴非常实用。目前，苍鹰的保护现状为“无危”，因为其种群数量保持稳定。然而，随着森林砍伐的加剧，这些强大鸟类的栖息地丧失，其种群可能会动摇。O'Reilly 封面上的许多动物都处于濒危状态；它们对世界都至关重要。

封面插图由 Karen Montgomery 绘制，基于 *British Birds* 中的一幅黑白雕刻。封面字体为 Gilroy Semibold 和 Guardian Sans。正文字体为 Adobe Minion Pro；标题字体为 Adobe Myriad Condensed；代码字体为 Dalton Maag 的 Ubuntu Mono。

