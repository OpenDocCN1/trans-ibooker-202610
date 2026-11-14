

### G

*   `global condition context keys` (`IAM`), [步骤](#ch01.html#idm46022860366080)
*   `Glue 服务`
    *   `使用 Glue 爬网程序自动发现元数据`, [问题](#ch07.html#ix_Glueser)\-[挑战](#ch07.html#idm46022855771264)
        *   `爬网程序配置摘要`, [步骤](#ch07.html#idm46022855809040)
        *   `创建 Glue Data Catalog 数据库`, [步骤](#ch07.html#idm46022855825552)
    *   `使用 Glue DataBrew 转换数据`, [问题](#ch07.html#ix_Gluesertrnsf)\-[挑战](#ch07.html#idm46022855629824)
*   `用户组` (`IAM`), [步骤](#ch01.html#idm46022860442336)

### H

*   针对 `PHI` 的 `HIPAA` 合规性, [讨论](#ch08.html#idm46022855240192)
*   `HTTP`
    *   `HTTP 301` 响应, [步骤](#ch02.html#idm46022859235632)
    *   允许 `HTTP` 流量的 `安全组规则`, [步骤](#ch02.html#idm46022859263408)
*   `HTTPS`
    *   `使用应用程序负载均衡器将 HTTP 流量重定向到 HTTPS`, [问题](#ch02.html#ix_HTTPS)\-[挑战](#ch02.html#idm46022859199984)
    *   `Session Manager`, `通过 AWS Systems Manager (SSM) 进行通信`, [讨论](#ch01.html#idm46022860111056)

### I

*   `IAM` (`Identity and Access Management`, 身份与访问管理)
    *   `访问密钥`, `AWS SSO` 和, [讨论](#ch09.html#idm46022854741152)
    *   `访问点策略`, [步骤](#ch03.html#idm46022858553632), [讨论](#ch03.html#idm46022858496064)
    *   `创建并代入用于开发人员访问的角色`, [问题](#ch01.html#ix_IAMcr)\-[挑战](#ch01.html#idm46022860536544)
    *   `创建用于密钥访问的策略`, [步骤](#ch01.html#idm46022859976448), [步骤](#ch01.html#idm46022859975104)
    *   `创建用于 Kinesis Data Firehose 的角色`, [步骤](#ch07.html#idm46022855887088)
    *   `创建用于 RDS Proxy 的角色`, [步骤](#ch04.html#idm46022857873424)
    *   `使用权限边界委派管理功能`, [问题](#ch01.html#ix_IAMdeladm)\-[挑战](#ch01.html#idm46022860184976)
    *   `启用 IAM 用户和角色对账单控制台的访问`, [讨论](#ch09.html#idm46022854846080)
    *   `基于访问模式生成最小权限 IAM 策略`, [问题](#ch01.html#ix_IAMLPpol)\-[挑战](#ch01.html#idm46022860472336)
    *   `允许 S3 将对象从源存储桶复制到目标存储桶的角色`, [步骤](#ch03.html#idm46022858714080)
    *   `用于 Lambda 函数执行的角色`, [`IAM role for Lambda function execution`](#ch05.html#idm46022857291200)
    *   `服务相关角色`, [步骤](#ch06.html#idm46022856352464)
    *   `在 AWS 账户中为根用户设置多因素认证`, [步骤](#ch09.html#idm46022854867248)
    *   `使用 IAM Policy Simulator 测试策略`, [问题](#ch01.html#ix_IAMtstpol)\-[挑战](#ch01.html#idm46022860329824)
    *   `用户密码策略`, `在 AWS 账户中强制执行`, [`Enforcing IAM User Password Policies in Your AWS Account`](#ch01.html#IAMuspwd)\-[挑战](#ch01.html#idm46022860393424)
    *   `将 IAM 身份验证与 RDS 数据库配合使用`, [问题](#ch04.html#ix_IAMauth)\-[挑战](#ch04.html#idm46022857905424)
*   `IAM Access Analyzer` (请参阅 `Access Analyzer`)
*   `身份联合`, `用于访问 AWS 账户`, [解决方案](#ch01.html#idm46022860458640)
*   `IGW` (请参阅 `internet gateway`)
*   `Infrequent Access` 存储类, [解决方案](#ch03.html#idm46022858854288), [讨论](#ch03.html#idm46022858820288), [讨论](#ch03.html#idm46022858762112)
*   `实例元数据` (`EC2`), [步骤](#ch01.html#idm46022860132112)
*   `实例配置文件` (`EC2`), [步骤](#ch01.html#idm46022860140576)
    *   `创建并关联允许访问密钥的配置文件`, [解决方案](#ch01.html#idm46022860003152)
*   `Intelligent-Tiering` 归档策略, `用于自动归档 S3 对象`, [问题](#ch03.html#ix_IntTier)\-[挑战 2](#ch03.html#idm46022858744384)
*   `互联网网关`
    *   `仅出站`, `用于具有 IPv6 功能的 VPC 上的私有子网`, [讨论](#ch02.html#idm46022859485168)
    *   `使用互联网网关将 VPC 连接到互联网`, [问题](#ch02.html#ix_intgtwy)\-[挑战](#ch02.html#idm46022859545904)
*   `互联网`, `使用 NAT 网关从私有子网进行出站访问`, [问题](#ch02.html#ix_intNATgtwy)\-[挑战](#ch02.html#idm46022859472944)
*   `IP 地址`
    *   `AWS 提供的 IP 范围列表`, [解决方案](#ch02.html#idm46022859187408)
    *   `IPv4` 中的 `Elastic IP address` (弹性 IP 地址), [步骤](#ch02.html#idm46022859585600)
    *   `用于 ENI`, [讨论](#ch02.html#idm46022859645312)
    *   `在子网中新启动的 EC2 实例上自动分配的选项`, [讨论](#ch02.html#idm46022859558944)
    *   `从 EC2 实例的元数据中检索公有 IP`, [验证检查](#ch02.html#idm46022859568464)
*   `IPv4 CIDR 块`, [步骤](#ch02.html#idm46022859763152)
    *   `额外的`, `与您的 VPC 关联`, [讨论](#ch02.html#idm46022859739600)
    *   `配额`, [讨论](#ch02.html#idm46022859721616)
*   `IPv6 CIDR 块`
    *   `为 Amazon VPC 配置`, [讨论](#ch02.html#idm46022859734960)
    *   `配额`, [讨论](#ch02.html#idm46022859720432)

### J

*   `Java Database Connectivity (JDBC)` 数据存储, `使用 Glue 爬网程序扫描`, [讨论](#ch07.html#idm46022855777904)
*   `jq` 实用程序, [步骤](#ch02.html#idm46022859169008), [先决条件](#ch08.html#idm46022855587216)

### K

*   `Key Management Service (KMS)` (密钥管理服务), [简介](#ch06.html#idm46022856633776)
    *   `创建用于加密数据库快照的 KMS 密钥`, [步骤](#ch04.html#idm46022857750784)
    *   `使用 KMS 密钥加密 EBS 卷`, [问题](#ch01.html#ix_KMSkeyencr)\-[挑战 2](#ch01.html#idm46022860020064)
    *   `指定用于加密 RDS 数据库快照的密钥`, [步骤](#ch04.html#idm46022857732672)
    *   `结合 S3 存储桶密钥使用 KMS 以加密对象`, [问题](#ch03.html#ix_KMSS3)\-[挑战](#ch03.html#idm46022858425440)
*   `密钥轮换`, `自动`, `在 KMS 服务上`, [讨论](#ch01.html#idm46022860037008)
*   `Kinesis Client Library (KCL)`, [讨论](#ch07.html#idm46022855942592)
*   `Kinesis Data Analytics`, [讨论](#ch07.html#idm46022855938896)
*   `Kinesis Data Firehose`, `使用其将流数据传输到 S3`, [问题](#ch07.html#ix_KiDaFi)\-[挑战](#ch07.html#idm46022855857568)
*   `Kinesis Producer Library (KPL)`, [讨论](#ch07.html#idm46022855941024)
*   `Kinesis Stream`, `用于摄取流数据`, [问题](#ch07.html#ix_KiStr)\-[挑战](#ch07.html#idm46022855929760)
*   `KMS` (请参阅 `Key Management Service`)
*   `KMS.NotFoundException` 错误, [验证检查](#ch03.html#idm46022858442672)
*   `Kubernetes`, [简介](#ch06.html#idm46022856663856)

### L

*   `Lambda 函数`
    *   `使用其访问 VPC 资源`, [问题](#ch05.html#ix_LmbdaaccVPC)\-[挑战 1](#ch05.html#idm46022856676432)
    *   `配置应用程序负载均衡器以调用`, [问题](#ch05.html#ix_LmbdacfgALB)\-[挑战 2](#ch05.html#idm46022857219488)
    *   `配置以访问 EFS 文件系统`, [问题](#ch05.html#ix_LmbdacfgEFS)\-[另请参阅](#ch05.html#idm46022857019536)
    *   `与 RDS 数据库的连接`, `利用 RDS Proxy 实现`, [问题](#ch04.html#ix_LambdaDBconn)\-[挑战](#ch04.html#idm46022857790896)
    *   `用于执行的 IAM 角色`, [`IAM role for Lambda function execution`](#ch05.html#idm46022857294464)
    *   `将函数与 Secrets Manager 集成以轮换 RDS 数据库密码`, [问题](#ch04.html#ix_LambdaSecMgr)\-[挑战](#ch04.html#idm46022857579888)
    *   `按计划调用`, [问题](#ch05.html#ix_Lmdafuncinv)\-[挑战](#ch05.html#idm46022857087872)
    *   `将 Lambda 代码打包在容器镜像中`, [问题](#ch05.html#ix_LmbdapkgCI)\-[挑战](#ch05.html#idm46022856874976)
    *   `使用 Lambda Layers 打包库`, [问题](#ch05.html#ix_LmbdaLay)\-[挑战](#ch05.html#idm46022857155616)
    *   `使用预置并发减少启动时间`, [问题](#ch05.html#ix_Lmbdredstrt)

