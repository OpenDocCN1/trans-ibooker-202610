# 10. 故障排除 AlwaysOn
Peter A. Carter^(1 ) (1) Botley, UK   End Abstract 
SQL Server 公开了大量与高可用性和灾难恢复对象相关的元数据，尤其是在 AlwaysOn 功能集周围。这些元数据可用于快速识别配置、查找问题的根本原因，或为可能发生的事件编写自动响应。以下各节将讨论可用的元数据并提供如何使用它的示例。

## AlwaysOn 故障转移群集实例元数据
从数据库引擎内部，可以查看有关群集实例和承载它的 Windows 群集的大量元数据。这些信息对 DBA 来说可能非常宝贵。以下部分将介绍一些最有用和有趣的元数据对象。

### 发现承载实例的节点
自然地，DBA 需要知道群集中哪个节点正在承载故障转移群集实例，尤其是在尝试诊断连接或性能问题时。然而，如果您的组织有政策不允许 DBA 访问操作系统，则无法使用 Failover Manager。幸运的是，SQL Server 中有一个 `DMV`（动态管理视图）会公开此信息。`sys.dm_os_cluster_nodes` `DMV` 将返回表 10-1 中详细说明的列。表 10-1 
sys.dm_os_cluster_nodes 列

| 列 || 描述 |
| --- | --- |
| `NodeName` || 集群节点名称 |
| --- | --- |



| `status` || 当前节点的状态。可能的值为• 0 – 表示节点已启动• 1 – 表示节点已关闭• 2 – 表示节点已暂停• 3 – 表示节点当前正在加入集群• 4 – 表示状态未知 |

| `status_description` || 状态的文本描述。可能的值为• 启动• 关闭• 已暂停• 加入中• 未知 |

| `is_current_owner` || 表示实例是否当前由节点托管。可能的值为• 0 – 表示节点不拥有实例• 1 – 表示节点拥有实例 |

列出 10-1 中的查询将返回当前托管实例的集群节点的名称。

```
SELECT NodeName FROM sys.dm_os_cluster_nodes WHERE is_current_owner = 1;
```
列表 10-1

发现托管实例的节点

## 查看运行状况检查配置

如果在协助 Windows 管理团队处理集群实例的重复故障转移时，DBA 可能希望公开可能导致故障转移的条件详情，以确保配置了适当的级别。这可以通过使用 `sys.dm_os_cluster_properties` DMV 实现，该视图返回表 10-2 中详细列出的列。表 10-2

`sys.dm_os_cluster_properties` 列

| Column || Description |

| --- | --- |

| `VerboseLogging` || 表示集群使用的日志级别。可能的值为• 0 – 表示日志已关闭• 1 – 表示仅记录错误• 2 – 表示记录错误和警告 |

| --- | --- |

| `SQLDumperDumpFlags` || 指定 SQLDumper 将生成的转储文件类型。可能的值为• 0x0120 – 表示迷你转储• 0x0110 – 表示完整转储• 0x8100 – 表示筛选转储 |

| `SQLDumperDumpPath` || 指定 SQLDumper 将输出转储文件的文件路径 |

| `SQLDumperDumpTimeOut` || SQLDumper 创建转储文件时的超时值。以毫秒为单位指定 |

| `FailureConditionLevel` || 导致故障转移发生的故障级别。有关故障条件级别的完整描述，请参阅表 10-3 |

| `HealthCheckTimeout` || 数据库引擎在判定实例无响应之前等待返回健康信息的时间长度 |

由 `FailureConditionLevel` 列返回的可能故障条件级别在表 10-3 中详细说明。表 10-3

故障条件级别

| Condition Level || Description |

| --- | --- |

| 0 || 自动故障转移不会发生 |

| --- | --- |

| 1 || 当 SQL Server 服务关闭时自动故障转移会发生 |

| 2 || 自动故障转移将在以下情况发生时发生• 满足级别 1 条件• 超过 `HealthCheckTimeout` 值 |

