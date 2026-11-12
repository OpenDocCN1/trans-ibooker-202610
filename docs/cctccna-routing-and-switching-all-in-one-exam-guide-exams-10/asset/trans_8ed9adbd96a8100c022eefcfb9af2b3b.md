

路由协议的特性。要进入路由协议的配置模式，请使用以下命令：

```
Router(config)# router name_of_the_IP_routing_protocol
Router(config-router)#
```

**router** 命令用于访问您要配置的路由协议；它不会启用该协议。如果您不确定要启用的路由协议的名称，请使用上下文相关的帮助功能：

```
Router(config)# router ?
  bgp               Border Gateway Protocol (BGP)
  eigrp             Enhanced Interior Gateway Routing Protocol (EIGRP)
  isis              ISO IS-IS
  iso-igrp          IGRP for OSI networks
  mobile            Mobile routes
  odr               On Demand stub Routes
  ospf              Open Shortest Path First (OSPF)
  rip               Routing Information Protocol (RIP)
Router(config)#
```

从上下文相关的帮助输出中可以看出，您可以使用大量的 IP 路由协议。

## network 命令

启用路由协议后，您需要指定哪些接口参与路由过程。默认情况下，没有接口参与路由过程。要指定参与的接口，请使用 **network** 模式命令：

```
Router(config-router)# network IP_network_#
```

一旦输入网络号，路由过程即变为 *活跃*。

## 距离矢量协议

*距离矢量* 路由协议基于最低跳数来衡量要使用的最佳路由。源和目的地之间的每个路由器会使跳数增加 1。使用距离矢量路由协议时，通常选择跳数最低的路由作为数据的目标路径。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_642_0.png)

**考试提示** 请记住，距离矢量路由协议包括 RIP 和 IGRP。

## RIP/RIPv2

RIP 是一种距离矢量协议，负责通过每 30 秒在用户数据报协议 (UDP) 上广播信息，与相邻路由器共享其路由表信息。这种与相邻路由器共享路由表信息的广播或 *宣告*，使网络管理员无需手动添加路由。

RIP 是一种行业标准路由协议，这意味着它受到许多不同供应商的支持，因此您可以将其用作路由协议来在不同制造商的路由器之间共享路由表信息。RIP 会选择跳数最低的路由，但如果两条不同的路由具有相同的跳数，RIP 会在这两条路由之间进行流量负载均衡。请注意，RIP 限制为 15 跳，因此仅用于小型网络。

RIPv1 仅适用于有类地址，因为它不随路由表一起发送子网掩码信息。RIPv2 是对 RIPv1 的更新，并支持无类地址和可变长度子网掩码，因为它随路由表一起发送子网掩码信息。

让我们回到图 11-1 看另一个示例。假设您是路由器 A 的管理员。默认情况下，路由器 A 将拥有到 192.168.1.0 和 192.168.2.0 网络的路由。如果您想在路由器 A 上配置 RIP 来宣告对这两个网络的了解，请在路由器 A 上输入以下命令：

```
ROUTERA> enable
ROUTERA# configure terminal
ROUTERA(config)# router rip
ROUTERA(config-router)# version 2
ROUTERA(config-router)# network 192.168.1.0
ROUTERA(config-router)# network 192.168.2.0
```

一旦 RIP 或任何其他路由协议在路由器 A 和路由器 B 上都已启用，两台路由器将共享它们所知道的任何网络的信息。路由器 A 接收有关 192.168.3.0 网络的信息，并将新网络构建到其路由表中；这称为收敛。图 11-6 显示了在两台路由器上都启用 RIP 后路由器 A 上的路由表。请注意 192.168.3.0 路由旁边的字母 R，表示该路由是通过 RIP 学习的。

```
ROUTERA>show ip route
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
  D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
  N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
  E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
  i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
  * - candidate default, U - per-user static route, o - ODR
  P - periodic downloaded static route

Gateway of last resort is not set

C       192.168.1.0/24 is directly connected, Ethernet0
C       192.168.2.0/24 is directly connected, Serial0
R       192.168.3.0/24 [120/1] via 192.168.2.2, 00:00:14, Serial0
ROUTERA>_
```

图 11-6 路由表显示了通过 RIP 学习的新路由。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_643_0.png)

**视频 11.06。** 随本书附带的数字资源包含有关如何配置 RIP 的多媒体演示。

## IGRP

IGRP 是由 Cisco 构建的一种有类路由协议，因此您仅在仅使用 Cisco 路由器的网络上使用它。IGRP 旨在改进 RIP 的局限性；其最大跳数为 255，并使用 *自治系统* (AS) 的概念。自治系统是共享路由表信息的路由器组。使用 IGRP 的路由器只会与 AS 中的其他路由器共享路由。另一项改进是，完整的路由表每 90 秒宣告一次，而不是像 RIP 那样每 30 秒一次。

要在 Cisco 路由器上启用 IGRP，请输入以下命令：

```
ROUTERA> enable
ROUTERA# configure terminal
ROUTERA(config)# router igrp 10
ROUTERA(config-router)# network 192.168.1.0
ROUTERA(config-router)# network 192.168.2.0
```

在此代码清单中，请注意，在 Cisco 路由器上启用 IGRP 与启用 RIP 类似。然而，区别在于，当您启用 IGRP 时，您要为路由器指定 AS 号。在此示例中，我将 10 用作 AS 号，因此任何其他具有 10 AS 号的路由器都将接收有关 192.168.1.0 和 192.168.2.0 网络的路由信息。

## 链路状态协议

链路状态路由协议比距离矢量路由协议稍微高级一些，因为链路状态路由协议了解整个网络拓扑。链路状态协议负责监控路由器之间链路的状态。然后使用这些链路状态信息来确定到目标网络的最佳路由。尽管像 RIP 这样的协议了解相邻路由器，但链路状态协议了解整个网络拓扑，并将路由表信息多播到整个网络。

链路状态路由协议的好处之一是，如果链路断开，该信息将存储在路由表中，并且不会使用该路径。因为距离矢量路由协议不存储链路状态信息，所以它可能在一段时间内不知道链路不可用，并且可能仍然通过该路径发送流量。

## OSPF

*开放最短路径优先 (OSPF)* 是链路状态协议的一个示例。OSPF 是一种行业标准协议，这意味着它可用于不同制造商构建的路由器，并且您可以使用它在不同的路由器之间共享路由信息。

与 IGRP 类似，OSPF 使用自治系统，但它还具有将 AS 划分为称为 *区域* 的逻辑组的功能。OSPF 支持可变长度子网掩码 (VLSM)，并且跳数无限。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_645_0.png)

**注意** 因为 OSPF 是 CCNA 考试需要知道的重要路由协议，第 12 章全部专门介绍了它。

## IS-IS

*中间系统-中间系统 (IS-IS)* 路由协议作为一种用于网络内部（内部网关）的链路状态路由协议，与 OSPF 竞争。IS-IS 由数字设备公司 (DEC) 开发，已成为一种行业标准协议，尽管它不如 OSPF 流行。与 OSPF 类似，IS-IS 使用链路状态算法来维护网络上所有链路和路由的状态信息，以便每台运行 IS-IS 协议的路由器都了解整个网络拓扑。

## 混合协议

混合路由协议结合了距离矢量和链路状态的特性。Cisco 使用两种混合协议：EIGRP 和 BGP。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_645_1.png)

