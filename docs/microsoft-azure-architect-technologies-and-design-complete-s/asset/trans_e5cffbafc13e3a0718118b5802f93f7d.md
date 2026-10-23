

## EXERCISE 3.1 (continued)

8. Score 值是一个从 1 到 100 的范围，其中 1 为最差（可能意味着存在中断），100 为完美且很少能达到。从该报告来看，28 日的延迟似乎不尽如人意，但与前一天相比仍然可以接受，而前一天接近完美。由于中部美国区域与测试位置非常接近，我们预期延迟为零或相对延迟极小。接下来，看看从德国的情况如何。

9. 执行以下命令并查看结果：

```
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcherCentralUS `
  -ResourceGroupName CSHARPGUITAR-DM1-RG `
  -Location "Central US" `
  -Country "Germany" `
  -State "bavaria" `
  -StartTime "2019-05-25" `
  -EndTime "2019-05-30"
```

10. 中部美国与德国之间的可达性报告输出结果如预期所示，得分为 93 到 94 分，考虑到位置与国家之间的距离，这仍然是可以接受的。尝试在国内进行以下可达性报告，结果得分会好得多，我执行时得分为 98 和 99：

```
Get-AzNetworkWatcherReachabilityReport `
  -NetworkWatcherName NetworkWatcherCentralUS `
  -ResourceGroupName CSHARPGUITAR-DM1-RG `
  -Location "Germany Central" `
  -Country "Germany" `
  -State "bavaria" `
  -StartTime "2019-05-25" `
  -EndTime "2019-05-30"
```

11. 测试发生在德国中部区域与国家本身之间。如果一家公司开始从德国的客户或员工那里获得大量消耗，那么将该工作负载部署得更靠近他们可能就合理了。

12. 要生成受支持的位置、国家、州和城市列表，请执行此 cmdlet：

```
Get-AzNetworkWatcherReachabilityProvidersList `
  -NetworkWatcherName NetworkWatcherCentralUS `
  -ResourceGroupName CSHARPGUITAR-DM1-RG