| 3 || 自动故障转移将在以下情况发生时发生• 满足级别 2 条件• 运行状况检查返回系统错误 |

| 4 || 自动故障转移将在以下情况发生时发生• 满足级别 3 条件• 运行状况检查返回资源错误 |

| 5 || 自动故障转移将在以下情况发生时发生• 满足级别 4 条件• 运行状况检查返回查询处理错误 |

列出 10-2 中的查询将返回当前的故障转移条件级别和当前的运行状况检查超时值。

```
SELECT
    FailureConditionLevel
    , HealthCheckTimeout
FROM sys.dm_os_cluster_properties;
```
列表 10-2

返回运行状况检查配置

实例当前的运行状况可以通过使用 `sp_server_diagnostics` 系统存储过程手动确定。该过程接受一个参数：`@repeat_interval`，指定过程应多久返回一次结果，以秒为单位。如果省略参数，则结果仅返回一次。如果为参数传递值，则必须大于 5。该过程返回表 10-4 中详细说明的结果集。表 10-4

`sp_server_diagnostics` 返回的列

| Column || Description |

| --- | --- |

| `creation_time` || 表示行创建时间 |

| --- | --- |

| `component_type` || 表示组件的类型。可能的值为• Instance• AlwaysOn: 可用性组 |

| `component_name` || 表示组件的名称。可能的值为• system• resource• query_processing• io_subsystem• events• [可用性组名称] |

| `state` || 组件的运行状况。可能的值为• 0 – 表示状态未知• 1 – 表示状态正常（意味着健康）• 2 – 表示存在警告• 3 – 表示存在错误 |

| `state_desc` || 组件状态的文本描述。可能的值为• 未知• 正常• 警告• 错误 |

| `data` || 组件特定数据的 XML 表示。例如，资源组件包括指定可用物理内存和可用虚拟内存的元素。它还包括属性，包括超出内存异常的计数 |

列出 10-3 中的脚本将返回 `sp_server_diagnostics` 系统存储过程的完整结果集，以及从 XML 列中提取的值，以便快速查看服务器整体 CPU 利用率、实例的 CPU 利用率以及可能发生的超出内存异常计数。

提示

