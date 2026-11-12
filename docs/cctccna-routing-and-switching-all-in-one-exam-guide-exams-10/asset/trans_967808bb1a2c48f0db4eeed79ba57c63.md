
设备通过物理拓扑进行通信的方式。两种拓扑会因所使用的网络技术/标准而有所不同。

- 网状连接通常描述设备如何连接。在部分网状网络中，并非每个设备都与其他每个设备相连。在全网状网络中，每个设备都与其他所有设备相连。

## Types of Cabling
- 铜缆在以太网中通常使用 UTP，即四对线。它使用成本低廉，易于安装和故障排除。然而，UTP 电缆只能跨越短距离，且易受 EMI 和 RFI 影响。UTP 电缆使用 RJ-45 连接器。
- 直通 UTP 电缆用于连接不同类型的设备，例如计算机到交换机或交换机到路由器。交叉 UTP 电缆用于连接相似设备（网络上的两个主机），例如计算机到计算机、交换机到交换机，或计算机到路由器。
- 在光纤电缆中，玻璃芯传输光脉冲且不受电气干扰影响。光纤电缆通常用作建筑物之间的骨干，但也可用于工作站。光纤的两种实现包括用于较短距离的 MMF 和用于较长距离的 SMF。

## Access Methods
- CSMA/CD 是所有以太网架构使用的访问方法，涉及系统在发送数据到线路上之前先感知线路。
- 在令牌传递中，系统需要在线路上传递令牌之前等待令牌；一旦系统获得令牌且令牌中无数据，系统即可将数据放置在线路上。系统在发送数据之前需要令牌。令牌环网络架构使用令牌传递作为其访问方法。

## Network Architectures
- 以太网架构有不同的分类：*Ethernet* 运行速度为 10 Mbps，*Fast Ethernet* 运行速度为 100 Mbps，*Gigabit Ethernet* 运行速度为 1000 Mbps（1 Gbps），而 *10-Gigabit Ethernet* 运行速度为 10 Gbps。
- 对于 CCT 和 CCNA 考试，应熟悉 Fast Ethernet，例如 100BaseTX，它使用 UTP CAT 5/6/6A 电缆，采用星型拓扑，运行速度为 100 Mbps。100BaseFX 使用 MMF 光纤电缆可达 400 米，传输速率为 100 Mbps。
- 一些常见的千兆以太网标准是 1000BaseT，它使用 UTP 电缆，最大距离为 100 米，传输速率为 1000 Mbps。1000BaseLX 的传输速率为 1000 Mbps，但使用 SMF 光纤电缆可达 10 km。1000BaseSX 使用 MMF 光纤电缆可达约 275 米的较短距离。

## Virtualization Fundamentals
- 虚拟化是一种方法，可让你更好地利用服务器上的资源，例如内存和处理能力，同时使用更少的电力，因为运行虚拟服务器所需的物理服务器更少。
- 了解不同类型的 hypervisor 以及虚拟组件的类型，例如虚拟网卡和虚拟交换机。

## Questions
以下自测题将帮助你衡量对本章所介绍材料的理解程度。请仔细阅读所有选项，因为可能存在多个正确答案。选择每个问题的正确答案。

1. 下列哪项网络特性是 MTBF 所关心的？
A. Cost
B. Security
C. Reliability
D. Availability

2. 哪种网络拓扑通常涉及单个网络设备，该设备充当交换机、路由器和接入点，同时提供 DHCP、NAT 和防火墙等功能？
A. SOHO
B. Spine-leaf
D. CSU/DSU

3. 将以下每项技术匹配到 (1) LAN 或 (2) WAN：
A. Switch
B. Router
C. 1000BaseT
D. CSU/DSU

4. ________ 拓扑使用中央连接设备将所有系统连接到网络。
A. Bus
B. Star
C. Point-to-point
D. Ring

5. 在 spine-leaf 设计模型中，哪一层涉及接入层交换机，以便客户端连接到网络？
A. Core
B. Spine
C. Distribution
D. Leaf

6. 哪种千兆以太网架构使用单模光纤电缆可达 10 km 的距离？
A. 1000BaseT
B. 1000BaseCX
C. 1000BaseSX
D. 1000BaseLX

7. 下列哪些是 UTP 电缆的特性？（选择两项。）
A. Carries pulses of light
B. Uses multiple two-pair wires
C. Is susceptible to EMI and RFI
D. Is used between two campus buildings

