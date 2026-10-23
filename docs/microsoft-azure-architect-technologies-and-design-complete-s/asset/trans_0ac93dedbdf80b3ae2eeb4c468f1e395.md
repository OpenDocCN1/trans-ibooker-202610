
强制隧道的工作原理是通过配置用户定义的路由表条目来实现的。回顾一下图3.13。默认情况下，任何与现有路由都不匹配的目标IP地址都会默认使用0.0.0.0/0地址前缀，并被分配一个Internet类型的NextHopType。这意味着即使在配置了S2S连接和VPN网关之后，托管在VNet中的Azure VM发往互联网的流量也会直接进入互联网。要强制该流量通过VPN网关，必须将S2S连接的0.0.0.0/0的-NextHopType设置为VirtualNetworkGateway。

当前没有Azure门户界面可以执行此操作。设置NextHopType值的推荐方法是使用PowerShell。以下PowerShell脚本将强制隧道添加到S2S VPN网关：

```powershell
New-AzRouteTable -Name "CSHARPGUITAR-DB3-ROUTE-TABLE" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG" \
    -Location "North Europe"

$rt = Get-AzRouteTable -Name "CSHARPGUITAR-DB3-ROUTE-TABLE" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG"

Add-AzRouteConfig -Name "ForcedTunnelRoute" \
    -AddressPrefix "0.0.0.0/0" \
    -NextHopType VirtualNetworkGateway \
    -RouteTable $rt

Set-AzRouteTable -RouteTable $rt

$vnet = Get-AzVirtualNetwork -Name "CSHARPGUITAR-VNet-A" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG"

Set-AzVirtualNetworkSubnetConfig -Name "csharp" -VirtualNetwork $vnet \
    -AddressPrefix "10.0.0.0/24" \
    -RouteTable $rt

Set-AzVirtualNetwork -VirtualNetwork $vnet
```

你可以在这里找到这个PowerShell脚本：

github.com/benperk/ASA/blob/master/Chapter03/ForcedTunnelRoute.ps1

无法更改Azure提供的默认路由规则。要实现任何更改，都需要创建新的路由表，并在其中添加、更新或删除路由。这就是前面的PowerShell脚本所做的。一旦创建了名为CSHARPGUITAR-DB3-ROUTE-TABLE的路由表，它就会绑定到csharp子网，该子网位于CSHARPGUITAR-VNET-A虚拟网络中。请记住，Source值等于User的路由在具有相同前缀的默认创建路由中优先级更高。这里就是这种情况，因为在为VirtualNetworkGateway添加前面的路由后，会有两条AddressPrefix值为0.0.0.0/0的路由。

执行以下PowerShell cmdlet以查看添加了实现强制隧道的路由后的有效路由：

