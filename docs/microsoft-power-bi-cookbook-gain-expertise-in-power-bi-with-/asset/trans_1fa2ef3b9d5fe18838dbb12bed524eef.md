
(MB)] +
[用户层次结构大小 (MB)] + [字典大小 (MB)] + [数据大小 (MB)]

```
Last Refresh Message = VAR RefreshDate = MAX('Columns'[Date Retrieved])
RETURN "Last Refreshed: " & RefreshDate
```

+   7. 基于检索到的并已建模的 DMV 数据创建 Power BI 报表页，如图 10.5 所示：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_469_0.png)

图 10.5：Power BI 数据集的摘要内存报告

## 工作原理...

对于 Segments 查询，使用 Server 和 Database 参数，以及针对 DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS DMV 的 SQL 语句，从正在运行的 Power BI Desktop 文件（其中包含 Analysis Services 实例）中提取并访问内存使用数据。添加条件列 Structure Type 以标识每一行所表示的内存结构，并创建串联列 ColumnKey 以支持与 Columns 表之间的关系。

对于 Columns 查询，会根据 Column_Type 列对 DISCOVER_STORAGE_TABLE_COLUMNS DMV 进行筛选，并创建串联列 ColumnKey 用于与 Segments 表建立关系。此外，还添加了一个动态的 Date Retrieved 列，以支持报表层。

两个源 DMV 的内存列均以字节为单位。由于 MB 的位数更少，因此更直观、更易于展示；因此，使用 BytesPerMB 度量值（硬编码为 1,048,576）对 USER_SIZE 和 DICTIONARY_SIZE 列的总和进行 DIVIDE 运算。在 Segments 查询中创建的 Structure Type 列用于支持总体内存使用的不同组成部分，并为汇总级报表创建了两个总计度量值。

使用卡片视觉对象显示总体内存使用情况；两个带有“前 N 项”视觉级筛选器的表视觉对象提供更多详细信息。在此示例中，**Reseller Sales XL** 表是模型中目前最大的表，具体而言，该表的 **CarrierTrackingNumber**、**SalesOrderNumber** 和 **CustomerPONumber** 列消耗的内存最多。单击 **Top 5 Tables** 视觉对象中的表名会交叉筛选 **Top 5 Columns** 视觉对象。

可以添加更多 DMV 和逻辑，以构建一个更强大的模板报表，供各 Power BI 项目使用。请注意，当所分析的 PBIX 文件被关闭并重新打开时，需要更新 **Server** 和 **Database** 参数。

## 更多内容...

除了每列的压缩数据内存结构外，导入的数据模型还会在内部创建字典和列层次结构（**H$**）以支持查询。通过 columns DMV 检索到的字典结构存储列的唯一值，因此对于高基数列，其占用空间更大。另外两种内存结构包括用户定义的层次结构（**U$**）和表关系（**R$**）。

在本技巧中，分配给列层次结构（**H$**）的内存在 **Total Column Size** 度量值中被排除，但被包含在 **Total Size** 度量值中。这样做的目的是将分析重点放在内存的较大组成部分上，而这些部分可能会受到数据模型修订的直接影响。同样，用于关系和用户定义层次结构的内存相对于模型列的数据和字典大小而言很小。

导入数据模型的基本性能调优主要侧重于尽量减少高基数列和关系，以减少扫描的内存。更高级的调优（通常用于 SSAS 模型）涉及表分区、段大小以及优化引擎在压缩期间使用的排序顺序。对于 Power BI 数据集，只有排序顺序可用。

Vertipaq Analyzer 利用 DMV 收集数据集的信息，现已集成到 DAX Studio 的 **Advanced** 菜单以及 Tabular Editor 3 中。与自行编写针对 DMV 的查询相比，使用 Vertipaq Analyzer 是一种更快速、更轻松的分析数据模型内存大小、基数和其他方面的方法。

## 另请参阅

- Kasper de Jonge 发表了一篇关于使用 Power BI 构建 SSAS 内存报表的博客：http://bit.ly/2tDumgk.
- SQL BI 的 Vertipaq Analyzer Excel 文件：http://bit.ly/2sTTuS0.
- Analysis Services 中的**动态管理视图 (DMVs)**：https://bit.ly/33etDoc.

Vertipaq Analyzer 用于 Tabular Editor 2 的脚本：https://bit.ly/2YA97zv

