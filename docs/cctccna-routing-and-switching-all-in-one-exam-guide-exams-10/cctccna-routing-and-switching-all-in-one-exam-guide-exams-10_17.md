# 术语表

**100BaseT** 一种使用 CAT 5 双绞线以 100 Mbps 速率传输的以太网架构，也称为快速以太网。每个段的最大距离为 100 米。

**1000BaseT** 一种使用 CAT 5e 双绞线以 1000 Mbps 速率传输的以太网架构，也称为千兆以太网。每个段的最大距离为 100 米。

**802.1Q** 这种 IEEE 干线标准支持两种类型的帧：带标记帧和不带标记帧。不带标记帧不携带任何 VLAN 识别信息——基本上，这是一个标准以太网帧。802.1Q 标记会修改原始以太网帧。一个名为标记字段的 4 字节字段被插入到原始以太网帧头的中间，并且原始帧的 FCS（校验和）会根据此更改重新计算。执行标记是为了帮助其他连接的交换机将帧保留在源 VLAN 中。

**802.2** IEEE 已将 OSI 参考模型的数据链路层分为两个组件：介质访问控制 (MAC) 和逻辑链路控制 (LLC)。LLC 在软件中执行，在 802.2 中定义，负责识别封装的上层协议。LLC 有两种实现：服务访问点 (SAP) 和子网络访问协议 (SNAP)。MAC 在硬件中执行。不同类型 MAC 的示例包括 802.3 以太网和 802.5 令牌环网。

**802.3** 此 IEEE 标准定义了以太网的实现方式。它负责定义用于在两个网络接口卡 (NIC) 之间传输信息的帧格式。帧标准化了帧中的字段及其长度，以便每个设备都了解如何读取帧的内容。802.3 具有长度字段，而 Ethernet II 具有类型字段。802.2 LLC 帧在从以太网接口发送之前会被封装在 802.3 帧中。

**AAA (认证、授权和计费)** 一种身份验证服务，使用户能够登录，控制谁可以登录，并跟踪系统的使用情况。

**访问控制列表 (ACL)** 以其在流量进入或离开接口时过滤流量的能力而闻名，ACL 还可用于其他目的，包括：限制对路由器的 Telnet（虚拟类型终端 [VTY]）访问、过滤路由信息、通过排队优先处理广域网流量、使用按需拨号路由 (DDR) 触发电话呼叫，以及更改路由的管理距离。ACL 是一组语句。Cisco IOS 自上而下处理 ACL：一旦数据包的内容匹配条件，就会执行操作（允许或拒绝）。如果未找到匹配项，则会应用隐式拒绝规则。

**接入层** Cisco 三层分层模型的底层。接入层位于校园网络的边缘，通过分发层与核心层分隔。接入层的主要功能是为用户提供到网络的初始连接。通常，此连接由交换机提供。

**访问链路连接** 到设备的此连接具有标准以太网 NIC，仅能理解标准以太网帧——换句话说，是能够理解 IEEE 802.3 和/或 Ethernet II 帧的普通 NIC。访问链路连接只能与一个 VLAN 关联。它有时也称为访问端口。

**访问方法** 这些是管理各种设备使用物理网络的规则。访问方法决定了数据如何在线路上提交。访问方法的两个示例是令牌传递和带有碰撞检测的载波侦听多路访问 (CSMA/CD)。

**接入点** 又称无线接入点；无线客户端通过接入点连接到有线网络。

**访问速率** 您的路由器与中继交换机（例如 T1）之间物理连接的速度。

**帐户** 网络上存储有关网络访问信息的位置。它包含使用户能够使用网络的信息，包括用户名和登录规范、密码以及对目录和资源的权限。

**帐户锁定** 一项功能，在一定数量的不成功登录尝试后锁定帐户（三次错误尝试是常见选择）。通常，锁定的帐户在管理员解锁前无法再使用。这有助于防止黑客入侵帐户。

**确认 (ACK)** 从接收计算机发送到发送计算机的信息包，用于验证传输是否已接收。传输不成功会生成否定确认 (NACK)。

**适配器** 插入系统以提供功能的卡，例如网络适配器，它提供网络连接。

**地址重载** 请参见端口地址转换 (PAT)。

**地址解析协议 (ARP)** 一种 Internet 层协议，帮助 TCP/IP 设备查找同一广播域中的其他设备。ARP 使用本地广播来发现相邻设备。基本上，ARP 将目标的第 3 层 IP 地址解析为目标的第 2 层 MAC 地址。

**管理距离** 一种 Cisco 专有机制，用于对 IP 路由协议进行排序。如果路由器正在从两个不同的路由协议（例如开放最短路径优先 (OSPF) 和增强内部网关路由协议 (EIGRP)）中学习同一条路由，则将其用作平局决胜器。

**备用端口** 在快速生成树协议 (RSTP) 中是新增的功能，备用端口拥有通往根桥的备用路径或多条备用路径，但目前处于


discarding state. When the root port fails, the switch can speed up convergence by using the alternate port.

### American Standard Code for Information Interchange (ASCII)
A representation of standard alphabetic and other keyboard characters in a computer-readable, binary format.

### analog modem
A device used to connect to a remote network via a standard public switched telephone network (PSTN) line. Although there are many different types and makes of modems, they can be categorized into three groups: single external, single internal, and multiline rack mounted.

### anycast address
This IPv6 address identifies one or more interfaces (not devices), sometimes called nodes. An anycast is a hybrid of a unicast address and a multicast address. With a unicast, one packet is sent to one destination; with a multicast, one packet is sent to all members of the multicast group; and with an anycast, a packet is sent to any one member of a group of devices that are configured with the anycast address. By default, packets sent to an anycast address are forwarded to the closest interface (node), which is based on the routing process employed to get the packet to the destination. Given this process, anycast addresses are commonly referred to as one-to-the-nearest addresses.

### application layer
The seventh layer, or topmost layer, of the OSI Reference Model, the application layer provides the interface that a person uses to interact with the application.

### application-specific integrated circuit (ASIC)
These specialized processors can do very few tasks but do them extremely well. Processors, on the other hand, can perform many tasks but are not necessarily optimized for these tasks. Many types of networking hardware, including switches, use ASICs.

### asynchronous communication
Communication that does not require both the sender and the receiver to be online and participating in the communication at the same time.

### Asynchronous Transfer Mode (ATM)
A packet-switching network technology for LANs and WANs that can handle voice, video, and data transmissions simultaneously.

### authentication
The process of a user proving her identity to a system so that she can then access a system and its resources. Users are typically authenticated to the network after supplying correct username and password information.

### authorization
The process by which the system grants a user access to a resource such as a file or printer. Authorization follows authentication.

### Automatic Private IP Address (APIPA)
This IP address has the first two octets of 169.254.x.y and is automatically assigned by the system when a Dynamic Host Configuration Protocol (DHCP) server cannot be found on the network.

### autonomous system (AS)
A group of networks under a single administrative control, which could be your company’s network, a division within your company, or a group of companies’ networks.

### AutoSecure
An IOS feature on newer model routers, such as the Cisco ISR series, that enables you to add a basic security configuration on your router. It is a privileged EXEC script similar to the System Configuration Dialog: where the latter creates a basic configuration for a router, AutoSecure focuses only on security functions for securing a router.

### backbone
The main cable that interconnects networks.

### backdoor
A method a hacker uses to gain access to an unauthorized system. This backdoor may be available because of a vulnerability in the system or as a result of a previous attack.

### backup
The process of saving files to a separate location, usually an offline storage location, such as a different system or the cloud.

### backup port
Used in RSTP, a backup port on a segment could be used to leave a segment, even though there is already an active designated port for the segment. When a designated port fails, you can use a switch with a backup port to speed up convergence.

### bandwidth
The amount of data that the media can transfer. Bandwidth is usually measured in bits per second (bps).

### bandwidth domain
All of the devices on the same layer 2 physical segment are said to be in the same bandwidth domain. The more devices you have on a physical segment, the less bandwidth each device has. You can use routers or switches to create separate bandwidth domains.

### baseband transmission
A technique used to transmit encoded signals over cable using digital signaling at the same frequency. *See also* broadband transmission.

### Basic Service Set Identifier (BSSID)
This is the wireless network name—commonly known as the SSID. When the network is made up of one access point, it is referred to as the BSSID, but if multiple access points make up the wireless network, it is known as an Extended Service Set Identifier (ESSID).

### binary representation
Any value represented with just 0s and 1s.

### bits
A basic binary representation of information as a single protocol data unit (PDU). Two bit values—on (1) and off (0)—are used by computers to encode information. Bits are physical layer (layer 1) PDUs.

### blocking state
When STP is enabled, ports go into a blocking state under one of three conditions: when a root switch is elected, when a switch receives a Bridge Protocol Data Units (BPDU) on a port that indicates a better path to the root than the port the switch is currently using, and when a port is not a root port or a designated port. A port in a blocking state will remain there for 20 seconds by default. During this state, the only thing the port is doing is listening to and processing BPDUs on its interfaces.

### bootstrap program
This program brings the router or switch up and determines how IOS image and configuration files will be found and loaded, based on the configuration register and/or the existence of any boot commands in the configuration file.

