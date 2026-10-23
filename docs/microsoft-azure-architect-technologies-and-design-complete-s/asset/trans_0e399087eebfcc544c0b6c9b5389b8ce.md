

## 图 3.4 Azure 区域中的可用性区域

![](img/95043b3df926bcdadadb082b49262037_160_0.png)

最后，并非所有 Azure 区域都提供可用性区域，而且提供可用性区域的区域也并非支持所有 Azure 产品和功能。因此，你必须了解迁移到 Azure 平台的工作负载是否需要可用性区域，还必须了解需要哪些产品和功能，然后选择部署位置。将这份列表以书面形式列出来并不明智，因为它可能并且确实会以相对较快的速度发生变化。有关支持可用性区域的区域及其支持产品的简要汇总，请参阅表 3.2。

## 表 3.2 区域、可用性区域和产品

|  | Central US | East US | North Europe | Southeast Asia |
|---|---|---|---|---|
| 虚拟机 | ✓ | ✓ | ✓ | ✓ |
| 托管磁盘 | ✓ | ✓ | ✓ | ✓ |
| Azure VPN 网关 | ✓ | × | ✓ | ✓ |
| Azure ExpressRoute | ✓ | × | ✓ | ✓ |
| Azure 负载均衡器 | ✓ | ✓ | ✓ | ✓ |
| Azure Cosmos DB | × | × | × | ✓ |

此列表在此处在线更新：

```
docs.microsoft.com/en-us/azure/availability-zones/az-overview#services-support-by-region
```

请注意，表 3.2 显示 Azure VPN 网关和 ExpressRoute 在美国东部区域不受支持；换句话说，这些产品在可用性区域中不受支持。这意味着如果美国东部的一个可用性区域发生意外中断，而你配置了其中一种产品，那么你将受到影响。请看下面的问题，它将测试你关于如何测试 Azure 区域之间延迟的知识。

你可以使用哪种 Azure 工具来测量某个区域与特定位置之间的网络延迟？

- A. Network Logger
- B. Network Watcher
- C. Wireshark
- D. Network Monitor

答案是 Network Watcher，你将在练习 3.1 中使用它。没有名为 Network Logger 的产品，至少不是 Azure 的产品。Wireshark 和 Network Monitor 都可以用来监视网络流量，但两者都不是测量 Azure 区域与特定位置之间延迟的最佳选择。

![](img/95043b3df926bcdadadb082b49262037_161_0.png)

> 第 1 章介绍了 Azure 存储。我们将在第 5 章更深入地探讨该产品。还记得 LRS、ZRS、GRS 和 RA-GRS 这些术语吗？这些选项不仅对 Azure 存储至关重要，而且是提供可用性区域所提供功能的基础。这些冗余选项是必须了解的概念。

一旦你考虑了应用程序需要多高的冗余度，并缩小了提供所有必需产品和功能的 Azure 区域范围，就可以进入下一步：找到离你的客户或员工最近的区域。请记住，客户无法容忍延迟或性能不佳的应用程序。他们也许以后会再来，或者干脆转向竞争对手，再也不回来。从员工的角度来看，如果必须使用延迟高的应用程序来完成工作，这种缓慢会导致极大的挫败感和生产力损失。

如果工作负载供内部使用，回答这个问题可能很简单。Azure 区域的位置只需选择离公司所在地最近的区域即可。随着公司所在位置数量的增加，情况会变得复杂一些，并在某个时刻扩展到全球。那么问题来了：如果应用程序面向客户，客户主要位于何处？是只有一个或少数几个位置，还是客户群遍布全球？这些问题需要由公司来回答。下面的练习有助于为做出此类关键决策提供输入。

在练习 3.1 中，你将在 Central US 区域（位于爱荷华州）托管工作负载。Central US 支持可用性区域，并且可在其中运行的所有 Azure 产品和功能也都受支持，但 Azure Cosmos DB 除外。在不久的将来，你预计将在德国和日本设立公司办事处，并希望测试来自这些位置的延迟（参见图 3.5）。

