

包含在弹出菜单中。单击 File Recovery，然后会提示您下载并运行名为 IaasVMILRExeForWindows.exe 的可执行文件，该文件将连接到磁盘的快照。图 9.37 显示了该可执行文件的输出。

## 图 9.36 Azure 虚拟机灾难恢复

![](img/95043b3df926bcdadadb082b49262037_713_0.png)

## 图 9.37 Azure 虚拟机文件恢复

![](img/95043b3df926bcdadadb082b49262037_713_1.png)

该可执行文件将恢复点的磁盘挂载为本地驱动器。请注意图 9.37 中存在 D:\ 驱动器和 E:\ 驱动器，这使您可以浏览内容并复制要检索的文件或目录。在提供挂载驱动器功能的同一刀片上，还存在一个 Unmount Disks 按钮。完成恢复后单击该按钮，驱动器将保持挂载 12 小时并自动卸载，以防您忘记。

## 恢复 Azure App Services

这一点已经涵盖得很充分了；您需要记住，在运行 PaaS 云托管模型时，服务提供商拥有操作系统。您的主要关注点是确保应用程序代码安全，并且您拥有包含部署到 PaaS 后对应用程序所做的配置的文档，即 Azure App Service。尽管配置被安全存储，但拥有描述任何自定义配置的文档是有意义的。Azure App Services 确实提供了执行和恢复备份的功能；请参阅图 9.30 作为提醒，了解其外观。此外，请回顾图 9.35，其中讨论了 ATM 作为响应可能需要执行 BCDR 计划的事件的恢复选项。

## 恢复 Azure 数据服务

Azure 数据服务主要分为两大类，即数据库型数据存储和涉及文件或文档的数据存储。本书中重点关注并且很可能在考试中出现的数据库是 Azure SQL 和 Azure Cosmos DB。数据存储产品是存在于 Azure Storage 账户中的产品，具体为 Azure Blobs、Azure Tables 和 Azure Files。Azure 队列也驻留在 Azure Storage 账户中；然而，Azure Queues 更多是出于历史原因而存在。让我们确保您尽可能多地了解这些 Azure 产品各自的备份、恢复和冗余功能。请注意，其中大部分内容已在第 5 章中介绍过；这里的内容只是为了收尾可能存在的遗留问题。

- Azure SQL、SQL 托管实例和 SQL 虚拟机
- Azure Cosmos DB
- Azure Storage

从数据恢复的角度来看，一个重要的概念是数据保留。这是指您计划保留备份副本的时间长短。您已学习过 LTR 和 PITR 等概念，它们决定了备份的频率以及备份将保存多长时间；最长为 10 年。默认情况下，Azure SQL 数据库每 12 小时备份一次，事务日志每 5 到 10 分钟采集一次。回想一下图 5.48，您将在其中看到数据库备份计划的自定义配置。如果您想恢复其中一个备份，您将在 Azure 门户中 Azure SQL 或 SQL 托管实例的 Overview 刀片上找到 Restore 菜单项，如图 9.38 所示。单击 Restore 后，您可以选择最新的 PITR 备份或 LTR 备份。

## 图 9.38 Azure SQL 或 Azure 托管实例恢复

![](img/95043b3df926bcdadadb082b49262037_715_0.png)

当您选择 LTR 时，会提供一个下拉列表，让您根据日期选择要使用的特定备份。从 SQL 虚拟机的角度来看，您已经知道这一点，但数据库的备份会与运行它的 Azure VM 的备份一起执行。Azure Backup 和 Azure Site Recovery Services (ASR) 是在 VM 上执行此操作的工具，并且刚刚介绍过。从 Azure Cosmos DB 的角度来看，请查看图 5.41，其中显示了 Azure Cosmos DB 的 Replicate Data Globally 刀片。一旦启用复制，Manual Failover 菜单项将被启用。如果您在主 Azure Cosmos DB 实例上发现任何问题，可以使用 Manual Failover 菜单使另一个实例成为主实例。或者，考虑启用 Automatic Failover，它会监控该区域中的数据库，如果确定不健康，平台将为您执行到另一个区域的故障转移。在配置 Automatic Failover 时，您将指定首选的故障转移实例区域。Azure Cosmos DB 数据库每四小时由平台自动备份。如果您必须从备份中恢复数据库，您将需要联系 Microsoft 支持。目前没有在线功能支持此场景。

当您的 Azure Storage 账户运行在 GRS 或 RA-GRS 服务层时，您可以配置地理复制，如图 9.39 所示。如您所见，主 Azure Storage 账户位于 West Europe (AM2)，辅助账户位于 North Europe (DB3)。

如果您的主 Azure Storage 账户（包括 blob、表和文件）开始出现问题或发生中断，请单击 Prepare For Failover 按钮开始故障转移过程。该过程将简单地使 North Europe 的存储成为主存储，并将所有存储端点重定向到该新区域的实例。请记住，您的 Azure Storage 端点类似于以下内容：

- Blob: https://*.blob.core.windows.net
- Files: https://*.file.core.windows.net
- Queue: https://*.queue.core.windows.net
- Tables: https://*.table.core.windows.net
- Static Website: https://*.z6.web.core.windows.net

## 图 9.39 Azure Storage 账户地理复制

![](img/95043b3df926bcdadadb082b49262037_716_0.png)

您也可以使用以下 Azure CLI 命令执行存储账户的故障转移：

```
az storage account show --name <accountName> --expand geoReplicationStats
az storage account failover --name <accountName>
```

将 <accountName> 替换为您的 Azure Storage 账户名称。第一个命令将显示主和辅助信息，如您之前在图 9.39 中看到的那样。第二个命令将执行实际的故障转移过程。

## 恢复 Azure 消息服务

第 6 章详细介绍了 Azure 消息服务，其中关于备份和恢复的内容并不多。从概念上讲，在很多方面，备份的需求并不大。因为队列中的数据不会在那里停留很长时间。消息服务的目的是存储信息并通知消费者有信息可供处理。一旦消息被处理，它就会从消息服务队列中删除。因此，从恢复的角度来看，您在消息方面的首要考虑应集中在冗余上。也就是说，如果某个区域发生中断，请确保您可以快速将生产者和消费者转移到不同区域的可用端点。

从 Azure Storage Queue 的角度来看，冗余方法刚刚在上一节中提供。Azure Queue Storage 包含在 Azure Storage 账户中，因此如果需要执行故障转移，在启用地理复制后您拥有该功能，这需要 GRS 或 RA-GRS。关于 Azure Service Bus 和 Event Hub，您只能在初始配置期间选择使其区域冗余。区域冗余，如您所回忆的，是指产品部署到区域内的所有数据中心；每个区域的数据中心典型数量为三个。并非所有区域都支持可用性区域；您可以在此处找到支持可用性区域的 Azure 区域的最新列表：

```
docs.microsoft.com/en-us/azure/availability-zones/az-overview
```

Azure Service Bus 的地理复制仅在使用高级定价层时可用。对于 Event Hub，实现地理复制所需的定价层为标准层。这两个消息服务被归为一类，因为其流程和地理复制 Azure 门户刀片彼此非常相似。Azure Service Bus 和 Event Hub 都有类似图 9.40 所示的地理复制刀片。您也一定会注意到图 9.39 和图 9.40 之间的相似之处，这是有道理的，因为该功能的目标是相同的，即冗余。

## 图 9.40 Azure Service Bus 地理复制

![](img/95043b3df926bcdadadb082b49262037_717_0.png)

