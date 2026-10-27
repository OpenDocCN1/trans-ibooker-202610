## 索引

A
数据访问管理，83
    Klodars Corporation，87
ACID（原子性、一致性、隔离性、持久性）合规性
    确保数据仓库，42
    Delta Lake 启用，154
Amazon Athena，44
Amazon Kinesis，33
Amazon Macie，82
Amazon Redshift，34
    Redshift Spectrum 和湖仓，40
Amazon S3（简单存储服务），26
    用于组织数据区域的存储桶，78
数据湖存储，26，184
数据湖仓实现，44
Amazon Web Services（见 AWS）
大数据分析引擎，28-33
    Apache Hadoop，29-31
    Apache Spark，31
数据仓库架构，36
    MapReduce，28
    实时流处理，32
Apache Airflow，129
Apache Atlas，87
Apache Flink，33
Apache Hadoop，29-31
    Apache Hive 表 SQL 类似查询，158
数据湖仓数据格式，46
数据处理引擎，105，108
选择使用哪个，30
Apache Hive 表，158
Apache Hudi，162-168
    关于，162-167
数据湖技术，44
文件格式，164-167
增量修改，163
构建其上的托管平台，168
开放数据格式，44，164
    Apache Parquet 架构，139
实时洞察，164
优势，47，167
Upserts，163
何时使用，167
为何成立，163
Apache Iceberg，157-162
    关于，157-161
数据湖技术，44
数据分区优化，162
文件结构，159-161
开放数据格式，28，44，117
    Apache Parquet 架构，139，159
扫描规划，162
模式演进，162
快照隔离，162
优势，47，161
时间旅行，162
何时使用，161
为何成立，157-159
Apache Kafka，33，38
Apache Nutch，29
Apache ORC，159
Apache Parquet
    关于，135
        解释的在线视频，139
    关于数据湖仓数据格式，46
    Apache Spark 效率，110，117
    通过降低成本，139


compression and storage costs, 135, 139
data optimization, 76
documentation online, 136, 139
file format, 136-136
  Apache Hudi, 139
  Apache Iceberg, 139, 159
  Delta Lake, 139, 155
performance and data format, 135-139
data copy performance, 130
New York City taxis example, 137-139
optimization for reads, 135, 137
Apache Ranger, 84
Apache Software Foundation, 195
Apache Spark, 31-31
  about, 31, 152
data processing engine, 105
  about Apache Spark, 111
  about ELT/ETL internals, 108
  components, 108, 142
  driver, 108, 142
  executors/workers, 108, 142
  resource utilization, 109
installation link, 108
Klodars Corp. processing data, 38
performance
  cluster configurations, 131
  configuration choices, 142-144
drivers of performance, 130-133
job configurations, 133
Klodars Corporation, 126
performance tuning, 142-144
performance tuning documentation, 144
service-level indicators, 126
service-level objectives, 125
tools for monitoring performance, 144
Resilient Distributed Datasets, 31
scalability
data format and, 110, 117
IaaS versus PaaS versus SaaS, 113
selecting which to use, 31
Apache Storm, 33
architecture of big data on the cloud
  analytics engines for big data, 28-33
assessment questionnaire, 170-172
  about, 170
  analysis, 172
assess phase, importance of, 174
improving an existing cloud data lake, 174
migrating an existing system to the cloud, 173
starting from scratch, 173
cloud data lake architecture (see architecture of cloud data lake)
cloud data lake storage, 26-28
performance optimization, 133
tiers of data storage, 94
cloud data warehouses (see cloud data warehouses)
data lakehouse (see data lakehouse)
data mesh, 51-56
hybrid approaches, 60
Klodars Corporation
data lakehouse architecture, 48
data mesh architecture, 54
drivers of move to cloud, 22
modern data warehouse architecture, 38-40
which architecture, 56-60
business drivers, 57
customers, 56
decision framework for architecture, 182-184
design considerations, 58-60
growth and future scenarios, 58
hybrid approaches, 60
architecture of cloud data lake, 14
about legacy systems, 12-14
article overviewing modern architectures, 21
assessment questionnaire, 170-172
about, 170
analysis, 172
assess phase, importance of, 174
improving an existing cloud data lake, 174
migrating an existing system to the cloud, 173
starting from scratch, 173
benefits of, 15, 43
cloud data warehouses, 34
modern data warehouse architecture, 36-40
compute and storage disaggregated, 27, 89
designed for the future, 16
fundamentals, 23-25
big data analytics engines, 28-33
cloud data lake storage, 26-28
variety of data, 5, 21, 23-25, 37
Lego pieces, 17, 21, 63
moving and modernizing, 19
Ashdown, Jessi, 79
assessment questionnaire for architecture decision framework, 170-172
  about, 170
  analysis, 172
  assess phase, importance of, 174
  improving an existing cloud data lake, 174
  migrating an existing system to the cloud, 173
  starting from scratch, 173
