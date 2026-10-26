## 附录：复习题答案

**4.**   B. A不正确。使用CLI接口通过Internet连接迁移50 TB数据到Amazon S3将非常耗时。由于要求是快速完成此迁移，因此此解决方案不可行。

B正确，因为使用Snowball进行数据迁移是更好的选择。数据进入S3后，使用Glue进行爬取以构建目录，并使用Athena进行分析将是更可取的，因为客户希望尽可能减少管理的服务器数量，而Athena和Glue都是无服务器的。

C不正确，因为使用Snowball进行数据迁移是可行的，但使用EMR上运行的Hive配合Spark来构建目录成本更高，并且需要更多资源和额外的脚本编写，如果选择选项B则可以避免这些。

D不正确，因为使用CLI接口迁移Amazon S3数据（成本高昂）且更复杂（使用Glue编目）。

**5.**   C. A不正确，因为该解决方案成本高昂。由于只需要30 TB数据，将数据加载到HDFS中并支付Hadoop集群上的相关存储成本是不必要的，从成本角度来看也不太可能为客户带来任何收益。

B不正确，原因已在选项A的解释中提及。虽然Redshift是云数据仓库的良好解决方案，但将所有热数据和冷数据都保存在Redshift上并不是一个好方法，而且考虑到之前关于数据湖的讨论，这是一种反模式。该解决方案可行，但维护成本更高。虽然Redshift具有成本优势，比传统数据仓库解决方案便宜10倍，但客户可以从其他架构中获得更好的成本节省。

C正确，是设置多温度仓库的良好解决方案。该解决方案不仅能在短期内降低成本，而且在数据增长时也是一种良好的长期架构。数据增长意味着客户可以更多地使用S3，并在需要连接热数据和冷数据时使用Redshift Spectrum。

D不正确。Elasticsearch不是构建数据仓库的好工具，因为需要创建多种访问路径来访问数据。在ES上存储30 TB数据将非常昂贵，而且每个索引都会使存储需求成倍增加。Amazon Redshift是构建数据仓库的更好架构选择。

**6.**   D. A不正确，因为CSV是行格式。使用Athena查询CSV意味着要读取大量不必要的数据属性，这将导致性能降低和数据扫描成本升高。

B不正确，因为将数据加载到VARCHAR列中，然后使用JSON提取函数进行查询将更加昂贵，并且连接性能会非常差。

C不正确，因为属性提取会获得更好的性能，但Redshift上的数据存储比S3上更昂贵。此外，从长远来看，该解决方案不具备成本效益。





D 是正确的，因为它使用无服务器选项将数据转换为 Parquet 等列式格式，并且提供了一个更健壮的解决方案，即使数据量增长也能保持未来可用性。

- **7. C.** A 是不正确的，因为不应首先查看列的数量和类型。虽然在某些情况下列类型可能会影响查询连接，但应先查看基础项，其中包括查看谓词和排序键，这些会限制查询中扫描的数据量。
  
  B 是不正确的。Redshift 主键约束仅用于信息目的。
  
  C 是正确的，因为排序键充当索引，负责限制从磁盘扫描的数据。
  
  D 是不正确的。Redshift 是 MPP 数据库，如果设计得当，表中行的数量不应直接影响查询性能。
  
  E 是不正确的。数据在数据库中，Redshift 不支持分区。

- **8. C.** A 是不正确的。AWS 可以捕获初始转储，但对于没有增量键的数据库，AWS Glue 无法捕获更改。请参阅以下网页：aws.amazon.com/blogs/database/how-to-extract-transform-and-load-data-for-analytic-processing-using-aws-glue-part-2/
  
  B 是不正确的，因为 AWS DMS 不能用于捕获 CSV。
  
  C 是正确的，因为初始转储可以用 Glue 完成，增量捕获可以用 DMS 用于数据库，以及 AWS Glue 用于 CSV 文件。
  
  D 是不正确的，因为 DMS 无法从 CSV 文件捕获更改或初始转储。

- **9. A.** Amazon QuickSight 不允许您对报表中的数据进行更改。

- **10. A.** Amazon QuickSight 允许您创建自动叙述。请参阅以下网页：https://docs.aws.amazon.com/quicksight/latest/user/narratives-creating.html

## Chapter 2: Data Collection

- **1. D.** 选项 A 不提供任何实时分析数据的支持。选项 B 不正确且含糊不清。选项 C 涉及 Kinesis Firehose，有助于聚合数据而非实时数据分析。选项 D 是正确的，因为它涉及 Kinesis Analytics 和 KCL。

- **2. A.** Amazon Kinesis 是用于摄取实时流数据的服务。Data Pipeline 主要用于批处理，Amazon SQS 用于基于消息的通信，而 Amazon EMR 具有多种主要为批处理的引擎。您可以使用 Spark Streaming，但选项中并未明确提及。

