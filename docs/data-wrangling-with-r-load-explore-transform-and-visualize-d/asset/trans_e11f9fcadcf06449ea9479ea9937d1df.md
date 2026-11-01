
```
client = boto3.client('s3')
# covid-19 data lake - https://registry.opendata.aws/aws-covid19-lake/
src_bucket = 'covid19-lake'  # SOURCE_S3_BUCKET_NAME
target_bucket = 'S3_TARGET_BUCKET_NAME'
src_object = client.get_object(
    Bucket=src_bucket,
    Key='rearc-covid-19-testing-data/csv/us_daily/us_daily.csv'
)
# Read CSV and Transform to JSON
df = pd.read_csv(src_object['Body'])
jsonBuffer = io.StringIO()
df.to_json(jsonBuffer, orient='records')
# Write JSON to target location
client.put_object(
    Bucket=target_bucket,
    Key='target_prefix/data.json',
    Body=jsonBuffer.getvalue()
)
```

提供的代码片段利用 get_object() 方法从 Amazon S3 检索数据，然后使用 pandas 库将数据从 CSV 格式转换到 JSON 格式。最后，利用 put_object() 方法将转换后的数据写入目标位置。

## 从数据库摄取数据（JDBC 数据存储）

AWS Glue 通常用于将数据从数据库引入数据湖。这通常以批处理方式使用 JDBC 连接完成。要连接到数据库，可以使用 AWS Glue Connection，它存储数据库的登录凭据、URI 字符串和虚拟私有云（VPC）信息。AWS Glue 支持多种类型的连接，包括 JDBC、Amazon RDS、Amazon Redshift、Amazon DocumentDB、Kafka 和 MongoDB。要创建 JDBC 连接，可以使用以下格式：

```
jdbc:protocol://host:port/db_name
```

您可以通过阅读 AWS Glue 文档（https://docs.aws.amazon.com/glue/latest/dg/connection-properties.html#connection-properties-required）了解更多关于为各种数据库创建连接的信息。

以下是 Microsoft SQL Server 数据库连接的示例：

![](img/ec1933a597ce63afa3bcf7496364f412_216_0.png)

图 6.24 – AWS Glue JDBC 连接

以下是代码片段。在这里，我们正在连接 MSSQL 数据库并将数据从 MSSQL 数据库以 Parquet 文件格式移动到 Amazon S3：

```
MicrosoftSQLServer = directJDBCSources(glueContext,
    connectionName="mssql",
    connectionType="sqlserver",
    database="ec2-00-000-000-00.compute-1.amazonaws.com:1433",
    databaseName="dms_sample",
    table="dms_sample.sports",
    redshiftTmpDir="",
    transformation_ctx="MicrosoftSQLServer",
)
S3bucket_node3 = glueContext.write_dynamic_frame.from_options(
    frame=MicrosoftSQLServer,
    connection_type="s3",
    format="parquet",
    connection_options={"path": "s3://target_dir"},
    format_options={"compression": "uncompressed"},
    transformation_ctx="S3bucket_node3",
)
```

## 从 SaaS 数据存储摄取数据

到目前为止，我们已经学习了如何使用 AWS Glue 将数据从对象存储（如 Amazon S3）和关系数据库（如 Microsoft SQL Server）引入。现在，我们将学习如何使用 AWS Glue 从 SaaS 数据存储摄取数据。

要连接到 SaaS 数据存储，需要使用 AWS Glue Marketplace 连接器；这些连接器于 2020 年 12 月推出。在撰写本文时，Marketplace 总共有 66 个连接器，其中包括 AWS 和第三方连接器，如 SnowFlake、ServiceNow、Google BigQuery、ElasticSearch、Apache Iceberg、Delta Lake 等：

![](img/ec1933a597ce63afa3bcf7496364f412_217_0.png)

图 6.25 – AWS Glue Marketplace 连接器

要连接 ServiceNow 并将数据引入 Amazon S3 数据湖，请按照以下步骤操作：

