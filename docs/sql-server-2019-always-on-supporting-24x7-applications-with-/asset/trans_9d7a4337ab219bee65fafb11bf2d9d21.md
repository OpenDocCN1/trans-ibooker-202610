
## 严重程度级别

| 级别 | 描述 |
|---|---|
| 2 | • 任何级别 1 条件。 • SQL 实例未连接到集群且健康检查阈值已超出。 • 可用性副本处于 FAILED 状态。 |
| 3 (默认) | • 任何级别 1–2 条件。 • SQL Server 实例内部出现关键错误。 |
| 4 | • 任何级别 1–3 条件。 • SQL Server 实例内部出现中等错误。 |
| 5 | • 任何级别 1–4 条件。 • SQL Server 实例内部出现任何故障。 |

当使用 `REPLICA ON` 子句添加副本时，还可以使用额外的 `WITH OPTIONS`。这些内容在表 [5-3](#394392_3_En_5_Chapter.xhtml#Tab3) 中有详细说明。

## ON REPLICA WITH 选项

表 5-3

| 选项 | 描述 |
|---|---|
| `ENDPOINT_URL` | 主机副本的 SQL Server 实例上端点的 URL。 |
| `FAILOVER_MODE` | 指定副本是否应支持自动或手动故障转移。 |
| `AVAILABILITY_MODE` | 指定副本上的事务应同步或异步提交。可用性模式也可以设置为 `CONFIGURATION_ONLY` 模式，以支持外部集群模式。这将在第 [6](#394392_3_En_6_Chapter.xhtml) 章中讨论。 |
| `SESSION_TIMEOUT` | 指定会话超时时间（以秒为单位）。 |
| `BACKUP_PRIORITY` | 为副本提供权重，用于决定应从哪个副本获取备份。0 表示无法从副本获取备份。 |
| `SEEDING_MODE` | 指定副本的播种方式。可接受的值为 `AUTOMATIC` 和 `MANUAL`。 |
| `PRIMARY_ROLE` | 用于指定 `ALLOW_CONNECTIONS` 设置并传递 `READ_ONLY_ROUTING_LIST`。 |
| `SECONDARY_ROLE` | 用于指定 `ALLOW_CONNECTIONS` 的值并传递 `READ_ONLY_ROUTING_URL`。 |

因此，清单 [5-6](#394392_3_En_5_Chapter.xhtml#PC6) 中的脚本将创建 `Accounts` 可用性组。请注意，我们选择手动播种副本。

### Listing 5-6 创建 Accounts 可用性组
```sql
CREATE AVAILABILITY GROUP Accounts WITH (
      AUTOMATED_BACKUP_PREFERENCE = PRIMARY,
      DB_FAILOVER = OFF,
      DTC_SUPPORT = NONE,
      REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 0
) FOR REPLICA ON
      'CLUSTERNODE1\PROD' WITH (
            ENDPOINT_URL = N'TCP://CLUSTERNODE1.AlwaysOn.com:5022',
            FAILOVER_MODE = AUTOMATIC,
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
            SESSION_TIMEOUT = 10,
            BACKUP_PRIORITY = 50,
            SEEDING_MODE = MANUAL,
            PRIMARY_ROLE(ALLOW_CONNECTIONS = ALL),
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)
      ),
      'CLUSTERNODE2\SYNCHA' WITH (
            ENDPOINT_URL = N'TCP://CLUSTERNODE2.AlwaysOn.com:5022',
            FAILOVER_MODE = AUTOMATIC,
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
            SESSION_TIMEOUT = 10,
            BACKUP_PRIORITY = 50,
            SEEDING_MODE = MANUAL,
            PRIMARY_ROLE(ALLOW_CONNECTIONS = ALL),
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)
      ),
      'CLUSTERNODE3\ASYNCDR' WITH (
            ENDPOINT_URL = N'TCP://CLUSTERNODE3.AlwaysOn.com:5022',
            FAILOVER_MODE = MANUAL,
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            SESSION_TIMEOUT = 10,
            BACKUP_PRIORITY = 50,
            SEEDING_MODE = MANUAL,
            PRIMARY_ROLE(ALLOW_CONNECTIONS = ALL),
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)
      );
GO
```

现在我们已经创建了 `Accounts` 可用性组，我们需要创建一个可用性组侦听器，以便集群知道将请求定向到何处。我们将使用带有 `ADD LISTENER` 子句的 `ALTER AVAILABILITY GROUP` 命令来完成此操作。`ADD LISTENER` 子句的 `WITH` 选项在表 [5-4](#394392_3_En_5_Chapter.xhtml#Tab4) 中指定。

## ADD LISTERNER WITH 选项

表 5-4

| 选项 | 描述 |
|---|---|
| `IP` | 指定侦听器将监听的 IP 地址（们），以及与每个 IP 地址相关的网络的子网掩码 |
| `PORT` | 侦听器将监听的端口 |

因为我们的 `CLUSTERNODE3\ASYNCDR` 实例与 `CLUSTERNODE1\PROD` 和 `CLUSTERNODE2\SYNCHA` 位于不同的子网中，所以我们需要指定两个 IP 地址，每个子网一个。因此，清单 [5-7](#394392_3_En_5_Chapter.xhtml#PC7) 中的脚本将为 `Accounts` 可用性组创建可用性组侦听器。

### Listing 5-7 创建可用性组侦听器
```sql
ALTER AVAILABILITY GROUP Accounts
      ADD LISTENER 'Accounts' (
            WITH IP
                  (
                        (N'10.0.0.8', N'255.255.255.0'),
                        (N'10.0.1.8', N'255.255.255.0')
                  ),
            PORT=1433
      );
GO
```

## 使用 PowerShell 实现可用性组

正如您所料，Microsoft 提供了 PowerShell cmdlet，可用于创建和管理 SQL Server 可用性组和可用性组侦听器。随着世界向 DevOps 文化转变，PowerShell 成为许多希望实现构建自动化的组织的首选语言，这一点变得越来越重要。

`sqlserver` PowerShell 模块随 SQL Server Management Studio 一起安装，但也可以通过清单 [5-8](#394392_3_En_5_Chapter.xhtml#PC8) 中的命令进行安装。

### Listing 5-8 安装 sqlserver PowerShell 模块
```powershell
Install-Module sqlserver
```

注意
如果这是您在服务器上首次安装 PowerShell 模块，则会提示您安装 NuGet 提供程序。

安装 `sqlserver` 模块后，您将可以使用 cmdlet 通过 PowerShell 创建和管理可用性组。`New-AvailabilityReplica` cmdlet 允许您创建指定每个副本属性的对象。然后可以将这些对象传递给 `New-SqlAvailabilityGroup` cmdlet，后者将创建可用性组。

例如，假设我们想要创建一个名为 `Foo` 的可用性组，其中包含 `Foo` 数据库，`CLUSTERNODE1\PROD` 主机初始主副本，`CLUSTERNODE2\SYNCHA` 主机辅助副本以实现高可用性。我们可以使用清单 [5-9](#394392_3_En_5_Chapter.xhtml#PC9) 中的脚本来实现这一点。

### Listing 5-9 创建 Foo 可用性组
```powershell
#Create connection to CLSUETERNODE1\PROD
$PrimaryServer = Get-Item "SQLSERVER:\\SQL\\CLUSTERNODE1\\PROD" -Verbose
#Create connection to CLUSTERNODE2\SYNCHA
$SecondaryServer = Get-Item "SQLSERVER:\\SQL\\CLUSTERNODE2\\SYNCHA"
#Set the properties of the primary Replica
$PrimaryReplicaOptions = @{
    Name             = "CLUSTERNODE1\PROD"
    EndpointUrl      = "TCP://CLUSTERNODE1.ALWAYSON.COM:5022"
    FailoverMode     = "Automatic"
    AvailabilityMode = "SynchronousCommit"
    Version          = ($PrimaryServer.Version)
}
#Create the primary Replica Object
$PrimaryReplica = New-SqlAvailabilityReplica @PrimaryReplicaOptions  -AsTemplate
#Set the properties of the secondary Replica
$SecondaryReplicaOptions = @{
    Name             = "CLUSTERNODE2\SYNCHA"
    EndpointUrl      = "TCP://CLUSTERNODE2.ALWAYSON.COM:5022"
    FailoverMode     = "Automatic"
    AvailabilityMode = "SynchronousCommit"
    Version          = ($SecondaryServer.Version)
}
#Create the secondary replica object
$SecondaryReplica = New-SqlAvailabilityReplica @SecondaryReplicaOption -AsTemplate
$AvailabilityGroupOptions = @{
    InputObject         = $PrimaryServer
    Name =              = "Foo"
    AvailabilityReplica = ($PrimaryReplica, $SecondaryReplica)
    Database            = @("Foo")
}
New-SqlAvailabilityGroup @AvailabilityGroupOptions
```