- **3. B, C, D.** 说明：A 不正确，因为 AWS Glue 内置分类器不能用于任何数据类型。B、C、D 正确 – 参见：https://docs.aws.amazon.com/glue/latest/dg/populate-data-catalog.html。

- **4. A.** IoT 规则引擎使您能够转换从 IoT 设备接收的数据。

- **5. D.** AWS Database Migration Service 可帮助您快速、安全地将数据库迁移到 AWS。迁移期间源数据库保持完全运行状态，将依赖于数据库的应用程序停机时间降至最低。AWS Database Migration Service 可将您的数据迁移到和来自最广泛使用的商业和开源数据库。
  
  https://aws.amazon.com/dms

- **6. D.** AWS Glue 是推荐的选项，因为它具有无服务器特性，并提供按需付费模式，无固定成本。

- **7. A.** 说明：使用队列长度将有助于根据需求扩展服务器，并在需求较低时将其缩减。请参阅以下链接：https://amzn.to/3bWbDma。

- **8. B.** 说明：AWS Schema Conversion Tool 是将架构从 Oracle 转换为 Amazon Aurora 的正确选择。

- **9. A.** 说明：AWS Snowmobile 是大规模 PB 级数据传输的推荐选项。

- **10. A, B.** 说明：由于数据量很大，Data Pipeline 不是可行选项，因为您需要它简单且快速。数据管道需要您构建自定义管道。同样，AWS Snowball 也不是快速选项，因为周转时间可能相当高。但是，如果您拥有 Direct Connect 并能够进行导入/导出，那将是最快的方式将数据带到 Amazon S3 并复制到 Amazon Redshift。

## Chapter 3: Data Storage

- **1. B.** A 不正确，因为 Amazon DynamoDB 不适合存储音频/视频和 PDF 文件。
  
  C 不正确，因为将元数据存储在 DynamoDB 中可能适合更快访问，但对于 BI 应用而言并非理想选择。
  
  D 不正确，因为 HDFS 不是存储元数据的理想选择。
  
  B 正确，因为 Redshift 是分析的绝佳工具。

Author note: If you got the answer wrong, do not worry as you should be able to get a better handle on this after the Chapter 4.

作者注：如果您答错了，请不要担心，在阅读完第 4 章后，您应该能更好地掌握这一点。

- **2. B.** A 不正确，因为问题期望几乎不编码，而 Spark streaming 需要付出努力将数据批处理到 Amazon S3。
  
  C 不正确，因为 Amazon Kinesis Data Streams 不适合将数据刷新到 Amazon S3。
  
  D 不正确，因为 Redshift 不适合流用例。
  
  E 不正确，因为 Amazon EMR 本身提供流选项，如 Flink 和 Spark Streaming，但两者都需要额外努力。
  
  B 正确，因为 Amazon Kinesis Data Firehose 提供与 Amazon S3 的原生连接。

- **3. A.** 请求的性质表明它是低频访问，因此选项 A 最佳。所有其他选项要么更昂贵，要么由于访问和可靠性需求而不符合要求。

- **4. B.** DAX 是 DynamoDB 兼容服务。
  
  参见链接：https://aws.amazon.com/dynamodb/dax

- **5. B.** A 不正确，因为访问必须基于订阅者 ID。

- **6. B, D.** A 不正确，因为 Amazon DocumentDB 不兼容 Cassandra，实际上是 MongoDB 兼容。
  
  C 不正确，因为 Amazon DocumentDB 可扩展至 64 TB。

- **7. A.** Amazon DocumentDB 集群只能部署在 VPC 内。

- **8. D.** 所有用例都是图用例。

## Chapter 4: Data Processing and Analysis

- **1. B.** AWS Glue 是实现数据转换的最简单方法，使用主要是点按式界面，并利用内置的去重选项，使用 FindMatches ML Transform。

- **2. A.** 选项 A 正确，因为 MapReduce 是 Hadoop 2.0 出现之前 Hadoop 上的默认处理引擎。
  
  选项 B 不正确，因为 YARN 是 Hadoop 上应用程序的资源管理器。
  
  选项 C 不正确，因为 Hive 是 SQL 层，可使用 MapReduce、Spark、Tez 等处理引擎。
  
  选项 D 不正确，因为 ZooKeeper 是分布式配置和同步服务，可作为大型分布式系统的命名注册表。

- **3. B.** Amazon Athena 是一种交互式查询服务，可让您使用标准 SQL 轻松分析 Amazon S3 中的数据。Athena 是无服务器的，因此无需管理基础设施，您只需为运行的查询付费。
  
  https://aws.amazon.com/athena

- **4. A, B, D.** Amazon Athena 支持多种数据格式，如 CSV、TSV、JSON 或 TextFile，也支持开源列式格式，如 Apache ORC 和 Apache Parquet。Athena 还支持 Snappy、Zlib、LZO 和 GZIP 格式的压缩数据。通过压缩、分区和使用列式格式，您可以提高性能并降低成本。
  
  https://aws.amazon.com/athena/faqs