### BPDU Guard
A feature used on ports configured with the PortFast feature. If a PortFast port receives a BPDU, the switch’s BPDU Guard feature immediately disables the port. Remember that PortFast is used on non-switch ports to keep them in a forwarding state; the assumption is that a PortFast port is not connected to a switch and therefore shouldn’t be receiving BPDUs.

### bridge
A device that solves the layer 2 bandwidth and collision problem by performing a switching function in software. A bridge supports only half-duplex connections. It typically supports 2–16 ports and performs store-and-forward switching.

### bridge (or switch) ID
Each layer 2 device running STP is assigned a unique identifier, which is also used in the BPDUs that the layer 2 devices advertise. The bridge ID has two components: the bridge’s or switch’s priority (2 bytes) and the bridge’s or switch’s MAC address (6 bytes).

### Bridge Protocol Data Unit (BPDU)
For STP to function, the switches need to share information with each other. BPDUs are sent out as multicasts every 2 seconds by default, and only other layer 2 devices are listening to this information. Switches use BPDUs to learn the topology of the network, including loops.

### broadband transmission
A technique used to transmit encoded signals over differing frequencies using analog signaling. *See also* baseband transmission.

### broadcast
A PDU sent to all devices. The destination MAC address denotes all devices on a segment (FFFF.FFFF.FFFF). A destination IP address of 255.255.255.255 represents all devices.

### broadcast domain
A group of systems that can receive one another’s broadcast messages.

### broadcast storm
An issue that occurs when there are so many broadcast packets on the network that the capacity of the network bandwidth approaches or reaches saturation.

### bus topology
A network topology that connects all computers to a single shared cable. In a bus topology, if one computer fails, the network fails. *See also star topology.*

### cable modem
A network device that connects to a network or the Internet using standard TV coaxial cable.

### Carrier Sense Multiple Access/Collision Detection (CSMA/CD)
A media access control method used in an Ethernet environment, where only one NIC can successfully send a frame at a time but all NICs can simultaneously listen to information on the wire. Before an Ethernet NIC puts a frame on the wire, it will first sense the wire to ensure that no other frame is currently on it. The NIC must go through this sensing process because the Ethernet medium supports multiple access—another NIC may already have a frame on the wire. If the NIC doesn’t sense a frame on the wire, it will transmit its own frame; if it detects a frame on the wire, the NIC will wait for the completion of the transmission for the frame on the wire and then transmit its own frame. If two or more machines simultaneously sense the wire and see no frame, and both place their frames on the wire, a collision will occur. When a collision occurs, the systems will detect the collision and resend their data at varying intervals.

### Challenge Handshake Authentication Protocol (CHAP)
An identity verification protocol that uses a three-way handshake process to perform authentication for a Point-to-Point Protocol (PPP) connection. First, the source sends its username (not its password) to the destination. The destination sends back a challenge, which is a random value generated by the destination. Both sides then run the source’s username, the matching password, and the challenge through the MD5 hashing function. The source will then send the result of this function to the destination. The destination compares this value to the hashed output that it generated—if the two values match, the password used by the source must be the same password used by the destination, and thus the destination will permit the connection.



## 信道服务单元/数据服务单元 (CSU/DSU)

一种位于网络与数字电话线之间的硬件设备，用于在两种格式之间转换数据。CSU/DSU 最常用于将网络路由器连接到 T1 或其他数字电话线。

## 电路交换连接

包括调制解调器和数字综合业务数字网 (ISDN) 连接在内的拨号连接。

## 类全路由协议

仅理解类子网的路由协议。路由信息协议第 1 版 (RIPv1) 和 IGRP 是示例。类全协议在路由更新中不发送子网掩码信息。RIP 和 IGRP 可以使用非默认子网掩码，但所使用的子网掩码必须对类地址的所有子网相同。

## 无类别域间路由 (CIDR)

VLSM 和路由汇总的扩展。使用 VLSM，您可以将子网汇总回 A 类、B 类或 C 类网络边界。CIDR 进一步推进了这一过程，使您能够汇总一组连续的 A 类、B 类和 C 类网络。这通常称为超网，由当今的无类别协议支持。

## 无类别路由协议

这些协议接受子网掩码为任何位值的路由更新时没有任何问题，并允许非标准子网掩码，例如默认路由。类全路由也可以接受默认路由，但需要配置 **ip 类无类** 命令。然而，这会覆盖类全协议的机制。无类别路由协议包括 RIPv2、EIGRP、OSPF、IS-IS 和 BGP。

## 同轴电缆（coax 电缆）

OSI 物理层可使用的三种物理介质之一。在同轴电缆中，一根芯线（实心导线）沿电缆中央延伸，周围包有绝缘层。常用的以太网同轴电缆有两种不同类型：粗缆和细缆。*另请参见* 双绞线电缆和光纤电缆。

## 冲突域

一组分段的联网系统，其中同时的数据传输可能相互冲突。

## 命令行

计算机应用程序的字符模式接口，依赖命令而非图形界面来处理信息。

## 突发提交速率 (B_C)

供应商为 Frame Relay VC 保证的平均数据速率（在比下文定义的 CIR 更短的固定时间段内测量）；换句话说，该数据速率针对更短的时间段，但产生的平均数据速率高于 CIR。这允许数据流中出现小突发。

## 提交信息速率 (CIR)

运营商在固定时间段内为 Frame Relay VC 保证的平均数据速率。

## 公共生成树 (CST)

运行于整个交换网络（所有 VLAN）的单个生成树协议 (STP) 实例。

## 计算机病毒

旨在破坏或摧毁计算机或网络的计算机程序。

## 配置寄存器

引导程序使用的四位十六进制值，用于确定应从何处加载 IOS 映像和配置文件。IOS 设备启动后，您可以使用 **显示版本** 命令查看配置寄存器值。

## 直连路由

路由器用于连接到子网或网络的接口路由。路由器会查看其活动接口，检查接口上配置的地址并确定相应的网络号，然后使用路由和接口填充路由表。这些接口路由称为直连路由，因为路由器与子网或网络直接相连。

## 面向连接的通信

一种确保数据从发送方可靠传送到接收方且双方无需干预的通信形式。源到目的地的各个点提供纠错和流量控制。

## 无连接通信

一种在接收到信息时目标计算机不通知源的通信形式。由于没有通知来保证交付，这可能不可靠。无连接通信可能比面向连接的通信更快，因为初始会话设置中没有开销（如面向连接的通信），也没有通知发送方信息已接收的开销。

## 内容可寻址内存 (CAM) 表

旧桥接术语，也称为端口或 MAC 地址表，描述了保存设备 MAC 地址及其连接端口的表。第 2 层设备使用此表做出交换决策。

## 核心层

网络的骨干，在不同的分发层设备之间提供非常高速的连接。由于需要高速连接，核心由高速交换机组成，通常不会执行任何类型的数据包或帧操作，例如过滤或服务质量。由于核心使用交换机，因此核心被称为第 2 层核心。穿过核心的流量通常旨在访问企业资源，例如 Internet、网关、电子邮件服务器和企业应用程序。

## 计数至无穷大

路由环路的一个问题。当发生路由环路且一个或多个数据包陷入环路时，它们会在环路中持续循环，浪费分段上的带宽和处理数据包的路由器上的 CPU 周期。为防止数据包在环路中永久循环，距离向量协议通常会限制数据包合法传输的最大跳数。

## 交叉线缆

一种以太网电缆，它交叉连接两组导线：一侧的引脚 1 连接到另一侧的引脚 3，引脚 2 连接到引脚 6。将数据终端设备 (DTE) 连接到另一台 DTE 或将数据通信设备 (DCE) 连接到另一台 DCE 时，应使用交叉线缆。使用交叉线缆将集线器连接到另一台集线器、交换机连接到另一台交换机、集线器连接到交换机，或者 PC、路由器或文件服务器连接到另一台 PC、路由器或文件服务器。

## 客户场所设备 (CPE)

您网络中的设备，包括 DCE（调制解调器、NT1、CSU/DSU）和 DTE（路由器、接入服务器）。这些设备位于现场并连接到运营商的 WAN。

## 直通交换

交换机在做出交换决策前只读取帧的开头部分的过程。一旦交换设备读取目标 MAC 地址，它就开始转发帧（即使帧仍可能正在进入接口）。

## 数据通信设备 (DCE)

终止物理连接并提供两个站点之间连接的时钟和同步的设备。它连接到 DTE 设备。DCE 包括 CSU/DSU、NT1 和调制解调器等设备。

## 数据链路层

OSI 参考模型中的第二层，数据链路层提供物理或硬件地址，通常称为介质访问控制 (MAC) 地址。数据链路层还通过定义介质的帧类型来定义网络设备如何访问其连接的介质。这包括数据链路层（或第 2 层）帧的字段和组件。

## 数据终端设备 (DTE)

终端用户设备，例如路由器或 PC，通过 DCE 连接到 WAN。

## 数据报

另请参见数据包。

## 默认网关

知道如何将本地广播域的流量发送到远程目的地的路由器。如果分段上的设备想要访问不同广播域（即不同网络）上的设备，则必须知道将流量转发到哪个默认网关。

