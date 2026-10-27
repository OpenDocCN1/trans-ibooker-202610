

## 工作原理...

两个仪表盘视觉对象和KPI视觉对象用于显示相对于预定义阈值或基线具有最高优先级的计数器。例如，磁盘每秒读取时间（Disk Seconds Per Read）以绿色突出显示，因为其值低于目标值 .003；磁盘每秒写入时间（Disk Seconds Per Write）以红色突出显示，因为其值高于目标值 .004。这四个视觉对象都遵循左下角的小时筛选器控件（一个自定义的Chiclet切片器），并且使用Time维度表中的 Minute of Day Time 数据类型列作为KPI视觉对象的趋势轴。

一组“今日最高值和最低值”卡片视觉对象忽略时间筛选器选择（例如切片器中的9:00 P.M.），但应用当前日期筛选器。在此示例中，CPU（处理器百分比）、每秒批处理请求数和可用内存与前一天的值在折线图中进行对比。七天和三十天的平均值度量通常用作性能基线。

## 更多内容...

如果按照之前的配方实现，还可以为等待统计和配置值创建报表视觉对象。要了解其工作原理，请执行以下步骤：

- 1. 创建DAX度量值以识别数据库实例配置更改：

```
Config Value = IF(AND(HASONEVALUE('Configuration Values'[ConfigurationID]), HASONEVALUE('Dates'[Date])), MAX('Configuration Values'[Configuration Value]),BLANK())

Config Value (Today) = CALCULATE([Config Value], FILTER(ALL('Dates'),'Dates'[Date] = [Today]))

Config Value (Yesterday) = CALCULATE([Config Value], FILTER(ALL('Dates'), 'Dates'[Date] = [Yesterday]))

Config Change (Today) = IF([Config Value (Today)] <> [Config Value (Yesterday)], "Config Change", "No Change")

Config Changes = IF([Config Value] = [Prior Day Config],0,1)
```

性能监控报表：实例配置

上次配置捕获日期：7/15/2017

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_465_0.png)

图10.2：实例配置报表页面：当前日期及过去30天的更改历史

- 2. “当前日期配置更改”表视觉对象在 Config Change (Today) 度量值上使用视觉级筛选器，以便仅显示（当天的）已更改配置。“前30天配置更改”表视觉对象使用两个视觉级筛选器。一个筛选器应用于Dates维度表中的 Date 列，并使用相对日期筛选功能来检索过去30天但不包括当天；另一个筛选条件应用于 Config Changes 度量值，筛选值为1。

- 3. 为Wait Statistics表创建类似的DAX度量值，例如当前日期的平均等待秒数。

- 4. 在新页面上，将当前日期的等待统计捕获数据的平均值与先前日期进行比较：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_465_1.png)

图10.3：等待统计报表示例：KPI、瀑布图和散点图视觉对象

> “等待统计信息可能是开始排查SQL Server性能问题的最佳单一途径。SQL Server知道执行为何停滞（换句话说，线程为何必须等待），因此利用这些信息来识别瓶颈和进一步调查的方向，可以显著减少诊断工作负载性能下降原因所需的时间。”
> 
> ——Paul Randal，SQLskills CEO，微软数据平台MVP

在此有限的等待统计报表示例中，页面级别应用了相对日期筛选，仅包含过去30天；因此，两个KPI视觉对象的背景中显示了30天趋势。视觉级别还向瀑布图和散点图应用了相对日期筛选，分别仅包含最近2天和当天。瀑布图的“分解字段”井用于自动识别从昨天到当前日期的等待秒数（等待类型）变化的最大驱动因素。

高级等待统计报表可用作分析的快速起点，以识别系统中的瓶颈。此外，在具备成熟且可预测的基线数据后，该报表可用于排查性能下降问题。例如，PAGEIOLATCH_EX等待的突然飙升可能表明存在缺失索引问题、相关数据库架构或代码更改问题。

## 另请参阅

- SQL Server等待类型库：https://www.sqlskills.com/help/waits/。

## 导入和可视化动态管理视图（DMV）数据

