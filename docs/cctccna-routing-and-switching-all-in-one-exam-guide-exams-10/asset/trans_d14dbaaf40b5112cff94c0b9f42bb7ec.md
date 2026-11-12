

假设根交换机与Switch-C之间的链路故障是物理链路故障，而不是连续丢失三个Hello BPDU。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_599_0.png)

图10-13 快速迁移示例

![](img/0eafdcebe219ef3756f0e57bcf1eb997_599_1.png)

**考试提示** 请记住，在CCNA考试中，RSTP在点对点（P2P）链路上的收敛速度远快于原始IEEE STP（802.1d）。

## RSTP 配置

思科交换机支持三种STP类型，如表10-4所示。思科交换机的默认配置是每个VLAN单独运行一个STP实例，所有VLAN共享一个根交换机，并且没有负载分担。

**表10-4 STP类型**

| STP | 描述 |
|---|---|
| PVST+ | 802.1d 每VLAN，带有思科专有扩展（PortFast、UplinkFast、BackboneFast） |
| PVRST+ | 802.1w (RSTP) 每VLAN |
| 多生成树协议（MSTP） | 802.1s，称为多生成树，将思科PVST+与IEEE标准相结合 |

本书仅关注PVRST+，而且只是简要介绍。要启用PVRST+，请使用以下命令：

