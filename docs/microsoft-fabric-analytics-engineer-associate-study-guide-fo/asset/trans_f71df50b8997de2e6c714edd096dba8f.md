

# 查看查询计划

你可以在查询框内右键单击某个特定步骤，以查看该步骤的查询计划（见图 4-14）。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_205_0.png)

图 4-14. 从视觉查询编辑器查看查询计划

在此查询计划视图中，我们现在可以开始看到数据从 SQL 数据库（右侧）流向原生查询（左侧）的流程。当你在 Value.NativeQuery 中单击“查看详细信息”时，它会在屏幕右侧显示 Value.NativeQuery 语句。当你在 Sql.Database 中单击“查看详细信息”时，它会显示服务器名称、数据库名称以及连接的选项配置（见图 4-15）。

你可以通过此选项查看查询计划，将整个查询计划导出为 JSON 对象。单击图 4-14 所示屏幕底部的导出按钮，即可下载视觉查询编辑器的 JSON 定义。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_206_0.png)

图 4-15. 视觉查询编辑器中查询计划视图中的 Sql.Database 详细信息

下面是一个此 JSON 的示例，已格式化并截断以提高可读性。

原始导出的 JSON 在需要时包含转义字符，并且换行结构与此处所示不同：

```
{
  "queryName": "Merge",
  "stepName": "Expanded Geography",
  "diagnostics": [
    {
      "queryName": "Trip",
      "queryPlan": {
        "nodes": [
          {
            "kind": "Invocation",
            ...
            ...
            "argumentsByName": {}
          }
        ]
      }
    },
  "removedStructuredColumns": false
```

```
},
"queryPlanAsSteps": "
let
#"Sql.Database" = Sql.Database("<server address>", <database>,
[ CreateNavigationProperties = false,
EnableCrossDatabaseFolding = true ]),
#"Value.NativeQuery" = Value.NativeQuery(#"Sql.Database",
select [$Table].[DateID] as [DateID],
...
...
as [TotalAmount]
from [<database>].[dbo].[Trip] as [$Table])
in
#"Value.NativeQuery"",
"queryPlanStatus": "Succeeded",
"queryScript": "
let
Source = Sql.Databases("<server address>",
[CreateNavigationProperties=false,
EnableCrossDatabaseFolding=true]),
Database = Source{[Name = "<database>"]}[Data],
Table = Database{[Schema = "dbo", Item = "Trip"]}[Data]
In
Table
},
{
"queryName": "Geography",
"queryPlan": {
"nodes":
...
```

你可能会认出此 JSON 对象中使用的语言：Power Query。`let` 和 `in` 运算符就是线索。此示例展示了众所周知的 Power Query 引擎如何作为此视觉查询编辑器的底层查询引擎传递给最终用户。而且，如果你愿意，可以复制 Power Query 语句，粘贴到 Dataflow Gen2 中，并在那里继续你的工作。

# CI/CD

在撰写本书时，视觉查询编辑器的查询不是 Fabric 提供的 CI/CD 流程的一部分。

在使用 Fabric 时需要了解的一件重要事情是哪些项目（湖库、仓库、数据流等）受内置 CI/CD 功能支持。还重要的是深入了解哪些子项目（查询、视觉查询等）受 CI/CD 流程支持。

这将为你作为开发者提供一个非常好的基线，以便从开发治理的角度使用 Fabric（尽管这目前不在 DP-600 考试范围内）。

正如你所见，视觉查询编辑器是一个全面的服务，Power Query 语言的功能和强大性以及将其转换为 T-SQL 并后续工作的能力都有其优势。首先，你不需要深厚的 T-SQL 技能就能开始满足你的数据操作需求，其次，你可以学习新技能，例如同时使用 T-SQL 和 Power Query。

话虽如此，也有一个选项可以直接使用 T-SQL 语言。在下一节中，我们将帮助你了解 Microsoft Fabric 的 T-SQL 格局以及如何使用它来操作数据。

# 使用 SQL 操作数据

在 Microsoft Fabric 中，你可以选择使用 T-SQL 语言来操作和转换数据以满足你的需求。本节提供 SQL 语言的指南，说明在哪里使用它以及其当前限制。