automation
  cloud provider tools, 72
  data retention policies for auto-deletion, 94
  resource management to manage costs, 72
autoscaling by Databricks, 132
AWS (Amazon Web Services)
  Amazon S3 for data lake storage, 26, 184
  automation and observability tools, 72
  getting started guide, 72
  AWS Data Pipeline, 129
  AWS Glue, 44, 129
  AWS Lake Formation, 184
Azure (Microsoft) getting started guide, 72
Azure Blob storage, 27
Azure Data Factory, 129
  Data Integration Units, 129
Azure Data Lake Storage (ADLS), 27, 184
  filesystem, 78
Azure Event Hub, 33
Azure Monitor, 72
Azure Resource Manager, 72
Azure Synapse Analytics, 35

## B

backups stored in cloud data lake, 38, 192
backward compatibility of data system, 67, 181
batch processing
  about, 104
  Apache Hudi, 164
  data copy internals, 106-107
  Delta Lake unifying with real time streaming, 152
  ELT/ETL internals, 108-110
  other interactive query technologies, 111
BI (see business intelligence)
bias in datasets, 193, 194

big data, 2-7
analytics engines, 28-33
  Apache Hadoop, 29-31
  Apache Spark, 31-31
  data warehouse architecture, 36
  MapReduce, 28
  real-time stream processing, 32
big data processing, 3
elastic infrastructure, 8
  capacity planning, 116
  cloud data lakehouse, 50
Vs of big data, 4-7
  variety, 5, 21, 23-25, 37
block in Apache Parquet file, 136
breaches in data, 78
business intelligence (BI)
  about, 2
  BI analytics, 34
  consumption layer in on-premises, 12
  data consumption in an organization, 36
    curated data, 76
  data lakehouse architecture, 41-44
    benefits and challenges of, 49
    data format, 45
    data stored in one place, 48, 152
  investing in a cloud data lake, 192
  Klodars Corporation data warehouse, 38-40
SQL
  Apache Hadoop and Apache Hive tables, 158
  data lakehouse data format, 45
  data warehouse optimization, 43
  Delta Lake queries on data lake, 47, 156

## C

Cafarella, Mike, 29
California Consumer Privacy Act (CCPA), 78, 194
capacity planning, 115-117
  forecasting demand, 115
  proof of concept (PoC), 116
CCPA (California Consumer Privacy Act), 78, 194
change requiring adaptability, 196
churn versus change, 196
cloud computing fundamentals, 8
  terminology, 8-10
  value proposition, 10
cloud data lake
about starting, 192
(see also journey to a cloud data lake)
advantages of, 15, 43
architecture, 14
(see also architecture of cloud data lake)
article overviewing modern architectures, 21
backups stored in, 38, 192
business intelligence queries, 41
requests burying management team, 52
centrally managed infrastructure, 52
cloud providers, 71
(see also cloud providers)
cost management
demystifying cloud data lake costs, 90
low cost of ownership, 43, 89
storage costs versus warehouses, 6, 37
strategy for, 92
data retention policies for auto-deletion, 94
extract, load, and transform pattern, 38
central team responsible for, 52
implementation (see implementation of cloud data lake)
modern data warehouse architecture, 36
organizing data, 72-78
Apache Spark efficiency, 110
data lake zones, 73-76
data partitioning, 141
Klodars Corporation, 141
lifecycle of data in data lake, 73
master data management, 80
organization mechanisms, 77
performance improvements from, 140-142
performance, 125
(see also performance optimization)
processing system internals
about, 104, 111
batch processing, 105
data copy internals, 106-107
ELT/ETL internals, 108-110
real-time data processing engines, 105
quality of data, 85, 86
scalability, 101-104
about, 97
big data processing job, 117
cloud platform choices, 111
data formats, 117
definition, 97
horizontal scaling, 116
Klodars Corp. cloud offerings, 114
peak capacity planning, 115-117
processing system internals, 104
solutions for scalability, 111-114
vertical scaling, 116
why needed when all is well, 104
storage (see cloud data lake storage)
tabular data format, 150-151
(see also tabular data)
cloud data lake storage, 26-28
Amazon S3, 26
Azure Data Lake Storage, 27
cloud providers, 184
Google Cloud Storage, 27
performance optimization, 133
tiers of data storage, 94
cloud data lakehouse, 50
(see also data lakehouse)
cloud data warehouses, 34
advantages of, 42
capacity planning, 116
cost of storage versus lakes, 6, 37
extract, transform, and load pattern, 38
Klodars Corp. infrastructure evaluation, 68
modern architecture
about, 36
benefits and challenges of, 40
cloud data lake intermediary, 37
Klodars Corp., 38-40
Klodars Corp. infrastructure evaluation, 69-71
reference architecture, 36
version 2.0 in cloud data lakehouses, 50
Cloud Monitoring (Google) observability tool, 72
cloud providers
automation and observability tools, 72
resource utilization tools, 115
choosing, 184
classification of data, 82
copy jobs by ingestion services, 129
cost management of data lake, 89
getting started guides, 72
implementing cloud data lake, 71
organization of data, 78
premium offerings to improve performance, 146
bigger virtual machines, 146
flash storage, 146
scalability considerations, 111-114
Klodars Corporation, 114
resource utilization tools, 115
CloudWatch (AWS) observability tools, 72
column chunk in Apache Parquet file, 136
columnar format of data
  Apache Spark efficiency, 110
  performance and data format, 135-139
  tabular data, 46
    (see also tabular data)