## 默认路由

一种特殊的静态路由。静态路由指定路由器应使用的到达特定目的地的路径，而默认路由指定当路由器不知道如何到达目的地时应使用的路径。

## 分界点

WAN 运营商责任转移给您的点；它可能位于您的本地设施内部或外部。请注意，这是逻辑边界，不一定是物理边界。




### 非军事区 (DMZ)

介于 Internet 与专用局域网之间的区域，可用于放置可供 Internet 访问的服务器。Web 服务器、FTP 服务器、电子邮件服务器和 DNS 服务器等通常放置在 DMZ 中。

### 拒绝服务 (DoS)

一种会导致系统崩溃或无响应的攻击形式。

### 指定端口

在 STP 中，每个网段在单个二层设备上只能有一个处于转发状态的端口，称为指定端口。累积路径开销最优的二层设备会将其连接到该网段的端口用作指定端口。

### 指定路由器 (DR)

OSPF 路由器不会与任意路由器形成邻接关系。相反，OSPF 实现了一种客户端/服务器设计。对于每个网络多点接入网段，都有一个 DR 和一个备份指定路由器 (BDR) 以及其他路由器。例如，如果您在交换区域中有十个 VLAN，则会有十个 DR 和十个 BDR。网段没有这两台路由器的唯一例外情况是 WAN 点对点链路。当 OSPF 路由器启动时，它会与其连接的每个多点接入网段上的 DR 和 BDR 形成邻接关系。路由信息的任何交换都在这些 DR/BDR 路由器与网段上的其他 OSPF 邻居之间进行（反之亦然）。OSPF 路由器使用 IP 组播地址 224.0.0.6 与 DR 通信。DR 和 BDR 使用 224.0.0.5 组播 IP 地址与所有路由器通信。

### DHCP 中继

在较大的网络中，需要通过 DHCP 获取地址的网络设备与 DHCP 服务器通常不在同一子网（广播域）中。解决此问题的方法是使用名为 DHCP 中继的 IOS 功能，其中 IOS 路由设备充当中继代理来处理 DHCP REQUEST 消息。当路由器接收到来自客户端的广播地址请求时，它会将其更改为单播消息，路由器作为源，DHCP 服务器作为目标。代理包含网络编号，以帮助 DHCP 服务器选择正确的寻址池。代理与 DHCP 服务器之间的所有消息均使用单播传输。客户端认为它正在与 DHCP 服务器本地通信。

### 指向广播地址

如果 IP 网络号中的所有主机位都设置为 1（即该地址是最后一个地址），则该地址为指向广播地址。该地址表示网段上的所有主机，并可由路由器路由。

### 丢弃优先级 (DE)

DE 位用于将中继帧标记为低优先级帧。您可以手动完成此操作，或者运营商会针对不符合流量合同（超过 CIR/Bc 值）的帧执行此操作。

### 距离矢量路由协议

这些协议使用距离（度量）和方向（矢量）来查找通往目标的路径。有时这被称为“路由谣言”，因为路由器通过直接连接的邻居的广播来学习路由信息，而这些邻居可能已从其他相邻路由器了解到这些网络。IP 距离矢量路由协议的示例包括 RIPv1 和 IGRP。

### 分发层

与核心层和接入层不同，分发层执行大多数连接任务。通常在分发层使用路由器将接入层连接到核心层。该层的职责包括：包含广播、保护流量、通过三层逻辑寻址和路由汇总提供层次结构，以及在介质类型之间进行转换。

### 域名系统 (DNS)

一个将 Internet 域名映射到 IP 地址的数据库。DNS 是一种 TCP/IP 应用程序，FTP、telnet、Web 浏览器和电子邮件等其他应用程序使用它来解析用户输入的名称以获取实际 IP 地址。

### 点分十进制

一种将 IP 地址转换为人类易读格式的表示方式。IPv4 地址长度为 32 位。为了使地址可读，它们被拆分为 4 个字节（称为八位组），每个字节之间用句点（十进制点）分隔。为了便于人类理解，这四组二进制数随后被转换为十进制数。

### 双栈

诸如 PC 和路由器之类的设备同时运行 IPv4 和 IPv6，因此拥有两组地址，这称为双栈。

### 双工

发送和接收帧的方法。在*半双工*配置下，接口只能发送或接收帧——无法同时进行两者操作。半双工连接用于共享环境，例如网络中使用集线器的情况。在*全双工*配置下，接口可以同时发送和接收。当设备连接到交换机时，使用全双工连接。

### 动态主机配置协议 (DHCP)

该协议使设备能够动态获取其 IP 寻址信息。它基于客户端/服务器模型构建，并定义了两个组件：服务器（提供主机配置信息）和客户端（请求并获取主机配置信息）。

### 动态路由协议

这些协议会宣传其熟悉的路由，并传递度量值、其他路由器的数量，或者从其主机到达另一网络所需的跳数（直接或通过另一台路由器间接到达）。

### 动态中继协议 (DTP)

一种用于在两台 Cisco 交换机之间动态形成和验证中继连接的协议。DTP 是 Cisco 专有协议，并同时支持 802.1Q 和 ISL 中继。

### 简易 VPN

Cisco 的一项设计功能，可简化 VPN 部署，使其能够扩展到大量用户，并集中管理远程访问的策略配置。Easy VPN 服务器集中管理 Easy VPN 远程设备的策略配置，并提供对企业资源的访问。Easy VPN 远程设备使用户或用户组能够通过 Easy VPN 服务器安全地访问企业资源。Easy VPN 远程设备上只需很少配置即可建立隧道——这也是使用“easy”一词来描述此解决方案的另一个原因。

### 加密

一种算法，通过故意打乱构成项目的元素来隐藏消息、其他文件或通信的内容。项目必须先解密为其原始形式，然后才能读取。

### 增强型 IGRP (EIGRP)

一种基于 IGRP 的 Cisco 专有路由协议，包含许多内置增强功能。由于其源于 IGRP，因此配置相似；但是，它具有许多附加的链路状态特征，使 EIGRP 能够扩展到企业网络规模。这些特征包括快速收敛、无环拓扑、可变长度子网掩码 (VLSM) 和路由汇总、组播及增量更新，以及对多种路由协议（IP、Internetwork Packet Exchange [IPX] 和 AppleTalk）的路由支持。EIGRP 是一种混合协议。

### EtherChannel

一种二层解决方案，可用于汇总直接连接设备之间的多个二层以太网连接。基本上，EtherChannel 将设备之间的多个以太网端口捆绑在一起，提供看似单个逻辑接口的功能。STP 将 EtherChannel 视为连接设备之间的单个逻辑连接，这意味着您可以在创建的信道中同时使用所有单独连接。

### 以太网

一种在数据链路层运行的 LAN 媒体类型。以太网使用载体侦听多路访问/碰撞检测 (CSMA/CD) 机制在共享环境中发送信息。以太网最初开发的构想是许多设备将连接到同一根物理布线上。

### 扩展访问控制列表

一种可以匹配以下所有信息的 ACL：源 IP 地址和目标 IP 地址、IP 协议（IP、TCP、UDP、ICMP 等）以及协议信息，例如 TCP 和 UDP 的端口号或 ICMP 的消息类型。

### 扩展服务集标识符 (ESSID)

多个接入点用于创建具有相同名称的单个无线网络。在这种情况下，SSID 称为 ESSID。

### 外部网关协议 (EGP)

一种处理不同自治系统之间路由的协议。当前，只有一种活跃的 EGP：边界网关协议 (BGP)。BGP 用于在不同自治系统之间路由 Internet 骨干上的流量。




**外部网** 扩展的内联网，其中某些内部服务提供给远程位置的已知外部用户或业务合作伙伴。这些外部用户和内部服务使用的连接通常通过防火墙和 VPN 进行保护。

**光纤电缆** 光纤电缆是一种使用玻璃芯传输光脉冲的网络布线形式。光纤布线可达到超过 10 KM 的距离。

**文件传输协议 (FTP)** 一种主要用于跨网络传输数据的协议。FTP 既指协议，也指用于此目的的实用程序。它的设计目的是在不影响远程主机资源的情况下，快速高效地将数据文件从一台主机传输到另一台主机。

**防火墙** 软件或硬件，通过检查在两个网络上传输的数据包来防止两个网络之间的未经授权的流量。防火墙查看数据包中的地址和端口信息，然后确定是否允许该类型流量。

**转发状态** 在 STP 中，前延时计时器过期后处于学习状态的端口将被置于转发状态。在转发状态下，端口将处理 BPDU，使用其接收的帧更新其内容可寻址存储器 (CAM) 表，并通过端口转发用户流量。只有根端口和指定端口最终会进入转发状态。

**无碎片交换** 直通交换的一种修改形式。直通交换在做出交换决策前读取帧中直到目标 MAC 地址字段的内容，而无碎片交换确保帧长度至少为 64 字节。

**帧** 数据链路层使用的 PDU。在 IEEE 中，使用两个 PDU：一个用于 LLC (802.2)，一个用于 MAC (802.2 或 802.5)。

**全双工** 一种使数据能够同时双向流动的通信类型。

