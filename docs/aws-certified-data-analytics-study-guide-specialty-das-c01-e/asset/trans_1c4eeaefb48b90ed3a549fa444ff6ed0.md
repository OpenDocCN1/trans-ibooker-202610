

- 对数据访问和检索模式的理解
- 对适当的数据布局、模式、结构和格式的理解
- 基于使用模式和业务需求对数据生命周期理解
- 确定用于编目数据和元数据的适当系统
- 根据业务 SLA、数据量和成本确定最合适的数据处理解决方案
- 设计数据转换的解决方案并为进一步分析做好准备
- 为给定场景自动化适当的数据可视化解决方案
- 识别适当的身份验证和授权机制
- 应用数据保护和加密技术
- 应用数据治理和合规控制

## 目标映射

下表列出了每个领域在考试中的权重，以及本书中涵盖该领域目标和子目标的章节。

| 领域 | 考试占比 | 章节 |
|---|---|---|
| **领域 1.0：数据收集** | 18% | 1, 2, 3 |
| 1.1 – 确定收集系统的运营特征。 |  |  |
| 1.2 – 选择能够处理数据频率、数据量和来源的收集系统。 |  |  |
| 1.3 – 选择能够处理数据关键属性的收集系统，例如顺序、格式和压缩。 |  |  |
| **领域 2.0：存储和数据管理** | 22% | 3, 4 |
| 2.1 – 确定分析存储解决方案的运营特征。 |  |  |
| 2.2 – 确定数据访问和检索模式。 |  |  |
| 2.3 – 选择适当的数据布局、模式、结构和格式。 |  |  |
| 2.4 – 基于使用模式和业务需求定义数据生命周期。 |  |  |
| 2.5 – 确定用于编目数据和管理元数据的适当系统。 |  |  |

- B. 使用 AWS Snowball 将数据迁移到 S3，使用 AWS Glue 爬取数据并构建目录，并使用 Amazon Athena 进行分析。
- C. 使用 Amazon Snowball 将数据迁移到 Amazon S3，使用在 EMR 上运行 Spark 的 Hive 构建目录，并使用 Spark SQL 分析数据。
- D. 使用 CLI 接口将数据迁移到 Amazon S3，使用在 EMR 上运行 Spark 的 Hive 构建目录，并使用 Spark SQL 分析数据。

5. 一家领先的金融机构希望将其企业数据仓库迁移到 AWS。其数据仓库中有 30 TB 数据，但仅使用 500 GB 用于报表和仪表板，而剩余数据偶尔需要用于合规目的。以下哪种方案更具成本效益？

- A. 将数据迁移到 AWS。创建带有附加 HDFS 存储的 EMR 集群托管 30 TB 数据。使用 Amazon QuickSight 进行仪表板，使用 Hive 进行合规报表需求。
- B. 将数据迁移到 Amazon S3。创建托管 30 TB 数据的 Redshift 集群。使用 QuickSight 进行仪表板，使用 Athena 从 S3 查询。
- C. 将数据迁移到 Amazon S3。创建托管 500 GB 数据的 Redshift 集群。使用 Amazon QuickSight 进行仪表板，使用 Redshift Spectrum 在需要时查询剩余数据。
- D. 将数据迁移到 AWS 中的 Amazon Elasticsearch Service 集群。使用 Kibana 进行仪表板，使用 Logstash 从 ES 集群查询数据。

6. 一家即将推出的游戏初创公司正在收集其最近发布且非常受欢迎的游戏的游戏日志。日志以 JSON 格式到达，每个记录有 500 个不同的属性。CMO 请求基于六个属性的仪表板，这些属性指示根据营销部门 ML 团队生成的游戏内购买推荐所产生的收入。数据以原始 JSON 格式存储在 S3 上，并且使用 QuickSight 对可用数据生成报表。目前报表创建需要一个小时，而发布报表则非常快。此外，IT 部门抱怨 S3 数据扫描的成本。

他们已要求您作为解决方案架构师提供一种提高性能并优化成本的解决方案。以下哪个选项最适合满足要求？