compute engines for data lake data, 44
cost management, 91
data copy resource utilization, 107
data lakehouse requirements, 47
evolution of cloud data lakehouse, 50
metadata used by, 47
scalability of compute resources, 102
computing resources
  infrastructure as a service, 10
  performance tuning, 146
  pizza analogy, 10
  platform as a service, 10
  software as a service, 10
consumption layer of data, 12
costs
  Apache Parquet data format lowering, 139
    compression and storage costs, 135, 139
  architecture costs versus complexity, 182
  cloud data lake business driver, 57
  cloud data lake cost management
    demystifying cloud data lake costs, 90
    low cost of ownership, 10, 43, 89
      strategy for, 92
  cloud providers, 184
  data warehouses versus lakes, 6, 37
  implementation of cloud data lake, 67, 180, 188
  network costs, 90, 91
  paying for what you use of cloud services, 11, 15, 89
  risks of not investing in data, 192
  tiered storage, 26
curation of data, 73
  curated data zone in data lake, 75, 76
  data processing, 104
customer needs
  assessment of, 176
  customers leading technology, 195

data consumers, 80
data transparency and privacy, 194
deliverables planned, 186
empathized and prioritized, 197
forecasting demand, 115
implementation of cloud data lake infrastructure, 64-66
selecting big data architecture, 56
Cutting, Doug, 29

## D

DAG (directed acyclic graph) of Apache Spark application execution plan, 109
data
  amount of data captured and collected, 1
  architecture, 21
    (see also architecture of big data on the cloud; architecture of cloud data lake; data formats)
  bias in datasets, 193, 194
  big data, 2-7
  cloud data lake defining the data, 44-47
  consumption patterns, 36
  curation, 73
    curated data zone in data lake, 75, 76
    data processing, 104
  data flow
    data lakehouse, 41-44
    ETL versus ELT pattern, 38
    modern data warehouses, 36
  data lakehouse data in one place, 48, 152
  data lakehouse evolution, 50
  data-first approach, 14
  elastic infrastructure, 8
    capacity planning, 116
  cloud data lakehouse, 50
  formats (see data formats)
  goals for organization identified, 64-66
  governance (see governance of data)
  importance of, 1, 169
    responsibly managing data, 193-194
    risks of not investing in, 192
  legacy systems, 12-14
  lifecycle in data lake, 73
    data lake zones, 73-76
    grouping data by zone, 77
  organization and performance, 140-142
    data partitioning, 141
    Klodars Corporation, 141


