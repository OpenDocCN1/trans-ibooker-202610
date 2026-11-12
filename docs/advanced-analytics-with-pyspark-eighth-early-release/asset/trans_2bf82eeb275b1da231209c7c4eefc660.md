

```python
from pyspark.sql.functions import col
parsed.groupBy("is_match").count().orderBy(col("count").desc()).show()
...
```

| is_match | count |
| --- | --- |
| false | 5728201 |
| true | 20931 |

与其编写一个函数来提取 `is_match` 列，不如直接将其名称传递给 DataFrame 上的 `groupBy` 方法，调用 `count` 方法来，嗯，计算每个分组内的记录数量，根据 `count` 列将结果数据按降序排序，然后使用 `show` 在 REPL 中整洁地呈现计算结果。底层，Spark 引擎会确定执行聚合并返回结果的最高效方式。这代表了 Spark 提供的干净、快速且富有表现力的数据分析方式。

请注意，引用 DataFrame 中列名称有两种方式：要么作为字符串常量，如 `groupBy("is_match")` 中所示，要么通过使用我们在 `count` 列上使用过的 `col()` 函数来创建 `Column` 对象。在大多数情况下，这两种方法都有效，但我们需要使用 `col` 函数来在生成的 `count` 列对象上调用 `desc` 方法。

## DataFrame 聚合函数

除了 `count`，我们还可以使用 DataFrame API 的 `agg` 方法，结合 `pyspark.sql.functions` 集合中定义的聚合函数，来计算求和、最小值、最大值、平均值和标准差等更复杂的聚合。例如，要查找整体 `parsed` DataFrame 中 `cmp_sex` 字段的平均值和标准差，可以输入：

```
from pyspark.sql.functions import avg, stddev

parsed.agg(avg("cmp_sex"), stddev("cmp_sex")).show()
```

| avg(cmp_sex) | stddev_samp(cmp_sex) |
| --- | --- |
| 0.955001381078048 | 0.207301111689795 |

请注意，默认情况下，Spark 计算的是样本标准差；还有一个 `stddev_pop` 函数用于计算总体标准差。

你可能已经注意到，DataFrame API 上的函数与 SQL 查询的组成部分相似。这并非巧合，事实上，我们可以将创建的任何 DataFrame 视为数据库表，并使用熟悉且强大的 SQL 语法来表达我们的问题。首先，我们需要告诉 Spark SQL 执行引擎，它应该将哪个名称与 `parsed` DataFrame 关联，因为变量本身的名称（“parsed”）对 Spark 不可用：

```
parsed.createOrReplaceTempView("linkage")
```

由于 `parsed` DataFrame 仅在此 PySpark REPL 会话期间可用，因此它是一个 *临时* 表。如果我们配置 Spark 连接到跟踪结构化数据集架构和位置的 Apache Hive 元存储，Spark SQL 还可用于查询 HDFS 中的持久表。

将我们的临时表注册到 Spark SQL 引擎后，我们可以像这样查询它：

```
spark.sql("""
SELECT is_match, COUNT(*) cnt
FROM linkage
GROUP BY is_match
ORDER BY cnt DESC
""").show()
```

| is_match | cnt |
| --- | --- |
| false | 5728201 |
| true | 20931 |

你可以在创建 `SparkSession` 实例时通过其 Builder API 调用 `enableHiveSupport` 方法，选择使用 ANSI 2003 兼容版本的 Spark SQL（默认）或 HiveQL 模式运行 Spark。

## 将 Spark SQL 连接到 Hive

你可以通过 `hive-site.xml` 文件连接到 Hive 元存储，也可以通过在 SparkSession Builder API 上调用 `enableHiveSupport` 方法在查询中使用 HiveQL：

```
spark_session =
SparkSession.builder.master("local[4]").enableHiveSupport().getOrCreate()
```

你可以将 Hive 元存储中的任何表视为 DataFrame，针对元存储中定义的表执行 Spark SQL 查询，并将这些查询的输出持久化到元存储中，以便其他工具（包括 Hive 本身、Apache Impala 或 Presto）可以查询它们。

