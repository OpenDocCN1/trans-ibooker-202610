

6. 在浏览器窗口顶部选择“我的项目”选项卡，你将看到刚刚创建的映像。选择它并输入用户名和密码。选择“允许所选端口”单选按钮，然后启用 HTTP（80）、HTTPS（443）和 RDP（3389）。单击“查看 + 创建”按钮，查看详细信息，特别是“网络”部分。单击“创建”按钮。如果你在上一个练习中使用了相同的资源组和订阅，则网络值默认为，例如，VNET = CSHARPGUITAR-VNET-B 和子网 = csharp。

7. 部署完成后，获取该 VM 的公共 IP 地址并通过浏览器导航到它。

现在你有一个映像，如我在练习 4.6 中创建的示例那样，响应为“Hello from CSHARPGUITAR-SC”。你可以在创建冗余和故障转移解决方案时多次使用你创建的此映像。该映像也可用于虚拟机规模集（VMSS），这将在本章后面更详细地讨论。现在让我们转换思路，更多地了解一下可用于 Azure VM 的两种最受欢迎的操作系统：Windows 和 Linux。Azure 解决方案架构师专家必须掌握 Azure 上支持的每个操作系统的哪个版本。还要知道，操作系统受订阅、CPU、IOPS、RAM 和可用存储的限制。你可以预期考试中会出现有关此方面的问题。例如，以下哪种类型的 VM 无法部署到 Azure？

## Windows

Windows Server 操作系统是运行企业级工作负载最常用的软件产品之一。在本节和后面专门针对 Linux Azure VM 的部分中，我们将重点介绍受支持的操作系统版本、这些操作系统版本的推荐 VM 大小子集，以及针对操作系统和版本特定的一些配额限制。回想一下练习 4.5，你从 Azure Marketplace 中选择了 Windows Server 2019 Datacenter Server Core 的预构建映像。查看通过门户从 Azure Marketplace 可用的以下 10 个受支持的 Windows 映像：

- Windows Server 2008 R2 SP1
- Windows Server 2012 Datacenter
- Windows Server 2012 R2 Datacenter
- Windows Server 2016 Datacenter
- Windows Server 2016 Datacenter - Server Core
- Windows Server 2016 Datacenter - with Containers
- Windows Server 2019 Datacenter
- Windows Server 2019 Datacenter Server Core
- Windows Server 2019 Datacenter Server Core /w Containers
- Windows Server 2019 Datacenter with Containers

Azure VM 的推荐大小基于其中每个操作系统版本。推荐是根据前面在表 4.2 中介绍的类别排列的。因此，了解你的应用程序属于哪个类别非常重要。表 4.3 提供了基于所选工作负载类别的推荐 Azure VM 大小列表。实例/系列列中的值，在我的示例中为 DS1V2，表示许多计算组件的分组，例如 CPU 速度、CPU 与内存比率、临时存储范围、最大数据磁盘、IOPS 和网络带宽吞吐量。每个系列中这些组件之间的比率不同，因此具有预期的目标应用程序类别。你可以在以下位置找到每个 Windows OS Azure VM 的具体信息：

```
docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-memory.
```

作为 Azure 解决方案架构师专家，你应该能够在给出部署到 Azure VM 的工作负载规范时推荐类别和 VM 系列。

### TABLE 4.3 Windows Versions to Azure VM Size Recommendation

| Windows Version | Category | Instance/Series |
|---|---|---|
| 2008 R2 SP1 | General Purpose | DS1V2, DS2V2, D2SV3, D1V2, D1, DS1, DS2 |
| 2012 Datacenter |  |  |
| 2012 R2 Datacenter | Memory Optimized | DS11V2, E2SV3, DS11 |
| 2016 Datacenter * | General Purpose | DS1V2, DS2V2, D4SV3, D1V2, D1, DS1, DS2 |
| 2019 Datacenter * | Memory Optimized | DS11V2, E2SV3, DS11 |

如果你看到星号（*），它表示该 Windows 版本的所有变体。如“迁移 Azure 虚拟机”部分后面所述，可以将你自己的本地 VM 迁移到 Azure 平台。这意味着几乎任何可以导出并转换为虚拟硬盘（VHD）格式的操作系统及其配置都可以部署到 Azure VM。听起来比实际操作容易一些。当然，你可以部署运行 Windows Server 2003 的 VM；但是，一些功能例如集群故障转移、Azure VM 代理或 VM 扩展将无法工作或不受支持。一些在 Azure 上不受支持的其他常见 Windows Server 功能包括在以下列表中：