organizing data in a data lake, 72-78
Apache Spark efficiency, 110
data lake zones, 73-76
data partitioning, 141
implementation of data lake, 188
Klodars Corporation, 141
lifecycle of data in data lake, 73
master data management, 80
organization mechanisms, 77
performance improvements from, 140-142
processing system internals
about, 104, 111
batch processing, 105
data copy internals, 106-107
ELT/ETL internals, 108-110
real-time data processing engines, 105
schema
about, 86
Apache Iceberg schema evolution, 162
data cleansing, 75
data format for data lakehouse, 45
data lake schema defined on demand, 14
data warehouse schema enforcement, 42
defining, 75
Delta Lake enforcing, 154, 156
observability of data, 86
sources of, 1
variety of, 23-25
(see also variety in big data)
data analytics frameworks (see analytics engines)
Data and AI Summit (2020) online
keynote announcing data lakehouse, 41
learning resource, 199
data breaches, 78
data catalog, 82
Klodars Corporation, 87
data consumers, 80
lineage of data, 86
data copy
about, 104
internals, 106
components, 106
resource utilization, 107
performance drivers, 128-130
scalability bottlenecks, 110
data engineers, 79
data formats
about, 149
Apache Hudi
about, 162-167
file format, 164-167
incremental modifications, 163
managed platform built on, 168
real-time insights, 164
strengths of, 47, 167
upserts, 163
when to use, 167
why founded, 163
Apache Iceberg
about, 157-161
data partition optimization, 162
file structure, 159-161
scan planning, 162
schema evolution, 162
snapshot isolation, 162
strengths of, 47, 161
time travel, 162
when to use, 161
why founded, 157-159
Delta Lake
about, 152, 155
batch and real time streaming unified, 152
bulk updates supported, 153
schema enforcement, 154, 156
silo elimination, 152
time travel, 156
when to use, 157
why founded, 152-154
Klodars Corporation selecting, 139
open data formats needed, 149-151
(see also open data formats)
performance and
Apache Parquet, 135-139
Apache Spark performance, 133
data copy performance, 130
optimizing performance, 134-140
scalability and, 117
Data Governance (Eryurek, Gilad, Lakshmanan, Kibunguchy-Grant, and Ashdown), 79
data ingestion
Apache Hudi, 164
architecture of cloud data lake, 14
batch processing, 105
copy jobs by ingestion services, 129
data processing, 104
data warehouse architecture, 36
Delta Lake unifying batch with real-time streaming, 152
project-deliverables planning, 186
raw data, 74, 75
real-time data processing engines, 105
streaming data by Apache Kafka, 33, 38
data lake (see cloud data lake)
data lake storage (see cloud data lake storage)
data lakehouse
about, 40
data stored in one place, 48, 152
Delta Lake underpinning, 152-154
evolution of cloud data lakehouse, 50
implementation made easier blog, 44
Klodars Corp. infrastructure evaluation, 69
reference architecture, 41-44
benefits and challenges of, 49
columnar format of data, 46
compute engine selection, 47
data formats, 45
defining the data stored in data lake, 44-47
enabling via open data technology, 44
Klodars Corporation, 48
metadata, 47
data marts, 12
data mesh
about decentralized data mesh, 51-53
culture shift, 52, 54
architecture foundations article online, 51
challenges and benefits, 55
Klodars Corporation, 54
infrastructure evaluation, 69
reference architecture, 53
Data Mesh (Dehghani), 52
data officers, 79
data page in Apache Parquet file, 136
data partitioning, 141
optimization via Apache Iceberg, 162
data producers, 80
lineage of data, 86
Data Quality Fundamentals (Moses, Gavish, and Vorwerck), 85
data scientists
data consumption in an organization, 36
data lakehouse architecture, 41-44
benefits and challenges of, 49
data stored in one place, 48, 152
modern data warehouse, 40
Klodars Corporation, 39
their own datasets, 73
data retention managed, 78
workspace data zone in data lake, 75, 76
data sharing, 83
data type, 81
data warehouses
advantages of, 42
cost of storage versus lakes, 6, 37
extract, transform, and load pattern, 38
layer of on-premises data storage, 12
modern architecture
about, 36
benefits and challenges of, 40
cloud data lake intermediary, 37
Klodars Corporation, 38-40, 69-71
reference architecture, 36
scalability of data, 6
capacity planning, 116
scalability of hardware, 13
storage and compute offered, 27
open data formats, 28
unstructured data support, 6, 51
Databricks
Apache Spark cluster configuration, 131
Delta Lake (see Delta Lake)
lakehouses and, 40
statistics on Parquet lowering costs, 139
Datadog tool for Spark performance tuning, 144
dataset bias, 193, 194
dataset for New York City taxis, 137
debugging cloud data lakes, 190
decision framework for architecture
about, 169
template for planning, 201-210
assessment questionnaire, 170-172
about, 170
analysis, 172
assess phase, importance of, 174
improving an existing cloud data lake, 174
migrating an existing system to the cloud, 173
starting from scratch, 173
Klodars Corporation
about, 175
business drivers, 178
cloud provider, 185
customer requirements assessment, 176
opportunities for improvement, 177-178
prioritizing the requirements, 179
project deliverables planned, 186
technical design and project plan, 180
phase 1: assess
about, 175
business drivers, 178
customer requirements, 176
opportunities for improvement, 177-178
prioritizing the requirements, 179
phase 2: define, 180
architecture, 182-184
cloud provider, 184
finalizing design choices, 182-186
migration decision points, 185
project deliverables planned, 186
phase 3: implementation, 187-189
phase 4: operationalization, 190
Dehghani, Zhamak, 51, 52
deliverables planned, 186
delivery guarantees of real-time stream processing, 33
Delta Lake (Databricks), 152-157
about, 152
table structure, 155
ACID compliance in transactions, 154
batch and real time streaming unified, 152
bulk updates supported, 153
compute engine, 47
data lake technology, 44
data lakehouse underpinned by, 41, 152-154
documentation online, 155
open data format, 44, 117, 152
Apache Parquet architecture, 139, 155
optimized for SQL queries, 47, 156
schema enforcement, 154, 156
silo elimination, 152
time travel, 156
when to use, 157
why founded, 152-154
demand forecasting, 115
design of cloud data lake
about, 63
template for planning, 201-210
data organization, 72-78
Apache Spark efficiency, 110
data lake zones, 73-76
data partitioning, 141
format and scalability, 117
Klodars Corporation, 141
lifecycle of data in data lake, 73
master data management, 80
organization mechanisms, 77
performance improvements from, 140-142
decision framework phase 2: define, 180
architecture, 182-184
cloud provider, 184
finalizing design choices, 182-186
migration decision points, 185
project deliverables planned, 186
decision framework phase 3: implement, 187-189
immediate needs first, 16, 195
infrastructure
about, 63
architecture and deliverables, 67-71
cloud providers, 71
goals for organization identified, 64-66
release and operationalization, 72
Klodars Corporation
architecture and deliverables, 68-71
goals identified for infrastructure, 65-66
directed acyclic graph (DAG) of Apache Spark
application execution plan, 109