在 PySpark 中进行分析时，是应该使用 Spark SQL 还是 DataFrame API？两者各有优劣：SQL 的优点是广泛熟悉且对于简单查询很有表现力。它还允许你使用 JDBC/ODBC 连接器从 PostgreSQL 等数据库或 Tableau 等工具查询数据。SQL 的缺点是，以动态、可读且可测试的方式表达复杂的多阶段分析可能会很困难——所有这些都是 DataFrame API 擅长的领域。在本书的其余部分，我们同时使用 Spark SQL 和 DataFrame API，并留给读者作为练习，去检查我们所做的选择，并将我们的计算从一个接口翻译到另一个接口。

我们可以逐个向 DataFrame 应用函数来获取计数和平均值等统计信息。不过，PySpark 提供了一种更好的方式来获取 DataFrame 的摘要统计信息，这将是下一节要介绍的内容。

## DataFrame 快速摘要统计

虽然有多种分析可以用 SQL 或 DataFrame API 同样好地表达，但有一些我们希望对数据框执行的常见操作，用 SQL 表达起来可能会很繁琐。其中一种尤其有用的分析就是计算数据框中数值列所有非空值的最小值、最大值、平均值和标准差。在 PySpark 中，这个函数的名称与 Pandas 中相同，称为 `describe`：

```
summary = parsed.describe()
...
summary.show()
```

`summary` DataFrame 为 `parsed` DataFrame 中的每个变量都有一个列，以及另一个名为 `summary` 的列，用于指示行中其余列所包含的指标——是 count、mean、stddev、min 还是 max。我们可以使用 `select` 方法选择列的子集，以便更轻松地读取和比较摘要统计信息：

```
summary.select("summary", "cmp_fname_c1", "cmp_fname_c2").show()
```

| summary | cmp_fname_c1 | cmp_fname_c2 |
| --- | --- | --- |
| count | 5748125 | 103698 |
| mean | 0.7129024704436274 | 0.9000176718903216 |
| stddev | 0.3887583596162788 | 0.2713176105782331 |
| min | 0.0 | 0.0 |
| max | 1.0 | 1.0 |

请注意 `cmp_fname_c1` 和 `cmp_fname_c2` 之间 `count` 变量值的差异。虽然几乎每条记录都有 cmp_fname_c1 的非空值，但不到 2% 的记录有 cmp_fname_c2 的非空值。要创建一个有用的分类器，我们需要依赖于数据中几乎总是存在的变量——除非它们的缺失表明记录是否匹配有某种意义。

当我们对数据中变量的分布有了整体了解后，我们希望了解这些变量的值与 `is_match` 列的值是如何相关的。因此，下一步是仅针对与匹配和非匹配记录对应的 `parsed` DataFrame 子集计算相同的摘要统计信息。我们可以使用类似 SQL 的 `where` 语法或使用 DataFrame API 的 Column 对象来过滤 DataFrame，然后在生成的 DataFrame 上使用 `describe`：

```
matches = parsed.where("is_match = true")
match_summary = matches.describe()

misses = parsed.filter(col("is_match") == False)
miss_summary = misses.describe()
```

传递给 `where` 函数的字符串内的逻辑可以包含在 Spark SQL 的 `WHERE` 子句中有效的语句。对于使用 DataFrame API 的过滤条件，我们在 "`is_match`" 列对象上使用 `==` 运算符来检查是否与布尔对象 `False` 相等，因为那只是 Python，而不是 SQL。请注意，`where` 函数是 `filter` 函数的别名；我们可以将上面代码片段中的 `where` 和 `filter` 调用互换，一切都会以同样的方式工作。

现在我们可以开始比较 `match_summary` 和 `miss_summary` DataFrames，以查看变量的分布如何取决于记录是匹配还是未匹配。尽管这是一个相对较小的数据集，但进行这种比较仍然有些繁琐——我们真正想要的是转置 `match_summary` 和 `miss_summary` DataFrames，使行和列互换，这样我们就可以按变量将转置后的 DataFrame 连接在一起并分析摘要统计信息，这是大多数数据科学家称为对数据集进行“透视”或“重塑”的做法。在下一节，我们将向你展示如何执行这些转换。

## 透视和重塑 DataFrames

