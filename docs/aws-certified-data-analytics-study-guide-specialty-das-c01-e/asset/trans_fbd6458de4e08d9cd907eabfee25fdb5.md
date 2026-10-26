

- Redshift 会在工作负载较低期间在后台自动执行 VACUUM 和 ANALYZE，但 Redshift 用户仍可在添加、删除或修改大量行（> 5%）后，作为工作流程的一部分显式调用 VACUUM 然后执行 ANALYZE。
  - 显式调用 VACUUM 然后执行 ANALYZE 可确保表在下一个工作流程步骤受益之前立即且优先地进行排序、去碎片化和分析。
- 如果相对于整体表大小加载的数据量很小，请禁用 COPY 中的列编码分析（COMPUPDATE）和统计信息更新（STATUPDATE）。
- 在 Amazon Redshift 之外强制执行主键、唯一或外键约束。
- 将工作流/语句包装在显式事务中。
- 考虑使用 DROP TABLE 或 TRUNCATE 而不是 DELETE。
- 当移动表中的所有行时（例如，从源表到暂存表，或反之亦然），请使用更快的 ALTER TABLE APPEND（移动数据而非复制数据）而不是 INSERT INTO SELECT。
- 暂存表：
  - 使用具有 BACKUP NO 选项的临时表或永久表。
  - 通过使用 CREATE TABLE LIKE 或使用 ANALYZE COMPRESSION 将列级压缩添加到 DDL 中，从生产表中镜像压缩设置；此外，在 COPY 命令中关闭自动压缩分析（即 COMPUPDATE OFF）。
  - 如适用，在暂存表和生产表上（在同一键列上）使用 DISTSTYLE KEY 以加快 INSERT INTO SELECT 语句的速度。

## 从其他 AWS 服务加载数据

您可以从其他 AWS 服务（包括但不限于以下）将数据加载到 Redshift：

**Kinesis Data Firehose**  Kinesis Firehose 可以自动将数据加载到 Redshift。它将流数据微批处理到 S3，然后使用 COPY 命令和清单文件将数据加载到 Redshift。您可以指定批大小和加载发生前的秒数。Kinesis Firehose 可以在加载前对数据进行多可用区缓冲，并管理所有压缩和加密。

**Amazon EMR**  您可以从 Amazon EMR（本质上是 HDFS）加载数据。但是，集群需要使用 Redshift 兼容引导操作启动。

```
COPY <table> from 'emr://emr_cluster_id/hdfs_file_path' iam_role "arn"
```

**DynamoDB**  您可以使用 COPY 命令从 DynamoDB 加载数据。但是，COPY 命令会占用您的预置读取容量。

```
COPY <redshift_tablename>
from 'dynamodb://<dynamodb_table_name>' iam_role "arn" readratio
'<integer>';
```

### SSH 端点

您可以通过 SSH 连接的任何端点将数据加载到 Amazon Redshift。此选项对于 SFTP、NAS 或本地远程主机很有用。它需要一个 SSH 清单文件，其中包含 COPY 用于打开 SSH 连接和执行远程命令的信息。

```
COPY <redshift_tablename>
from 's3://ssh_manifest_file' iam_role "arn"
SSH [| optional-parameters];
```

### AWS Lambda

您可以使用 AWS Lambda 通过 aws-lambda-redshift-loader 将数据加载到 Amazon Redshift。连接器可在 GitHub 上获取，地址为 github.com/awslabs/aws-lambda-redshift-loader。如果您计划从 AWS Lambda 将数据加载到 Amazon Redshift，应考虑 AWS Lambda 会话限制。

### 联邦查询

您可以使用联邦查询跨运营数据库、数据仓库和数据湖查询和分析数据。此功能允许您将 Amazon RDS for PostgreSQL 和 Amazon Aurora PostgreSQL 中实时数据的查询与 Amazon Redshift 和 Amazon S3 环境中的查询集成。Redshift 的智能优化器将计算的一部分下推并直接分发到远程运营数据库，以减少网络上传输的数据量。

因此，您可以将实时数据作为业务智能（BI）和报表应用程序的一部分。这使得通过直接查询运营数据库、即时应用转换并将数据加载到目标表而无需复杂的 ETL 管道，可以轻松地将数据摄取到 Redshift。