**完全限定域名 (FQDN)** 一种用于引用系统或设备的互联网样式名称，例如 *www.gleneclarke.com*。

**网关负载平衡协议 (GLBP)** 一种思科专有协议，类似于思科的热备用路由器协议 (HSRP)。HSRP 和虚拟路由冗余协议 (VRRP) 的局限性之一是，HSRP 组中只有一台路由器处于活动状态并可以为组转发流量，其余路由器处于空闲状态。这是一个低效的过程，因为一台或多台路由设备不处理任何流量，并且没有利用这些其他路由设备所连接的连接的带宽。思科设计 GLBP 来解决这个问题。GLBP 使一组虚拟地址能够动态分配给终端设备。使用 GLBP 时，组中最多四台路由设备可以参与流量转发。此外，如果 GLBP 路由设备发生故障，故障检测会自动发生，另一台 GLBP 路由设备将接管失败路由设备的数据包转发。

**通用路由封装 (GRE) 隧道** 这种点对点隧道是两台路由器之间的逻辑连接，用于承载所有类型的 IP 和非 IP 流量。GRE 隧道通常与 IPsec 站点到站点 VPN 解决方案一起使用，以提供可扩展性和灵活性。GRE 最初由思科开发，随后在 RFC 1701 和 1702 中标准化，它在专用或公共网络中为两台路由器之间提供逻辑连接（子网）。为在设备之间提供逻辑连接，思科 IOS 使用逻辑隧道接口。当进行“隧道”处理数据包时，会向原始数据（数据包或帧）添加信息，以指示正在使用 GRE 以及要将其转发到的目标 GRE 设备。思科支持封装第 3 层数据包（如 IPv4 或 IPv6、IPX 等）或第 2 层帧（桥接流量）。

**千兆网络** 以 1000 Mbps 或每秒 1 千兆比特 (Gbps) 运行的网络。

**gratuitous ARP** 在没有相应 ARP 请求的情况下生成的 ARP 响应。当设备可能更改其 IP 地址或 MAC 地址，并希望通知段上的所有其他设备有关更改，以便其他设备在其本地 ARP 表中拥有正确信息时，这很常见。

**半双工** OSI 会话层使用，用于使数据能够双向流动，但每次只能向一个方向流动。使用半双工对话时，可以进行回复和确认。

**握手** 面向连接的服务必须确保数据可靠地跨网络发送。握手发生在通信会话开始时，此时两台计算机确定通信规则，例如传输速度和要使用的端口。握手还确定完成时正确终止会话的方式，以确保通信以有序的方式结束。

**硬件地址** 参见介质访问控制 (MAC) 地址。

**分层寻址** 用于构建网络，以便可以汇总路由信息。IP 地址的布局方式是，随着在层次结构中逐层向上，路由可以汇总为较小的一组路由。

**高可用性 (HA)** 指可以承受组件故障的环境，因为在任何组件故障时都有可用的冗余组件。

**高级别数据链路控制 (HDLC)** 该协议基于 ISO 标准。它可用于同步和异步连接，并定义数据链路层中两台设备之间的帧类型和交互。思科对 HDLC 的实现基于 ISO 标准，但思科更改了帧格式，使其成为专有格式。换句话说，只有远端也支持思科协议，思科 HDLC 才能工作。HDLC 是思科路由器同步串行接口上的默认封装。

**主机** 参与网络的系统或设备。

**主机 ID** 32 位地址中用于标识 TCP/IP 网络上设备的部分。

**HMAC 函数** 哈希消息身份验证码函数通常用于验证数据包来自可信源且未被篡改。源将要发送的数据包的信息与对称密钥一起通过 HMAC 函数运行，生成数字签名。然后将签名添加到原始数据包并发送到目标。目标重复该过程：它将原始数据包输入与相同的对称密钥一起考虑，并应能生成数据包中发送的相同签名。如果生成的签名相同，则表示数据包来自知道对称密钥的人且未被篡改；如果计算出的签名不同，则丢弃数据包，因为其中的签名是伪造的，或者数据包在源和目标之间被篡改了。

**阻塞定时器** 这种机制用于为运行距离矢量协议的路由器提供足够的时间来传播毒化路由，并确保在传播过程中不发生路由循环。在此期间，路由器会在其路由表中冻结毒化路由，持续时间为阻塞定时器的周期，该周期通常是路由广播更新间隔的三倍。

**热备用路由器协议 (HSRP)** 这种思科专有协议在终端设备上提供单个默认网关定义和第 3 层冗余。使用 HSRP 时，一组路由设备代表一个具有虚拟 IP 地址和虚拟 MAC 地址的单个虚拟默认网关。如果主路由设备发生故障，HSRP 组中的另一台路由设备将接管并处理终端设备发送到虚拟 MAC 地址的帧。HSRP 组的一个优势是，不同子网 (VLAN) 可以有不同的默认网关，从而提供负载平衡。此外，在每个 HSRP 组中，都有一个主默认网关，并具有使用多台路由器执行备份功能的能力。

**集线器** 一种物理层设备，为以太网提供逻辑总线结构。集线器会将物理层信号从一个接口复制到其所有其他接口。




### hybrid routing protocol
一种将距离矢量路由协议和链路状态路由协议的优势合并到新协议中的协议。通常，混合路由协议基于距离矢量协议，但包含许多链路状态协议的特性和优势。混合路由协议的示例包括 RIPv2 和 EIGRP。

### Hypertext Transfer Protocol (HTTP)
用于在 Internet 或 intranet 上通过 Web 浏览器将网页从 Web 服务器传送到客户端的不安全协议。

### Hypertext Transfer Protocol Secure (HTTPS)
HTTP 的安全版本，使用 Web 浏览器以加密格式向客户端传送网页。

### implicit deny
在 ACL 中，如果路由器将数据包与列表中的每条语句进行比较，却未在数据包内容中找到匹配项，则路由器将基于每个 ACL 末尾不可见的隐式拒绝语句丢弃该数据包。

### inside global IP address
内部网络中具有关联公共 IP 地址的 IP 地址。

### inside local IP address
内部网络中具有关联私有 IP 地址的 IP 地址。

### Interior Gateway Protocol (IGP)
用于处理单个自治系统内路由的路由协议。IGP 包括 RIP、IGRP、EIGRP、OSPF 和 IS-IS。

### Internet
使未知外部用户能够通过 Web 浏览器访问网络中特定内部资源的全球网络。例如，您的公司可以允许外部用户通过 Internet 访问其网站以购买产品。

### Internet Control Message Protocol (ICMP)
用于在 TCP/IP 设备之间发送错误和控制信息的协议。定义于 RFC 792，它包括设备可以生成或响应的许多不同类型的消息。

### Internet layer
TCP/IP 协议栈中相当于 OSI 参考模型网络层（第 3 层）的层。

### Internet Protocol (IP)
一种建立跨网络传输数据机制的通用协议。IP 为 TCP/IP 套件中的所有其他协议提供分组交付服务。

### Internet Protocol (IP) address
用于标识网络上计算机的唯一数字标签。它长度为 32 位，由点分隔的四个八位组组成。此数字转换为二进制并用作唯一标识符。

### Internetwork Operating System (IOS)
一种 Cisco 专有操作系统，提供与 Microsoft Windows XP 或 Linux 类似的功能：它控制和管理正在运行的硬件。基本上，IOS 在您与硬件之间提供接口，使您能够执行命令来配置和管理 Cisco 设备。

### intranet
局限于公司内部的网络。换句话说，公司内部用户可以查找内部资源和信息，而无需访问公司外部的网络，例如 Internet。

### Inverse ARP
一种用于发现 Frame Relay 虚拟电路（VC）另一端第 3 层协议地址的协议。它类似于 TCP/IP 中的反向 ARP。

### IP Security (IPsec)
一种在多个不同 RFC 中定义的开放标准。IPsec 在网络层运行并保护 IP 数据包。IPsec 既可用于 LAN 到 LAN (L2L) VPN，也可用于远程访问。与所有其他 VPN 相比，IPsec 当今是最安全的商业解决方案，也是使用最广泛的，但也是最难设置和排除故障的。

### IPv6-over-IPv4 (6to4) tunneling
通过将 IPv6 数据包封装在 IPv4 数据包中，在 IPv4 网络上传输 IPv6 数据包的过程。这需要配置双协议栈的路由器。

### learning state
在 STP 中，端口从监听状态转移到学习状态。在学习状态期间，端口仍在监听和处理端口上的 BPDU；但是，它与监听状态的不同之处在于，端口开始处理用户帧：交换机检查帧中的源地址并更新其 CAM 表，但交换机仍不将这些帧转发到目标端口。端口在该状态中停留的时间为转发延迟时间（默认值为 15 秒）。

### leased line
WAN 中的专用电路或点到点连接。

### light-emitting diode (LED)
Cisco 使用这些半导体二极管发光，以显示其产品各种物理组件的状态。例如，LED 通常用于显示接口的状态。在 Cisco 设备中，LED 可以变为各种颜色，例如绿色、琥珀色或橙色、红色或关闭，以指示各种状态。