```powershell
Get-AzEffectiveRouteTable -NetworkInterfaceName csharpguitar-vm810 `
-ResourceGroupName CSHARPGUITAR-DB3-RG | Format-Table
```

图3.60提供了该cmdlet的输出。

**图3.60 强制隧道路由表示例**

| 名称 | DisableBgpRoutePropagation | 状态 | 来源 | 地址前缀 | 下一跳类型 | 下一跳IP地址 |
|---|---|---|---|---|---|---|
|  | False | 活动 | 默认 | {10.0.0.0/16} | VnetLocal | {} |
| ForcedTunnelRoute | False | 活动 | VirtualNetworkGateway | {10.5.0.0/24} | VirtualNetworkGateway | {40.115.104.24} |
|  | False | 活动 | VirtualNetworkGateway | {0.0.0.0/0} | VirtualNetworkGateway | {} |
|  | False | 无效 | 默认 | {0.0.0.0/0} | Internet | {} |
|  | False | 活动 | 默认 | {10.1.0.0/16} | VnetGlobalPeering | {} |

最后，请注意PowerShell脚本中提供了应用强制隧道的子网。回顾一下，CSHARPGUITAR-VNET-A有两个子网：csharp和guitar。该脚本仅将强制隧道路由应用于csharp子网，因此也应用于该子网中的所有Azure VM。它并未应用于guitar子网。这意味着，尽管VPN网关绑定到CSHARPGUITAR-VNET-A，但路由是应用于子网的，guitar子网中的Azure VM的互联网流量将直接路由到互联网。它们不会通过S2S连接经本地网络进行强制隧道。

## 其他Azure网络产品

本章其余部分介绍一些之前讨论中未涉及的网络产品。这些是类似网络的产品，但在讨论网络时并不总是被提及。此处涵盖的其他网络组件如下：

- Azure应用程序网关（非WAF）
- Azure负载均衡器
- Azure Front Door
- Azure内容分发网络
- 流量管理器
- Azure中继/混合连接管理器

第一个产品是应用程序网关，之前已经讨论过；但只讨论了它的WAF部分。应用程序网关除了WAF之外还提供更多功能。

### 应用程序网关

自20世纪90年代末以来，将Web应用程序运行在某种网络负载均衡设备后面的多台服务器上已成为常态。这种架构配置被称为*Web服务器场*。从冗余的角度来看，让多台服务器运行相同的应用程序是很有意义的。这样就不会有单点故障。自20世纪90年代以来，这些网络负载均衡设备已经变得复杂得多，其功能也已不仅仅是简单地将流量引导到其后面配置的一组服务器的设备。

应用程序网关产品中的Web应用程序防火墙功能是世界级的。它提供的网络功能主要聚焦于OSI第7层，这意味着它可以基于HTTP请求中包含的属性（如请求头值和请求的资源路径）做出路由决策。这提供了超越其他网络设备（仅限于套接字中的内容，即源IP、源端口、目标IP、目标端口）的额外功能。

除了WAF之外，应用程序网关虚拟设备还可以充当负载均衡器、SSL终止点和重定向器，以及许多其他功能。其中许多附加功能将在后面更详细地讨论。如果你还没有完成练习3.10，建议现在完成。以下内容期望你对应用程序网关是什么以及如何配置它有所了解。

### SSL终止

*安全套接字层（SSL）*是使用HTTPS协议加密客户端和服务器之间数据传输的旧名称。如今，*传输层安全性（TLS）*才是正确的术语。它加密方式与SSL相同，但更安全。SSL是最常用的术语；继续使用它可能也没问题，因为人们会知道你在说什么，但SSL的时代已经过去了。客户端和服务器之间的数据加密/解密在资源利用方面确实有一些开销。在两者之间的某个位置，可能有一个设备被配置为终止安全连接，这将消除加密/解密，从而带来更快的事务处理和更少的资源消耗。

这种安全连接的终止称为*SSL终止*或*SSL卸载*，通常发生在网关、智能防火墙或前端服务器等边缘设备上。所谓边缘设备，我指的是位于网络入口点的网络设备；传输进入网络后，就不再需要加密了。假设网络是安全的，因此基础设施不需要承担在安全隧道内部通信时SSL解密所带来的额外性能损失和开销。除了性能提升之外，安全连接的终止和请求内容的解密还提供了更智能地路由请求的机会。诸如请求头、URL、Cookie等属性可用于路由规则和逻辑。

应用程序网关同时提供SSL终止和端到端SSL加密的能力。一些客户或应用程序可能要求端到端加密。具有高度敏感范围或传输高度敏感数据的应用程序，在合规性要求下可能需要这种加密类型。应用程序网关真正令人称奇的一点是，即使采用端到端加密，仍然可以访问请求属性。应用程序网关在请求到达时仍然会终止SSL会话；但在将出站流量发送到后端池之前，会再次对其进行加密。回想练习3.10中你创建了一个后端池（例如CSHARPGUITAR-WAF-BACKEND-POOL）并向其中添加了一个Azure VM（例如CSHARPGUITAR-V2）。在练习3.13中，使用之前在练习3.10中创建的应用程序网关（例如CSHARPGUITAR-SN1-AGWAF）配置SSL终止。

### 练习3.13：在应用程序网关中配置SSL终止

1. 登录Azure门户（portal.azure.com）。
2. 导航到现有的应用程序网关（例如CSHARPGUITAR-SN1-AGWAF），从导航菜单中选择“侦听器”，单击“+基础”，这将显示“添加基础侦听器”边栏选项卡。
3. 输入名称（例如CSHARPGUITAR-LISTENER-HTTPS），为前端端口名称输入port_443，输入443作为端口，并选择HTTPS协议。
4. 上传从github.com/benperk/ASA/tree/master/Chapter03/Ch03Ex12下载的csharpguiternet.pfx证书。或者使用New-SelfSignedCertificate PowerShell cmdlet创建自己的证书。
5. 提供名称（例如CSHARPGUITARNET），输入密码Csh@rPguitarN3t，然后点击OK按钮。该表单应类似于图3.61。
6. 接下来添加HTTP设置。从导航菜单中点击“HTTP设置”。点击“+添加”。提供一个名称（例如CSHARPGUITAR-HTTPS-SETTING）。选择HTTPS作为协议。提供证书名称（例如CSHARPGUITAR-HTTPS-CER）。选择CER证书并从github.com/benperk/ASA/tree/master/Chapter03/Ch03Ex12下载测试CER。其余保持默认，然后点击OK按钮。