## 图 3.5 测试从 Central US 区域到其他区域和全球位置的连接

![](img/95043b3df926bcdadadb082b49262037_162_0.png)

要测试这三个位置之间的延迟，请完成练习 3.1。目前没有用于执行此活动的 Azure 门户界面；相反，你将使用 PowerShell。

## 练习 3.1

## 测试从某个区域到其他州和国家的网络延迟

- 1. 如果尚未安装新的 Az PowerShell cmdlet，请以管理员身份打开 Windows PowerShell ISE 后运行以下命令。Windows 机器上默认安装了 PowerShell。

```
Install-Module -Name Az -AllowClobber -Force
```

## 练习 3.1（续）

- 2. 安装 PowerShell 后，使用 Connect-AzAccount cmdlet 登录。键入 Connect-AzAccount 并按 Enter。系统将自动提示你输入凭据。像通过门户访问 Azure 时一样输入凭据。

- 3. 完成身份验证后，请确保你处于所需的 Azure 订阅上下文中。这仅在你拥有多个 Azure 订阅时相关。执行以下 cmdlet 以设置 Azure 订阅。将你的订阅作为 SubscriptionId 的值。

```
$subscription = Get-AzSubscription -SubscriptionId "########-####-######".
Set-AzContext $subscription
```

- 4. 通过执行以下 cmdlet 创建一个资源组来放置 Azure 资源。位置为 Central US，因为我们将在此放置 Network Watcher 实例。资源组及其中的资源不需要位于同一区域；但是，从维护的角度来看，这样做是合理的。（DM1 是 Central US 区域的内部命名。）

```
New-AzResourceGroup -Name CSHARP GUITAR-DM1-RG -Location centralus
```

资源组已在第 2 章中提到过，从那一章中你知道资源组是可以授予访问权限的资源组。适当地命名资源组非常重要，这样你无需逐一检查即可知道其内容的目的。在本书中，我将向多个资源组添加资源；但是，它们将遵循标准的命名约定。这有助于我删除它们，因为一旦完成练习，我就不再需要这些资源。

- 5. 接下来，创建 Network Watcher 实例。

```
New-AzNetworkWatcher -Name NetworkWatcherCentralUS \
    -ResourceGroupName CSHARP GUITAR-DM1-RG \
    -Location centralus
```

- 6. Network Watcher 实例已创建完毕，可以执行一些测试了。首先，生成 Central US 区域与明尼苏达州之间的可达性报告。执行以下命令：

```
Get-AzNetworkWatcherReachabilityReport \
    -NetworkWatcherName NetworkWatcherCentralUS \
    -ResourceGroupName CSHARP GUITAR-DM1-RG \
    -Location "Central US" \
    -Country "United States" \
    -State "minnesota" \
    -StartTime "2019-05-25" \
    -EndTime "2019-05-30"
```

- 7. 输出可能类似于以下内容。报告的开始时间和结束时间跨越五天。最大时间为 30 天，且仅存在滚动 30 天期间的数据。

```
AggregationLevel  : State
ProviderLocation  :
{
  "Country": "United States",
  "State": "minnesota"
}
ReachabilityReport :
[
{
  "Provider": "Comcast Cable Communications, LLC - ASN 7922",
  "AzureLocation": "Central US",
  "Latencies": [
    {
      "TimeStamp": "2019-05-29T00:00:00Z",
      "Score": 96
    },
    {
      "TimeStamp": "2019-05-28T00:00:00Z",
      "Score": 94
    },
    {
      "TimeStamp": "2019-05-27T00:00:00Z",
      "Score": 98
    },
    {
      "TimeStamp": "2019-05-26T00:00:00Z",
      "Score": 98
    },
    {
      "TimeStamp": "2019-05-25T00:00:00Z",
      "Score": 98
    }]
}
]
```

