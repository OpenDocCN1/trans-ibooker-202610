

![](img/d31345bbfd15f63be2920f0de5fa30dc_61_0.png)

Figure 3.2 – Network with four nodes

为了消除这种冗余，我们可以引入一个中央设备来负责让不同的设备相互通信。有不同类型的中央设备，我们将在 Components of a basic computer network 部分中全部探讨：

![](img/d31345bbfd15f63be2920f0de5fa30dc_61_1.png)

Figure 3.3 – Network with a central device

上图与我们家庭中的网络相似。一个中央设备——在我们的例子中是路由器——帮助我们与其他设备通信。这是一种非常原始的网络形式；现实生活中的网络比这个复杂得多。回到网络构成的概念，网络仅仅是两个通过介质相互通信并共享数据的设备。现在我们已经了解了网络是什么并开始讨论构建网络所需的内容，让我们详细查看组件。

## Components of a basic computer network

在本节中，我们将学习计算机网络的不同组件。在计算机网络文献中，你经常会看到 *node* 这个术语用来表示网络中的一台计算机。在网络领域中，使用特定的命名法来识别网络中的特定设备。我们接下来将查看这些术语。

### Node

节点通常是连接到 *central* 设备的设备。从某种意义上说，它是参与通信网络的计算机。这对于简单的小型网络是可行的，但随着网络中添加的设备越来越多，不同的设备开始承担不同的角色，因此我们只能在一定程度上将设备的角色在网络中简化为节点。在合格的场景中，节点可以是你的笔记本电脑、台式电脑、打印机、平板电脑、手机或任何其他网络连接设备。

### Server

服务器是保存一些可以通过网络共享给需要它们的设备的信息的计算机。服务器通常 *online*，这意味着它们通过持续可用于其他设备来 *serve* 设备。

### Transmission media

设备在网络中相互连接并能够通信所使用的资源/链路称为 *transmission media*。它可以是有线和无线的。有线传输介质的例子是 *Ethernet cable*，通常用于本地网络。*Wi-Fi* 是无线传输介质的例子。

### Network interface card

要参与网络，连接的节点/设备必须具有称为 **Network Interface Card (NIC)** 的东西。NIC 的作用是将你想要传输的内容转换为传输介质所接受的形式。

### Hub

集线器是网络中的中央设备。如果你想与网络中的一个节点通信，你可能不会直接链接到该节点。相反，你应该通过某个中央设备——在这种情况下是集线器——建立链接。你的消息/数据将发送到集线器，然后集线器会将其广播到整个网络。根据消息的内容，相应的设备会做出回应。

### Switch

交换机是一种特殊的集线器。与广播消息给所有节点的集线器不同，交换机只将消息发送给预期的接收者。这大大减少了网络上的流量，因为不打算接收消息的设备不应接收消息。

### Router

到目前为止，我们一直在谈论单个网络。如果一台计算机想与不在你网络中的计算机交谈怎么办？如果这台计算机在法国，而预期的接收计算机在美国的网络中怎么办？我们可以将计算机互连的概念扩展到网络互连。路由器是帮助我们与外部网络通信的设备。

### Gateway

网关是网络中的端点路由器。所有进出网络的流量都经过它。它充当互联网与本地设备之间的中介。对于我们自己的网络之外的设备来说，网关是本地网络中任何设备的通信主要点。

### Firewall

防火墙是某些网络中的可选设备。防火墙可以是基于软件的，例如操作系统的防火墙，也可以是整个网络的基于硬件的设备。防火墙的作用是增强系统的安全性并监控网络流量。这确保了不会对网络进行未经授权的访问。防火墙通常会阻止所有进入本地网络的连接请求，除非这些请求已被授权并在防火墙的规则引擎中提及：

![](img/d31345bbfd15f63be2920f0de5fa30dc_64_0.png)

Figure 3.4 – Components in a network

顾名思义，它充当本地网络的入口点，也是出口点。出于实际原因和小型网络的原因，网络的小组件，如路由器、交换机、网关，有时甚至是防火墙，都合并到单个物理设备中。

在本节中，我们了解了网络的不同组件以及每个组件的作用。接下来，我们将讨论这些不同网络是如何分类的。

## Classifying network

随着越来越多的计算机开始连接，将它们分类到不同的类别变得至关重要，以便我们可以使用它们。有各种方法可以进行分类；然而，最常见的是基于地理的网络分类。我们接下来将讨论这一点。

### Local area network

当你将笔记本电脑或手机连接到家中 Wi-Fi 路由器时，你本质上参与了一个 **local area network (LAN)**。连接到 LAN 的方式有多种，例如使用 Wi-Fi，这是一种无线连接，或使用有线连接，例如以太网电缆。LAN 的构成没有严格定义。然而，LAN 通常由位于同一建筑物中同一区域的设备组成。LAN 可以简单到两个设备连接到路由器，也可以复杂到大学和办公室的 LAN。

### Ethernet

以太网是 LAN 中使用最广泛的技术之一。现代以太网协议在 LAN 中提供非常高的速度。与无线介质相比，它非常可靠且安全。以太网协议定义了数据如何在 LAN 上传输。现代以太网可以提供每秒吉比特量级的速度。

### Wi-Fi

作为以太网的补充，以太网使用物理电缆将设备连接到网络，而 Wi-Fi 允许设备通过无线介质相互连接。这消除了对电线的需求。需要注意的是，即使是无线，LAN 上设备之间的通信也不是直接的。数据仍然通过一个中央路由器，即 **Access Point (AP)**，转发给预期接收者。

这两种介质的比较如下：

| Feature | Wi-Fi | Ethernet |
|---|---|---|
| Transmission media | Over a wireless channel | Connected through cables |
| Accessibility | Everywhere we can receive signals | Only by cables |
| Speeds | Slow compared to cables | Fastest option |
| Reliability | Depends on the environment | Consistent |
| Security | If unencrypted, data can be intercepted easily over wireless channels | More secure due to the need for physical interception |
| deployment | Easy | Requires network infrastructure such as ports and cables to be setup |
| Latency | High and variable | Low and consistent |

Table 1.1 – Wi-Fi versus ethernet

这两种介质各有利弊。无线对于普通用户来说使用起来容易得多，并给予他们在网络中更大的移动自由，而基于电缆的以太网速度快得多，并且在网络对移动性需求较低时经常使用。现在我们已经了解了 LAN，我们将开始查看其他基于地理的网络。

### Personal area network

与 LAN 相比，个人区域网络 PAN 通常非常小。PAN 的范围仅在几十米量级。PAN 的一个例子是两个基于蓝牙的设备相互通信。在极少数情况下，PAN 也连接到 LAN。

### Metropolitan area networks

有时，我们倾向于将几个小型局域网合并为一个单一类别。它们通常称为 metropolitan area networks (MANs)。MAN 的一个例子是位于城市不同区域的政府办公室连接到单一网络。这些网络通常仅限于一个城市。