## E

enterprise data warehouses, 12
Eryurek, Evren, 79
ethical governance of data, 193-194
extract, load, and transform (ELT) pattern
about, 108
batch processing of data internals, 108-110
about, 104
central team responsible for, 52
data lakes, 38, 73
new pattern for data for analysis, 73
extract, transform, and load (ETL) pattern
about, 108
batch processing of data internals, 108-110
about, 104
data warehouses, 38
previous pattern for data for analysis, 73




## F
实时流处理的容错, 33
虚构公司用例（参见 Klodars Corporation）
需求预测, 115
以可扩展性实现前瞻性, 104

## G
Gavish, Lior, 85
通用数据保护条例（GDPR）, 78, 194
Ghodsi, Ali, 41
Gilad, Uri, 79
Global DataSphere, 1
确定组织目标, 64–66
  云数据湖基础设施的实施, 64–66
  确定云数据湖需求的优先级, 179
  选择大数据架构, 56–60
Google BigQuery, 35, 184
Google Cloud
  自动化与可观测性工具, 72
  数据湖在线说明, 184
  入门指南, 72
Google Cloud Storage（GCS）, 27, 184
  用于组织数据区域的存储桶, 78
Google 专用互联, 130
数据治理
  关于, 79, 188
  关于需求, 78
  访问管理, 83
  涉及的参与者, 79
  建立对数据的信任, 88
  数据目录, 82
  数据分类, 81
  数据共享, 83
  数据的道德治理, 193–194
  数据湖的实施, 188
  Klodars Corporation, 87
  元数据, 82
  可观测性, 85–87
  政策, 81
  隐私法规, 78
  数据质量, 85, 86

## H
Hadoop（Apache）（参见 Apache Hadoop）
水平扩展, 116
混合云方法, 9
  可扩展性考量, 112

## I
IaaS（基础设施即服务）, 10
  数据处理引擎, 27
    Apache Spark, 108
  性能
    Apache Spark 集群配置, 131
    Apache Spark 虚拟机配置, 132
    用于复制作业的虚拟机, 129
  可扩展性考量, 113
    容量规划, 116
身份管理系统, 71
云数据湖的实施
  关于, 63
  规划模板, 201–210
  数据组织, 72–78
    Apache Spark 效率, 110
  数据湖区域, 73–76
  数据分区, 141
  格式与可扩展性, 117
  Klodars Corporation, 141
  数据在数据湖中的生命周期, 73
  主数据管理, 80
  组织机制, 77
  由此带来的性能提升, 140–142
架构决策框架, 187–189
优先考虑即时需求, 16, 195
基础设施
  关于, 63
  架构与可交付成果, 67–71
  云服务提供商, 71
  确定组织目标, 64–66
  发布与运营化, 72
Klodars Corporation
  架构与可交付成果, 68–71
  为基础设施确定的目标, 65–66
信息类型, 81
基础设施即服务（参见 IaaS）
数据湖基础设施
  数据仓库加数据湖, 40
  弹性基础设施, 8
    容量规划, 116

## J
云数据湖之旅, 16–19
  关于开始, 192
  关于评估, 170
    最初步骤相同, 63
  评估, 170–172
  评估分析, 172
  设计云数据湖（参见云数据湖设计）
  经验教训（参见经验教训）
  将现有系统迁移到云端, 173
  从零开始, 173
  规划模板, 201–210
    （另见架构决策框架）

## K
Kibunguchy-Grant, Anita, 79
Klodars Corporation（示例公司）
  关于, xi
  大数据架构
    数据湖仓架构, 48
    数据网格架构, 54
    现代数据仓库架构, 38–40
  架构决策框架
    关于, 175
    业务驱动因素, 178
    云服务提供商, 185
    客户需求评估, 176
    改进机会, 177–178
    确定需求优先级, 179
    规划的项目可交付成果, 186
    技术设计与项目计划, 180
  迁移到云端的驱动因素, 22
  云数据湖的实施
    架构与可交付成果, 68–71
    数据治理, 87
    为基础设施确定的目标, 65–66
  云数据湖仓, 50
  云数据湖的实施
    关于, 63
    架构与可交付成果, 67–71
    云服务提供商, 71
    确定组织目标, 64–66
    发布与运营化, 72
  数据摄取（参见数据摄取）
  经验教训
    共情客户并确定优先级, 198
    客户引领技术, 195
    为不可避免的变化做准备, 197
    负责任地管理数据, 193–194
  性能优化
    数据格式, 139
    数据组织策略, 141
    数据分区, 146
    SLA、SLO 和 SLI, 126
    小文件传输, 146
  通过云产品实现可扩展性, 114
  Kubernetes 和 Spark, 31