关于 XML 分片的讨论超出了本书的范围。然而，我推荐 Apress 出版的 *SQL Server DBA 专家脚本和自动化*，其中可以找到关于为管理目的处理 XML 的讨论。该书可在 [www.apress.com/9781484219423](http://www.apress.com/9781484219423) 购买。

```
CREATE TABLE #Server_Diagnostics (
    creation_time      DATETIME,
    component_type     NVARCHAR(8),
    component_name     NVARCHAR(128),
    [state]            TINYINT,
    state_desc         NVARCHAR(8),
    [data]             XML
);
INSERT INTO #Server_Diagnostics EXEC sp_server_diagnostics;
SELECT *,
    data.value('(/system/@systemCpuUtilization)[1]','int') AS SystemCPUUtilization,
    data.value('(/system/@sqlCpuUtilization)[1]','int') AS SQLServerCPU,
    data.value('(/resource/@outOfMemoryExceptions)[1]','int') AS OutOfMemoryExceptions
FROM ##Server_Diagnostics;
DROP TABLE #Server_Diagnostics;
```
列表 10-3

检索诊断信息

## AlwaysOn 可用性组元数据

元数据也可以用于排查可用性组的问题。以下各节将讨论一些最有用和有趣的与可用性组相关的元数据对象。

### 确定上次故障转移原因

如果可用性组已发生故障转移，您可能最想回答的第一个问题是“何时以及为什么？”。这个问题可以使用 `sys.dm_hadr_availability_replica_states` DMV 回答。该对象返回表 10-5 中详细说明的列。表 10-5

`sys.dm_hadr_availability_replica_states`

| Column || Description |

| --- | --- |

| `replica_id` || 副本的 GUID |

| --- | --- |

| `group_id` || 可用性组的 GUID |

| `is_local` || 表示副本是本地还是远程。可能的值为• 0 – 表示远程辅助副本• 1 – 表示本地副本 |

| `role` || 表示当前分配给副本的角色。可能的值为• 0 – 表示角色当前正在解析• 1 – 表示副本具有主要角色• 2 – 表示副本当前具有辅助角色 |

| `role_desc` || 副本当前角色的文本描述。可能的值为• 解析中• 主要• 辅助 |



## 可用性副本状态列

| 列 | 描述 |
|---|---|
| `operational_state` | 表示副本的当前运行状态。可能的值为 • 0 – 表示故障转移待处理 • 1 – 表示状态待处理 • 2 – 表示在线 • 3 – 表示离线 • 4 – 表示失败 • 5 – 表示失败，无仲裁 • NULL – 表示副本不是本地的 |
| `operational_state_desc` | 运行状态的文本描述。可能的值为 • PENDING_FAILOVER • PENDING • ONLINE • OFFLINE • FAILED • FAILED_NO_QUORUM • NULL |
| `connected_state` | 表示辅助副本当前是否连接到主副本。可能的值为 • 0 – 表示副本与主副本断开连接 • 1 – 表示副本已连接到主副本 |
| `connected_state_desc` | 连接状态的文本描述。可能的值为 • DISCONNECTED • CONNECTED |
| `recovery_health` | 表示可用性组中数据库是否在线。可能的值为 • 0 – 表示至少有一个数据库未在线 • 1 – 表示所有数据库均在线 • NULL – 表示可用性组不是本地的 |
| `recovery_health_desc` | `recovery_health` 的文本描述。可能的值为 • ONLINE_IN_PROGRESS • ONLINE • NULL |
| `synchronization_health` | 表示可用性组数据库的同步状态。可能的值为 • 0 – 表示至少有一个数据库处于 NOT SYNCHRONIZING 状态。这称为不健康 • 1 – 表示至少有一个数据库未处于理想同步状态。这称为部分健康 理想状态为 • SYNCHRONIZED – 用于同步提交副本 • SYNCHRONIZING – 用于异步提交副本 • 2 – 表示所有数据库均处于理想状态。这称为健康 |
| `synchronization_health_desc` | 同步健康状态的文本描述。可能的值为 • NOT_HEALTHY • PARTIALLY_HEALTHY • HEALTHY |
| `last_connect_error_number` | 最后一次连接错误的错误号 |
| `last_connect_error_description` | 最后一次连接错误的描述 |
| `last_connect_error_timestamp` | 最后一次连接错误的日期和时间 |

## 确定最后一次故障转移的时间和原因

Listing `10-4` 中的查询演示了如何返回最后一次连接错误的时间和原因。这将指示故障转移发生的时间和原因。每个副本和可用性组的组合将返回一行。你会注意到我们将 `sys.dm_hadr_availability_replica_states` DMV 连接到 `sys.availability_replicas` 和 `sys.availability_groups` DMV，以检索托管副本的节点名称和可用性组名称。

```sql
SELECT
    ar.replica_server_name
    ,ag.name
    ,ars.last_connect_error_description
    ,ars.last_connect_error_timestamp
FROM sys.dm_hadr_availability_replica_states ars
INNER JOIN sys.availability_replicas ar
    ON ar.group_id = ars.group_id
    AND ars.replica_id = ar.replica_id
INNER JOIN sys.availability_groups ag
    ON ag.group_id = ar.group_id;
```
清单 10-4

### 评估可用性数据库的状态

你可能已经注意到，`sys.dm_hard_availability_replica_states` DMV 会提供包含不健康状态数据库的可用性组的详细信息。然而，结果不够细化，无法让你发现哪些数据库不健康。此信息可以从 `sys.dm_hadr_database_replica_states` DMV 中检索，该视图返回表 `10-6` 中详细列出的列。

表 10-6
`sys.dm_hadr_database_replica_states` 列

| 列 | 描述 |
|---|---|
| `database_id` | 数据库的 ID |
| `group_id` | 可用性组 GUID |
| `replica_id` | 可用性副本 GUID |
| `group_database_id` | 可用性组内的数据库 ID |
| `is_local` | 表示数据库是本地还是远程。可能的值为 • 0 – 表示数据库不是实例本地的 • 1 – 表示数据库是实例本地的 |
| `is_primary_replica` | 表示数据库副本当前是否具有主或辅助角色。可能的值为 • 0 – 表示辅助数据库副本 • 1 – 表示主数据库副本 |
| `synchronization_state` | 表示数据库同步的状态。可能的值为 • 0 – 表示未同步 • 1 – 表示正在同步 • 2 – 表示已同步 • 3 – 表示状态为回滚。这意味着辅助副本处于撤销阶段的部分，正在从主副本检索页面 • 4 – 表示状态为初始化。这意味着辅助副本处于撤销阶段的部分，所需的日志记录当前正在传输并硬化 |
| `synchronization_state_desc` | 同步状态的文本描述。可能的值为 • NOT SYNCHRONIZING • SYNCHRONIZING • SYNCHRONIZED • REVERTING • INITIALIZING |
| `is_commit_participant` | 表示事务提交是否已同步。异步副本上的数据库始终报告 0，且该值仅对同步副本上的主数据库准确。可能的值为 • 0 – 表示事务提交未同步 • 1 – 表示事务提交已同步 |
| `synchronization_health` | 表示数据库的同步状态。可能的值为 • 0 – 表示不健康。这意味着数据库未同步 • 1 – 表示部分健康。这意味着数据库正在同步 • 2 – 表示健康。这意味着数据库已同步 |
| `synchronization_health_desc` | 同步健康的文本描述。可能的值为 • NOT_HEALTHY • PARTIALLY_HEALTHY • HEALTHY |
| `database_state` | 表示数据库的当前状态。该值反映 `sys.databases` 目录视图中的值。可能的值为 • 0 – 表示数据库为联机 • 1 – 表示数据库正在还原 • 2 – 表示数据库正在恢复 • 3 – 表示数据库处于恢复挂起状态 • 4 – 表示数据库为可疑 • 5 – 表示数据库处于紧急模式 • 6 – 表示数据库为脱机 |
| `database_state_desc` | 数据库状态的文本描述。可能的值为 • ONLINE • RESTORING • RECOVERING • RECOVERY_PENDING • SUSPECT • EMERGENCY • OFFLINE |
| `is_suspended` | 表示数据库是否已挂起。可能的值为 • 0 – 表示已恢复 • 1 – 表示已挂起 |
| `suspend_reason` | 如果数据库已挂起，`suspend_reason` 列指示原因。可能的值为 • 0 – 表示用户手动挂起数据移动 • 1 – 表示强制故障转移后的挂起 • 2 – 表示在重做阶段发生错误 • 3 – 表示在日志捕获期间发生错误 • 4 – 表示写入日志时发生错误 • 5 – 表示数据库在重新启动前已挂起 • 6 – 表示在撤销阶段发生错误 • 7 – 表示日志链不匹配错误 • 8 – 表示在计算辅助副本同步点时发生错误 |
| `suspend_reason_desc` | `suspend_reason` 列的文本描述。可能的值为 • SUSPEND_FROM_USER • SUSPEND_FROM_PARTNER • SUSPEND_FROM_REDO • SUSPEND_FROM_CAPTURE • SUSPEND_FROM_APPLY • SUSPEND_FROM_RESTART • SUSPEND_FROM_UNDO • SUSPEND_FROM_REVALIDATION • SUSPEND_FROM_XRF_UPDATE |



### 副本状态列

| 列 | 描述 |
|---|---|
| `recovery_lsn` | 在主副本上，`recovery_lsn` 表示事务日志的末尾（用于时间点恢复的事务日志中的最终点）。在辅助副本上，该列表示需要重新同步的点。然而，如果该值等于或大于 `last_hardened_lsn`，则表示不需要重新同步 |
| `truncation_lsn` | 对于主副本，该列表示所有辅助副本中最小的日志截断 LSN。对于辅助副本，该列表示该特定数据库副本的日志截断点 |
| `last_sent_lsn` | 表示已发送的最后一个日志块的末尾 |
| `last_sent_time` | 最后日志块发送的日期和时间 |
| `last_recieved_lsn` | 表示要接收的最后一个日志块的末尾 |
| `last_hardened_lsn` | 表示要硬化的最后一个日志块的开始。对于异步提交副本，该值将为 NULL |
| `last_hardened_time` | 已硬化 LSN 的日期和时间 |
| `last_redone_lsn` | 辅助副本上要重做的最后一个日志记录的 LSN |
| `last_redone_time` | 辅助副本上要重做的最后一个 LSN 的时间戳 |
| `log_send_queue_size` | 尚未发送到辅助副本的日志记录的大小，以千字节为单位指定 |
| `log_send_rate` | 向辅助副本发送日志记录的速度，以千字节/秒为单位指定 |
| `filestream_send_rate` | 向辅助副本发送 FILESTREAM 文件的速度，以千字节/秒为单位指定 |
| `end_of_log_lsn` | 日志缓存中最终日志记录的 LSN |
| `last_commit_lsn` | 事务日志中最后一个已提交事务的 LSN |
| `last_commit_time` | 事务日志中最后一个已提交 LSN 的时间戳 |
| `low_water_mark_for_ghosts` | 幽灵清理任务（物理删除已逻辑删除的行）使用数据库所有副本中该列的最小值来确定开始清理记录的位置 |
| `secondary_log_seconds` | 辅助副本落后于主副本的秒数 |

### 评估可用性数据库的状态

Listing 10-5 中的脚本演示了如何在 HR 可用性组中评估可用性数据库的健康状况。你会注意到我们使用 `DB_NAME()` 函数返回数据库的名称，并将 `sys.dm_hadr_database_replica_states` DMV 连接到 `sys.availability_groups` 和 `sys.availability_replicas` 目录视图，以返回可用性组和副本的名称。

```sql
SELECT
    DB_NAME(database_id),
    ag.name,
    ar.replica_server_name,
    is_primary_replica,
    synchronization_state_desc,
    synchronization_health_desc,
    database_state_desc
FROM sys.dm_hadr_database_replica_states drs
INNER JOIN sys.availability_groups ag
    ON drs.group_id = ag.group_id
INNER JOIN sys.availability_replicas ar
    ON drs.replica_id = ar.replica_id
WHERE ag.name = 'HR';
```
Listing 10-5 评估可用性数据库的状态

## 摘要

SQL Server 公开了大量元数据，可帮助您排查问题并审核配置。虽然本章讨论了一些最有用的元数据，但我强烈建议您进一步探索 hadr DMVs。

对于故障转移群集实例，`sys.dm_os_cluster_nodes` DMV 公开了群集中节点的健康状态。进一步的故障排除详细信息可以通过调用 `sp_server_diagnostics` 系统存储过程找到。

提示

`sp_server_diagnostics` 系统存储过程也可用于排查 AlwaysOn 可用性组的问题，并为实例上托管的每个可用性组返回一行。

`sys.dm_hadr_availability_replica_states` DMV 公开了托管 AlwaysOn 可用性组的副本的健康状态详细信息。要深入查看参与可用性组的数据库的健康状态，可以查询 `sys.dm_hadr_database_replica_states` DMV。

前面提到的两个 DMV 都可以连接到 `sys.availability_groups` 和 `sys.availability_replicas` DMV，以获取有关配置的文本信息，而不是 GUID。`sys.dm_hadr_database_replica_states` DMV 还可以连接到 `sys.databases` 以获取有关数据库配置的进一步信息。



