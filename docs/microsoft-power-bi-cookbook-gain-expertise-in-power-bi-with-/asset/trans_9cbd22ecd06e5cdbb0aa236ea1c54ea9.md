

```
VAR __mins = SELECTCOLUMNS(GENERATESERIES(0,59,1), "Minutes", [Value])
VAR __secs = SELECTCOLUMNS(GENERATESERIES(0,59,1), "Seconds", [Value])
VAR __hoursMinutes = GENERATEALL(__hrs, __mins)
VAR __hoursMinutesSeconds = GENERATEALL(__hoursMinutes,__secs)
VAR __final = ADDCOLUMNS(ADDCOLUMNS(__hoursMinutesSeconds,
    "Time", TIMEVALUE(FORMAT([Hour],"00") & ":" &
        FORMAT([Minutes],"00") & ":" & FORMAT([Seconds],"00")),
    "SecondOfDay", FORMAT([Hour],"00") & FORMAT([Minutes],"00")
        & FORMAT([Seconds],"00")))
RETURN __final
```

- 8. 将 **Dates** 表中的 **Date** 列的**数据类型**更改为 **Date**。
- 9. 将 **Time** 表中的 **SecondOfDay** 列的**数据类型**更改为 **Text**。
- 10. 在**模型**视图中，在 **Dates** 表的 **Date** 列与 **SSAS Memory** 表之间创建关系。
- 11. 在 **Time** 表的 **SecondOfDay** 列与 **SSAS Memory** 表之间创建关系。
- 12. 创建 DAX 度量值以支持报表和分析，如下所示：

```
KB to GB Conversion = 1048576

Avg Memory Limit Hard (GB) =
    DIVIDE(AVERAGE('SSAS Memory'[Memory Limit Hard KB]),[KB to GB Conversion])

Avg Memory Usage GB (Today) =
    CALCULATE([Avg Memory Usage (GB)],
        FILTER(ALL('Date'),'Date'[Date] = [Current Date]))

Max Memory Usage GB (Today) =
    CALCULATE([Max Memory Usage (GB)],
        FILTER(ALL('Date'),'Date'[Date] = [Current Date]))

Max Memory GB (Today, All Time) =
    CALCULATE([Max Memory Usage GB (Today)],ALL('Time'))
```

- 13. 利用合并后的计数器文件、模型关系和度量值创建 SSAS 表格内存报表：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_486_0.png)

图 10.14：SQL Server Analysis Services 服务器属性 – 内存属性

### 工作原理

SSAS 表格模式在处理操作期间，除了用于现有数据的内存外，还需要内存来加载新数据。此外，有时会创建临时内存结构来处理某些查询。这三个组成部分构成了 2.5 倍 RAM 的建议（2 倍用于当前数据和新数据，0.5 倍用于临时结构）。

当 SSAS 实例所需的内存超过特定内存限制或阈值时，根据服务器可用 RAM 量以及分析服务器属性中定义的内存属性，SSAS 会采取各种操作，从清除低优先级内存缓存（LowMemoryLimit）到强制终止用户会话（HardMemoryLimit）。有关 SSAS 内存属性文档的参考，请参见“另请参阅”部分。

DAX 度量值将内存计数器值从 KB 转换为 GB，并便于在不同筛选上下文中比较当天与前一天。例如，Avg Memory Usage GB (Today) 度量值筛选到当前日期，但会遵循 Time 维度表上用户或报表筛选器的选择。而 Max Memory GB (Today, All Time) 度量值将同时忽略 Dates 和 Time 筛选选择，始终显示当前日期的最高内存使用值。

内存使用量的显著峰值可能表明 DAX 度量值不够优化，或报表查询效率低下，需要大型临时内存结构。BI 团队需要确保内存使用量不超过计数器识别的内存限制，以避免性能下降。增加 SSAS 内存限制属性设置，或仅为 SSAS 服务器增加总 RAM，是避免内存不足的两种选择。

### 更多内容...

