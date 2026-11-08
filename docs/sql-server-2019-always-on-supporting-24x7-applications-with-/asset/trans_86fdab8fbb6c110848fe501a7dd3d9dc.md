
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