## L
Lake Formation 自动化工具, 72
湖仓（参见数据湖仓）
Lakshmanan, Valliappa, 79
遗留数据系统, 12–14
云数据湖架构的乐高积木, 17, 21, 63
经验教训
  关于, 191
  重大影响需要时间, 198
  共情客户并确定优先级, 197
  客户引领技术, 195
  关注“如何”和“何时”, 192
  为不可避免的变化做准备, 196
  负责任地管理数据, 193–194
数据在数据湖中的生命周期, 73
  数据湖区域, 73–76
  按区域对数据分组, 77
  数据湖的实施, 188
直接迁移方法, 185
  本地部署与云端架构对比, 17
数据血缘, 86

## M
机器学习作为数据消费者, 36
MapReduce, 28
主数据管理（MDM）, 80
元数据
  关于, 47, 82
  Apache Hudi 事务, 165
  Apache Iceberg 表格数据结构, 162
  Apache Parquet 文件, 136
  数据目录, 82
定义数据湖数据, 44, 47
数据治理, 82
与主数据管理对比, 80
读取文件或对象, 140
微软 AI 聊天机器人, 78
Microsoft Azure
  自动化与可观测性工具, 72
  入门指南, 72
  （另见 Azure 相关条目）
将现有系统迁移到云端, 173
迁移的决策点, 173
现代数据仓库
  关于, 36
  优点与挑战, 40
  云数据湖中介, 37
  Klodars Corporation, 38-40
  基础设施评估, 69-71
  参考架构, 36
Moses, Barr, 85
多云方法, 9
  可扩展性考量, 112
多租户系统, 9

## N
Netflix（参见 Apache Iceberg）
网络带宽与性能
  Apache Spark 作业, 133
  复制作业, 130
网络成本, 90, 91
网络数据传输开销, 145
可扩展性中的网络资源, 103
  容量规划, 116
纽约市出租车与礼车委员会数据集, 137

## O
对象存储服务, 26
  Amazon S3, 26
  Azure Blob 存储, 27
  Google Cloud Storage, 27
可观测性
  云服务提供商工具, 72
  数据可观测性, 85-87
  用于管理成本的资源管理, 72
通过无服务器功能按需提供的资源, 94
本地数据存储
  局限性, 13
  三层, 12
开放数据格式
  关于需求, 149-151
  Apache Hudi, 44, 164
  Apache Parquet 架构, 139
  Apache Iceberg, 28, 44, 117
  Apache Parquet 架构, 139, 159
  数据的列式格式, 46
  Apache Spark 效率, 110
  性能与数据格式, 135-139
  数据湖仓由……实现, 44
  支持……的数据仓库, 28
  定义数据湖数据, 44-46
  Delta Lake, 44, 117, 152-157
  云数据湖仓的演进, 50
  表格数据
    对开放数据格式的需求, 149
    为何使用表格数据, 150-151
    （另见表格数据）
云数据湖的运营化, 190
优化性能（参见性能优化）
组织成熟度, 68, 181
数据传输开销最小化, 145

## P
PaaS（平台即服务）, 10
  云数据仓库, 34
  公有云上的数据湖存储, 26, 27
  数据处理引擎, 27
    已安装的 Apache Spark, 108
性能
  Apache Spark 集群配置, 131
  复制作业优化, 129
  实时流数据管道, 33
  可扩展性考量, 113
    容量规划, 116
Parquet 数据格式
  关于, 135
  在线解释视频, 139
  关于数据湖仓数据格式, 46
  Apache Spark 效率, 110, 117
  通过……降低成本, 139
  压缩与存储成本, 135, 139
  数据优化, 76
  在线文档, 136, 139
  文件格式, 136-136
  Apache Hudi, 139
  Apache Iceberg, 139, 159
  Delta Lake, 139, 155
  性能与数据格式, 135-139




数据复制性能, 130
纽约市出租车示例, 137-139
针对读取的优化, 135, 137
按消费付费定价模型, 11, 15, 89
PB（拍字节）的数据, 4
  云数据湖, 4, 37
峰值容量规划, 115-117
花生酱果酱三明治
  性能响应时间和吞吐量, 119-124
  可伸缩性, 98
    端到端执行与流水线对比, 99
性能优化
  关于性能, 119
  性能测量基础
    关于, 119
    目标和指标, 121
    测量性能, 122
    优化以获得更快性能, 123-124
  跨多个作业的百分位数, 123, 124
  响应时间, 119
  吞吐量, 119
