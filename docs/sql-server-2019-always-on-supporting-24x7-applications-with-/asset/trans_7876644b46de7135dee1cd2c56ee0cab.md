
由于在此拓扑中无法使用 Active Directory 身份验证，`database_mirroring` 端点需要使用证书相互通信。我们还需要 SQL 登录名，这些登录名将被授予连接到端点的权限。因此，我们首先需要创建证书。这可以通过清单 [7-11](#394392_3_En_7_Chapter.xhtml#PC11) 中的脚本实现。在这里，我们首先在 `master` 数据库中创建数据库主密钥。然后我们创建证书并将该证书备份到操作系统中。这应在将参与可用性组的每个节点上重复执行，为每个实例使用不同的证书名称。

### 创建证书

```
USE master
GO
--Create the Database Master key
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Pa$$w0rd';
GO
-- Create the certificate
CREATE CERTIFICATE Node1Cert WITH SUBJECT = 'ClusterNode1 Certificate';
GO
--Back up the certificate
BACKUP CERTIFICATE Node1Cert TO FILE = 'c:\\Certificates\\Node1Cert.cer';
GO
```

我们现在需要为每个节点创建一个登录名，该登录名将被授予连接到端点的权限。我们可以使用清单 [7-12](#394392_3_En_7_Chapter.xhtml#PC12) 中的脚本实现。这些登录名应在将参与可用性组的所有实例上创建。因此，应针对所有实例运行该脚本。

### 创建登录名

```
CREATE LOGIN ClusterNode1_AOAG WITH PASSWORD = 'Pa$$w0rd';
GO
CREATE USER ClusterNode1_AOAG FOR LOGIN ClusterNode1_AOAG;
GO
CREATE LOGIN ClusterNode2_AOAG WITH PASSWORD = 'Pa$$w0rd';
GO
CREATE USER ClusterNode2_AOAG FOR LOGIN ClusterNode2_AOAG;
GO
```

我们现在可以将每个实例的证书复制到所有其他实例，并使用清单 [7-13](#394392_3_En_7_Chapter.xhtml#PC13) 中的命令将其恢复。对于每个实例，您应适当更改证书和用户名。此清单显示了在我们的拓扑中应在 `CLUSTERNODE1` 上运行的命令。

### 恢复证书

```
CREATE CERTIFICATE Node2Cert AUTHORIZATION ClusterNode2_AOAG FROM FILE = 'C:\\Certificates\\Node2Cert.cer'
```

最后，现在证书已存在于每个节点上，我们可以继续在每个节点上创建 `database_mirroring` 端点。这可以通过清单 [7-14](#394392_3_En_7_Chapter.xhtml#PC14) 中的脚本实现。此脚本需要在每个节点上运行，并将证书更改为与节点匹配。

### 创建端点

```
--Create the endpoint
CREATE ENDPOINT AOAG_Endpoint STATE = STARTED AS TCP (
      LISTENER_PORT = 5022,
      LISTENER_IP = ALL
) FOR DATABASE_MIRRORING (
      AUTHENTICATION = CERTIFICATE Node1Cert,
      ROLE = ALL
)
--Grant each user permissions to connect to the endpoint
GRANT CONNECT ON ENDPOINT::AOAG_Endpoint TO ClusterNode1_AOAG;
GO
GRANT CONNECT ON ENDPOINT::AOAG_Endpoint TO ClusterNode2_AOAG;
GO
```

您现在可以使用本书第 [5](#394392_3_En_5_Chapter.xhtml) 章中讨论的技术创建可用性组。

## 分布式可用性组

分布式可用性组 (`DAG`) 是可用性组的扩展，允许在两个独立的可用性组之间同步数据。这是一项令人兴奋的技术，具有许多不同的用例。例如，它允许在基于 Windows 和 Linux 的可用性组之间进行数据同步，它允许可读的辅助副本数量扩展到 8 之外（这是标准可用性组的限制），并且允许跨站点复制，而无需伸展集群的复杂性。分布式可用性组还可以通过在就地升级不可行且需要并行迁移时提供数据同步来帮助服务器迁移。它们也可以用作域独立可用性组的替代方案，当您在不同域中拥有服务器时。

虽然分布式可用性组的每一侧可以是 Windows 故障转移群集，但这不是必需的，因为重点非常集中在维护数据库上，并且不会进行群集配置。

在本节中，我们将通过为我们在第 [5](#394392_3_En_5_Chapter.xhtml) 章中创建的 HR 可用性组配置 DAG 来说明该技术，该组位于我们的 `ALWAYSON-C` 集群和一个名为 `Linux_AOAG` 的基于 Linux 的可用性组之间，后者托管在两个参与 Pacemaker 集群的 Linux 服务器上。

第一步是在 Linux 集群上创建分布式可用性组。这可以通过使用清单 [7-15](#394392_3_En_7_Chapter.xhtml#PC15) 中的脚本实现。请注意 `WITH (DISTRIBUTED)` 语法，后跟每个可用性组的规范。

> **注意**
> 开始之前，您应从 HR 可用性组中删除现有数据库；否则，它将无法加入分布式可用性组，因为辅助可用性组必须为空。

```
CREATE AVAILABILITY GROUP DistributedAG
   WITH (DISTRIBUTED)
   AVAILABILITY GROUP ON
      'HR' WITH
      (
         LISTENER_URL = 'tcp://HRListener.alwayson.com:1433',
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
         FAILOVER_MODE = MANUAL,
         SEEDING_MODE = AUTOMATIC
      ),
      'Linux_AOAG' WITH
      (
         LISTENER_URL = 'tcp://LinuxListener:5022',
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
         FAILOVER_MODE = MANUAL,
         SEEDING_MODE = AUTOMATIC
      );
GO
```

我们现在可以针对 `PROSQLADMIN-C` 集群运行清单 [7-16](#394392_3_En_7_Chapter.xhtml#PC16) 中的命令，以将其加入分布式可用性组。

```
ALTER AVAILABILITY GROUP DistributedAG
   JOIN
   AVAILABILITY GROUP ON
      'HR' WITH
      (
         LISTENER_URL = 'tcp://HRListener.alwayson.com:1433',
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
         FAILOVER_MODE = MANUAL,
         SEEDING_MODE = AUTOMATIC
      ),
      'Linux_AOAG' WITH
      (
         LISTENER_URL = 'tcp://LinuxListener:5022',
         AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
         FAILOVER_MODE = MANUAL,
         SEEDING_MODE = AUTOMATIC
      )
;
GO
```

> **提示**
> 数据库需要手动加入到辅助可用性组内的辅助副本中。

## 摘要

可用性组提供大量灵活性，使 DBA 能够在几乎任何情况下配置高可用性、灾难恢复或读扩展。非典型可用性组拓扑的示例包括在 `Azure VMs` 跨越配置可用性组、在没有底层集群的情况下配置可用性组，以及跨域或完全没有域的情况下配置可用性组。

在 `Azure VMs` 中创建可用性组时，高可用性最好通过将虚拟机拆分到 `Availability Sets` 和 `Availability Zones` 来实现，而灾难恢复最好通过将 DR 节点放置在不同的 `Azure Region` 中来实现。在此场景中，可以通过使用 `Azure paired regions` 来维护数据主权，这些区域位于相同的政治地理范围内，同时仍在地理上分散解决方案。

`Clusterless Availability Groups` 不能用于实现高可用性。这是因为没有集群来执行节点健康检查。然而，它们非常适合读扩展场景，在这些场景中可以避免集群组件的复杂性。

域独立可用性组（也称为 `workgroup availability groups`）允许在多个域之间或完全没有域的情况下配置可用性组。它们特别适合混合云场景，在这些场景中，您的组织选择在云中创建单独的域，而不是扩展其本地域。
