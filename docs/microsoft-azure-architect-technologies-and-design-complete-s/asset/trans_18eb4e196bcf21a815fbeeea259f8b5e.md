

将数据与消费方就近部署（co-locating the data near those who consume it）。如果没有 Azure Cosmos DB 层，Azure Table 将绑定到单个区域，该区域可能距离当前或潜在的用户群有数个大洲之遥。关于 Azure Table 已没有太多需要补充的了。查看图 5.42 可了解此产品如何与分区配合工作，如果你出于兴趣来到这里，请浏览本章稍早的“Table Storage”部分。Azure Tables 出现在此处的原因完全是因为它被归入 Azure Storage 账户结构。 （另见图 5.10，其中说明了 Blobs、Files、Queues 和 Tables 都必须包含在 Azure Storage 账户内。）

## 其他 Azure 存储服务

前四种产品构成了最常用的 Azure 存储产品。请记住命名上的差异以及 Azure Data Store 的作用，即数据库与 Azure Storage 存储数据但具有特定用例，后者现在应该已经清楚。还有一些其他功能和产品属于存储范畴，但尚未得到直接关注，却值得关注。示例包括 Storage Explorer、共享访问签名（SAS）、Azure Data Lake Storage 和 Azure NetApp Files。

## Storage Explorer

这是一个可下载的工具，可让与 Azure 存储产品的交互变得简单。下载并安装该工具后，你可以根据需要创建任意四种 Azure 存储产品来插入、更新和删除 blob、文件、消息或键/值对。在此工具之前，唯一的方法是通过使用 Visual Studio、Azure Storage SDK 或 REST API。 后两者需要进行大量编码。图 5.47 说明了使用 Storage Explorer 查看 Azure Table 内容视图的外观。

FIGURE 5.47 View the contents of an Azure Table using Storage Explorer.

![](img/95043b3df926bcdadadb082b49262037_474_0.png)

此外，还提供一些监控和管理功能，例如清空队列、创建共享和查看使用统计信息。该工具对于将中小型数据源从本地迁移到 Azure 平台也很有用。

## Shared Access Signature (SAS)

共享访问签名（SAS）密钥最初在第 2 章中介绍，它提供了限制对 Azure Storage 账户中特定资源的访问和权限的手段。在 Azure 门户中，导航到 Azure Storage 容器，然后单击导航窗格上的 Shared Access Signature 链接。这将打开一个刀片，允许你创建具有有限访问权限的访问密钥。例如，你可以创建一个只能向 Azure Queue 添加消息的 SAS。这意味着任何使用该连接字符串（包括 SAS 令牌）的客户端只能插入消息。该客户端无法读取、删除或列出任何消息。作为替代方案，导航到 Access Keys 刀片，其中包含主或管理员级别的连接字符串并生成不同的令牌。当客户端使用该连接字符串连接到 Azure Queue 时，他们还可访问账户中的所有存储选项以及其上的所有 CRUD 操作。SAS 应在你的客户端连接到账户时使用 Azure Active Directory（AAD）而无法或不使用时使用，AAD 是在 Azure Storage 上实施安全性的首选或推荐方法。本章稍后“Securing Azure Data”部分对此有更多介绍。

## Azure Data Lake Storage

Azure Data Lake Storage（ADLS）产品的第一代（Gen1）基于 Azure Blobs，用于存储支持大数据分析工作负载的数据。使用 ADLS Gen1，你大致可以期望获得与 Azure Blob 容器相同的冗余、容量、吞吐量和可扩展性。存储到 ADLS 的数据处于类似暂存的状态（数据仓库）。数据可以以任何格式存在，从数据库数据集到视频。数据保留在 ADLS 中，直到准备好进行分析。一旦由 Azure Data Lake 产品分析，结果将被发布并可使用前端工具（如 Power BI）查看。存在于 ADLS 中的数据可使用前面讨论的 Storage Explorer 查看。

## Azure NetApp Files

Azure NetApp 提供的服务与 Azure Files 相同，只有一个很大的区别。你无需管理或配置任何类型的存储产品。Azure NetApp 会为你完成此操作。它基于专有的但由 Microsoft 完全支持的 NetApp 软件解决方案，可在 www.netapp.com 找到。此产品有助于迁移和运行需要企业级文件共享解决方案的混合解决方案。与 Azure Files 相比，NetApp 需要更少的手动维护，例如。

## Zone Replication

区域复制在本书的许多地方都有提及。现在很高兴向你提供有关众多 Azure 存储冗余选项的详细信息。

- Locally redundant storage (LRS)
- Zone-redundant storage (ZRS)
- Geo-redundant storage (GRS)
- Read-access geo-redundant storage (RA-GRS)
- Geo-zone-redundant storage (GZRS)
- Read-access geo-zone-redundant storage (RA-GZRS)