- Wireless LAN Service
- SNMP Services
- Dynamic Host Configuration Protocol (DHCP)
- Storage manager for SAN
- Hyper-V
- DirectAccess
- Network Load Balancing
- BitLocker Drive Encryption
- RRAS
- Rights Management Services
- Peer Name Resolution Protocol
- Windows Deployment Services

限制和配额的主要原因是防止意外消耗资源导致高额、意外的费用。大多数限制被认为是软限制，这意味着如果需要更多，可以通过联系 Microsoft 并请求更多来获得更多。这些限制通常绑定到订阅或区域。我见过许多客户创建多个订阅或部署到多个区域以绕过软限制，而不是意识到该限制是为了保护，可以增加。联系 Microsoft 增加软限制将使你在 Azure 上管理工作负载更直观。将资源移动到其他订阅或区域，而这些资源都是同一解决方案的一部分，会使管理变得更困难。然而，确实存在一些硬限制，几乎所有客户都必须遵守。这些数字很大，大多数公司都不需要（或负担不起）这么多。它们也不总是记录在案。这是因为如果硬限制没有硬编码到产品中，那么如果业务案例合理，限制甚至可以超过记录的限制。表 4.4 描述了一些与 Azure VM 相关的订阅限制。

### TABLE 4.4 Azure VM Limits

| Azure Resource | Soft/Default Limit | Hard/Max Limit |
|---|---|---|
| Virtual machines | 25,000 per region | Contact Microsoft |
| Virtual networks | 100 per region | 1000 per region |
| Managed disks | 50,000 per region | Contact Microsoft |
| Storage accounts | 250 per region | Contact Microsoft |
| Virtual machine cores | 30 per region | Contact Microsoft |

还请注意，这些限制经常变化。表 4.4 中列出的限制以及大多数数值和关系限制在 Azure 解决方案架构师专家考试创建时就已存在。学习这些将帮助你回答考试中关于此主题的任何问题。通过关系匹配，我指的是表 4.3，其中显示了操作系统版本、类别和 VM 大小之间的链接。

表 4.4 中每区域 30 个核心的限制指的是特定场景，其中限制适用于特定实例/系列类型。例如，你不能在同一区域拥有超过 30 个核心的系列 A 和 D VM。此外，Dv2 和 F 系列 VM 在同一区域也有 30 个核心的限制。你可以在同一区域拥有 10 个 A1 核心和 20 个 D1 核心，总计 30 个。你也可以在同一区域拥有 30 个 D1 核心和 30 个 F1 核心，因为实例/系列在限制逻辑中没有被分组在一起。这在使用 IaaS 时是一个重要点；然而，我不期望 Azure 解决方案架构师专家考试会出现这样的问题，所以在你成为不仅是认证的 Azure 解决方案架构师专家，而且成为资深且高度胜任的专家时，请记住这一点。

## Linux

Linux 正在迅速成为运行中小型工作负载最常用的操作系统。Microsoft 通过提供将其简单实现到 Azure 平台的手段来帮助支持这种增长。正如你已经构建了 Azure VM，你知道这有多容易。与你之前构建 Windows VM 的练习不同之处在于，你会从下拉框中选择 Linux 映像而不是 Windows 映像。其他步骤相同；没有明显的尝试使部署 Windows VM 比 Linux 更容易。映像默认为 Ubuntu 映像。目前 Azure Marketplace 中有六种 Linux 操作系统发行版。

- Ubuntu Server
- Red Hat Enterprise 7
- Container Linux by CoreOS
- Clear Linux OS
- SUSE Linux Enterprise
- Debian

表 4.5 列出了每个发行版在 Azure Marketplace 中可用的映像。

### TABLE 4.5 Azure Marketplace Linux Versions

| Linux OS | Linux Version |
|---|---|
| Ubuntu Server | 14.04 LTS, 16.04 LTS, 18.04 LTS |
| Red Hat Enterprise | 7.2, 7.3, 7.6 |
| CoreOS | Alpha, Beta, Stable 7.5 |