## 提高 DBA 生产力

SQL Server 查询存储（Query Store）是一项监控功能，适用于 SQL Server 2016 及更高版本的所有版本——它极大地简化和加快了查询调优和故障排除。查询存储数据库提供关于查询执行、查询计划和等待统计的聚合指标，使性能趋势和使用模式可见。

> “查询存储是执行计划的绝佳飞行数据记录器。它将帮助你排查参数嗅探问题、连接设置问题、计划回归、糟糕的统计信息以及更多问题。”

— Brent Ozar，作者、Microsoft 认证大师（SQL Server）

查询存储包含图表和用户控件的图形界面，其架构非常适合自定义 T-SQL 查询，例如过去一小时内运行时间最长的 10 个查询。虽然这些功能很棒并且足以应对某些场景，但管理员通常不得不在 T-SQL 的灵活性与查询存储提供的图形控件之间进行权衡。在本技巧中，将简单的 T-SQL 语句从 Power BI 传递给 SQL Server 查询存储，以识别和分析最近的性能问题以及特定存储过程的性能。

## 准备工作

若要准备本技巧，请执行以下步骤：

- 1. 在最新版本的 SQL Server Management Studio (SSMS) 中启用查询存储，可以通过对象资源管理器界面中的“数据库属性”对话框（右键单击数据库，选择“属性”），也可以通过以下 T-SQL 语句：

```
ALTER DATABASE AdventureWorksDW2019 SET QUERY_STORE = ON;
```

- 2. 根据你的要求配置查询存储设置，例如统计信息收集间隔、保留期和最大大小 (MB)。

在此示例中，单个查询的性能将被聚合或汇总到 5 分钟的时间范围内。较小的收集时间间隔可提供更多详细信息，但也会消耗更多的存储和收集资源。若要获得不对查询进行分组时的最大详细信息，可以计划一个扩展事件 (Extended Events) 会话。有关更多详细信息，请参阅本章后面的“分析扩展事件”技巧。

## 如何提高 DBA 生产力

若要实施本技巧，请执行以下步骤：

- 1. 生成并测试用于检索所需查询存储统计信息的 T-SQL 语句：

```
SELECT
    [rs].[avg_duration], [rs].[avg_logical_io_reads], [qst].[query_text_id],
    [qsq].[query_id],
    [qst].[query_sql_text], CASE WHEN [qsq].[object_id] = 0
        THEN N'Ad-hoc' ELSE OBJECT_NAME([qsq].[object_id])
        END AS [ObjectName],
    [qsp].[plan_id], GETUTCDATE() AS CurrentUTCtime,
    [rs].[last_execution_time],
        CAST((DATEADD(MINUTE,
            -(DATEDIFF(MINUTE, GETDATE(), GETUTCDATE())),
            [rs].[last_execution_time])) AS datetime2(0))
        AS LocalLastExecutionTime
FROM
    [sys].[query_store_query] [qsq]
    JOIN [sys].[query_store_query_text] [qst] ON
        [qsq].[query_text_id] = [qst].[query_text_id]
    JOIN [sys].[query_store_plan] [qsp] ON
        [qsq].[query_id] = [qsp].[query_id]
    JOIN [sys].[query_store_runtime_stats] [rs] ON
        [qsp].[plan_id] = [rs].[plan_id]
WHERE [rs].[last_execution_time] > DATEADD(hour, -8, GETUTCDATE())
```

- 2. 创建另一个 T-SQL 语句，其中包含类似的与性能相关的列，并筛选到特定的存储过程，该存储过程同时检索收集间隔时间：

```
SELECT * FROM
[sys].[query_store_query] [qsq]
JOIN [sys].[query_store_query_text] [qst] ON
    [qsq].[query_text_id] = [qst].[query_text_id]
JOIN [sys].[query_store_plan] [qsp] ON
    [qsq].[query_id] = [qsp].[query_id]
JOIN [sys].[query_store_runtime_stats] [rs] ON
    [qsp].[plan_id] = [rs].[plan_id]
JOIN [sys].[query_store_runtime_stats_interval] [rsi] ON
    [rs].[runtime_stats_interval_id] = [rsi].[runtime_stats_interval_id]
WHERE [qsq].[object_id] = OBJECT_ID(N'Sales.usp_GetFullProductInfo')
```

