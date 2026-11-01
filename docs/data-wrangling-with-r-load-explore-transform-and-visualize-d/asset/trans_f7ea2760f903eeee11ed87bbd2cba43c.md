
你可以看到第一次运行查询花费了 3.42 秒，而第二次查询在 ~300 ms 内运行。这相当于与初始运行相比减少了 11 倍的时间。然而，基于初始查询的复杂性，我们只需重用现有结果而不再次执行它即可节省大量时间。另外，请注意查询在表名之前有一些额外的空格以及尾部的分号，在比较查询之前会被移除。

然而，正如前面提到的，只有在执行类似查询时才会实现这些好处。请参考以下示例，其中我们仅从查询中选择了部分列。我们发现 referrer 列在我们特定的数据集中没有任何数据，并且我们希望将其从结果集中移除。如你所见，不带 referrer 列的查询需要 2.63 秒来检索数据。

```
%%time
wr.athena.read_sql_query("SELECT date,time,bytes,method,uri,referrer,result_type from cloudfront_logs limit 1")
```

CPU times: user 83.9 ms, sys: 0 ns, total: 83.9 ms
Wall time: 298 ms

| 日期 | 时间 | 字节 | 方法 | uri | 引用者 | 结果类型 |
|---|---|---|---|---|---|---|
| 0 | 2022-02-18 | 16:12:03 | 481 | GET | / | - | Error |

```
%%time
wr.athena.read_sql_query("SELECT date,time,bytes,method,uri,result_type from cloudfront_logs limit 1")
```

CPU times: user 263 ms, sys: 2.45 ms, total: 265 ms
Wall time: 2.63 s

| 日期 | 时间 | 字节 | 方法 | uri | 结果类型 |
|---|---|---|---|---|---|
| 0 | 2022-02-18 | 16:12:03 | 481 | GET | / | Error |

图 3.54：Athena 集成 – SQL 更改时 Athena 缓存功能限制

## 挑战

你可以扩展查询架构以匹配之前具有更宽数据集的查询，然后应用过滤器以获得更小的数据集。扩展以下列表中列出的高级流程。

- 1. 识别旧查询，并在比较之前将查询中的 where 子句和 from 子句拆分。
- 2. 以类似格式解析新查询并将其与旧查询进行比较。如果新查询的过滤器比旧查询多和/或列比旧查询少，我们可以使用旧查询的数据并在 pandas DataFrame 中应用过滤器并返回结果，例如以下内容：

```
Query 1: select * from hr. employee where city='NY'
Query 2: select * from hr. employee where city='NY' and join_date>'2020-10-01'
```

在这里，我们可以看到 Query 2 是 Query 1 的子集，我们可以使用已执行的数据并在 pandas DataFrame 的列上应用进一步的过滤器。

此外，改进查询匹配逻辑以提高缓存命中率有着巨大的改进空间，你所做的更新可以贡献回 AWS SDK for pandas 仓库。祝编程愉快！！！

此外，它提供了许多有用的模块，其中一些列在这里：

- **修复表**：这会运行 `MSCK REPAIR` 命令，以使用存储层中的最新数据更新表元数据。此函数执行 Hive 的元存储一致性检查 – `MSCK REPAIR TABLE <tablename>`。该命令恢复分区和与分区相关的数据，并更新表定义。这是为了查询在这些分区下可用数据而需要的。当从不更新 Glue 目录的 ETL 作业创建新分区时，这很有用。
- **等待查询**：这会每 250 ms 迭代查询状态，直到查询移动到 FAILED、SUCCEEDED 或 CANCELLED 状态之一（查询执行的最终状态）。如果你想使用更短或更长的轮询机制来验证查询执行状态，你必须自定义你的代码。

`awswrangler` 包在内部处理数据类型转换。结果文件夹中的 Parquet 文件使用 S3 读取函数读取（它反过来使用 `pyarrow` 读取 S3 对象）。Parquet 文件的数据类型转换 – arrow – `athena` 在 `awswrangler` 包内部处理。以下来自 `data_types.py` 模块的片段显示了 `pyarrow` 和 `athena` 数据类型的转换。

