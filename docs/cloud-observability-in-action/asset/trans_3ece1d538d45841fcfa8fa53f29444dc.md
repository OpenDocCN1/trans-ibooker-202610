
* 在 Jaeger 中，搜索 checkout 服务的跟踪，并检查一个跟踪。问问自己，哪个调用占用了大部分时间？
* 现在，搜索 payment 服务并做同样的操作。你是否注意到与 checkout 服务相似之处？
* 搜索操作 `oteldemo.ShippingService/GetQuote`。这些跟踪有什么共同点？
* 搜索 `frontendproxy`。为什么你看到的跟踪相对较短？
* 在 load-generator 选项卡中，将用户数量增加到更大的数字（10 倍、50 倍等）。现在，仪表板整体如何变化？跟踪持续时间如何变化？

在 OpenTelemetry 项目的背景下，社区也在开发各种演示场景；例如，查看“使用指标和跟踪诊断内存泄漏”场景（[`http://mng.bz/ZqN9`](`http://mng.bz/ZqN9`)），该场景已在线提供。

# 索引

## A

[代理模式，收集器部署 83](`../Text/04.htm#marker-1088512`)

[代理模式，Prometheus 76](`../Text/04.htm#marker-1059080`)

代理

[定义 3](`../Text/01.htm#marker-1012045`)

[日志路由器 67](`../Text/04.htm#marker-1011883`) – [74](`../Text/04.htm#marker-1050385`)

[指标收集 74](`../Text/04.htm#marker-1050412`) – [77](`../Text/04.htm#marker-1060043`)

[概述 66](`../Text/04.htm#marker-1011806`) – [67](`../Text/04.htm#marker-1011856`)

[选择 91](`../Text/04.htm#marker-1138257`) – [93](`../Text/04.htm#marker-1141593`)

[日志的遥测成本 24](`../Text/02.htm#marker-1024218`)

[AIOps 145](`../Text/07.htm#marker-1012185`)

[警报疲劳，避免 148](`../Text/07.htm#marker-1473065`)

[告警，云操作 144](`../Text/07.htm#marker-1473864`)，[146](`../Text/07.htm#marker-1014126`) – [155](`../Text/07.htm#marker-1051524`)

[告警，指标 33](`../Text/02.htm#marker-1112495`)

[Alertmanager 147](`../Text/07.htm#marker-1014180`)，[149](`../Text/07.htm#marker-1473188`) – [154](`../Text/07.htm#marker-1018858`)

[一体式云提供商服务 121](`../Text/06.htm#marker-1011832`)

[前端 131](`../Text/06.htm#marker-1043536`) – [139](`../Text/06.htm#marker-1061247`)

[选择 139](`../Text/06.htm#marker-1061268`) – [140](`../Text/06.htm#marker-1062471`)

[AMA（Azure Monitor 代理）91](`../Text/04.htm#marker-1098922`)

[Amazon CloudWatch Agent 90](`../Text/04.htm#marker-1154171`)

[Amazon CloudWatch Logs 97](`../Text/05.htm#marker-1012155`)

[Amazon CloudWatch Metrics 103](`../Text/05.htm#marker-1056739`)

[Amazon CodeGuru Profiler 186](`../Text/09.htm#marker-1041186`)

[Amazon Managed Service for Prometheus 103](`../Text/05.htm#marker-2278346`)

[Amazon QuickSight 130](`../Text/06.htm#marker-1463306`)

[Amazon Timestream 103](`../Text/05.htm#marker-2278350`)

[异常检测 42](`../Text/02.htm#marker-1124526`)，[145](`../Text/07.htm#marker-1472970`)

[Apache Cassandra 110](`../Text/05.htm#marker-1075006`)，[135](`../Text/06.htm#marker-1051406`)

[Apache Druid 110](`../Text/05.htm#marker-1075030`)

[Apache Pinot 110](`../Text/05.htm#marker-1075054`)

[Apache SkyWalking 136](`../Text/06.htm#marker-1051450`)

[Apache 软件基金会（ASF）135](`../Text/06.htm#marker-1051425`)

[Apache Superset 130](`../Text/06.htm#marker-1030318`)

[AppDynamics 138](`../Text/06.htm#marker-1061024`)

[Application Insights（Azure）106](`../Text/05.htm#marker-1060796`)

[应用级指标 27](`../Text/02.htm#marker-1102031`)

[应用日志 67](`../Text/04.htm#marker-1012789`)

[应用日志，Kubernetes 52](`../Text/03.htm#marker-1025710`) – [54](`../Text/03.htm#marker-1026121`)

[应用性能监控（APM）108](`../Text/05.htm#marker-1073680`)，[137](`../Text/06.htm#marker-1060938`)

[ASF（Apache 软件基金会）135](`../Text/06.htm#marker-1051428`)

[Aspecto 137](`../Text/06.htm#marker-1463594`)

[异步服务 200](`../Text/10.htm#marker-1013187`)

[属性，信号关联 214](`../Text/11.htm#marker-1489791`)

[审计日志，Kubernetes 54](`../Text/03.htm#marker-1026170`)

[自动插桩 64](`../Text/03.htm#marker-1106285`)，[79](`../Text/04.htm#marker-1070030`)