根据数据集的大小以及其 M 查询是否可以作为 SQL 语句折叠到数据源，计划刷新导入到 Power BI 的数据集可能在刷新时需要大量资源。例如，如果使用在源 Oracle 数据库中没有等效表达式的 M 函数，则网关中的 M 引擎将用于执行筛选、排序和聚合等逻辑。

然而，DirectQuery 和 SSAS 实时连接的资源消耗较低，因为只有查询和查询结果数据通过网关传输。尽管如此，重要的是要考虑以下事实：这些连接根据并发用户数、用户对已发布报表的使用或交互、可视化的类型和数量，以及是否配置了**行级安全性（RLS）**角色，会产生高频率的查询。

还可以为 Power BI 网关创建性能报表。要了解具体做法，请执行以下步骤：

- 1. 创建并计划一个新的性能监视器数据收集器集，其中包含本地数据网关计数器：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_487_0.png)

图 10.15：本地数据网关性能计数器

- 2. 在包含 SSAS 计数器的同一个 Power BI Desktop 文件中，对网关计数器文件的父文件夹创建额外的查询，并应用相同的 M 查询转换来筛选导入的文件、调整数据类型、重命名列，以及添加 **Date** 和 **Time** 列，以支持与 **Dates** 和 **Time** 维度表的关系。
- 3. 针对不同的网关计数器列构建基本聚合度量值。
- 4. 按照与 **SSAS Memory** DAX 度量值相同的表达式模式，构建额外的 DAX 度量值，以应用或移除来自 **Dates** 和 **Time** 表的筛选上下文。
- 5. 为网关性能计数器设计一个专用的网关报表页面：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_488_0.png)

图 10.16：网关性能报表

在此示例中，该组织使用 SSAS 2014 表格服务器作为 Power BI 报表和仪表板内容的主要数据源。因此，使用基于 ADOMD 网关计数器的度量值来展示此工作负载的量（底部图表）。顶部图表使用了 **每秒执行的查询总数** 性能计数器，以及折线图上方的平均和最大卡片视觉对象。虽然不太常见，该组织还使用此网关来支持某些 Power BI 数据集的导入刷新（Mashup 计数器）和 DirectQuery 数据集的刷新（ADO.NET 计数器）。

将卡片、仪表和标准 KPI 视觉对象固定为仪表板磁贴后，可以触发数据警报和电子邮件通知。在本方案的上下文中，超过 VertiPaq 和其他内存限制的内存使用量可能触发数据警报。同样，大量查询失败或网关计数器报告的意外查询类型活动也可能触发数据警报。例如，如果某个网关专门用于导入（Mashup）工作负载，则计数器不应报告 ADO.NET（DirectQuery）或 OLEDB 连接的查询活动。

应注意，本地数据网关支持通过管理应用程序的**诊断**选项卡进行日志记录。可以通过打开**附加日志记录**选项来启用详细日志记录，但此选项仅应在故障排除时使用，不应永久启用，因为这会显著增加日志文件的数量和大小。网关日志可以在同一个**诊断**选项卡上导出。

### 另请参阅

- Power BI 数据网关部署指南：http://bit.ly/2t8hk9i
- SQL Server Analysis Services 内存属性：http://bit.ly/2vuY1I2
- 监视和优化本地数据网关性能：https://bit.ly/3nffIJQ

## 分析扩展事件

扩展事件是一个高度可配置且轻量级的性能监视系统，可用于 SQL Server 关系数据库引擎和 Analysis Services。特定会话可以使用大量事件库，这些事件可以被保存、计划，然后进行分析，以支持性能调优、故障排除和常规监视。然而，与其他监视工具（如 Windows 性能监视器和 SQL Server 查询存储）类似，扩展事件的图形界面缺乏 Power BI 等报表工具所具备的丰富分析能力和灵活性。

在本方案中，将包含查询执行统计信息的扩展事件会话输出提取到专用的 Power BI 事件分析报表文件中。来自该文件的 140 万行事件数据在导入过程中得到增强，并开发了报表可视化，以突出最有意义的趋势和度量值，并支持进一步的自助分析。

### 准备就绪

要准备此方案，请执行以下步骤：

- 1. 确定与主要监视和故障排除用例关联的事件。

