
# 从集群中移除节点时所需的强制参数

| 参数 | 用法 |
|---|---|
| `/ACTION` | 必须配置为 `RemoveNode`。 |
| `/INSTANCENAME` | 你要添加额外节点以支持的实例。 |
| `/CONFIRMIPDEPENDENCYCHANGE` | 允许为多子网集群指定多个 IP 地址。传入 `1` 表示 True，`0` 表示 False。 |

清单 8-3 中的脚本在我们从 SQL Server 安装介质的根目录运行它时，会从我们的集群中移除一个节点。 `.\setup.exe /ACTION="RemoveNode" /INSTANCENAME="ALWAYSON-SQL-C" /CONFIRMIPDEPENDENCYCHANGE=0 /qs`

## 移除节点

# 管理 AlwaysOn 可用性组

一旦你的可用性组的初始设置完成，你仍需执行管理任务。这些包括故障转移可用性组、监控，以及在极少数情况下添加额外的侦听器。以下各节将讨论这些主题。

## 故障转移

如果副本处于同步提交模式并且配置为自动故障转移，则在主副本满足错误条件时，可用性组会自动切换到冗余副本。然而，有时你需要手动故障转移可用性组。这可能是由于 DR 测试、主动维护，或需要在主副本或主数据中心发生故障后启动异步副本。

### 同步故障转移

如果你希望故障转移处于同步提交模式的副本，请在对象资源管理器中右键单击可用性组，从上下文菜单中选择“故障转移”以启动“故障转移可用性组”向导。跳过“简介”页后，你会看到“选择新的主副本”页（见图 8-4）。在此页上，选中你想要故障转移到的副本的复选框。但在执行此操作之前，请查看“故障转移就绪状态”列，以确保副本已同步且不会发生数据丢失。

Figure 8-4 ¶ 选择新主副本页

在图 8-5 所示的“连接到副本”页上，使用“连接”按钮建立与新主副本的连接。

Figure 8-5 ¶ 连接到副本页

在“摘要”页上，你会看到要执行的任务详情，然后在“结果”页上显示进度指示器。故障转移完成后，请检查所有任务是否成功，并调查收到的任何错误或警告。

我们也可以使用 T-SQL 来故障转移可用性组。清单 8-4 中的命令可实现相同结果。请确保从将成为新主副本的副本运行此脚本。如果你从当前主副本运行，请使用 `SQLCMD` 模式并在脚本中连接到新主副本。

```
ALTER AVAILABILITY GROUP HR FAILOVER;
GO
```

故障转移可用性组

### 异步故障转移

如果你的可用性组处于异步提交模式，从技术角度来看，你可以以与同步提交模式副本类似的方式进行故障转移，只是需要强制故障转移，从而接受数据丢失的风险。你可以使用清单 8-5 中的命令强制故障转移。你应在将成为新主实例的实例上运行此脚本。为使其生效，集群必须拥有法定人数。如果没有，则需要在强制可用性组上线之前先强制集群上线。

```
ALTER AVAILABILITY GROUP HR FORCE_FAILOVER_ALLOW_DATA_LOSS;
```

强制故障转移

从流程角度来看，你只有在主站点完全不可用时才应这样做。如果不是这种情况，请先将应用程序置于安全状态。这可避免任何数据丢失的可能性。我在生产环境中通常通过执行以下步骤来实现：

1. 禁用登录。
2. 将副本模式更改为同步提交模式。
3. 故障转移。
4. 将副本改回异步提交模式。
5. 启用登录。

你可以使用清单 8-6 中的脚本执行这些步骤。从 DR 实例运行此脚本时，它会在故障转移前将 `HR` 中的数据库置于安全状态，然后重新配置应用程序以在正常操作下工作。

```
--DISABLE LOGINS
DECLARE @AOAGDBs TABLE ( DBName NVARCHAR(128) );
INSERT INTO @AOAGDBs SELECT database_name FROM sys.availability_groups AG INNER JOIN sys.availability_databases_cluster ADC ON AG.group_id = ADC.group_id WHERE AG.name = 'HR';
DECLARE @Mappings TABLE ( LoginName NVARCHAR(128), DBname NVARCHAR(128), UserName NVARCHAR(128), AliasName NVARCHAR(128) );
INSERT INTO @Mappings EXEC sp_msloginmappings;
DECLARE @SQL NVARCHAR(MAX)
SELECT DISTINCT @SQL = ( SELECT 'ALTER LOGIN [' + LoginName + '] DISABLE; ' AS [data()] FROM @Mappings M INNER JOIN @AOAGDBs A ON M.DBname = A.DBName WHERE LoginName <> SUSER_NAME() FOR XML PATH ('') )
EXEC(@SQL)
GO
--SWITCH TO SYNCHRONOUS COMMIT MODE
ALTER AVAILABILITY GROUP HR MODIFY REPLICA ON N'CLUSTERNODE3\ASYNCDR' WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
GO
--FAIL OVER
ALTER AVAILABILITY GROUP HR FAILOVER
GO
--SWITCH BACK TO ASYNCHRONOUS COMMIT MODE
ALTER AVAILABILITY GROUP HR MODIFY REPLICA ON N'CLUSTERNODE3\ASYNCDR' WITH (AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT);
GO
--ENABLE LOGINS
DECLARE @AOAGDBs TABLE ( DBName NVARCHAR(128) );
INSERT INTO @AOAGDBs SELECT database_name FROM sys.availability_groups AG INNER JOIN sys.availability_databases_cluster ADC ON AG.group_id = ADC.group_id WHERE AG.name = 'HR';
DECLARE @Mappings TABLE ( LoginName NVARCHAR(128), DBname NVARCHAR(128), Username NVARCHAR(128), AliasName NVARCHAR(128) );
INSERT INTO @Mappings EXEC sp_msloginmappings
DECLARE @SQL NVARCHAR(MAX)
SELECT DISTINCT @SQL = ( SELECT 'ALTER LOGIN [' + LoginName + '] ENABLE; ' AS [data()] FROM @Mappings M INNER JOIN @AOAGDBs A ON M.DBname = A.DBName WHERE LoginName <> SUSER_NAME() FOR XML PATH ('') );
EXEC(@SQL)
```

将应用程序置于安全状态并进行故障转移

### 故障转移分布式可用性组

分布式可用性组不支持自动故障转移；仅支持手动故障转移。当你需要在分布式可用性组内故障转移到辅助可用性组时，应执行以下步骤：

* 将同步模式设置为同步提交。
* 等待辅助可用性组同步完成。
* 将主可用性组设置为承担辅助角色。
* 强制故障转移。