- A. 使用 AWS Glue 将 JSON 数据转换为 CSV 格式。使用 Glue Crawler 爬取转换后的 CSV 格式数据并使用 Amazon Athena 构建报表。在 Amazon QuickSight 上构建前端。
- B. 将 JSON 数据加载到 Amazon Redshift 的 VARCHAR 列中。使用 SQL 构建报表并在 QuickSight 上构建前端。
- C. 将 JSON 数据加载到 Amazon Redshift。从 JSON 列的表中提取可报表属性。使用 SQL 构建报表并在 QuickSight 上构建前端。
- D. 使用 AWS Glue 将 JSON 数据转换为 Parquet 格式。使用 Glue Crawler 爬取转换后的 Parquet 格式，并使用 Athena 构建报表。在 Amazon QuickSight 上构建前端。

7. 一家领先的制造组织正在运行一个大型 Redshift 集群，并抱怨查询响应时间缓慢。您会检查哪些配置选项来识别问题的根本原因？

- A. 表中列的数量和类型
- B. 主键和外键约束
- C. 表的排序键与 SELECT 语句中谓词的对齐
- D. 表中的行数
- E. 数据库的分区模式

8. 您的客户要求您帮助在 S3 上构建数据湖。源系统为 MySQL、Oracle 和标准 CSV 文件。数据没有增量键。客户期望您在数据湖构建阶段捕获初始数据转储和更改。您会推荐哪些工具/技术来降低迁移的总体成本？

- A. 使用 AWS Glue 加载初始转储和更改。
- B. 使用 DMS（数据库迁移服务）加载初始转储和更改。
- C. 使用 AWS Glue 加载初始转储，并使用 AWS Glue 和 DMS（数据库迁移服务）捕获更改。
- D. 使用 DMS（数据库迁移服务）加载初始转储，并使用 AWS Glue 进行数据更改。

9. QuickSight 仪表板允许您查看源数据但不能对其进行任何更改。

- A. 真
- B. 假

10. Amazon QuickSight 可以为您解读图表和表格，并以纯英语建议见解。

- A. 真
- B. 假

## 参考文献

1. www.weforum.org/agenda/2015/02/a-brief-history-of-big-data-everyone-should-read/
2. www.kdnuggets.com/2017/07/4-types-data-analytics.html
3. research.google.com/archive/gfs-sosp2003.pdf
4. research.google.com/archive/mapreduce-osdi04.pdf
5. en.wikipedia.org/wiki/Amazon_Web_Services
6. open.blogs.nytimes.com/2007/11/01/self-service-prorated-super-computing-fun/
7. AWS re:Invent Presentation by Siva Raghupathy in 2017 www.youtube.com/watch?v=a3713oGB6Zk

![](img/fc6a7083d65b0716921d7bd4a5298460_70_0.png)

## 第 2 章

### 数据收集

system. AWS IoT Analytics 未直接涵盖在考试中，因此我们在此书中不会更详细地介绍它。如果您想了解更多关于 AWS IoT Analytics 的信息，请访问链接 https://aws.amazon.com/iot-analytics

**AWS IoT SiteWise** 是一款完全托管的服务，旨在简化大规模收集和组织来自工业设备的难度。您可以监控工业设施中的设备以识别浪费，例如设备和流程的故障、生产效率低下以及产品缺陷。AWS IoT SiteWise 未直接涵盖在考试中，因此我们在此书中不会更详细地介绍它。如果您想了解更多关于 AWS IoT SiteWise 的信息，请访问链接 https://aws.amazon.com/iot-sitewise。

**AWS IoT Events** 是一款完全托管的服务，使检测和响应来自物联网传感器和应用程序的事件变得更容易。事件基本上是识别出比正常预期更复杂情况的数据模式。一个例子可能是装配厂中传送带卡住时设备变化，或运动探测器使用移动信号激活灯光和安全摄像头。

FIGURE 2.3 AWS IoT 数据服务

![](img/fc6a7083d65b0716921d7bd4a5298460_74_0.png)

### AWS IoT 的常见用例

客户正在为许多有趣的用例使用 AWS IoT，但其中最主要的有以下几种：

- 预测性维护
- 智能库存管理
- 健康和 wellness 解决方案
- 最优资产利用
- 生产力和流程优化