云数据湖性能
  关于, 125
  Klodars 公司, 126
  SLA、SLO 和 SLI, 125
  性能驱动因素
    Apache Spark 作业, 130-133
    数据复制作业, 128-130
    数据传输最小化的开销, 145
    花生酱果酱三明治, 119-124
    云提供商的高级服务, 146
      更大的虚拟机, 146
      闪存存储, 146
  调优原则和技术
    关于, 134
    Apache Spark 性能调优, 142-144
    Apache Spark 性能调优文档, 144
    Apache Spark 性能调优工具, 144
    数据格式, 134-140
    数据组织, 140-142
    数据分区, 141
个人身份信息（PII）
  数据分类, 81
  政策和法规, 78, 81
  清洗脱敏, 87
    （另见 隐私）
  敏感数据, 82
拍字节（PB）的数据, 4
  云数据湖, 4, 37
平台即服务（参见 PaaS）
数据治理中的政策, 81
  用于自动删除的数据保留策略, 94
隐私
  数据访问管理, 83
  数据泄露, 78
  合乎道德的数据管理, 194
  个人身份信息
    数据分类, 81
    清洗脱敏, 87
    政策和法规, 78, 81
    敏感数据, 82
私有云, 9
  混合云方案, 9
  多云方案, 9
项目交付物计划, 186
Project Zamba 数据使用, 2
公有云
  云数据仓库, 34
  定义, 9
  混合云方案, 9
  多云方案, 9

## Q
数据质量, 85, 86

## R
原始数据, 74, 75
  非表格格式, 150
实时数据处理引擎, 105
实时流处理, 32
  Apache Hudi 见解, 164
  Delta Lake 与批处理统一, 152
重新平台化并改进方法, 186
弹性分布式数据集（RDD）, 31
Google 资源管理器（用于自动化）, 72
在线资源
Apache Parquet
  成本降低, 139
  文档, 136, 139
  解释视频, 139
Apache Spark
  安装链接, 108
  性能调优文档, 144
  性能调优工具, 144
云提供商, 184
  自动化和可观测性工具, 72
  入门指南, 72
会议、客户会议, 199
数据湖仓实现博客, 44
数据网格文章, 51
Data+AI Summit 大会, 199
Delta Lake 文档, 155
现代数据架构概述, 21
纽约市出租车数据集, 137
性能测量中的响应时间, 119
推倒重来方法, 186
Apache Parquet 文件中的行组, 136

## S
SaaS（软件即服务）, 10
数据处理引擎, 27
可伸缩性考量, 113
数据湖中的沙箱数据区, 75, 76
数据保留管理, 78
数据系统的可伸缩性
Databricks 自动伸缩, 132
数据批处理
  关于, 104
数据复制内部机制, 106-107
ELT/ETL 内部机制, 108-110
其他交互式查询技术, 111
云数据湖架构的优势, 15
云服务弹性, 11
容量规划, 116
云数据湖仓, 50
数据复制瓶颈, 110
数据湖, 101-104
  关于, 97
大数据处理作业, 117
云平台选择, 111
数据格式, 117
水平扩展, 116
Klodars 公司的云产品, 114
峰值容量规划, 115-117
处理系统内部机制, 104-111
可伸缩性解决方案, 111-113
垂直扩展, 116
数据组织与性能, 140
定义, 97
“大规模”, 26
遗留数据系统, 13
花生酱果酱三明治, 98
  端到端执行与流水线对比, 99
大数据的数据量, 4
为何在一切正常时需要, 104
横向与纵向扩展, 116
模式
  关于, 86
Apache Iceberg 模式演进, 162
数据清洗, 75
数据湖仓的数据格式, 45
数据湖模式按需定义, 14
数据仓库模式强制, 42
定义, 75
Delta Lake 强制, 154, 156
数据可观测性, 86
搜索引擎与大数据, 28
安全
  数据访问管理, 83
数据泄露, 78
敏感数据, 82
  （另见 隐私）
半结构化数据, 24
敏感数据，遵循政策和法规, 82
按需资源的无服务器能力, 94
服务级别协议（SLA）
  云数据湖性能, 125
数据质量, 86
Klodars 公司, 126
服务级别指标（SLI）
  云数据湖性能, 126
Klodars 公司, 126
服务级别目标（SLO）
  云数据湖性能, 125
Klodars 公司, 126
Shiran, Tomer, 50
数据孤岛
  云数据湖架构的优势, 15
数据湖仓数据集中存放, 48, 152
Delta Lake 消除, 152
遗留数据系统, 13
SLA（参见 服务级别协议）
SLI（参见 服务级别指标）
SLO（参见 服务级别目标）

snapshot isolation in Apache Iceberg, 162
Snowflake 数据云, 35, 69
软件即服务（参见 SaaS）
Spark（参见 Apache Spark）
SQL
    Apache Hadoop 和 Apache Hive
    表, 158
    数据湖仓数据格式, 45
    数据仓库优化, 43
    数据湖上的 Delta Lake 查询, 47, 156