### Link Control Protocol (LCP)
主要负责建立、配置、验证和测试 PPP 连接的协议。LCP 在设置 PPP 连接时会协商的一些事项包括使用的身份验证方法（PAP 或 CHAP）、使用的压缩算法、要使用的回拨电话号码和多链路。

### link-local address
IPv6 链路本地地址的范围比站点本地地址更小，即只能在本地链路（数据链路层）上传输。路由器会处理目标为链路本地地址的数据包，但不会将其转发到其他链路。其最常见的用途是让设备获取单播站点本地或全球单播寻址信息，从而发现默认网关并发现网段上的其他第 2 层邻居。

### link state advertisement (LSA)
OSPF 路由器使用 LSA 相互通信。一种 LSA 类型是 Hello，用于建立邻居关系并充当保活功能。Hello 每 10 秒生成一次。在共享链路信息（直连路由）时，链路会发送到指定路由器（DR，224.0.0.6），然后由 DR 将其传播到网段上的其他路由器（224.0.0.5）。

### link state protocols
这些协议使用 Dijkstra 发明的最短路径优先 (SPF) 算法，以找到到达目标的最佳第 3 层路径。与依赖其他邻居关于远程路由传闻的距离矢量协议不同，链路状态协议会学习网络的完整拓扑：哪些路由器连接到哪些网络。OSPF 是链路状态协议的一个示例。

### listening state
20 秒阻塞定时器过期后，STP 中的根端口或指定端口将转移到监听状态。任何其他端口都将保持阻塞状态。在监听状态期间，端口仍在监听 BPDU 并再次检查第 2 层拓扑。此状态中处理的唯一流量是 BPDU——所有其他流量都将被丢弃。端口将在该状态中停留转发延迟定时器指定的时间。该值的默认值为 15 秒。

### local area network (LAN)
用于连接位于非常近的地理区域内（如建筑物的一个楼层、整个建筑物或校园环境）的所有网络设备的网络。

### local loop
WAN 连接中从承载商交换机到分界点的连接。

### local management interface (LMI)
一种定义 Frame Relay DTE（例如路由器）如何与 Frame Relay DCE（例如交换机）交互的机制。LMI 是本地的，不会发送到目标 DTE。

### logical address
网络层负责逻辑地址寻址方案。所有第 3 层寻址方案都有两个组成部分：网络和主机（或节点）。网络中的每个段（物理或逻辑）都需要一个唯一的网络号。这些段上的每个主机都需要从分配的网络号中获取一个唯一的主机号。分配给设备的网络号和主机号的组合为整个网络提供了唯一的第 3 层地址。

### Logical Link Control (LLC)
参见 802.2。

### logical topology
描述设备如何在物理拓扑之间相互通信。





**环回地址** 任何以 127.x.x.x 开头的 IPv4 地址或 IPv6 中的 ::1 地址。环回地址用于测试和验证 TCP/IP 软件栈已加载并在系统上正常工作。

**环回接口** 路由器上的一个逻辑虚拟接口。这些接口在路由器上被视为物理接口：你可以为它们分配寻址信息，将它们的网络编号包含在路由更新中，甚至可以在它们上终止 IP 连接，例如 telnet 和 SSH。环回接口通常用于向 OSPF 路由器分配路由器 ID、用于测试目的以及终止隧道连接（如 GRE 和 IPsec）。

**介质访问控制 (MAC) 地址** 数据链路层使用 MAC（或硬件）地址进行通信。对于 LAN 通信，同一连接媒体类型上的每台机器都需要一个唯一的 MAC 地址。MAC 地址长度为 48 位，表示为 12 位十六进制数。为了便于阅读，MAC 地址以点分十六进制格式表示，例如：FF:FF:FF:FF:FF:FF。

**度量** 路由协议将使用一种称为度量的度量值来确定哪条路径是最佳路径。度量的示例包括跳数、代价、带宽和延迟。

**组播帧** 使用组播帧时，目标 MAC 地址表示一组设备，该组可能不包含任何设备、包含一些设备或包含所有设备。

**Nagle 算法** 一种通过允许将多个字符在同一个 TCP 段中发送（而不是不同段）来处理 TCP 拥塞的算法，类似于 telnet。这对于高 CPU 利用率运行的设备很有用，可以允许远程 CLI 访问仍然保持响应。

**本机 VLAN** 802.1Q 干线支持两种类型的帧：带标记的帧和未带标记的帧。未带标记的帧不携带任何 VLAN 标识信息——基本上，这是一个标准的、未更改的以太网帧。支持未带标记帧的 VLAN 是本机 VLAN。

**NetFlow** 一种 Cisco 专有技术，用于分类和识别流量，并为流量提供统计信息。最初它用于 QoS 和流量管理目的，但可以通过查找流量会话中的异常来检测攻击。NetFlow 的常见用途包括网络流量计费、基于使用情况的网络计费、网络容量和规划、安全、拒绝服务 (DoS) 监控功能以及网络监控和故障排除。NetFlow 就像电话账单，你可以看到谁（源）正在连接到谁（目标）、他们正在使用什么应用程序和/或协议、连接持续了多久、跨连接传输了多少信息（源与目标），以及更多更多。

**网络** 连接短距离和长距离计算机所涉及的所有组件（硬件和软件）。网络用于提供更便捷的信息访问，从而提高用户的生产力。

**网络地址转换 (NAT)** 一种将一个 IP 地址转换为另一个 IP 地址的技术，通常是从私有地址转换为公有地址，反之亦然。

**网络控制协议 (NCP)** 一种定义两个 PPP 对等体如何协商将在 PPP 连接上使用的网络层协议（例如 IP 和 IPX）过程的协议。

**网络层** OSI 参考模型的第三层，网络层使用逻辑（或第 3 层）地址为你的网络提供逻辑拓扑。

**网络时间协议 (NTP)** 一种开放标准，可让你定期将设备的时间与集中式时间服务器同步以获取当前日期和时间。NTP 使用端口 123 上的 UDP。服务器的可靠性是指其时钟源的层级。最准确的是原子钟，但大多数网络通常不需要那种精度，而是从 GPS 源获取时间。NTP 在时间服务器和 NTP 客户端之间传递时间消息有三种基本方法：单播、广播和组播。

**非广播多点接入 (NBMA)** 用于描述使用 VC 进行连接的 WAN 网络的术语。使用 VC 的 WAN 网络中，每个设备通过点对点 VC 连接到另一个设备——一个 VC 上只能连接两个设备。这会导致部分网状 NBMA 环境出现问题，其中设备位于同一子网中。

**非易失性 RAM (NVRAM)** 一种持久性 RAM：当设备关闭时，NVRAM 的内容会被保留。这是存储 IOS 启动配置文件的位置。

**开放最短路径优先 (OSPF)** 一种用于处理 IP 流量路由的链路状态协议。它使用 Dijkstra 开发的 SPF 算法来提供无环拓扑。它还通过链路状态通告 (LSA) 提供由触发的增量更新实现的快速收敛。OSPF 是一种无类别协议，允许使用 VLSM 和路由汇总进行分层设计。它使用代价作为度量。

**开放系统互连 (OSI) 参考模型** 国际标准化组织（ISO）开发了 OSI 参考模型，以帮助描述信息如何从一台机器传输到另一台机器：从用户使用键盘和鼠标输入信息的那一刻起，到将其转换为电气或光信号以跨越外部介质进行传输。重要的是要理解，OSI 参考模型以一般方式描述概念和术语，并且并非每个网络协议都能很好地适应 ISO 模型中解释的方案（例如 IP 和 IPX 就不适合）。因此，OSI 参考模型最常用作教学和故障排除工具。

**外部全局 IP 地址** 具有注册公共 IP 地址的外部设备。

**超额订阅** 当你将接口上所有 VC 的承诺信息速率 (CIR) 相加，并且它们超过了接口的访问速率时，结果就是超额订阅；你在赌你的所有 VC 都不会同时以其流量合同速率运行。

**分组** 在网络层使用的 PDU。它在 TCP/IP 协议栈中也称为数据报。

**分组交换连接** 一种连接，例如 Frame Relay 和 X.25，它使用承载商网络上的虚拟电路来提供 WAN 连接。

**路径代价** 在 STP 中，从交换机到根交换机路径的累积端口代价。当 BPDU 进入一个端口时，BPDU 中的路径代价值会增加入端口的端口代价。此值会在第 2 层设备之间递增。路径代价值帮助第 2 层设备确定哪些端口应为根端口和指定端口。

**每VLAN生成树协议 (PVST)** 一种协议，其中每个 VLAN 都有自己的 STP 实例，具有自己的根交换机、自己的优先级集和自己的 BPDU 集。根据此信息，每个 VLAN 都将发展其自己的无环拓扑。

**永久虚电路 (PVC)** 一种类似于租用线路的虚拟连接。在发送任何数据之前，必须在每个路由器上手动配置 PVC，并在承载商的交换机上构建它们。PVC 的一个缺点是，它们需要大量的前期手动配置来建立 VC。另一个缺点是它们不够灵活：如果 PVC 失败，则不会在故障周围动态重建 PVC。但是，一旦配置了 PVC，除非源和目标之间出现故障，否则它将始终可用。

