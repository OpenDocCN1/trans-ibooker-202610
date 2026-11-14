

### S

- 将 Lambda 函数配置为访问 EFS，[问题](#ch05.html#ix_srvlsscfgLmbdEFS) – [另请参阅](#ch05.html#idm46022857023184)
- 将 Lambda 代码打包在容器镜像中，[问题](#ch05.html#ix_srvlsspkgLmbCI) – [挑战](#ch05.html#idm46022856878608)
- 使用 Lambda 层打包库，[问题](#ch05.html#ix_srvlsspkglib) – [挑战](#ch05.html#idm46022857159856)
- 先决条件，Lambda 函数执行的 IAM 角色，[Lambda 函数执行的 IAM 角色](#ch05.html#idm46022857292528)
- 使用预置并发减少 Lambda 启动时间，[问题](#ch05.html#ix_srvlssredLmbdstart) – [挑战 2](#ch05.html#idm46022856737040)
- 使用 AWS Signer 在 Lambda 中运行受信任代码，[问题](#ch05.html#ix_srvlssruntrst) – [问题](#ch05.html#idm46022856952080)
- AWS 上可用的服务，[简介](#ch05.html#idm46022857314160)
- Session Manager（请参阅 SSM Session Manager）
- 分片，[步骤](#ch07.html#idm46022855970896)
- Lambda 函数代码的签名过程，[解决方案](#ch05.html#idm46022857007968)
- 单点登录 (SSO)，[解决方案](#ch01.html#idm46022860453904)，[简介](#ch09.html#idm46022855156640)，[讨论](#ch09.html#idm46022854842864)
  - 设置 AWS Organizations 和 SSO，[问题](#ch09.html#ix_SSOset) – [挑战 2](#ch09.html#idm46022854725712)
- 快照
  - 使用快照加密 RDS 数据库的存储，[解决方案](#ch04.html#ix_snapencrypt) – [挑战](#ch04.html#idm46022857701584)
  - 删除 RDS 集群时跳过最终快照，[清理](#ch04.html#idm46022858053504)
- SNS 主题，创建并订阅它，[解决方案](#ch09.html#ix_SNStpc) – [挑战](#ch09.html#idm46022854909792)
- 源 S3 存储桶，为其配置复制策略，[步骤](#ch03.html#idm46022858695392)
- SQL 查询，在存储于 S3 中的文件上运行，[问题](#ch07.html#ix_SQLqry) – [挑战](#ch07.html#idm46022855686976)
- `SSH`，允许 EC2 实例之间连接，[问题](#ch02.html#ix_SSHallow) – [挑战](#ch02.html#idm46022859393632)
- SSL
  - 证书，创建，[步骤](#ch02.html#idm46022859279264)
  - `OpenSSL`，[先决条件](#ch02.html#idm46022859289408)
- SSM Session Manager
  - 通过它连接到 VPC 子网中的 EC2 实例，[验证检查](#ch02.html#idm46022859576480)
  - 使用它连接到 EC2 实例，[问题](#ch01.html#ix_SSMSMconn) – [挑战](#ch01.html#idm46022860089632)
- SSML (Speech Synthesis Markup Language) 标签，[步骤](#ch08.html#idm46022855512176)
- SSO（请参阅单点登录）
- 标准存储类，[讨论](#ch03.html#idm46022858817344)
- 存储，[简介](#ch03.html#ix_store) – [挑战 2](#ch03.html#idm46022858173984)
  - 使用 AWS Backup 创建并将 EC2 备份恢复到另一个区域，[问题](#ch03.html#ix_storebkup) – [挑战](#ch03.html#idm46022858327856)
  - ElastiCache 服务实现 `redis` 或 `memcached` 用于存储，[讨论](#ch05.html#idm46022856687472)
  - 加密现有用于 `MySQL` 的 RDS 数据库的存储，[问题](#ch04.html#ix_storeencRDS) – [挑战](#ch04.html#idm46022857696672)
  - 从 EBS 快照恢复文件，[问题](#ch03.html#ix_storeres) – [挑战](#ch03.html#idm46022858255024)
  - 使用智能分层归档策略自动归档 S3 对象，[问题](#ch03.html#ix_storeIntTier) – [挑战 2](#ch03.html#idm46022858745216)
  - 使用 S3 生命周期策略降低存储成本，[问题](#ch03.html#ix_storeS3lfcy) – [挑战 2](#ch03.html#idm46022858809088)
- Storage Lens，使用它观察 S3 存储和访问指标，[问题](#ch03.html#ix_StorLens) – [挑战 2](#ch03.html#idm46022858586144)
- 流式数据
  - 使用 Kinesis Data Firehose 流式传输到 S3，[问题](#ch07.html#ix_strmdaS3) – [挑战](#ch07.html#idm46022855856800)
  - 使用 Kinesis Stream 摄取流式数据，[问题](#ch07.html#ix_strmda) – [挑战](#ch07.html#idm46022855931504)
- 子网
  - 创建数据库子网组，[步骤](#ch04.html#idm46022858119424)
  - 在 VPC 中使用子网和路由表创建网络层，[问题](#ch02.html#ix_subnt) – [挑战](#ch02.html#idm46022859627312)
  - 私有子网中 EC2 实例的仅出站互联网访问，[问题](#ch02.html#ix_subntprv) – [挑战](#ch02.html#idm46022859478512)
- 切换角色功能，[验证检查](#ch01.html#idm46022860788448)

### T

- 表，[讨论](#ch07.html#idm46022855701952)
- 标签编辑器，使用它一次性修改多个资源的标签，[问题](#ch09.html#ix_TagEd) – [挑战](#ch09.html#idm46022855037328)
- 标记容器，[步骤](#ch06.html#idm46022856558960)，[讨论](#ch06.html#idm46022856518992)
- AWS 资源的标签，[讨论](#ch09.html#idm46022855053952)
- 负载均衡器的目标组，[步骤](#ch02.html#idm46022859251408)
- 图像中的文本，确定其位置，[问题](#ch08.html#ix_txtimg) – [挑战](#ch08.html#idm46022855171504)
- 文本转语音转换，[问题](#ch08.html#ix_txtspch) – [挑战](#ch08.html#idm46022855495680)
- 文本，在视频中检测，[问题](#ch08.html#ix_txtdet)
- `Textractor` 工具
  - 分析 Amazon Textract 的输出以确定图像中文本的位置，[问题](#ch08.html#ix_Txtract) – [挑战](#ch08.html#idm46022855175152)
  - 用于表单数据的计算机视觉分析，[问题](#ch08.html#ix_textracttool) – [挑战](#ch08.html#idm46022855430368)
- 令牌 (认证)，[讨论](#ch04.html#idm46022857922208)
  - 允许 Lambda 函数生成令牌的 IAM 策略，[步骤](#ch04.html#idm46022857857136)
- 令牌 (授权)，[步骤](#ch06.html#idm46022856543072)
- Transcribe Medical 和 Comprehend Medical，用于医生口述分析，[问题](#ch08.html#ix_trnsmed) – [讨论](#ch08.html#idm46022855237888)
- 转录播客，[问题](#ch08.html#ix_trnscr) – [挑战](#ch08.html#idm46022855543888)
- 转换
  - 用于转换的 Lambda 函数，[讨论](#ch07.html#idm46022855860656)
  - 使用 AWS Glue DataBrew 转换数据，[问题](#ch07.html#ix_trnsfGDB) – [挑战](#ch07.html#idm46022855631440)
- Transit Gateway，启用跨 VPC 的传递性连接，[问题](#ch02.html#ix_TrnGtwy) – [挑战](#ch02.html#idm46022858953904)

### U

- 通用唯一标识符 (`UUID`)，[验证检查](#ch03.html#idm46022858274544)
- 用户密码策略，在 AWS 账户中强制执行，[在您的 AWS 账户中强制执行 IAM 用户密码策略](#ch01.html#ix_usrpwd) – [挑战](#ch01.html#idm46022860397184)

### V

- 视频，检测其中的文本，[问题](#ch08.html#ix_viddettxt) – [挑战](#ch08.html#idm46022855297264)
- VPC (Virtual Private Cloud)，[简介](#ch06.html#idm46022856646688)
  - 使用 Lambda 访问 VPC 资源，[问题](#ch05.html#ix_VPCaccLmbd) – [挑战 1](#ch05.html#idm46022856678816)
  - 使用互联网网关连接到互联网，[问题](#ch02.html#ix_VPCconn) – [挑战](#ch02.html#idm46022859545008)
  - 使用 VPC 端点控制从 VPC 对 S3 的网络访问，[问题](#ch02.html#ix_VPCs3acc) – [挑战](#ch02.html#idm46022859048512)
  - 创建 VPC，[问题](#ch02.html#ix_VPC) – [挑战](#ch02.html#idm46022859714912)
  - 在 VPC 中使用子网和路由表创建网络层，[问题](#ch02.html#ix_VPCntwk)
  - 默认 VPC，查找某个区域的，[快速修复](#app01.html#idm46022854675936)
  - 使用 Transit Gateway 启用跨 VPC 的传递性连接，[问题](#ch02.html#ix_VPCcross) – [挑战](#ch02.html#idm46022858956560)
  - 查找当前区域的接口 VPC 端点，[快速修复](#app01.html#idm46022854689568)

