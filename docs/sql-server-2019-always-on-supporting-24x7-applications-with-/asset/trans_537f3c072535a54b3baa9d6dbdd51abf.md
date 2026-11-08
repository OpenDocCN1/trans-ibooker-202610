
## 设置会话事件筛选器
设置会话事件筛选器页面，如图 [9-9](#394392_3_En_9_Chapter.xhtml#Fig9) 所示，允许您在 `Session` 上配置 `Predicates`。我们将配置一个 `Predicate` 来筛选对系统数据库的操作。![Figure 9-9 ¶ 设置会话事件筛选器页面](images/394392_3_En_9_Chapter/394392_3_En_9_Fig9_HTML.jpg)
图 9-9 ¶ 设置会话事件筛选器页面

## 指定会话数据存储
向导中的指定会话数据存储页面是配置 `Target` 的位置。向导提供文件或环形缓冲区目标的选择，以及指定大小和滚动选项的选项。我们将配置文件目标，如图 [9-10](#394392_3_En_9_Chapter.xhtml#Fig10) 所示。![Figure 9-10 ¶ 指定会话数据存储](images/394392_3_En_9_Chapter/394392_3_En_9_Fig10_HTML.jpg)
图 9-10 ¶ 指定会话数据存储

## 摘要页
向导的摘要页将确认向导将执行的操作。`Session` 创建后，完成页面将在退出时提供查看实时数据的选项。要使用 `T-SQL` 创建相同的 `Session`，您可以使用列表 [9-1](#394392_3_En_9_Chapter.xhtml#PC1) 中的脚本。

```sql
CREATE EVENT SESSION AlwaysOnTrace ON SERVER ADD EVENT sqlserver.hadr_db_marked_for_reseed(
    ACTION(sqlserver.nt_username,sqlserver.sql_text)
    WHERE (sqlserver.database_id>(4))), ADD EVENT sqlserver.long_io_detected(
    ACTION(sqlserver.nt_username,sqlserver.sql_text)
    WHERE (sqlserver.database_id>(4))) ADD TARGET package0.event_file(SET filename='C:\\MSSQL\\ASlwaysOnTrace.xel') WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON) ;
```
列表 9-1 
创建事件会话

### CREATE EVENT SESSION 参数
`CREATE EVENT SESSION` DDL 语句接受表 [9-4](#394392_3_En_9_Chapter.xhtml#Tab4) 中详细说明的参数。表 9-4 
CREATE EVENT SESSION 参数

| 参数 || 描述 |
| --- | --- |
| `event_session_name` || 指定您正在创建的事件会话的名称 |
| --- | --- |
| `ADD EVENT` | `SET` || 对添加到会话中的每个事件重复，后跟事件名称，格式为 `package.event`。您可以使用 `SET` 语句设置事件特定的自定义，例如包含非强制性事件字段 |
| `ACTION` || 如果有应为该事件捕获的全局字段，则在每个 `ADD EVENT` 参数之后指定 |
| `WHERE` || 如果事件应与其关联谓词，则在每个 `ADD EVENT` 参数之后指定 |
| `ADD TARGET` | `SET` || 为将添加到会话中的每个目标指定。您可以使用 `SET` 语句填充目标特定参数，例如 `event_file` 目标的 `filename` 参数 |

### CREATE EVENT SESSION WITH 选项
`CREATE EVENT SESSION` 语句还接受多个 `WITH` 选项，详细说明在表 [9-5](#394392_3_En_9_Chapter.xhtml#Tab5) 中。表 9-5 
CREATE EVENT SESSION WITH 选项

| WITH 选项 || 描述 |
| --- | --- |
| `MAX_MEMORY` || 指定事件会话在将事件分派到目标之前用于缓冲事件的最大内存量 |
| --- | --- |
| `EVENT_RETENTION_MODE` || 指定缓冲区满时的行为。可接受的值为 `ALLOW_SINGLE_EVENT_LOSS`，表示如果所有缓冲区都已满，则可以丢弃单个事件；`ALLOW_MULTIPLE_EVENT_LOSS`，表示如果所有缓冲区都已满，则可以丢弃整个缓冲区；以及 `NO_EVENT_LOSS`，表示导致事件触发的任务需等待缓冲区有空间 |
| `MAX_DISPATCH_LATENCY` || 指定事件在刷新到目标之前可在会话缓冲区中驻留的最长时间，以秒为单位 |
| `MAX_EVENT_SIZE` || 指定任何单个事件的事件数据的最大可能大小。可以以千字节或兆字节为单位指定，并且应仅配置为允许大于 `MAX_MEMORY` 设置的事件 |
| `MEMORY_PARTITION_MODE` || 指定事件缓冲区创建的位置。可接受的值为• `NONE` – 表示缓冲区将在实例内创建• `PER_NODE` – 表示缓冲区将为每个 NUMA 节点创建• `PER_CPU` – 表示将为每个 CPU 创建缓冲区 |
| `TRACK_CAUSALITY`  || 指定将额外的 GUID 和序列号与每个事件一起存储，以便关联事件 |
| `STARTUP_STATE` || • 指定会话在实例启动时是否自动启动。`ON` 表示会• `OFF` 表示不会 |

提示
有关 Extended Events 的更深入讨论，我强烈推荐 Apress 出版的书籍 *Pro SQL Server 2019 Administration*，可从 [www.apress.com/gp/book/9781484250884](http://www.apress.com/gp/book/9781484250884) 购买。

## 摘要
SQL Server 提供了丰富的工具来监控 AlwaysOn 可用性组的运行状况。AlwaysOn 仪表板是 SQL Server Management Studio 中的交互式报表，可让您评估可用性组和副本的运行状况。它还提供链接以查看法定配置信息和实时运行状况数据。
实时运行状况数据由扩展事件会话捕获，该会话在实例上创建第一个可用性组时创建，并在后台运行，捕获预配置的事件。可以自定义此跟踪；我建议将其保留为默认配置，并在需要自定义捕获时创建新的事件会话。
创建事件会话允许您根据需要捕获非常细粒度的关注点或仅粗粒度的信息。扩展事件使用 WMI 实现，是一种非常轻量级的框架，这意味着您可以识别问题和趋势，而不会影响实例的性能。
© Peter A. Carter 2020 P. A. CarterSQL Server 2019 AlwaysOn[https://doi.org/10.1007/978-1-4842-6479-9\_10](https://doi.org/10.1007/978-1-4842-6479-9_10) Begin Abstract 
# 10. 故障排除 AlwaysOn
Peter A. Carter^([1](#394392_3_En_10_Chapter.xhtml#Aff2) ) (1) Botley, UK   End Abstract 
SQL Server 公开了大量与高可用性和灾难恢复对象相关的元数据，尤其是在 AlwaysOn 功能集周围。这些元数据可用于快速识别配置、查找问题的根本原因，或为可能发生的事件编写自动响应。以下各节将讨论可用的元数据并提供如何使用它的示例。

## AlwaysOn 故障转移群集实例元数据
从数据库引擎内部，可以查看有关群集实例和承载它的 Windows 群集的大量元数据。这些信息对 DBA 来说可能非常宝贵。以下部分将介绍一些最有用和有趣的元数据对象。

### 发现承载实例的节点
自然地，DBA 需要知道群集中哪个节点正在承载故障转移群集实例，尤其是在尝试诊断连接或性能问题时。然而，如果您的组织有政策不允许 DBA 访问操作系统，则无法使用 Failover Manager。幸运的是，SQL Server 中有一个 `DMV`（动态管理视图）会公开此信息。`sys.dm_os_cluster_nodes` `DMV` 将返回表 [10-1](#394392_3_En_10_Chapter.xhtml#Tab1) 中详细说明的列。表 10-1 
sys.dm_os_cluster_nodes 列

| 列 || 描述 |
| --- | --- |
| `NodeName` || 集群节点名称 |
| --- | --- |