表 5.9 提供了每个复制选项的冗余度、耐用性和可用性阈值。在查看之前，请注意其中两个选项具有读取访问能力。当你创建 Azure Storage 账户时，使用包含读取访问的选项，数据的副本只能读取。如果不需要对副本进行读取访问，使用不带读取访问的复制选项更具成本效益。如果 Microsoft 发起故障转移，过程中的一个步骤会使冗余副本可读。在常规使用中，该副本不可读。

**TABLE 5.9** Zone Replication and Storage Redundancy

|  | LRS | ZRS | GRS | RA-GRS | GZRS | RA-GZRS |
|---|---|---|---|---|---|---|
| Node | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Data center/zone | × | ✓ | ✓ | ✓ | ✓ | ✓ |
| Region | × | × | ✓ | ✓ | ✓ | ✓ |
| Read-access | × | × | × | ✓ | × | ✓ |
| SLA | 99.9% | 99.9% | 99.9% | 99.9% | 99.9% | 99.9% |
| Durability | 11 9’s | 12 9’s | 16 9’s | 16 9’s | 16 9’s | 16 9’s |

当你阅读每个存储选项的具体描述时，节点、数据中心/区域和区域在此上下文中的含义将会变得清晰。接下来将介绍每个选项的详细信息。表 5.9 前一段已解释了读取访问，这就剩下 SLA 和耐用性选项。SLA 通常与系统的可用性有关。正常运行时间是大多数技术人员都知道的内容。它通常是一个指标，就像表 5.9 中显示的那样。另一方面，耐用性与数据的长期保护有关。与侧重于可用性和通常侧重于冗余硬件的 SLA 不同，耐用性侧重于数据的冗余以及防止其以任何方式损坏或受损。数据的耐用性正是这些存储冗余选项存在的原因。

## Locally Redundant Storage (LRS)

快速回顾一下图 3.4，该图说明了区域和区域的概念。每个区域被视为一个数据中心；所有三个都存在于同一区域中，但距离足够远

## Geo-Zone-Redundant Storage (GZRS)

当 GRS 运行中时，LRS 规则适用，正如你刚才阅读的那样。这意味着数据位于单个区域中，但位于两个区域中。GZRS 仍会复制到其双区域，但在运行中它表现得像 ZRS 而不是 LRS。请记住，ZRS 将数据复制到区域中的每个区域，而 LRS 则将数据复制到同一区域中的三个节点。这为你提供了更大的冗余度并降低了需要故障转移的可能性。没有 IT 支持人员希望经历故障转移，因为在事件发生前、中、后都需要大量的注意力和测试。此外，还必须采取步骤在中断事件结束后恢复到主节点。这有成本；成本是 IT 支持专业人员的时间。如果该时间大于 GZRS 与其他冗余选项的成本，则此选项适合你。请记住，随着你逐步了解这些选项，随着副本数量和平台要求的增加，每个选项的成本都会增加。

## Read-access Geo-zone-redundant Storage (RA-GZRS)

最后一个选项是你肯定可以毫无失败地定义的一个。阅读之前，请花点时间向自己描述 RA-GZRS 是什么。与 RA-GRS 一样，GZRS 集群的辅助副本允许读取操作。这可能是你正在迁移到 Azure 或在 Azure 上创建的工作负载的要求，也可能不是。然而，你永远不想丢失数据或出现无法访问数据的场景，对吧？这完全取决于数据访问对你有多重要。这不是开发和测试场景的有用选项；相反，将其用于高度关键的生产数据场景。

## Azure Managed Disk Storage

托管磁盘存储在第 4 章中介绍过，但没有磁盘信息的存储章节是不完整的。托管磁盘是存储在 Azure Blob 容器中以 VHD 形式存在的页面 blob 的构造。拥有托管磁盘的好处是，你可以使用它的快照作为最多 10,000 个 Azure VM 的根或主磁盘。你可以拥有 10,000 个相同的 Azure VM，它们使用从存储为 Azure Blob 容器中页面 blob 的同一 VHD 构建的托管磁盘进行配置。读完本书至今并完成所有示例后，你知道可以想象上一句中所有技术术语和概念，真是令人惊叹。请再次注意，Azure Managed Disks 与 VMSS 深度集成。查看表 4.8 了解其限制和约束。

## Data Backup, Migration, and Retention

数据对组织很重要。它已经被写过无数次，但提醒一下往往有帮助。你的安全保护什么？你的网络给你访问什么？已配置的计算资源在什么代码上运行？这些问题的答案都是数据。老实说，很难想象一个应用程序