## 从 Amazon Redshift 卸载数据

构建完整的信息架构通常意味着您偶尔需要从 Redshift 卸载数据。有时可能是因为您想卸载在 Redshift 内部构建但现在托管冷数据的数仓，而在其他时候可能是由于其他应用程序将使用您 Redshift 集群上可用的数据。

您有三种常见的方法可以从 Redshift 卸载数据：

- AWS Glue
- AWS Data Pipeline
- UNLOAD 语句

您可以使用 UNLOAD 命令从 Redshift 提取数据。UNLOAD 基本上是 COPY 语句的逆向操作。UNLOAD 语句将数据从 Amazon Redshift 输出到 S3 中的 CSV 或 Parquet 格式。UNLOAD 的关键语义如下：

- 从 SELECT 语句运行。由于输出通常很大且性能是关注点，因此该语句在所有计算节点上并行运行。如果您想要有序输出，您需要在 SELECT 语句中定义 ORDER BY，并且还要设置 PARALLEL=OFF。
- 加密和压缩会自动处理。
- 生成的文件数大于切片数，通常每个切片多于一个文件。
- 您可以在输出操作中创建清单文件。如果您想将数据移动到另一个集群，这尤其有用。
- 您可以控制写入 S3 的最大文件大小以及是否覆盖输出的覆盖语义。

## Redshift 中的查询优化

Redshift 是一种符合 ACID 和 ANSI SQL 的云数据仓库，使用与 PostgreSQL 类似的语法，同时支持存储过程和自定义 UDF 以及 SQL SELECT 或 Python。典型的 Redshift 查询经过三个主要阶段：

1. 规划 – 用户提交的查询被解析并优化以创建查询计划。
2. 编译 – Redshift 检查其编译缓存以查看是否可以找到查询计划匹配项。如果可以找到查询计划匹配项，则重用现有的编译对象；否则将其转换为子任务，然后将这些子任务单独编译为 C++。
3. 执行 – 编译后的代码由计算节点切片并行执行，然后结果由 leader 节点汇总后发送回请求方。

查询计划是一个基本工具，可帮助您理解和分析复杂查询。它让您了解查询将在集群上实际如何运行。当查询计划被编译时，执行引擎将查询计划转换为步骤、段和流（见图 4.21）：

| 术语 | 定义 |
| --- | --- |
| Step | Step 是在查询执行期间所需的一个单独操作。 |
| Segment | Segment 是需要由单个进程完成的多个步骤的编译。这是可由计算节点单个切片执行的最小编译单元。计算节点切片将并行执行段。 |
| Stream | Stream 是要分发到可用计算节点切片的一组段。您可以有多个段在一个流中，并且段然后并行运行。 |

**图 4.21 流、段和步骤之间的关系**

![](img/fc6a7083d65b0716921d7bd4a5298460_258_0.png)

### Redshift 中的 Explain Plans

您可以使用 EXPLAIN 语句生成 explain plans，该语句允许您生成预格式化的输出，每步一行，单列指示步骤。如果您使用 Redshift Query Editor，您可以生成可视化 explain plan。Explain plans 从底部向上读取，并提供以下信息：

- 本地和 Lakehouse 部分步骤的执行顺序
- 步骤预计返回的行数
- 每个步骤在切片间传输的数据量（网络 I/O 成本高昂，应尽可能优化。）
- 步骤的相对成本

您可以使用 STL_EXPLAIN 表查看系统上先前运行的查询生成的 explain plans 的历史趋势。理解 explain plans 的关键组件之一是理解 explain plan 生成期间使用的术语。如前所述，Redshift 是一种 MPP 数据库，其中多个计算节点协同工作以产生高效结果，这意味着网络在数据处理期间成为一个非常重要的组件，尤其是在连接处理期间。在连接的特定步骤中，您可能需要处理未本地存储的数据，因此可能需要从同一节点上的不同切片获取数据，或者在最坏的情况下，需要从不同节点获取数据。由于网络传输是查询将执行的成本最高的操作，因此了解数据如何在节点间移动非常重要。表 4.15 描述了您将在 explain plans 中看到的术语。

