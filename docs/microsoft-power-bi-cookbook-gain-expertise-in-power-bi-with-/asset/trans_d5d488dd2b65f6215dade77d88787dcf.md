

## 管理和监控 Power BI

2. 创建一个名为 **PBIAuditLog** 的空白查询，该查询基于参数返回完整的文件路径；禁用加载：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_496_0.png)

图 10.22：查询编辑器中 PBIAuditLog 文件路径查询和其他参数

3. 创建一个名为 **O365PBIAuditLog** 的新查询：

```powerquery
let
    Source = Csv.Document(File.Contents(PBIAuditLog),
        [Delimiter=",", Columns=13, Encoding=65001,
        QuoteStyle=QuoteStyle.Csv]),
    RemoveTopRows = Table.Skip(Source,1),
    PromoteHeaders = Table.PromoteHeaders(RemoveTopRows,
        [PromoteAllScalars=true]),
    ApplyDateType = Table.TransformColumnTypes(PromoteHeaders,
        {{"CreationDate", type datetime}}),
    AddCreationDateColumn = Table.AddColumn(ApplyDateType,
        "CreationDateOnly", each DateTime.Date([CreationDate]), type date),
    AddLocalTime = Table.AddColumn(AddCreationDateColumn, "LocalCreationDate",
        each [CreationDate] + #duration(0,USEasternDSTOffset,0,0)),
    AddLocalCreationDateColumn = Table.AddColumn(AddLocalTime,
        "LocalCreationDateOnly", each DateTime.Date([LocalCreationDate]), type date),
    ParseJSON = Table.TransformColumns(AddLocalCreationDateColumn,
        {{"AuditData", Json.Document}})
in
    ParseJSON
```

4. 添加步骤以展开 **Audit** 列并为展开的列设置适当的数据类型。

5. 创建或重用 **Server** 和 **Database** 参数以创建一个名为 **SQLAgentHistory** 的查询来检索代理数据：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_496_1.png)

图 10.23：在“AdminProd”查询中公开的 SQL Server Agent 历史记录视图

6. 从 **Home** 选项卡的 ribbon 中单击 **Close & Apply**

7. 使用以下 DAX 代码创建 **Dates** 表：

```
Dates =
    VAR __Today = TODAY()
    RETURN
        CALENDAR(DATE(YEAR(__Today)-1,1,1),DATE(YEAR(__Today)+1,12,31))
```

8. 创建 **Time** 表：

```
Time =
    VAR __hrs = SELECTCOLUMNS(GENERATESERIES(1,23,1), "Hour", [Value])
    VAR __mins = SELECTCOLUMNS(GENERATESERIES(0,59,1), "Minutes", [Value])
    VAR __secs = SELECTCOLUMNS(GENERATESERIES(0,59,1), "Seconds", [Value])
    VAR __hoursMinutes = GENERATEALL(__hrs, __mins)
    VAR __hoursMinutesSeconds = GENERATEALL(__hoursMinutes, __secs)
    VAR __final = ADDCOLUMNS(ADDCOLUMNS(__hoursMinutesSeconds,
        "Time", TIMEVALUE(FORMAT([Hour],"00") & ":" &
            FORMAT([Minutes],"00") & ":" & FORMAT([Seconds],"00"))),
        "SecondOfDay", FORMAT([Hour],"00") & FORMAT([Minutes],"00") &
            FORMAT([Seconds],"00"))
    RETURN __final
```

9. 将 **Dates** 表中 **Date** 列的 **Data type** 更改为 **Date**

10. 将 **Time** 表中 **SecondOfDay** 列的 **Data type** 更改为 **Text**

11. 在 **Model** 视图中，在 **Dates** 和 **Time** 表与 **O365PBIAuditLog** 和 **SQLAgentHistory** 表之间创建关系

12. 创建 DAX 度量值和报表可视化以按步骤和随时间推移的持续时间分解代理作业

13. 创建报表页面以可视化数据：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_497_0.png)

图 10.24：SQL Server Agent 历史记录可视化——按运行日期、作业和步骤的平均持续时间

### How it works...

用于 PowerShell 的 Search-UnifiedAuditLog cmdlet 用于从 Office 365 审计日志中访问 Power BI 数据。可以定义完整 CSV 文件路径以及开始和结束日期的变量，评估并将其作为参数传递给 Search-UnifiedAuditLog cmdlet。有关更多信息，请参阅本食谱的“另请参阅”部分。

msdb 数据库的 dbo 架构中提供了 20 多个 SQL Server Agent 系统表。在本食谱“准备就绪”部分创建的视图中，run_date 和 run_time 列以整数形式存储在 SQL Server 中，因此分别转换为日期和时间数据类型。run_duration 列以 HHMMSS 格式以整数存储并转换为分钟。run_status 列被替换为 Execution Status 列以显示用户友好的值，例如 succeeded，并且同样从 enabled 源列创建 Job Status 列以显示 disabled 与 enabled 值。