**物理层** OSI 参考模型的第一层或最底层。物理层负责网络连接的物理机制，包括：网络设备上使用的接口类型、用于连接设备的电缆类型、电缆每端使用的连接器，以及电缆上每个连接使用的引脚定义。

**物理拓扑** 描述设备如何通过电缆物理连接在一起。

**点对点协议 (PPP)** 一种基于一组 WAN 标准的协议。PPP 执行链路的动态配置，允许



认证、压缩数据包头部、测试链路质量、执行差错检测、在同一链路上多路复用网络层协议，并支持将多个 PPP 物理连接绑定为单个逻辑连接。PPP 适用于异步和同步串行接口以及高速串行接口（HSSIs）和 ISDN 接口（BRI 和 PRI）。

### 点对点拓扑

一种两台设备之间仅有一条连接的拓扑，两台设备可直接相互通信，不受其他设备干扰。

### 毒性反向

当路由器向其邻居通告一条毒性路由时，其邻居将打破分离地平线规则，向发起方发送回相同的毒性路由，这称为毒性反向。这可确保所有设备都收到了毒性路由的原始更新。距离矢量协议使用此过程来防止路由环路。

### 毒性路由

分离地平线的一种衍生形式。当路由器检测到其某条直连路由失败时，路由器会通过为该路由分配无穷度量值来对其进行毒化。距离矢量协议使用此方法来防止路由环路。

### 端口地址重定向 (PAR)

静态端口地址转换（PAT）通常称为端口地址重定向。配置了 PAR 的地址转换设备会获取发往特定目标地址和端口号的数据包，并将其重定向到另一个目标地址及可能不同的端口号。这与 NAT 不同，NAT 仅执行一对一 IP 地址转换。

### 端口地址表

另请参阅内容可寻址内存（CAM）表。

### 端口地址转换 (PAT)

一种将内部 IP 地址转换为单个 IP 地址的过程，其中每个内部地址都会分配一个不同的端口号以确保唯一性。

### 端口开销

在 STP 中，每个端口都会被分配一个与接口带宽成反比的开销值。端口开销越低，优先级越高。当 BPDU 进入一个端口时，BPDU 中的路径开销值会增加入端口的端口开销。这有助于第 2 层设备确定哪些端口应为根端口和指定端口。

### 端口安全

交换机的一项功能，可根据与接口关联的 MAC 地址或地址锁定交换机端口，防止未经授权访问 LAN。

### PortFast

一项功能，可使端口始终处于转发状态——即使 STP 正在运行且根端口和指定端口正经历不同状态，此规则也适用。因此，当 STP 运行时，同一交换机上的 PortFast 端口仍可相互转发流量，在一定程度上限制了 STP 中断。PortFast 应仅配置在未连接到其他交换机的端口上，例如连接有 PC、服务器和路由器的端口。

### 上电自检 (POST)

Cisco 设备启动时执行硬件测试的过程。这些测试可以包括接口、线路和内存组件。对于许多组件，如果发生故障，Cisco 设备将无法启动。

### PPP 身份验证协议 (PAP)

PPP 身份验证协议中最简单但安全性最低的协议。在身份验证阶段，PAP 将进行两次握手过程。在此过程中，源将以明文形式将其用户名和密码发送到目标。目标将此信息与本地存储的用户名和密码列表进行比较。如果找到匹配项，目标会发送回接受消息；如果没有找到匹配项，则会发送回拒绝消息。

### 表示层

OSI 参考模型的第六层，负责定义文本、图形、视频和/或音频信息等信息在所使用的界面中如何呈现给用户。

### 私有 IP 地址

RFC 1918 是一份为解决 IP 地址短缺而制定的文档。当设备需要相互通信时，每个设备都需要一个唯一地址。RFC 1918 创建了任何公司均可在内部使用的专用地址空间。这些地址包括 10.0.0.0/8、172.16.0.0/16–172.31.0.0/16 和 192.168.0.0/24–192.168.255.0/24。私有 IP 地址不可在 Internet 上路由。如果要与公共网络（例如 Internet）上的设备通信，必须使用地址转换将私有地址转换为公共地址。

### 特权 EXEC 模式

Cisco 设备的一种访问级别，提供对 IOS 的高级管理访问权限，包括用户 EXEC 模式下可用的所有命令。此模式支持详细故障排除，也是进入配置模式的阶梯。如果提示信息末尾显示 # 字符，则表示您正在使用特权 EXEC 模式。

### 协议

定义技术中所用规则的系统。某些协议是开放标准，意味着许多供应商可以使用该协议，而其他协议是专有的，意味着它们仅适用于某个特定供应商。例如，系统中用于实现电子邮件功能的常见协议包括简单邮件传输协议（SMTP）、因特网消息访问协议版本 4（IMAP4）和邮局协议 3（POP3）。

### 协议数据单元 (PDU)

一个通用术语，用于描述数据及其开销，包括段、数据包和帧等项目。

### 代理 ARP

一种 ARP 形式，允许路由器在 ARP 响应中为不同网络段上的设备返回其自身 MAC 地址。当需要将设备从一个段移动到另一个段，但无法更改其当前 IP 寻址信息时，会使用代理 ARP。

### RADIUS (远程拨号用户身份验证服务)

一种中心身份验证服务，可用于在访问无线网络、连接交换机或连接 VPN 服务器时强制用户登录。

### 快速生成树协议 (RSTP)

一种 IEEE 标准（802.1w），可与 802.1d 互操作并对其进行扩展。然而，802.1d 的问题是，它是在等待 30 到 50 秒进行收敛尚不成问题的时代设计的。然而，在当今的网络中，这可能会对使用实时应用程序（例如 IP 语音（VoIP））的网络造成严重性能问题。RSTP 可在大多数情况下实现近乎即时的收敛。

### 只读存储器 (ROM)

非易失性内存：关闭设备后，ROM 的内容不会被擦除。ROM 包含启动路由器所需的固件，通常包含以下四个组件：POST、引导程序、ROMMON 以及可能的 Mini-IOS。

### 准备/未准备信号

这些信号可用于传输层以实现流量控制。使用准备/未准备信号时，当目标接收到的流量超过其处理能力时，可以向源发送未准备信号，指示源应停止发送数据。当目标有机会赶上并处理源的信息时，目标将以准备信号响应。源在收到准备信号后，可以恢复发送数据。

### 侦察攻击

一种威胁，发生在对手试图了解您的网络信息时。对手会通过发现网络组件和资源以及其中存在的漏洞来实现这一点。

### 远程访问 VPN

经典电路交换网络（如 POTS 或 ISDN）的扩展。此 VPN 可安全地将远程用户或小型办公室/家庭办公室（SOHO）连接到企业或分支机构。使用远程访问 VPN 时，VPN 提供虚拟化过程，使远程访问用户或办公室看起来像是物理连接到企业办公室网络。远程访问 VPN 常用的协议包括 IPsec、SSL、PPTP 和 L2TP。

### 中继器

一种物理层设备，会放大信号以便信号传输更远。当需要延长电缆距离时，通常会使用中继器。

### 反向地址解析协议 (RARP)

一种类似于 ARP 反向操作的协议。在 ARP 中，设备知道第 3 层地址，但不知道数据链路层地址。使用 RARP 时，设备没有 IP 地址且希望获取一个。该机器唯一拥有的地址是 MAC


地址。使用 RARP 的常见协议有引导程序协议（BOOTP）和 DHCP。

### RJ-45
此连接器位于双绞线缆的末端。它看起来像电话连接器，但更宽，因为它有八个针脚，而电话线缆（使用 RJ-11 连接器）只有四个针脚。

### 回卷线（rollover cable）
一种用于控制台连接的电缆类型，外观类似于以太网 CAT 5 电缆；但是，回卷线是 Cisco 的专有技术，不适用于其他类型的连接。回卷线的塑料护套内有八根导线。回卷线的每一端都会反转相对另一端的导线：一侧的引脚 1 连接到另一侧的引脚 8；引脚 2 连接到引脚 7，依此类推。

### ROM 监视器（ROMMON）
在 Cisco 路由器或交换机上，ROM 监视器加载引导程序，以便对 IOS 设备进行低级别诊断测试，执行密码恢复过程，并且如果闪存中的 IOS 镜像损坏或丢失，还可以执行紧急升级。

### 根桥或交换机
当 STP 运行时，首先会创建生成树，其本质上是一种倒置的树形结构。树的顶部是根桥。从根桥出发是分支（物理以太网连接），连接到其他交换机，这些交换机的分支又连接到其他交换机，依此类推。桥 ID（桥优先级 + MAC 地址）最低的第 2 层设备被选举为根。

### 根端口
在 STP 中，每个非根交换机都需要选择一个用于到达根交换机的单端口。这就是根端口，它到达根桥或交换机的累积路径代价最佳。

### 路由器
一个在网络层工作的网络设备。由于路由器的工作层高于第 2 层设备，并且使用逻辑寻址，因此它们提供许多优势。路由器执行以下功能：定义逻辑寻址方案、包含广播和组播、查找到达目的地的第 3 层路径、连接不同的介质类型、使用 VLAN 在同一接口上交换数据包，以及使用过滤和服务质量（QoS）等高级功能。