```
def pyarrow2athena(  # pylint: disable=too-many-branches,too-many-return-statements
    dtype: pa.DataType, ignore_null: bool = False
) -> str:
    """Pyarrow to Athena data types conversion."""
    if pa.types.is_int8(dtype):
        return "tinyint"
    if pa.types.is_int16(dtype) or pa.types.is_uint8(dtype):
        return "smallint"
    if pa.types.is_int32(dtype) or pa.types.is_uint16(dtype):
        return "int"
    if pa.types.is_int64(dtype) or pa.types.is_uint32(dtype):
        return "bigint"
    if pa.types.is_uint64(dtype):
        raise exceptions.UnsupportedType("There is no support for uint64, please consider
    if pa.types.is_float32(dtype):
        return "float"
    if pa.types.is_float64(dtype):
```

图 3.55：Athena 集成 – pyarrow 数据类型转换

## RDS 数据库

`awswrangler` 中有相当多的集成功能可用于连接关系数据库，例如 Postgres、Oracle、SQL Server 和 MySQL。除此之外，`awswrangler` 还支持基于 DataAPI 访问 Redshift 和 Aurora Serverless 数据库。

`awswrangler` 使用额外的库来连接数据库，这些库也需要安装。以下图表显示了连接不同数据库引擎所需的额外库。

![](img/ec1933a597ce63afa3bcf7496364f412_139_0.png)

图 3.56：RDS 集成 – 数据库引擎和推荐驱动程序

我们必须确保在尝试连接到这些数据库引擎时已安装额外的 Python 库。要连接 Redshift 和 Aurora Serverless 以及 IAM 身份验证，我们可以使用 DataAPI，这也有助于更有效地管理数据库连接。

以下通用模块/功能适用于所有数据库引擎：

- **连接到数据库：** 这会检查数据库引擎是否可用额外的 Python 库，并在用于数据库连接的 Python 库缺失时提供错误。connect 模块需要现有的 Glue 连接或 secret 标识符来提供数据库登录详细信息。例如，secret 应具有以下字段以进行数据库连接：

```
{ "host":"hostname.region.rds.amazonaws.com", "username":"testuser", "password":"testpassword", "engine":"mysql", "port":"3306"}
```

awswrangler 将在内部从 secrets manager 或 Glue 连接格式化连接字符串并使用它来创建连接对象（MySQL 的情况下为 pymysql，sqlserver 的情况下为 pyodbc 等）并返回给调用代码。此连接对象可从那时起用于读取和写入数据库。

- **从数据库读取**：从数据库引擎读取有几个选项 – 从 SQL 读取和从表读取。
从表读取将创建表的 select 查询并再次调用从 SQL 读取模块。

查询通过作为参数传递给从 SQL 读取模块的连接对象进行格式化和提交。代码将使用连接对象的游标执行查询。

一旦从执行中检索到数据，它将被加载到 pyarrow 数组中，以便更快地从游标执行中的记录对象加载数据。然后我们可以快速将其转换为 pandas DataFrame 并返回给调用函数以进一步处理数据。

以下屏幕截图显示了使用 awswrangler 从 S3 加载 Ookla 速度测试数据（410 万条记录）的快速示例，笔记本和数据库的实例大小最低为三分钟。此练习的目的是展示使用 awswrangler 可以轻松加载和查询数据。

```
import awswrangler as wr
import pandas as pd
```

```
dbcon= wr.postgresql.connect("Postgres-wrangler")
```

```
#load S3 data
speed_test=wr.s3.read_parquet("s3://ookla-open-data/parquet/performance/type=mobile/year=2021/quarter=4/")
```

```
len(speed_test)

4120347
```

```
%time
wr.postgresql.to_sql(speed_test, dbcon, schema="public", table="speed_test", mode="overwrite")

CPU times: user 2min 3s, sys: 2.64 s, total: 2min 5s
Wall time: 3min 7s
```

```
%time
df_result=wr.postgresql.read_sql_query("SELECT * FROM public.speed_test limit 10", con=dbcon)

CPU times: user 0 ns, sys: 4.43 ms, total: 4.43 ms
Wall time: 5.48 ms
```

df_result
| quadkey | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices |
|---|---|---|---|---|---|---|
| 0 0022133222330013 POLYGON((-160.032348632812 70.6399478155463, ...)) | 85 | 103 | 72 | 1 | 1 |
| 1 0022133222330201 POLYGON((-160.043334960938 70.6344840663086, ...)) | 14947 | 20624 | 70 | 1 | 1 |
| 2 0022302331031201 POLYGON((-166.1517333989438 68.8714387233513, ...)) | 13099 | 1281 | 91 | 2 | 1 |

图 3.57：RDS 集成 – 从 RDS 加载示例数据