- **5. D.** A – 不正确，因为 Glue 对自定义配置的选项有限。
  
  B – 不正确，因为使用集群模式的 EMR 更昂贵。
  
  C – 不正确，因为使用步骤模式的 EMR 比集群模式便宜，但使用按需实例时更昂贵。
  
  D – 正确

- **6. B.** EMR 文件系统 EMRFS 是 HDFS 的实现，所有 Amazon EMR 集群都使用它来直接从 Amazon EMR 读取和写入 Amazon S3 的常规文件。EMRFS 提供将持久数据存储在 Amazon S3 中以供 Hadoop 使用的便利性，同时还提供一致视图和数据加密等功能。
  
  https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-fs.html

- **7. D.** Elasticsearch 为您的应用程序、网站和数据湖目录提供快速、个性化的搜索体验，让用户能够快速找到相关数据。您可以访问 Elasticsearch 的所有搜索 API，支持自然语言搜索、自动补全、方面搜索和位置感知搜索。您还可以使用它存储、分析和关联应用程序和基础设施日志数据，以更快地查找和修复问题并提高应用程序性能。
  
  https://aws.amazon.com/elasticsearch-service

- **8. D.** 您可以从许多不同来源将流数据加载到 Amazon Elasticsearch Service 域中。某些来源，如 Amazon Kinesis Data Firehose 和 Amazon CloudWatch Logs，已内置对 Amazon ES 的支持。其他来源，如 Amazon S3、Amazon Kinesis Data Streams 和 Amazon DynamoDB，则使用 AWS Lambda 函数作为事件处理程序。Lambda 函数通过处理新数据并将其流式传输到域来响应新数据。
  
  https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-aws-integrations.html

- **9. C.** 请参阅以下文档。
  
  https://aws.amazon.com/redshift

- **10. A, D, F.** 请阅读第 4 章“数据处理和分析”中的“Redshift 架构”部分。
  
  https://docs.aws.amazon.com/redshift/latest/mgmt/overview.html




## 第5章：数据可视化

- 1. D. Amazon QuickSight 是 AWS 提供的原生工具。其他都是合作伙伴产品，可以与 AWS 工具配合使用，但不能作为原生 AWS 服务直接使用。

- 2. D. 相关性基本上是将两到三个度量值相互可视化。组合图适用于趋势和类别，而环形图用于比较维度中各项目的值，例如占总量的百分比。热力图显示两个维度交叉处的度量值。然而，散点图是一种可视化技术，通常用于理解不同变量之间的相关性，例如员工薪资与员工职级。这两者都是度量值，当它们相互绘制时，可以表明彼此的相关性。

- 3. B. 请参阅以下文档页面：docs.aws.amazon.com/quicksight/latest/user/aws-directory-service.html。

- 4. D. 该问题寻求与 S3、Redshift 和 Athena 集成的最简单方式。虽然 D3.js、MicroStrategy 和 Kibana 可用于可视化来自多个数据源的数据，但只有 Amazon QuickSight 提供与 S3、Redshift 和 Athena 的原生集成，并具备预测功能。

- 5. C. Amazon QuickSight、EMR Notebooks 和 Tableau 可用于报表生成，而 Tableau 和 QuickSight 特别适合构建仪表板。然而，对于来自 Elasticsearch 的数据，实现超低延迟的实时报表，最好使用 Kibana 作为 ELK 技术栈的一部分。

## 第6章：数据安全

- 1. D. 与其他账户共享资源是通过跨账户访问实现的。选项 A 不正确，因为供应商已经拥有一个账户。该答案模糊不清，因为它没有指定在哪里创建 IAM 组，并且缺少细节。选项 B 不正确，因为不存在跨账户角色这类东西。选项 C 不正确。请参阅此处记录的详细信息：https://aws.amazon.com/premiumsupport/knowledge-center/cross-account-access-s3

- 2. C. 默认情况下，Amazon Redshift 的访问是锁定的。为了允许访问集群，您需要为集群配置安全组，并设置正确的访问路径。

- 3. B. 每当遇到此类问题时，重要的是要理解，IAM 角色通常是比创建 IAM 用户更可取的方式。此外，使用提供 Web 身份联合的 STS（AWS Security Token Service），可以帮助您的联合应用程序从 Facebook、Amazon 和 Google 等身份提供商进行联合。这不仅允许细粒度的访问控制，还提供了更严格的控制。

- 4. B. 有关更多信息，请参阅以下链接：aws.amazon.com/about-aws/whats-new/2019/11/amazon-kinesis-data-firehose-adds-support-for-customer-provided-keys-for-server-side-encryption。

- 5. D. 有关更多信息，请参阅以下链接：docs.aws.amazon.com/athena/latest/ug/security-iam-athena.html。