[Autometrics 193](`../Text/09.htm#marker-1056714`)

[自动扩缩容，无服务器计算 56](`../Text/03.htm#marker-1909118`)

[AWS Distro for OpenTelemetry（ADOT）90](`../Text/04.htm#marker-1155491`)

[AWS X-Ray 106](`../Text/05.htm#marker-1060773`)

[Azure Application Insights Profiler 186](`../Text/09.htm#marker-1041203`)

[Azure Monitor 代理（AMA）91](`../Text/04.htm#marker-1098919`)

[Azure Monitor Logs 98](`../Text/05.htm#marker-1012173`)

[Azure Monitor Metrics 103](`../Text/05.htm#marker-1056809`)

## B

[B2I（业务逻辑到插桩）比率 16](`../Text/02.htm#marker-1016248`)

后端目标

[列式数据存储 108](`../Text/05.htm#marker-1073729`) – [114](`../Text/05.htm#marker-1126697`)

[关联层 213](`../Text/11.htm#marker-1012164`)

[成本 114](`../Text/05.htm#marker-1136535`) – [115](`../Text/05.htm#marker-1136678`)

[日志 97](`../Text/05.htm#marker-1012113`) – [101](`../Text/05.htm#marker-1053517`)

[指标 101](`../Text/05.htm#marker-1053552`) – [106](`../Text/05.htm#marker-1060695`)

[概述 95](`../Text/05.htm#marker-1011816`) – [96](`../Text/05.htm#marker-1011957`)

[选择 114](`../Text/05.htm#marker-1126718`) – [118](`../Text/05.htm#marker-1151886`)

[术语 96](`../Text/05.htm#marker-1011979`)

[跟踪 106](`../Text/05.htm#marker-1060716`) – [108](`../Text/05.htm#marker-1073702`)

[与前端对比 95](`../Text/05.htm#marker-1011825`)

[背压，摄取信号 116](`../Text/05.htm#marker-1136799`)

[基线信号值 42](`../Text/02.htm#marker-1124510`)

[伯克利包过滤器（BPF）183](`../Text/09.htm#marker-1039590`)

[BI（商业智能）130](`../Text/06.htm#marker-1463302`)

[二进制大对象（BLOB）60](`../Text/03.htm#marker-1036278`)

[无责事后分析，事件管理 146](`../Text/07.htm#marker-1474025`)

[商业智能（BI）130](`../Text/06.htm#marker-1463301`)

[业务逻辑到插桩（B2I）比率 16](`../Text/02.htm#marker-1016245`)

[业务指标 27](`../Text/02.htm#marker-1102034`)

[业务利益相关者，角色 6](`../Text/01.htm#marker-1012608`)

## C

[cAdvisor 50](`../Text/03.htm#marker-1017843`)

[基数爆炸 33](`../Text/02.htm#marker-1112439`)，[102](`../Text/05.htm#marker-2281011`)

基数问题

[后端 116](`../Text/05.htm#marker-1136829`)

[关联 223](`../Text/11.htm#marker-1068705`)

[子 span，微服务 162](`../Text/08.htm#marker-1012295`)

[Chronosphere 106](`../Text/05.htm#marker-1058088`)

[CI/CD 管道作为信号源 63](`../Text/03.htm#marker-1907291`)

[ClickHouse 110](`../Text/05.htm#marker-1075078`) – [114](`../Text/05.htm#marker-1126692`)，[136](`../Text/06.htm#marker-1056680`)

[云原生计算基金会（CNCF）5](`../Text/01.htm#marker-1021027`)

[云原生环境 2](`../Text/01.htm#marker-1011842`)，[11](`../Text/01.htm#marker-1013670`)

云操作

[告警 146](`../Text/07.htm#marker-1014123`) – [155](`../Text/07.htm#marker-1051521`)

[事件管理 143](`../Text/07.htm#marker-1011846`) – [146](`../Text/07.htm#marker-1014100`)

[概述 142](`../Text/07.htm#marker-1011796`) – [143](`../Text/07.htm#marker-1011824`)

[使用量跟踪 155](`../Text/07.htm#marker-1051545`) – [156](`../Text/07.htm#marker-1067815`)

云提供商

[告警解决方案 154](`../Text/07.htm#marker-1018984`)

[持续剖析 186](`../Text/09.htm#marker-1041167`)

[关联 221](`../Text/11.htm#marker-1023529`)

[成本跟踪 156](`../Text/07.htm#marker-1067800`)

[前端 130](`../Text/06.htm#marker-1463300`)

[日志功能 97](`../Text/05.htm#marker-1012135`) – [99](`../Text/05.htm#marker-1043348`)

[指标功能 103](`../Text/05.htm#marker-1056712`) – [104](`../Text/05.htm#marker-1056867`)

[跟踪 106](`../Text/05.htm#marker-1060746`)

[云提供商。*参见* 一体式云提供商服务](`../Text/06.htm#marker-1043533`)

[Cloud Trace 107](`../Text/05.htm#marker-1060819`)

[Clymene 105](`../Text/05.htm#marker-1056991`)

[冷数据，日志 25](`../Text/02.htm#marker-1086984`)

[collectd 91](`../Text/04.htm#marker-1098942`)
