

### A

*   AWS CodeDeploy，用于编排应用程序部署到 ECS，[问题](#ch06.html#ix_AWSCdDep) - [挑战](#ch06.html#idm46022856245632)
*   AWS Config，[讨论](#ch09.html#idm46022855112752)
*   AWS Control Tower，[讨论](#ch09.html#idm46022854745056)
*   AWS Copilot，使用它部署容器，[问题](#ch06.html#ix_AWSCopi) - [挑战](#ch06.html#idm46022856318912)
*   AWS Database Migration Service (`AWS DMS`)，[讨论](#ch04.html#idm46022858037104)
*   AWS Glue 爬网程序，使用它自动发现元数据，[问题](#ch07.html#ix_AWSGlue) - [挑战](#ch07.html#idm46022855773072)
*   AWS KMS CMK，配置 S3 存储桶以使用引用的密钥，[解决方案](#ch03.html#idm46022858470608)
*   AWS Lake Formation，[讨论](#ch07.html#idm46022855698048)
*   AWS Organizations，[简介](#ch09.html#idm46022855152640)
*   AWS Organizations 和单点登录，设置，[问题](#ch09.html#ix_AWSOrgSSO) - [挑战 2](#ch09.html#idm46022854724096)
*   AWS Policy Generator，[讨论](#ch01.html#idm46022860333760)
*   AWS Schema Conversion Tool，[讨论](#ch04.html#idm46022857425104)
*   AWS SDK，[讨论](#ch01.html#idm46022859951856)
    *   配合特定访问点使用，[步骤](#ch03.html#idm46022858529600)
*   AWS Security Token Service（请参阅 Security Token Service）
*   AWS 安全主题，相关资源，[简介](#ch01.html#idm46022860860928)
*   AWS 服务，列出所有，[快速修复](#app01.html#idm46022854668272)
*   AWS Signer，用于在 Lambda 函数中运行受信任代码，[问题](#ch05.html#ix_AWSSgn) - [问题](#ch05.html#idm46022856950176)
*   AWS Single Sign-On，[解决方案](#ch01.html#idm46022860448496)，[简介](#ch09.html#idm46022855155504)，[讨论](#ch09.html#idm46022854844288)
    *   设置 AWS Organizations 和它，[问题](#ch09.html#ix_AWSSSO) - [挑战 2](#ch09.html#idm46022854728864)
*   AWS SSM Session Manager
    *   通过它连接到 VPC 子网中的 EC2 实例，[验证检查](#ch02.html#idm46022859578448)
    *   使用它连接到 EC2 实例，[问题](#ch01.html#ix_AWSSSMSM) - [挑战](#ch01.html#idm46022860087168)
*   AWS Systems Manager (`SSM`) API，[讨论](#ch01.html#idm46022860110000)
*   AWS 托管 (`aws/ebs`) KMS 密钥，[讨论](#ch01.html#idm46022860039136)
*   `AWSLambdaBasicExecutionRole` 策略，[Lambda 函数执行的 IAM 角色](#ch05.html#idm46022857284864)，[步骤](#ch05.html#idm46022856836256)
*   `AWSLambdaVPCAccess` IAM 策略，[步骤](#ch04.html#idm46022857637680)

### B

*   备份
    *   在 Aurora Serverless 中自动化，[讨论](#ch04.html#idm46022858029536)
    *   使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，[问题](#ch03.html#ix_bkupEC2) - [挑战](#ch03.html#idm46022858324368)
*   大数据，[简介](#ch07.html#ix_bigd) - [挑战](#ch07.html#idm46022855623808)
    *   使用 AWS Glue 爬网程序自动发现元数据，[问题](#ch07.html#ix_bigdautometa) - [挑战](#ch07.html#idm46022855766864)
    *   使用 Amazon Athena 查询 S3 上的文件，[问题](#ch07.html#ix_bigdqry) - [挑战](#ch07.html#idm46022855691312)
    *   使用 Kinesis Data Firehose 将数据流式传输到 S3，[问题](#ch07.html#ix_bigdstrmKDF) - [挑战](#ch07.html#idm46022855851408)
    *   使用 AWS Glue DataBrew 转换数据，[问题](#ch07.html#ix_bigdtrnsf) - [挑战](#ch07.html#idm46022855625488)
    *   使用 Kinesis Stream 摄取流式数据，[问题](#ch07.html#ix_bigdKinStr) - [挑战](#ch07.html#idm46022855928768)
    *   工作站配置，[工作站配置](#ch07.html#idm46022855993696)
*   `Block Public Access` 功能 (S3)，[解决方案](#ch01.html#idm46022859922048)
*   蓝/绿部署，使用它更新容器，[问题](#ch06.html#ix_blgrdep) - [挑战](#ch06.html#idm46022856243280)
*   边界网关协议 (`BGP`)，[讨论](#ch02.html#idm46022858960224)
*   存储桶策略模板以强制对所有对象加密，[步骤](#ch03.html#idm46022858453680)

### C

*   数据库容量，[步骤](#ch04.html#idm46022858094960)
    *   Aurora Serverless 扩展以容量单位衡量，[讨论](#ch04.html#idm46022858043552)
    *   自动扩展容量目标，[步骤](#ch04.html#idm46022858098672)
    *   自动扩展 DynamoDB 表预置容量，[问题](#ch04.html#ix_capDBprov) - [挑战](#ch04.html#idm46022857498752)
    *   检查 RDS 集群，[验证检查](#ch04.html#idm46022858063296)
    *   DynamoDB 预置容量，容量单位的使用，[步骤](#ch04.html#idm46022857529232)
    *   缩减至 0，[步骤](#ch04.html#idm46022858091040)
    *   设置为上限，[讨论](#ch04.html#idm46022858047104)
    *   当数据库活动恢复时，[步骤](#ch04.html#idm46022858086800)
*   CDN（内容分发网络），[讨论](#ch01.html#idm46022859879968)
*   证书颁发机构 (`CA`)，RDS 根 CA，[验证检查](#ch04.html#idm46022857942224)
*   证书
    *   CloudFront 默认主机名上的 `HTTPS` 证书，[讨论](#ch01.html#idm46022859815024)
    *   为 `HTTPS` 创建，[步骤](#ch02.html#idm46022859280608)
    *   `SSL`，[讨论](#ch04.html#idm46022857919904)
*   `CI/CD` 流水线
    *   在其中嵌入 AWS Copilot 命令，[讨论](#ch06.html#idm46022856328096)
    *   蓝/绿部署模式，[讨论](#ch06.html#idm46022856247200)
*   `CIDR`（无类别域间路由）块，[解决方案](#ch02.html#idm46022859772496)
    *   为您的 VPC 仔细选择，[讨论](#ch02.html#idm46022859746464)
    *   创建具有 `IPv6` `CIDR` 块的 VPC，[讨论](#ch02.html#idm46022859731776)
    *   连接 VPC 时使用不重叠的范围，[讨论](#ch02.html#idm46022858901936)
    *   `IPv4` 和 `IPv6` 的配额，[讨论](#ch02.html#idm46022859722384)
    *   使用前缀列表简化安全组中 `CIDR` 的管理，[问题](#ch02.html#ix_CIDR) - [挑战](#ch02.html#idm46022859130416)
    *   为授权指定 `CIDR` 表示法，[讨论](#ch02.html#idm46022859402480)
*   CLI（请参阅 AWS CLI）
*   Cloud Map，[简介](#ch06.html#idm46022856641376)
*   CloudFront，[讨论](#ch01.html#idm46022859883104)
    *   使用它从 S3 安全地提供 Web 内容，[问题](#ch01.html#ix_ClFrsrv) - [挑战](#ch01.html#idm46022859802912)
*   CloudTrail 日志记录，[先决条件](#ch01.html#idm46022860514512)
    *   配置以记录 S3 存储桶上的事件，[步骤](#ch06.html#idm46022856138576)
    *   启用，[步骤](#ch01.html#idm46022860504400)
    *   为您的 AWS 账户启用，[问题](#ch09.html#ix_CldTrllog) - [挑战](#ch09.html#idm46022854969968)
*   CloudWatch，[步骤](#ch01.html#idm46022860247792)
    *   为 ECS 服务配置警报和扩展策略，[解决方案](#ch06.html#idm46022856220080)
    *   获取最近创建的日志组名称，[快速修复](#app01.html#idm46022854711744)
    *   Glue 爬网程序将信息记录到其中，[验证检查](#ch07.html#idm46022855791600)
    *   指标服务，[讨论](#ch06.html#idm46022856176304)
    *   将容器日志流式传输到其中，[解决方案](#ch06.html#ix_CldWtchstrm) - [挑战](#ch06.html#idm46022856015520)
    *   尾随日志组以观察 Lambda 函数调用，[验证检查](#ch05.html#idm46022857114944)
    *   尾随 CloudWatch 组的日志，[快速修复](#app01.html#idm46022854708864)
*   CloudWatch Events（请参阅 EventBridge）
*   `CloudWatchFullAccess` 策略，[验证检查](#ch01.html#idm46022860218480)
*   集群
    *   创建引擎模式为无服务器的数据库集群，[步骤](#ch04.html#idm46022858110592)
    *   数据库，检查其容量，[验证检查](#ch04.html#idm46022858064848)
    *   删除 RDS 集群，[清理](#ch04.html#idm46022858054144)