### 单臂路由（router-on-a-stick）
具有一个干线连接到交换机的路由器；它在该干线连接上对 VLAN 进行路由。

### 路由信息协议（RIP）
一种较旧的距离向量协议，使用跳数作为路由度量。RIP 有两个版本：版本 1 是距离向量协议。版本 2 是混合协议。RIPv1 使用本地广播共享路由信息。这些更新是周期性的，默认情况下每 30 秒发生一次。为防止数据包在循环中无限期传输，RIP 的两个版本都使用计数到无穷大，并设置跳数限制为 15——到达第 16 跳的任何数据包都将被丢弃。RIPv2 不使用广播，而是使用组播。并且为加速收敛，RIPv2 支持触发式更新。RIPv1 是有类协议，而 RIPv2 是无类协议。

### 路由表
路由器使用网络号做出路由决策：如何将数据包发送到其目的地。它们构建路由表，其中包含路径信息，包括网络号、路由器应使用哪个接口到达网络号、路径的度量值，以及路由器如何得知该网络号。

### 无残帧交换（runtless switching）
参见碎片帧过滤交换（fragment-free switching）。

### 段（segment）
传输层使用的协议数据单元（PDU）。

### 服务访问点（SAP）
LLC 通过使用 SAP 标识符来执行复用。当网络层协议在 802.2 帧中封装时，网络数据的协议被放置在 SAP 字段中。目标使用此字段来确定哪个第 3 层协议应处理该帧。

### 会话层
OSI 参考模型的第六层，会话层负责发起连接的建立和拆除。为执行这些功能，会话层必须确定数据是停留在本地计算机上，还是必须从远程网络设备获取或发送到远程网络设备。

### 简单邮件传输协议（SMTP）
用于通过 Internet 发送邮件的协议。

### 简单网络管理协议（SNMP）
一种常用于远程管理（配置和/或监控）网络设备的协议。SNMP 由三个组件组成：网络管理站（NMS）是访问和管理代理的设备。代理是由 NMS 管理的设备。管理信息库（MIB）定义了信息（配置、操作和统计信息）如何在代理上存储。交互发生在 NMS 和代理之间，可能涉及两种类型的连接。NMS 向代理发送“get”或“set”命令：get 命令用于检索 MIB 信息，set 命令用于更改 MIB 信息。代理向 NMS 发送“traps”或“informs”，它们是一种日志消息，指示设备上的重要条件。存储在代理上的信息位于 MIB 中。每个 MIB 都由对象标识符（OID）唯一标识。Get、set 和 trap 消息基于由特定 OID 标识的 MIB 信息。

### 站点本地地址
这些 IPv6 地址类似于 RFC 1918 IPv4 地址，代表特定站点或公司。它们可以在公司内部使用，而无需浪费任何公共 IP 地址——考虑到 IPv6 中可用地址数量庞大，这并非问题。但是，通过使用专用地址，您可以为 IPv6 设置地址转换策略，以轻松控制谁被允许离开您的网络以及谁可以接收返回流量。

### 站点到站点 VPN
有时称为 LAN-to-LAN 或 L2L VPN，这种 VPN 连接两个地点或站点，基本上扩展了经典的 WAN 设计。两个中间设备，通常称为 VPN 网关，实际上保护两个 LAN 之间的流量。这种类型的 VPN 在位置之间隧道数据包：一个 LAN 的原始 IP 数据包由一个网关加密，转发到目标网关，然后解密并转发到其端的本地 LAN 以到达目标。

### 小型办公室/家庭办公室（SOHO）
一个在家庭或小型办公室中工作的少数人组成的网络。

### 生成树协议（STP）
其主要功能是从拓扑中删除第 2 层循环的协议。DEC 最初开发了 STP；IEEE 在 802.1d 中增强了 STP 的初始实现。

### 水平分割（split horizon）
距离向量协议用于防止网络中路由循环的方法。使用水平分割时，如果相邻路由器将路由发送到路由器，接收路由器不会将此路由回传到同一接口上的通告路由器。

### 标准 IP 访问控制列表（ACL）
一种仅基于源 IP 地址匹配数据包的 ACL。

### 星形拓扑
一种涉及系统连接到中央设备（例如交换机）的网络设置。

### 无状态自动配置
使用此功能（DHCPv6 的扩展），客户端仍可以动态获取其寻址；但是，不需要服务器将 IPv6 寻址信息分配给客户端。相反，每个客户端使用路由器通告消息中的信息为接口配置 IPv6 地址。这是通过获取路由器通告源地址中的前 64 位（路由器地址的前缀）并使用 EUI-64（扩展唯一标识符）过程创建 64 位接口 ID 来实现的。无状态自动配置主要设计用于手机、PDA、家用网络和设备，以自动分配地址而无需管理 DHCP 服务器基础设施。

### 静态路由
手动配置在路由器上的路由。

### 粘性学习
端口安全功能，使交换机能够动态学习哪些 MAC 地址对应特定端口，然后为这些地址设置永久 CAM 表项。

### 存储转发交换
最基本的交换形式，第 2 层设备必须将整个帧拉入端口缓冲区并检查帧的循环冗余校验（CRC），之后该设备才会对帧执行任何其他处理。




### 直通电缆
一根以太网电缆，其中一端的引脚1与另一端的引脚1相连，引脚2与引脚2相连，以此类推。直通电缆用于DTE-to-DCE连接。DTE是路由器、PC或文件服务器，而DCE是集线器或交换机。

### 子接口
与单个物理接口关联的逻辑接口。一个物理接口可以支持许多子接口。思科路由器对待子接口的方式与对待物理接口相同。您可以关闭物理接口，这将关闭所有关联的子接口，或者关闭单个子接口，同时保持其余子接口正常运行。

### 子网掩码
每个TCP/IP地址包含三个组件：网络组件、主机组件和子网掩码。子网掩码的功能是区分网络或子网的网络地址、主机地址和定向广播地址。

### 子网零（子网0）
进行子网划分时，创建的第一个和最后一个子网称为子网零。一些较旧的TCP/IP协议栈不支持使用子网零，但当今的操作系统并非如此。

### 子网划分
将IP地址范围本地细分为多个IP地址范围。

### 子网接入协议（SNAP）
802.2支持两种帧类型：SAP和SNAP。802.2 SAP帧中原始SAP字段的一个问题是，尽管其长度为8字节，但只有前6位用于标识上层协议，这最多可支持64种协议。在20世纪80年代，可用的协议远多于64种，并且人们预期会创建更多协议。SNAP克服了这一限制，无需更改SAP字段的长度。要指示SNAP帧，SAP字段设置为十六进制0xAA，控制字段设置为0x03，OUI字段设置为0x0。类型字段标识封装在802.2帧有效载荷中的上层协议。AppleTalk是使用802.2 SNAP帧的协议示例。

### 子集通告
当服务器响应VTP客户端或VTP服务器的请求时，它会生成子集通告，其中包含详细的VLAN配置信息，包括VLAN编号、名称、类型和其他信息。

### 摘要通告
这是由VTP服务器模式下的交换机生成的。摘要通告默认每5分钟生成一次，或者在服务器交换机上发生配置更改时生成。与子集通告不同，摘要通告仅包含汇总的VLAN信息。

### 超网
参见无类别域间路由（CIDR）。

### 交换机
一种第2层设备，用于解决带宽和冲突问题。交换机在专用集成电路（ASICs）硬件中执行交换操作。所有交换机都支持存储转发交换，一些交换机还支持直通交换和无碎片交换。交换机通常同时支持半双工和全双工。交换机有多种尺寸，有些拥有超过100个端口。

### 交换式虚电路（SVC）
与电话呼叫工作原理类似，每个SVC设备都会被分配一个唯一地址，类似于电话号码。要使用SVC到达目标设备，您需要知道目标设备的地址。在WAN环境中，这通常在您的SVC设备上手动配置。您的设备将SVC地址发送到运营商交换机，由其建立连接。使用完电路后，您的设备会向运营商交换机发送信号以拆除连接。SVC用于间歇数据或备份目的。

### 同步光纤网络（SONET）
一种支持最高13.22 Gbps速率的光纤网络通信链路。

### syslog
一种将设备信息记录到远程服务器的方法。向syslog服务器记录日志，可以更轻松地管理和保留来自大量设备的日志信息历史记录。Syslog使用UDP并运行在端口514上。所有日志信息均以明文发送且没有数据包完整性检查，这使得黑客很容易向syslog服务器发送虚假数据。因此，强烈建议您在网络设备与syslog服务器之间加密信息，并在syslog服务器上设置过滤器，仅接受来自特定IP地址的日志信息。

### 系统配置对话框
当路由器启动、运行硬件诊断并加载IOS软件后，IOS会尝试在NVRAM中查找配置文件。如果找不到可加载的配置文件，IOS将运行系统配置对话框（通常称为设置模式），这是一个提示您输入配置信息的脚本。此脚本的目的是向您提问，以便在您的设备上设置基本配置。

