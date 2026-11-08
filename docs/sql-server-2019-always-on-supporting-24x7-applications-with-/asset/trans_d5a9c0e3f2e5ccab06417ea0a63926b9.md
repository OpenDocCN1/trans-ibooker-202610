
# 事件分类

事件是你可以追踪的感兴趣的发生。它可能是 SQL 批处理完成、缓存未命中或页面拆分，或数据库引擎中几乎任何其他可能发生的事情，具体取决于你配置的跟踪的性质。每个事件都按通道和关键字（也称为类别）进行分类。通道是高层次的分类，所有 SQL Server 2016 中的事件都属于表 [9-1](#394392_3_En_9_Chapter.xhtml#Tab1) 中描述的通道之一。

## 表 9-1 通道

| 通道 | 描述 |
|---|---|
| Admin | 众所周知的事件，具有众所周知的解决方案。例如，死锁、服务器启动、CPU 阈值被超过，以及使用已弃用的功能。 |
| Operational | 用于故障排除问题。例如，检测到损坏的内存、AlwaysOn 可用性组副本更改其状态，以及检测到长时间 IO，都是属于 Operational 通道的事件。 |
| Analytic | 用于故障排除性能等问题的海量事件。例如，事务开始、获取锁以及文件读取完成，都是属于 Analytic 通道的事件。 |
| Debug | 由开发人员使用，通过返回内部数据来诊断问题。Debug 通道中的事件在 SQL Server 的未来版本中可能会发生变化，因此在可能的情况下应避免使用它们。 |

关键字（或类别）更为精细。所有与 AlwaysOn 相关的事件都属于 `AlwaysOn` 和 `HADR` 类别。SQL Server 公开了 122 个与 AlwaysOn 相关的事件。这些事件列在表 [9-2](#394392_3_En_9_Chapter.xhtml#Tab2) 中。

## 表 9-2 AlwaysOn 事件

| 事件 | 描述 |
|---|---|
| `hadr_ddl_failover_execution_state` | 当 DDL 命令更改可用性组故障转移状态时触发 |
| `hadr_transport_dump_message` | 在整个系统中跟踪 HADR 传输消息 |
| `hadr_transport_dump_config_message` | 跟踪 HADR 配置消息 |
| `hadr_transport_dump_failure_message` | 跟踪 HADR 失败消息 |
| `hadr_transport_dump_preconfig_message` | 跟踪 HADR 预配置消息 |
| `hadr_transport_dump_dropped_message` | 跟踪整个系统中被丢弃的 HADR 传输消息 |
| `hadr_transport_session_state` | 当 HADR 传输会话更改状态时触发 |
| `hadr_transport_configuration_state` | 当会话状态更改时触发 |
| `hadr_transport_ucs_registration` | 当 UCS 注册状态更改时触发 |
| `hadr_transport_ucs_connection_info` | 当与 AlwaysOn 传输副本关联的 USC 连接 ID 被注册或更改时触发 |
| `hadr_transport_flow_control_action` | 当针对特定副本发生流量控制操作时触发 |
| `hadr_database_flow_control_action` | 当针对特定副本发生流量控制操作时触发 |
| `hadr_db_manager_state` | 当 db_manager 的状态更改时触发 |
| `hadr_db_manager_lsn_sync_msg` | 跟踪日志序列号同步消息 |
| `hadr_db_manager_establish_db_msg` | 当建立 DB 消息时触发 |
| `hadr_db_manager_status_change` | 跟踪 DBReplicaStatusChange 消息 |
| `hadr_db_manager_redo` | 跟踪辅助服务器上的重做处理 |
| `hadr_db_manager_undo` | 跟踪辅助服务器上的撤销处理 |
| `hadr_db_manager_db_queue_restart` | 响应队列重启 hadron 数据库而触发 |
| `hadr_db_manager_db_startdb` | 响应启动 hadron 数据库而触发 |
| `hadr_db_manager_db_shutdown` | 响应关闭 hadron 数据库而触发 |
| `hadr_db_manager_user_control` | 响应 AlwaysOn 受控数据库的用户状态更改而触发 |
| `hadr_db_manager_redo_control` | 跟踪 AlwaysOn 受控数据库的更改日志扫描状态 |
| `hadr_db_manager_scan_control` | 跟踪 AlwaysOn 受控数据库的更改日志扫描状态 |
| `hadr_db_manager_suspend_resume` | 响应 AlwaysOn 受控数据库的挂起/恢复状态更改而触发 |
| `hadr_db_manager_db_restart` | 响应 AlwaysOn 受控数据库的重启而触发 |
| `hadr_worker_pool_thread` | 跟踪 AlwaysOn 工作程序池线程操作 |
| `hadr_worker_pool_task` | 跟踪 AlwaysOn 工作程序池任务操作 |
| `hadr_thread_pool_worker_start` | 跟踪 AlwaysOn 线程池工作线程启动操作 |
| `hadr_db_manager_page_request` | 跟踪服务器之间的页面请求/响应 |
| `hadr_db_commit_mgr_update_harden` | 响应 AlwaysOn 受控数据库的 hardened 日志序列号更新而触发 |
| `hadr_db_commit_mgr_harden_still_waiting` | 跟踪事务提交 hardened，仍在等待 AlwaysOn 提交管理 |
| `hadr_db_commit_mgr_harden` | 跟踪来自 AlwaysOn 提交管理的事务提交 hardened 结果 |
| `hadr_db_commit_mgr_set_policy` | 响应事务提交管理器策略更新而触发 |
| `hadr_db_partner_set_policy` | 响应 AlwaysOn 伙伴提交策略更新而触发 |
| `hadr_db_partner_set_sync_state` | 响应 AlwaysOn 伙伴的同步状态更改而触发 |
| `hadr_apr_added_corrupted_page` | 当自动页面修复添加了一个损坏的页面时触发 |
| `hadr_apr_repaired_page` | 当自动页面修复修复了一个损坏的页面时触发 |
| `hadr_apr_skipped_page_repair` | 当自动页面修复跳过页面修复时触发 |
| `hadr_apr_failed_page_repair` | 当自动页面修复添加了一个损坏的页面时触发 |
| `hadr_apr_sent_repair_request_for_page` | 当自动页面修复发送页面修复请求时触发 |
| `hadr_apr_received_page_repair_request` | 当自动页面修复收到页面修复请求时触发 |
| `hadr_apr_deffering_page_repair_request` | 当自动页面修复正在延迟页面修复请求时触发 |
| `hadr_apr_page_repair_failed` | 当自动页面修复未能修复页面时触发 |
| `hadr_undo_of_redo_log_scan` | 跟踪在 Redo 的 Undo 中扫描的日志量，以及需要扫描的总日志量 |
| `hadr_db_manager_filemetadata_request` | 响应服务器之间的文件元数据请求/响应而触发 |
| `hadr_capture_compressed_log_cache` | 跟踪压缩日志块缓存的命中/未命中比率 |
| `hadr_db_manager_backup_sync_msg` | 响应备份同步消息而触发 |
| `hadr_db_manager_backup_info_msg` | 响应备份信息消息而触发 |
| `hadr_db_manager_primary_replica_file_list_msg` | 响应主副本文件列表消息而触发 |
| `hadr_db_manager_seeding_request_msg` | 响应播种请求消息而触发 |
| `hadr_physical_seeding_backup_state_change` | 响应备份端物理播种状态更改而触发 |
| `hadr_physical_seeding_restore_state_change` | 响应还原端物理播种状态更改而触发 |
| `hadr_physical_seeding_forwarder_state_change` | 响应转发器端物理播种状态更改而触发 |
| `hadr_physical_seeding_forwarder_target_state_change` | 响应转发器目标端物理播种状态更改而触发 |
| `hadr_physical_seeding_submit_callback` | 响应物理播种提交回调而触发 |
| `hadr_physical_seeding_failure` | 响应物理播种失败而触发 |
| `hadr_physical_seeding_progress` | 响应物理播种进度而触发 |
| `hadr_physical_seeding_schedule_long_task_failure` | 响应物理播种计划长任务失败而触发 |