8. 你的交换机网络端口已用完。你购买了一台额外的网络交换机并希望将两台交换机连接在一起。你会使用什么类型的电缆？
A. Straight-through UTP
B. Crossover UTP
C. Serial
D. Rollover

9. 你需要将笔记本电脑连接到交换机，将同一交换机连接到另一台交换机，并将第二台交换机连接到路由器。你需要多少根电缆以及应使用什么类型的 UTP 电缆？
A. Two straight-through cables and one crossover cable
B. Three straight-through cables and zero crossover cables
C. One straight-through cable and two crossover cables
D. One straight-through cable and one crossover cable

10. 下列哪一项是与 1000BaseT 网络一起使用的连接器？
A. ST
B. DB-15
C. RJ-45
D. SC

11. 你的公司正在运行千兆以太网网络。关于网络上设备何时可以传输数据的以下陈述中哪些是正确的？
A. When no other device is sending
B. When the device has the token
C. When the access point grants permission
D. When the router is not busy

12. 下列哪一项代表所有以太网网络使用的访问方法？
A. CSMA/CA
B. CSMA/CD
C. Token Passing
D. CSMA/TA

13. 哪种千兆以太网标准使用 STP 电缆可达 25 米？
A. 1000BaseTX
B. 1000BaseCX
C. 1000BaseLX
D. 1000BaseSX

14. 下列哪项网络服务负责将私有 IP 翻译为公共 IP 地址？
A. DHCP
B. CSU/DSU
C. NAT
D. DNS

15. 哪项网络服务负责为网络上的终端设备分配 IP 地址？
A. DHCP
B. CSU/DSU
C. NAT
D. DNS

16. 网络上的用户抱怨她无法连接到 www.cisco.com 网站。在故障排除过程中，你可以 ping 该站点的 IP 地址，但无法 ping FQDN。可能是什么问题？
A. DHCP
B. CSU/DSU
C. NAT
D. DNS

## Performance-based Questions
思科考试有基于性能的问题，你必须将屏幕左侧的项目拖动到屏幕右侧的适当位置以回答问题。以下是一些基于性能的示例问题。

1. 你需要使用 568B 标准制作自己的以太网电缆。通过在框中写入来将导线按正确顺序放置。

白线 / 绿条纹
棕线
白线 / 棕条纹
白线 / 蓝条纹
白线 / 橙条纹
蓝线
绿线
橙线

![](img/0eafdcebe219ef3756f0e57bcf1eb997_91_0.png)

2. 查看每个场景，在线上方写明使用的是交叉电缆还是直通电缆。请注意，在真正的考试中，如果这是性能问题，你将需要拖动适当的电缆类型来连接设备。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_91_1.png)

## Answers
- 1. **C.** 平均故障间隔时间（MTBF）通常用于衡量可靠性。
- 2. **A.** 小型办公室/家庭办公室（SOHO）网络涉及使用单个网络设备，例如家庭路由器，提供所有网络功能。
- 3. **A and C: 1 (LAN); B and D: 2 (WAN).** 以太网架构如 1000BaseT 和交换机是 LAN 技术，而路由器和 CSU/DSU 是用于连接到 WAN 的组件。
- 4. **B.** 星型拓扑使用中央连接设备将所有系统连接到网络。
- 5. **D.** spine-leaf 设计拓扑的 leaf 层涉及接入层交换机。该层使终端能够连接到网络。
- 6. **D.** 1000BaseLX 使用单模光纤电缆可达最长 10 km 的距离。
- 7. **B and C.** UTP 使用多对铜线，易受 EMI 和 RFI 干扰。
- 8. **B.** 交叉电缆用于连接相似设备。请记住，直通电缆用于连接不同类型的设备。
- 9. **A.** 笔记本电脑到交换机和交换机到路由器的连接需要两根直通电缆；交换机到交换机的连接需要一根交叉电缆。
- 10. **C.** 1000BaseT 使用 UTP 电缆，使用 RJ-45 连接器。
- 11. **A.** 以太网网络使用 CSMA/CD 作为其访问方法，这意味着设备只有在线路上没有信号（电缆未被使用）时才能发送数据。
- 12. **B.** 所有以太网架构都使用载波侦听多路访问/冲突检测（CSMA/CD）。该访问方法涉及系统在发送数据之前检查线路以确保其没有信号。如果数据与其他数据发生冲突，系统将检测到冲突并重新传输。
- 13. **B.** 1000BaseCX 使用 STP 电缆可达最大 25 米的距离。