```

你可以在 GitHub 上找到本练习使用的名为 NetworkWatcher.ps1 的 PowerShell 脚本。

github.com/benperk/ASA/tree/master/Chapter03/Ch03Ex01

有一些技术可以用来改善延迟，例如 *内容分发网络 (CDN)* 或 *流量管理器*，将在后面讨论。这两者都是在另一个区域部署应用程序并配置随之而来的任何所需复制所需投资的潜在替代方案。但就目前而言，我们就此打住，认识到为你的初始 Azure 工作负载选择区域与你的客户、员工或消费者使用其中功能的位置有关。

在将第一个或下一个资源部署到 Azure 时，最后一个最常见的决策点是确定应用程序是否必须遵守任何法律法规和数据驻留要求。某些配置可能会触发数据在区域之间复制，而某些区域仅允许在该区域开展业务的政府或公司在那里存储数据。此外，还有三个区域完全与其他 Azure 区域隔离。

如果为 blob 或表存储配置了全局冗余存储 (GRS)，则该数据将被复制到另一个区域。你的应用程序存储或捕获的数据是否有任何隐私限制或法律问题？如果你不知道，那么在配置 GRS 之前应弄清楚这一点。你还应该了解所有在配置时出于冗余原因执行此类复制的 Azure 产品。这里要说明的一点是，使得复制稍微更容易接受。记住我将区域比作数据中心，并且一个区域可以有多个数据中心来支持可用性区域。那些数据中心彼此位于同一区域。*地理* 是另一个适用于此的概念，如图 3.6 所示。

![](img/95043b3df926bcdadadb082b49262037_166_0.png)

你可以看到多个区域包含在地理中，并且每个区域包含两个或多个可用性区域。任何为了避免区域范围的灾难性事件而复制到另一个区域的数据仍将在数据驻留阈值内，并且除非配置为这样做，否则不会离开地理。

如果你仔细查看某个国家/地区的区域，你会发现很少只有一个。表 3.3 显示有四个已确定的地理包含至少两个区域。这不是完整列表；每个地理还有更多区域。这里的基本要点是，当为特定区域复制数据以实现冗余时，数据仍保留在地理中，这在大多数情况下解决了任何驻留和冗余问题。

**TABLE 3.3 Azure Geographies and Compliance**

| Geography | Region | Sovereign | Available To |
|---|---|---|---|
| Americas | North Central US<br>South Central US | No | All |
| Europe | Germany Central<br>Germany Northeast | Yes | Germany only |
| Asia Pacific | Australia Central<br>Australia East | No | Australia only |
| Middle East/Africa | UAE Central<br>UAE East | No | All |

请记住，某些 Azure 产品会将数据从一个区域复制到地理之外的另一个区域。例如，LUIS、Azure 机器学习，或经常处于 beta 或预览的产品和功能就是这样处理的。这些产品之所以有意义，是因为生成的数据用于改进产品本身。你作为 Azure 解决方案专家，需要知道这些事情，联系你的法务部门，并提出隐私相关的问题。

最后，在表 3.3 中，注意 Sovereign 和 Available To 列。主权区域意味着它未连接到全球 Microsoft Azure 网络，并且没有任何数据从该区域移动到任何其他区域。此好处主要针对希望对数据及其支持方式施加更高限制和控制的政府组织。目前，美国、德国和中国是唯一拥有主权云 Azure 实施的国家。Available To 列仅意味着当你在门户中时，它知道你的业务位置，因为你在创建 Azure 订阅时提供了该信息。如果你的业务位置不在例如德国或澳大利亚，当你尝试创建 Azure 资源时，这些区域将不会显示为可选择选项。如果你尝试通过 PowerShell、REST API 或任何其他受支持客户端使用这些区域，将会抛出错误。

可以配置订阅范围的 Azure Policy 实例来限制 VNet 和其他资源可以配置的位置。这可以防止意外地将任何资源放置到不符合客户、国家或公司要求的区域。Azure Policy 将在第 6 章中更详细地讨论。此外，第 6 章还讨论了 Microsoft Trust Center，它提供了大量关于合规性和政策制定的信息。如果你急于想了解更多关于这些主题的信息，请跳到前面看一下。如果不是，请继续阅读以了解更多关于一些关键 Azure 虚拟网络功能和能力的信息。

## Key Features and Capabilities

在上一节中，我们简要介绍了资源位置如何影响或限制其包含在 VNet 中的情况。我们还简要介绍了在 Azure 上运行的 VNet 之间的连接，以及这些 VNet 与可能存在于本地的前端或后端资源的连接。在本节中，我将更详细地讨论这一领域，首先介绍一些关键的 VNet 优势，然后以网络中过滤和路由流量的一些内容作为结尾。

本节涵盖以下领域：

- Connecting with other Azure resources
- Connecting with other Azure VNets
- Connecting with the internet
- Connecting with on-premise resources
- Filtering network traffic
- Routing network traffic

Azure 产品和功能这两个术语一直是定义特定时间范围内 Azure 实体范围的常用表达。例如，Azure 产品 SQL Azure、Azure VM 和 Azure App Service 都有可计算的价格。绑定到产品的功能通常不收费。例如 EasyAuth、网络安全组 (NSG) 和网络接口就是这种情况。 going forward，使用术语 Azure 资源涵盖所有 Azure 产品和功能。你可能现在已经能够区分计算价格和不收费的情况了。

### Some Key Azure VNet Features and Capabilities

毫不奇怪，使用 VNet 的主要原因是将一组 Azure 资源隔离到一个分段组中，其中隔离以 VNet 的形式出现，分段则以其中的子网形式出现。当创建 VNet 时，如你在练习 3.2 中将要做的那样，需要提供 VNet 的地址范围。此值的典型起始值为 10.0.0.0/16，它提供的 IP 地址范围从 10.0.0.0 到

