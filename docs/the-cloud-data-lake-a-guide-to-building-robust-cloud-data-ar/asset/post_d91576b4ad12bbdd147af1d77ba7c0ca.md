
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