```
Switch(config)# spanning-tree mode rapid-pvst
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_600_0.png)

**考试提示** 请记住，在CCNA考试中，**spanning-tree mode rapid-pvst** 命令可在IOS交换机上启用RSTP。

启用后，可以使用以下命令按VLAN查看STP：

```
Switch# show spanning-tree vlan VLAN_# [detail]
```

以下是该命令的示例：

```
Switch# show spanning-tree vlan 10
VLAN0010
Spanning tree enabled protocol rstp
Root ID Priority 32768
This bridge is root
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
Bridge ID Priority 32768 (priority 32768 sys-id-ext 10)
Address 0000.01c1.1111
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
Aging Time 300
Interface Role Sts Cost Prio.Nbr Type
-------- ---- --- ---- -------- ----
Fa0/1 Desg FWD 19 128.1 P2p
Fa0/2 Desg FWD 19 128.2 P2p
Fa0/3 Desg FWD 19 128.3 P2p
. 
. 
.
```

在此示例中，该交换机是VLAN 10的根交换机，并且正在使用RSTP。请注意，其所有端口都是指定端口（Desg）并处于转发状态（FWD）——如果所有活动端口都如此，那么该交换机就是根交换机，此处确实如此，如输出的第四行（This bridge is root）所示。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_601_0.png)

**考试提示** 请熟悉考试中 **show spanning-tree vlan** 和 **show spanning-tree interface** 命令的输出。

你还可以使用 **show spanning-tree interface** 命令按端口检查STP。以下是一个示例：

```
Switch# show spanning-tree interface fastethernet 1/0/5
Vlan Role Sts Cost Prio.Nbr Type
----- ---- --- ---- -------- ----
VLAN0001 Root FWD 19 128.1 P2P
VLAN0002 Altn DIS 19 128.2 P2P
VLAN0003 Root FWD 19 128.2 P2P
```

在此示例中，F1/0/5是VLAN 2中的备用端口，原因可能是它到根桥的路径开销更高，或者存在平局且该邻居的桥ID更大。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_602_0.png)

**提示** 要排除PVRST+的问题，请使用 **debug spanning-tree pvst+** 命令；要排除STP中端口状态变化的问题，请使用 **debug spanning-tree switch state**。

## PVST+ 和 RSTP 优化

为了理解PVST+和RSTP提供的优势，请查看图10-14。此示例显示了编号为1和2的两个VLAN。思科交换机的默认行为是，根据交换机ID最低的交换机，所有VLAN共用一台根交换机。在此例中，就是Switch-A。请注意，根据RSTP的计算，Switch-C为其两个VLAN都禁用了到Switch-B的端口。这种设计的缺点是，在到分布层的两条连接中，接入交换机上只有一条被使用。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_603_0.png)

**图10-14 PVST+ 和 RSTP 非优化**

一个更好的设计如图10-15所示。然而，要获得这种拓扑，你必须调整你的网络，确保Switch-A是VLAN 1的根交换机，Switch-B是VLAN 2的根交换机。通过这种设计，你实际上可以同时利用接入层交换机到分布层交换机的两条上行链路。在图10-15中，VLAN 1将使用左侧上行链路连接，VLAN 2使用右侧上行链路连接。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_604_0.png)

**图10-15 PVST+ 和 RSTP 优化**

![](img/0eafdcebe219ef3756f0e57bcf1eb997_604_1.png)

**注意** 基于图10-15的设计，你应该确保VLAN 1的默认网关是Switch-A（假设它是三层交换机），VLAN 2的默认网关是Switch-B（假设它是三层交换机）。如果你没有这样配置，而是将默认网关关联到Switch-A，那么VLAN 2的流量将不得不从接入层交换机发送到Switch-B，然后通过EtherChannel跨接到Switch-A，才能离开子网。你可以在思科的CCNP交换课程中了解更多相关信息。

请记住，图10-15中的拓扑是由你创建的：你必须手动更改每台交换机的优先级，以创建所需的拓扑。用于实现此目标的命令包括以下内容：

```
Switch(config)# spanning-tree vlan VLAN_# root primary
Switch(config)# spanning-tree vlan VLAN_# root secondary
Switch(config)# spanning-tree vlan VLAN_# priority priority_#
```

请记住，交换机的默认优先级是32,768。第一条命令将指定VLAN的交换机优先级更改为4096。第二条命令将指定VLAN的交换机优先级更改为8192。第三条命令允许你为指定的VLAN自定义优先级。

基于图10-15中的示例，Switch-A的配置如下所示：

```
Switch-A(config)# spanning-tree mode rapid-pvst
Switch-A(config)# spanning-tree vlan 1 root primary
Switch-A(config)# spanning-tree vlan 2 root secondary
```

Switch-B的配置如下所示：

```
Switch-B(config)# spanning-tree mode rapid-pvst
Switch-B(config)# spanning-tree vlan 2 root primary
Switch-B(config)# spanning-tree vlan 1 root secondary
```

这两个配置之间唯一的区别是，主备根优先级在两个VLAN上进行了互换，以便能够使用来自接入层交换机Switch-C的两条上行链路。

**注意** 你在交换机上所做的任何STP配置更改都会立即生效，这意味着二层网络必须重新收敛，从而导致二层网络出现短暂中断。

## STP 故障排除

排查环网引起的问题可能是一项困难的任务。本节介绍一些简单的步骤，你可以据此识别并修复二层环路问题。

在讨论不同的二层环路问题之前，我想先定义一下*广播风暴*。交换机会将广播消息发送到交换机上的所有端口（接收该消息的端口除外）。如果存在二层环路，这意味着交换机之间存在多条路径，广播消息会从一台交换机转发到另一台交换机，耗尽交换机的带宽和处理能力——这就是广播风暴。广播风暴是有害的，因为它们最终会使网络瘫痪。

### 环路识别

广播风暴的一个迹象是交换机上的CPU和端口利用率非常高。如本章开头所述，你可以使用 **show processes** 和 **show process cpu** 命令检查一台或多台交换机的CPU利用率。持续的高CPU利用率可能表明存在环路。为了确认二层环路导致了问题，请使用协议分析器捕获并分析流量，以确定同一数据包是否多次出现。这通常通过将协议分析器连接到交换机，并使用交换机上的交换机端口分析器（SPAN）功能来完成，该功能会将来自接口或VLAN的帧复制到SPAN端口。一个好的协议分析器应该能够检测到环路的存在，并通知你此问题。

一旦识别出环路，为了快速恢复连通性，你应该开始禁用属于环路一部分的端口；然后诊断问题，以确定是您这边的配置问题，还是新增了二层设备导致了问题。如果你无法确定环路原因，请打开STP的调试功能（**debug spanning-tree events**）。

### 配置补救措施

为了简化故障排除过程，请根据需要禁用尽可能多的功能。例如，如果你启用了EtherChannel，禁用该通道将有助于确定通道本身是否工作不正常，并可能造成二层环路。