对于本食谱中的 SQL Agent 报表，使用堆叠条形图显示构成每个作业的各个步骤；悬停在条形上会显示特定于作业步骤的详细信息。条形图上的用户选择会过滤折线图，从而可以轻松访问任何作业步骤的近期性能。在 Power BI 中分析 SQL Agent 作业历史记录比 SQL Server Management Studio 中的 Job Activity Monitor 和 Log File Viewer 接口容易得多且更灵活。

### There's more...

Power BI 服务为仪表板和已发布报表提供免费使用报表。这些使用报表可以轻松扩展以使用以下步骤分析应用程序工作区中包含的所有报表和仪表板活动：

1. 在 Power BI 服务中打开应用程序工作区并选择报表或仪表板的垂直省略号；选择 View usage metrics report（使用指标报表也可以通过打开报表或仪表板后通过工具栏省略号 (...) 并选择 Open usage metrics 来访问）：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_498_0.png)

2. 打开使用指标报表后，单击 File | Save as 以向应用程序工作区添加新报表：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_499_0.png)

图 10.26：保存为以创建工作区的使用指标数据集

3. 打开新报表并简单地删除报表级筛选器，以便包含所有报表和仪表板：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_499_1.png)

图 10.27：使用指标报表

请注意，单个用户包含在数据集和默认报表中，使得识别谁访问或未访问内容变得容易。

### See also

- SQL Server Agent Table documentation: http://bit.ly/2v7kWdc
- Monitor usage metrics in the new workspace experience: https://bit.ly/2Srjy1V
- Track user activities in Power BI: https://bit.ly/3raDXZD

## 利用 Power BI PowerShell 模块

Power BI 管理员可以通过开发利用 Microsoft Power BI Management PowerShell 模块中包含的命令（如 Get-PowerBIWorkspace 和 Add-PowerBIWorkspaceUser）的 PowerShell 脚本来自动化常见任务。此外，可以使用 Power BI REST API 和 .NET 客户端库开发自定义管理解决方案。使用 Power BI REST API，管理员可以自动化工作区和仪表板的创建、发布工作区内容、共享工作区内容、删除对工作区的访问、删除工作区等等。

本食谱演示了如何通过 PowerShell 连接到 Power BI 并执行简单的管理功能。

### Getting ready

为本食谱做准备，请执行以下步骤：

1. 在 Power BI 服务的租户设置中启用 Power BI API：

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_500_0.png)

图 10.28：Power BI 服务中 Power BI API 的租户设置

2. 确保您是 Power BI 管理员或具有 Power BI 权限

3. 以管理员身份运行 PowerShell 会话

4. 设置执行策略：

```
Set-ExecutionPolicy RemoteSigned
```

5. 运行以下命令安装 Power BI Management 模块：

```
Install-Module MicrosoftPowerBIMgmt -Force
```

6. 连接到 Power BI：`Login-PowerBI`

7. 验证访问：`Get-PowerBIWorkspace | Format-Table`

### How to leverage the Power BI PowerShell Module

要实现本食谱，请执行以下步骤：

1. 在 PowerShell 中获取所有新工作区的列表，包括个人工作区，并将其导出为 CSV 文件：

```powershell
Get-PowerBIWorkspace -Scope Organization -All | Export-Csv c:\temp\workspaces.csv
```

2. 获取所有工作区的列表，包括经典工作区，但排除个人工作区，并将其导出到 CSV 文件：

```powershell
Get-PowerBIWorkspace -All | Export-Csv c:\temp\workspaces2.csv
```

3. 获取所有新工作区的所有工作区用户列表：

```powershell
Get-PowerBIWorkspace -Scope Organization -Include All -All |
ForEach-Object {
    $Workspace = $_.name
    $WorkspaceId = $_.Id
    foreach ($User in $_.Users) {
        [PSCustomObject]@{
            Workspace = $Workspace
            Id = $WorkspaceId
            Role = $User.accessright
            User = $user.Identifier
        }
    }
} | Export-Csv "c:\temp\workspaceusers.csv" -NoTypeInformation
```

4. 创建一个名为 Workspaces2 的查询并禁用加载：

```powerquery
let
    Source = Csv.Document(File.Contents("C:\temp\workspaces2.csv"),
        [Delimiter=",", Columns=15, Encoding=1252,
        QuoteStyle=QuoteStyle.None]),
    RemoveRows = Table.Skip(Source,1),
    Headers = Table.PromoteHeaders(RemoveRows, [PromoteAllScalars=true]),
    ChangeTypes = Table.TransformColumnTypes(Headers,{{"Id", type text},
        {"Name", type text}, {"IsReadOnly", type logical},
        {"IsOnDedicatedCapacity", type logical}, {"CapacityId", type text},
        {"Description", type text}, {"Type", type text}, {"State", type text},
```