- 1. 访问 AWS Marketplace（https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/marketplace）并订阅 ServiceNow 连接器。在继续之前请查看文档、定价和用户指南。
- 2. 使用已订阅的 ServiceNow 连接器创建连接。
- 3. 使用 AWS Glue Studio 构建 ETL 管道：

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
args = getResolvedOptions(sys.argv, ["JOB_NAME"])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args["JOB_NAME"], args)
Script generated for node CData AWS Glue Connector for ServiceNow
ServiceNowCDataAWSGlueConnectorforServiceNow_node1 =
( glueContext.create_dynamic_frame.from_options( connection_type="marketplace.jdbc", connection_options={ "tableName": "incident", "dbTable": "incident", "connectionName": "service-now-glue-connector", }, transformation_ctx="CDataAWSGlueConnectorforServiceNow_node1", ))
```

您可以在 https://aws.amazon.com/blogs/big-data/extract-servicenow-data-using-aws-glue-studio-in-an-amazon-s3-data-lake-and-analyze-using-amazon-athena/ 找到一篇有用的博文，其中提供了有关如何构建管道以使用 AWS Glue Studio 连接 ServiceNow 并将数据引入 Amazon S3 数据湖的详细分步指南。该博文包括在 AWS Marketplace 订阅 ServiceNow 连接器、使用连接器创建连接以及使用 AWS Glue Studio 构建 ETL 管道的说明。此外，它还解释了在 ServiceNow 数据被摄取到数据湖后如何使用 Amazon Athena 分析该数据。

## 摘要

在本章中，我们介绍了 Apache Spark、它与 AWS Glue 的连接以及 AWS Glue 中提供的各种功能，包括用于数据发现的 AWS Glue Data Catalog、用于元数据提取的 AWS Glue Crawler 以及用于构建基于 UI 的 ETL 管道的 AWS Glue Studio。我们还探讨了如何使用 AWS Glue Marketplace 订阅不同的连接器，以便从 SaaS 应用程序中提取数据。

在下一章中，我们将讨论另一项在数据整理和发现过程中发挥重要作用的基本服务：Amazon Athena。

## 使用 Athena

在之前的章节中，您了解了 AWS Glue 和 Amazon S3 服务以及它们如何融入数据整理管道。在本章中，我们将探讨 Amazon Athena 服务如何帮助数据整理管道中的数据发现、数据结构化、数据丰富和数据质量阶段。我们将使用一些实际示例并对数据进行分析，以使其更具趣味性。

本章涵盖以下主题：

- 了解 Amazon Athena
- 使用 Athena 进行高级数据发现和数据结构化
- 使用 Athena 从多个来源丰富数据
- 使用 Athena 设置无服务器数据质量管道

### 了解 Amazon Athena

Amazon Athena 是一种交互式、无服务器的分析服务，可帮助在不加载数据的情况下探索 Amazon S3 中可用的数据。Athena 通过 SQL 语法或交互式 Spark 应用程序帮助分析数据。它可以帮助客户通过熟悉的 SQL 分析存储在 Amazon S3 上多种数据格式中的数据集。由于 Athena 是一种无服务器服务，用户按查询扫描的数据量付费，无需维护单独的服务器来启用用户查询。这使业务和分析团队能够快速分析、转换和可视化数据湖中的数据，以满足特定用例，而无需依赖数据工程团队构建复杂的 ETL 管道。

Amazon Athena 于 re:Invent 2016 推出，并在随后的时间里经历了重大改进。它最初作为一种无服务器机制推出，用于使用 Athena 目录（与 Glue 目录不同）中的托管 Presto 服务器查询 Amazon S3 数据。现在，它支持从 Glue 目录查询数据、查询联邦、CTAS 语句、Lake Formation 集成、对事务性数据湖格式的支持、Athena for Spark 等。

在 re:Invent 2022 宣布 Athena for Apache Spark 后，Athena 支持以下两种数据分析模式：

- 用户可以使用 SQL 语法查询数据，该语法类似于 Presto/Trino 但有限制。这使没有编程语言专业知识的分析师能够在没有他人支持的情况下分析 S3 数据湖中的数据。以下博文包含