星巴克数据用途, 2
实时流处理中的状态管理, 33
可伸缩性中的存储资源, 103, 116
流式数据
    Apache Hudi, 139, 164
    Apache Kafka 的数据摄取, 33, 38
    实时流处理, 32
    大数据的速率, 5
结构化数据
    大数据处理的数据类别, 23
    数据仓库, 6
    遗留数据系统, 13
云服务的订阅计费, 11, 71

## T
表格数据
    数据的列式格式, 46
        Apache Hudi, 164
        Apache Iceberg（通过元数据）, 162
        Apache Parquet, 135–139
        Apache Spark 效率, 110
        Delta Lake, 155
    数据湖仓的计算引擎, 47
    数据湖非结构化数据作为表格数据, 44
    数据分区, 141
    摄取后的数据准备, 73, 75, 108
    开放数据格式
        关于必要性, 149
        为什么是表格数据, 150–151
    模式规范或描述, 154
TCO（总拥有成本）, 10, 43, 89
云计算术语, 8–10
性能测量中的吞吐量, 119
分层存储, 26
实施所需时间, 67, 181
云数据湖的时间线, 182
总拥有成本（TCO）, 10, 43, 89
跟踪数据血缘, 79
事务
    数据仓库的 ACID 合规性, 42
    事务成本, 92
    策略, 95
云数据湖故障排除, 190

## U
Uber（参见 Apache Hudi）
非结构化数据
    大数据处理的数据类别, 25
    数据湖处理能力, 43
        定义数据, 44–47
    数据仓库支持, 6, 51
Apache Hudi 的更新插入, 163
用例（参见 Klodars 公司（示例公司））

## V
大数据中数据的价值
    关于, 7
    云数据湖架构的优势, 15, 37
    数据仓库与数据湖的成本对比, 6, 37
    数据的重要性, 1, 169
    负责任地管理数据, 193–194
    不投资数据的风险, 192
大数据的可变性
    关于, 6
    云数据湖架构的优势, 15, 37
大数据的多样性
    关于, 5
    云数据湖架构的优势, 15, 21, 37
    含义, 23–25
大数据的速率
    关于, 5
    实时流处理, 32
    （另见 流式数据）
大数据的真实性
    关于, 6
    ACID 合规性
        数据仓库, 42
        Delta Lake, 154
    现代数据仓库的挑战, 40
垂直扩展, 116
大数据的数据量
    关于, 4




- 数据湖与数据仓库，4，37
- 可扩展性，4
- Vorwerck, Molly, 85

## W

- 数据湖中的工作区数据域，75，76
- 数据保留管理，78

## 关于作者

Rukmani Gopalan 是一位产品管理领导者，曾在 Microsoft 和其他初创公司从事数据基础设施和平台方面的工作。她的目标是帮助数据架构师和数据开发者了解构建云数据湖平台的各个方面。她相信，对云上大数据处理建立扎实的概念性理解，能够带来数据平台的稳健实现，从而为组织提供变革性的洞察。她住在华盛顿州雷德蒙德，喜欢通过一次次对话和一杯杯咖啡来探索太平洋西北地区。

## 版权页说明

《*云数据湖*》封面上的动物是叉尾海燕（*Hydrobates furcatus*）。它们是银蓝色的海鸟，上半身颜色比下半身浅。名副其实，它们有一条分叉的尾巴。此外，它们前额陡直、呈灰色，眼睛周围有深色面罩，喙较小。

叉尾海燕的分布范围很广，遍及北太平洋寒冷水域，从日本到加利福尼亚。它们喜欢在丘陵起伏、有草丛和灌木良好遮蔽的岛屿上筑巢，通常出现在离海岸不远的地方。最常见于阿拉斯加南部近海。

这种鸟的一个特殊之处在于它有惊人的嗅觉，因此能够在数英里之外追踪食物。同样有帮助的是，它们偏爱的食物之一是从水面撇取的受伤或死亡动物的油脂。此外，它们还吃鱼类、甲壳类动物和浮游动物。

由于它们分布范围很广，种群数量也没有减少，因此它们在濒危名录中被列为“无危”物种。O’Reilly 封面上的许多动物都濒临灭绝；它们对世界都很重要。

封面插画由 Karen Montgomery 绘制，基于《*英国鸟类*》中的一幅古老线刻版画。封面字体为 Gilroy Semibold 和 Guardian Sans。正文字体为 Adobe Minion Pro；标题字体为 Adobe Myriad Condensed；代码字体为 Dalton Maag 的 Ubuntu Mono。

## O'REILLY

向专家学习。
让自己也成为专家。

- 图书 | 在线直播课程
- 即时答案 | 虚拟活动
- 视频 | 交互式学习

从 oreilly.com 开始吧。