Analysis Services数据模型和Power BI实例包含许多动态管理视图（DMV），这些视图可用于检索与各种数据库对象相关的架构元数据和资源使用情况。导入数据模型的查询性能直接受所消耗内存量的影响。因此，在性能调优工作中，了解内存消耗以及影响内存消耗的因素（如压缩和基数）至关重要。Power BI集成和可视化工具可以增强DMV提供的系统信息的价值，从而为Power BI和SSAS数据集的所有者提供一个直观、可持续的报表层，以支持这些资产。

### 准备工作

要准备此配方，请执行以下步骤：

- 1. 打开包含要分析的数据集的Power BI Desktop文件。在数据检索期间，该文件必须保持打开状态。
- 2. 从“外部工具”中打开DAX Studio。
- 3. 从DAX Studio状态栏的右下角检索与正在运行的Power BI Desktop文件关联的服务器名称和端口；例如 localhost:56514。
- 4. 在DAX Studio中，在左上角的“元数据”下，右键单击数据库ID，然后选择“复制数据库ID”。
- 5. 打开一个新的Power BI Desktop文件，并点击“开始”选项卡功能区中的“转换数据”，以打开Power Query编辑器窗口。
- 6. 创建两个参数 Server 和 Database，并将从DAX Studio检索到的值作为当前值应用：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_467_0.png)

图10.4：具有本地Power BI模型值的查询参数

### 如何导入和可视化DMV数据

要实现此配方，请执行以下步骤：

- 1. 创建一个 Segments 查询：

```
let Source = AnalysisServices.Database
(Server, Database,[Query="Select * From $SYSTEM.DISCOVER_STORAGE_TABLE_COLUMN_SEGMENTS"]),
    Segments = Table.AddColumn(Source, "Structure Type", each
        if Text.Range([TABLE_ID],1,1) <> "$" then "Data"
        else if Text.Start([TABLE_ID],2) = "H$" then "Column Hierarchy"
        else if Text.Start([TABLE_ID],2) = "U$" then "User Hierarchy"
        else if Text.Start([TABLE_ID],2) = "R$" then "Relationship"
        else "unknown", type text),
    RenameTable = Table.RenameColumns(Segments,{{"DIMENSION_NAME", "Table"}}),
    KeyColumn = Table.AddColumn(RenameTable, "ColumnKey", each [Table] & "-" & [COLUMN_ID], type text)
in KeyColumn
```

- 2. 创建一个 **Columns** 查询：

```
let Source = AnalysisServices.Database
    (Server, Database,[Query="Select * From $SYSTEM.DISCOVER_STORAGE_TABLE_COLUMNS"]),
        BasicData = Table.SelectRows(Source, each ([COLUMN_TYPE] = "BASIC_DATA")),
        RenameTable = Table.RenameColumns(BasicData,{{"DIMENSION_NAME", "Table"},{"ATTRIBUTE_NAME", "Column"}}),
        KeyColumn = Table.AddColumn(RenameTable, "ColumnKey", each [Table] & "_" & [COLUMN_ID], type text),
        DateRetrieved = Table.AddColumn(KeyColumn, "Date Retrieved", each DateTime.Date(DateTime.LocalNow()), type date)
    in DateRetrieved
```

- 3. 创建一个名为 **Calculations** 的空查询。

- 4. 点击 **“开始”** 选项卡功能区中的 **“关闭并应用”**。

- 5. 在 **“模型”** 视图中，在 **Segments** 和 **Columns** 表的 **ColumnKey** 列之间创建关系。

- 6. 创建以下DAX度量值：

```
BytesPerMB = 1048576

Segment Size (MB) = DIVIDE(SUM(Segments[USED_SIZE]),[BytesPerMB])

Dictionary Size (MB) = DIVIDE(SUM('Columns'[DICTIONARY_SIZE]),[BytesPerMB])

Data Size (MB) =
    CALCULATE([Segment Size (MB)],Segments[Structure Type] = "Data")

Column Hierarchies Size (MB) =
    CALCULATE([Segment Size (MB)],Segments[Structure Type] = "Column Hierarchy")

User Hierarchy Size (MB) =
    CALCULATE([Segment Size (MB)],Segments[Structure Type] = "User Hierarchy")

Relationship Size (MB) =
    CALCULATE([Segment Size (MB)],Segments[Structure Type] = "Relationship")

Total Column Size (MB) = [Data Size (MB)] + [Dictionary Size (MB)]

Total Size (MB) = [Column Hierarchies Size (MB)] + [Relationship Size (MB)]
```