还值得注意的是，笔记本使用的是 *SparkSQL* 版本的 SQL 脚本语言。Spark SQL 是 Apache Spark 的一个模块，它是一个强大的分布式计算引擎，专为大数据处理而设计。它基于美国国家标准学会结构化查询语言（ANSI SQL），并为跨海量分布式数据集的分析工作负载而设计。它利用 Spark 的可扩展能力，因此非常适合大规模 ETL 管道、实时流处理以及高级分析（如通过与 Spark MLlib 集成进行机器学习）。Spark SQL 针对从各种分布式源（如 Parquet、JSON 和 Delta Lake）读取数据并执行复杂的迭代转换进行了优化，其中处理速度和吞吐量优先于严格的事务控制。

# 在哪里找到 SQL 语言

在深入展示如何使用 SQL 语言之前，我们想向你展示在哪里可以找到它。在这里，我们提供了一个列表，说明你会在哪些区域找到使用 SparkSQL 语言的选项：

*在笔记本中*

在 Microsoft Fabric 中的 PySpark 笔记本项目中，你可以通过两种主要选项使用 SQL 语言。你可以使用单元格魔术 %%sql 或将单元格标记为 SQL 单元格。两种选项均显示在图 4-16 中。

在第二个选项中，你使用以下 Spark 函数：

```
spark.sql(<your sql statement>)"
```

在 `<your sql statement>` 占位符的空间中，你可以编写 SQL 代码以将数据读入 DataFrame 或操作现有数据库或表中的数据。

# TIP

笔记本方法是一个数据工程任务，因此其细节超出了本书的范围。你只需要知道你有此选项。

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_210_0.png)

图 4-16. PySpark 笔记本中的 SQL 选项

# 在仓库中

正如你在前几章中所学到的，Microsoft Fabric 中的仓库方法主要基于 SQL 方法进行数据存储和操作。仓库公开了所谓的 T-SQL 表面区域。这种 T-SQL 语言的特殊实现是与仓库交互的默认语言，但与完整的 SQL Server 体验相比，其功能有限。我们将在“ T-SQL 表面区域”部分介绍这些限制。

# 在 SQL 数据库中

Microsoft Fabric 中的原生 SQL 数据库项目将完整的 SQL 语言带给你作为开发者，并且它将拥有你操作数据所需的大部分功能。你可以访问与无服务器 Azure SQL 数据库中可找到的相同 SQL 语句和函数。Fabric 中原生 SQL 数据库与无服务器 Azure SQL 数据库之间 T-SQL 的差异细节超出了本次考试的范围，但你可以在以下 Microsoft Learn 网站找到它们的详细说明：

T-SQL differences between SQL Server and Azure SQL Database

Limitations in SQL database in Microsoft Fabric

# 在湖库中

在使用 Microsoft Fabric 中的湖库项目时，你也可以访问 SQL 分析端点，从中可以使用 T-SQL 语言进行数据操作。与仓库项目一样，存在一些限制，我们将在下一节中描述。

# T-SQL 表面区域

Microsoft Fabric 中 T-SQL 语言能力的特殊实现称为 T-SQL 表面区域，用于 SQL Analytics 端点和仓库项目。

此实现有一些限制，这些限制是 Delta Parquet 存储格式和 Microsoft Fabric 背后底层计算引擎的自然结果。以下是仓库与 SQL Analytics 端点之间 T-SQL 表面区域的这些限制和差异的详细列表：

- 在 Microsoft Fabric 中，仅在仓库中支持创建、更改和删除表，以及插入、更新和删除，而在湖库的 SQL 分析端点中不支持。
- 你可以在湖库的 SQL 分析端点中创建自己的 T-SQL 视图、函数和过程，以引用你的 Delta Lake 数据的表之上。
- 你可以通过两种方式在仓库中创建表：使用 CREATE TABLE 或 CREATE TABLE AS SELECT 语句。
- 你可以以以下显式命令创建临时表（名称前带 # 的表）：
- 全局临时表（名称前带 ## 的表）目前不支持。
- Fabric 仓库和 SQL 分析端点均支持标准、顺序和嵌套公共

```
CREATE TABLE #table_name (
  Col1 data_type1,
  Col2 data_type2
);
```

