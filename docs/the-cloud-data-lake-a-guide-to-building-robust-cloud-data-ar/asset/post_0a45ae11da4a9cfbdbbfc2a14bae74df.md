
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