### T1
一种广泛使用的数字传输链路，采用点对点传输技术，使用两对双绞线：一对用于发送，另一对用于接收。T1的传输速率为1.544 Mbps，可传输数字、语音、数据和视频信号。

### T3
与T1类似，但旨在以高速传输大量数据。T3是一种租用线路，可以45 Mbps的速率传输数据。

### telnet
一种终端仿真程序，用于远程连接到应用程序或服务器。

### 三次握手
在可靠的TCP会话中，主机在向另一台主机发送信息之前，必须进行三次握手过程以建立连接：SYN、SYN/ACK和ACK。

### 时分复用（TDM）
信道化服务（如T1或E1）使用TDM在单根线路上创建多个逻辑信道。每个信道（或时隙）都被分配自己的带宽和线路上的时间。每个信道不会与其他信道同时传输其信息。相反，每个信道都必须轮流发送少量信息。所有信道都获得相同的带宽和时间，在所有信道都有机会发送信息后，第一个信道会重新开始。

### 收发器
网络接口中实际通过传输介质发送和接收电信号的部分。它也是实际连接到介质的接口部分。

### 传输控制协议（TCP）
TCP/IP中负责在两台设备之间提供可靠逻辑连接的协议。它使用窗口机制实现流量控制，以防止源设备向目标设备发送过多段而使其不堪重负。

### 传输控制协议/互联网协议（TCP/IP）
一种包含多种协议的标准。它定义了互连网络中的机器如何相互通信。它最初由DARPA（国防高级研究计划署）资助并为其开发。TCP/IP最初在RFC 791中设计，已成为网络协议的事实上的标准。因特网使用TCP/IP在网络之间传输数据，当今大多数企业也使用TCP/IP构建其网络。

### 传输层
OSI参考模型的第四层，传输层负责连接的实际机制。它可以在连接上提供可靠和不可靠的数据传输。对于可靠连接，传输层负责错误检测和纠正：当发送信息时检测到错误，传输层将重新发送数据。对于不可靠连接，传输层仅提供错误检测——错误纠正由更高层（通常是应用层）负责。

### 中继连接
此类连接能够承载多个VLAN的流量。为了支持中继，原始以太网帧必须经过修改以承载VLAN信息，以确保广播完整性得以维持。

### 双绞线电缆
一种常见的网络电缆类型，使用八根由四对线组成的铜线。每对线都相互绞合，以帮助防止电缆中相邻线对之间的串扰。铜线从电缆的一端向另一端传输电信号。





### 单播帧
采用单播帧时，目标 MAC 地址表示单个设备。

### 非屏蔽双绞线 (UTP)
四对铜线，每对均定期绞合。其安装和故障排除成本低，但易受电磁干扰 (EMI) 和射频干扰 (RFI) 影响，且电缆传输距离仅限于短距离。

### 用户数据报协议 (UDP)
该协议在传输层提供不可靠连接。UDP 不会经过三次握手来建立连接——它只是直接开始发送信息。同样，UDP 也不会检查发送的报文段是否已被目标接收；换句话说，它没有确认机制。通常，如果需要确认机制，应用层会提供此验证。

### 用户 EXEC 模式
针对 Cisco 设备的一种访问级别，提供对 IOS 的基本访问权限，命令可用性有限（基本只能进行简单监控和故障排除）。如果在提示信息末尾看到 > 字符，即表示您处于用户 EXEC 模式。

### 可变长度子网掩码 (VLSM)
一种子网设计策略，在对原始 A 类、B 类或 C 类网络地址进行子网划分时，可为给定子网提供不止一个子网掩码。类别协议（如 RIP v1 和 IGRP）不支持 VLSM。部署 VLSM 需要一种无类别路由协议，例如 BGP、EIGRP、IS-IS、OSPF 或 RIP v2。VLSM 提供两大优势：更高效地使用地址空间，以及能够执行路由汇总。

### 虚电路 (VC)
两台设备之间的逻辑连接。因此，可以在同一物理连接上存在多个 VC。与使用租用线路相比，VC 能以更低的价格提供完全连通性。VC 用于异步传输模式 (ATM)、帧中继和 X.25。

### 虚拟局域网 (VLAN)
位于同一广播域、且在交换式网络中不受任何物理边界限制的一组网络设备（前提是所有设备均通过交换机互连，且中间没有第 3 层设备）。从逻辑上讲，VLAN 也是子网。

### 虚拟专用网 (VPN)
一种特殊的安全网络，用于在公共网络（如 Internet）上提供安全、受保护的隧道或连接。术语中的“网络”部分是指使用公共网络（如 Internet）来实现广域网解决方案。术语中的“虚拟”部分是指对内部网络组件（如用户和服务）隐藏公共网络。术语中的“专用”部分是指流量应保持私密——网络中的窃听者不得查看。这是通过加密来实现的，以保持数据机密性。

### 虚拟路由器冗余协议 (VRRP)
该协议的功能类似于 Cisco 的专有协议 HSRP。HSRP 的一个主要缺点是它是一种专有协议；而 VRRP 则是一种开放标准，定义在 IETF RFC 2338 中。与 HSRP 类似，VRRP 也有使用虚拟路由器作为默认网关的终端设备。VRRP 与 HSRP 的一个主要区别在于：HSRP 为默认网关使用虚拟 IP 地址，而 VRRP 可以使用虚拟 IP 地址或主路由器的接口地址。如果使用虚拟 IP 地址，则会进行选举过程来选择主路由器。VRRP 支持以太网介质类型，也支持 VLAN 和 MPLS VPN。

### 虚拟电传终端 (VTY)
Cisco 设备上用于管理 Telnet 和 SSH 连接的逻辑线路。

### VLAN 中继协议 (VTP)
一种 Cisco 专有协议，用于在中继连接上在 Cisco 交换机之间共享 VLAN 配置信息。VTP 使交换机能够共享和同步其 VLAN 信息，从而确保网络拥有统一的 VLAN 配置。

### IP 语音 (VoIP)
一组旨在通过 IP 网络传输语音和视频的技术。

### VTP 客户端模式
处于 VTP 客户端模式的交换机无法自行更改其 VLAN 配置——它需要处于 VTP 服务器模式的交换机告知其 VLAN 变更情况。当客户端交换机从服务器交换机接收到 VTP 消息时，它会合并这些更改，然后将 VTP 消息从其其余中继端口泛洪出去。需要注意的是，客户端交换机不会将其 VLAN 配置信息存储在 NVRAM 中，而是在每次启动时从服务器交换机学习这些信息。

### VTP 剪枝
Cisco VTP 的一项功能，可使交换机动态删除或添加中继上的 VLAN，从而创建更高效的交换网络。

### VTP 服务器模式
配置为 VTP 服务器模式的交换机可以添加、修改和删除 VLAN。VTP 服务器交换机在进行更改时，会在其所有中继端口上传播有关该更改的 VTP 消息。如果服务器交换机接收到 VTP 更新消息，它将合并该更新，并将消息从其其余中继端口转发出去。

### VTP 透明模式
配置为 VTP 透明模式的交换机可以添加、修改和删除 VLAN。对透明交换机所做的配置更改仅影响该交换机本身，不影响网络中的其他交换机。透明交换机会忽略 VTP 消息——它会在中继端口上接受这些消息，并从其其余中继端口转发出去，但不会合并消息中的更改。

### WebVPN
SSL VPN（尽管使用 SSL 作为保护协议）由每个厂商以不同方式实现，因此具有专有性。Cisco 的 SSL VPN 解决方案称为 WebVPN，提供三种安全连接方式：无客户端、瘦客户端和 SSL VPN 客户端。无客户端和瘦客户端实现使用带已安装 JavaScript 的普通 Web 浏览器来提供 VPN 解决方案。SSL VPN 客户端提供网络层保护，并使用户无需任何修改即可使用其日常应用程序。

### 广域网 (WAN)
一种连接局域网的网络，通常在需要连接的局域网相距较远时使用。在企业为局域网提供自身基础设施的情况下，广域网是从运营商网络（如电话公司）租用的。WAN 服务中使用四种基本类型的连接或电路：电路交换、单元交换、分组交换和专用连接。

## 通配符掩码
在处理 ACL 语句中的 IP 地址时，可以使用通配符掩码来匹配一系列地址，而无需手动输入要匹配的每个 IP 地址。通配符掩码不是子网掩码，但如同 IP 地址或子网掩码一样，通配符掩码由 32 位组成。使用通配符掩码时，某位位置为 0 表示 ACL 语句地址的相应位位置必须与被检查数据包中 IP 地址的相应位位置匹配。某位位置为 1 表示 ACL 语句地址的相应位位置无需与被检查数据包中 IP 地址的相应位位置匹配。OSPF **network** 语句也使用通配符掩码。

## 窗口机制
TCP 和其他传输层协议用于调节报文段的流量，确保一台设备不会向另一台设备发送过多报文段。TCP 使用滑动窗口机制来辅助流量控制。例如，窗口大小为 1 时，设备只能发送一个报文段，然后必须等待相应的确认，才能接收下一个报文段。

## 无线接入点 (WAP)
一种网络设备，可使无线客户端连接到无线网络。无线客户端连接到 WAP，并通过它将数据发送到网络上的其他主机。
