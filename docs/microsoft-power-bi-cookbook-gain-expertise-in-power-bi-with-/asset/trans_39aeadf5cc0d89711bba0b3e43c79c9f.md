

foreach ($Report in $_.Reports) {
[PSCustomObject]@{
Workspace = $Workspace
Id = $WorkspaceId
ReportID = $Report.Id
Report = $Report.Name}} | Export-Csv "c:\temp\wsreports.csv" -NoTypeInformation
```

- 8. 从 **Home** 选项卡的 功能区 中单击 **Close & Apply**。
- 9. 在 **Workspaces** 和 **WorkspaceUsers** 表之间建立 **ID** 列的关系。
- 10. 可选地，在 **Workspaces** 表与 Reports、Dashboards、Datasets、Dataflows 和 Workbooks 的其他表之间建立 **ID** 列的额外关系。
- 11. 创建一个用于连接用户的度量值：
  **Users = CONCATENATEX('WorkspaceUsers',[User],", ")**
- 12. 创建一个矩阵可视化，使用 **Workspaces** 表中的 **Name** 列作为 **Rows**，**WorkspaceUsers** 表中的 **Role** 列作为 **Column**，以及 **Users** 度量值作为 **Values**。

## How it works...

经典 Power BI 工作区的功能比新工作区少。对于经典工作区，用户权限列表无法通过 Power BI API 获取，因为访问权限是通过 Office 365 组授予的。此外，PSCustomObject 未包含在支持经典工作区的 cmdlet 调用中。作为 Power BI 管理员，您可以通过访问 Power BI **Admin portal** 并选择 **Workspaces** 来升级工作区。在列出的 **Workspaces** 中，选择一个或多个工作区，然后从工具栏单击 **Upgrade**。

通过本食谱中创建的矩阵可视化，可以轻松找到经典工作区，只需单击 **Rows** 字段中 **Name** 列的下拉菜单并选择 **Show items with no data**。没有列出用户、例如 **Admin**、**Contributor**、**Member** 或 **Viewer** 的非孤立工作区几乎肯定是应升级的经典工作区。

## There's more...

可用于 Power BI 的 PowerShell cmdlet 很多，包括以下内容：

- 1. 获取所有已删除工作区的列表：
  **Get-PowerBIWorkspace -Scope Organization -Deleted -All**

## Administering and Monitoring Power BI

- 2. 恢复工作区：
  
  ```powershell
  Restore-PowerBIWorkspace -Id "<Workspace GUID>" -RestoredName "<Workspace Name>" -AdminEmailAddress "<Admin UPN>"
  ```
  
- 3. 查找孤立工作区：
  
  ```powershell
  Get-PowerBIWorkspace -Scope Organization -Orphaned -All
  ```
  
- 4. 解决孤立工作区问题：
  
  ```powershell
  Add-PowerBIWorkspaceUser -Scope Organization -Id <Workspace ID> -UserPrincipalName <User UPN> -AccessRight Admin
  ```
  
- 5. 获取 Power BI 活动日志事件：
  
  ```powershell
  $events = Get-PowerBIActivityEvent -StartDateTime '2021-07-14T00:00:00' -EndDateTime '2021-07-14T23:59:59' -ActivityType 'ViewDashboard' | ConvertFrom-Json
  $events.Count
  $events[0]
  ```

并非所有 Power BI REST API 调用都有对应的 PowerShell cmdlet。然而，这并不意味着这些 REST API 调用不能仍从 PowerShell 中使用。Power BI 的 PowerShell cmdlet 包括通用的 Invoke-PowerBIRestMethod 方法。此方法允许调用任何 Power BI REST API 调用，例如以下示例，它创建一个包含所有工作区以及关联用户、报表、仪表板、数据集和工作簿的 JSON 文件：

```
```powershell
Invoke-PowerBIRestMethod -Url 'https://api.powerbi.com/v1.0/myorg/admin/Groups?$top=5000&$expand=users,reports,dashboards,datasets,workbooks' -Method Get > c:\temp\workspaces.json
```
```

此方法使用 Login-PowerBI、Login-PowerBIServiceAccount 或 Connect-PowerBIServiceAccount 调用设置的相同凭据。要在 PowerShell 之外使用经过身份验证的会话，请使用以下命令：

```
```powershell
Get-PowerBIAccessToken -AsString
```
```

## See also

- PowerShell cmdlets, REST APIs, and .NET client libraries for administrators: https://bit.ly/3vDJ5qc
- Power BI REST APIs: https://bit.ly/3eR70R6
- Power BI Cmdlets reference: https://bit.ly/3h1ShR2
- Track user activities in Power BI: https://bit.ly/3raDXZD

## Conclusion

本章的食谱重点介绍了最常见且影响最大的管理数据源，包括 Windows Performance Monitor、SQL Server Query Store、本地数据网关、MSDB 系统数据库和 Extended Events。构建在这些数据源之上的 Power BI 解决方案可主动评估使用趋势和资源瓶颈，同时提供识别根本原因所需的详细分析。此外，本章还介绍了通过 **dynamic management views (DMVs)** 暴露的现有 Power BI 和 SSAS 数据模型的元数据，例如度量值和关系定义以及资源消耗。

## 11

## Enhancing and Optimizing Existing Power BI Solutions

Power BI 项目通常从关注特定功能需求开始，例如为某个业务领域和团队提供一组仪表板和报表。在需求相对狭窄且数据集较小的情况下，通常无需对数据检索、模型和报表层进行设计和代码增强即可实现足够的性能和可靠性。此外，Power BI Premium 容量以及在某些情况下的 Analysis Services 资源提供了增强数据集可扩展性的可行选项。

对于更大的 Power BI 项目——特别是当 Power BI Premium 和 Analysis Services 选项不可用时——识别改进报表查询性能的机会以及更有效地使用系统资源来存储和刷新数据集变得很重要。

此外，数据导入过程通常可以变得更具弹性且资源消耗更少。此外，在 Power Query (M) 和 DAX 表达式中应用标准编码语法、变量和注释，进一步提高了 Power BI 数据集的可持续性。

本章的食谱包含增强 Power BI 数据集性能、可扩展性和可靠性的顶级数据建模、DAX 度量值和 M 查询模式。这包括数据模型和度量的性能调优示例、M 查询的错误处理和查询折叠示例，以及关于 DAX 和 M 查询引擎的支持细节。

在本章中，我们将介绍以下食谱：

- Enhancing Data Model Scalability and Usability
- Improving DAX Measure Performance
- Pushing Query Processing Back to Source Systems
- Strengthening Data Import and Integration Processes
- Isolating and Documenting DAX Expressions
- Improving Data Load Speeds with Incremental Refresh

## Technical Requirements

完成本章食谱所需的条件如下：

- Power BI Desktop
- 安装了 AdventureWorksDW2019 数据库的 SQL Server。此数据库及安装说明可在以下位置获取：http://bit.ly/20VQfG7
- 本章的文件可从以下 GitHub 仓库下载：https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition

## Enhancing Data Model Scalability and Usability

所有 Power BI 报表的性能都受数据模型设计的影响。在访问报表时执行的 DAX 查询以及在交互式、自助用户会话中动态更新报表可视化时所依赖的查询，都依赖于模型中定义的关系以及对模型表的优化。对于内存中模型，导入列的基数和这些列的压缩会影响数据集的大小和查询持续时间。对于 DirectQuery 数据模型，源表的引用完整性和关系源的优化在很大程度上决定了查询性能。

本食谱包含三个优化过程，均以包含 1170 万行的 Reseller Sales 事实表为重点。第一个示例利用第 10 章 Administering and Monitoring Power BI 中创建的 DMVs 和 Power BI 内存报告来识别和解决最昂贵的列。第二个示例将维度表拆分为两个较小的表，最后一个示例对导入的事实表应用自定义排序顺序，以优化报表中常用的列的压缩。

