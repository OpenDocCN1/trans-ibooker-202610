

**hostname**命令。该更新于4秒前通过本设备的Ethernet0/0接口接收（180秒的保持时间减去176秒的保持时间）。Port ID指的是对端设备通告CDP消息所用的端口。

可以在上述命令后添加可选的**detail**参数，以查看所连接的思科设备的详细信息。也可以使用`show cdp entry *`命令。下面是CDP详细列出的示例：

```
Router# show cdp neighbor detail
--------------------------
Device ID: Router-A
Entry address(es):
  IP address: 192.168.1.1
Platform: cisco 2621,  Capabilities: Router
Interface: Ethernet0/0,  Port ID (outgoing port): FastEthernet0/1
Holdtime : 127 sec

Version :
Cisco Internetwork Operating System Software
IOS (tm) C2600 Software (C2600-IK903S3-M), Version 12.2(15)T9,
        RELEASE SOFTWARE (fc2)
TAC Support: http://www.cisco.com/tac
Copyright (c) 1986-2003 by cisco Systems, Inc.
Compiled Sat 01-Nov-03 04:43 by ccai

advertisement version: 2
Duplex: half
```

在此示例中，可以看到所连接的设备是一台运行IOS 12.2(15)T9的2621系列路由器，并在所连接的接口上配置了IP地址192.168.1.1。

要列出特定邻居的详细信息，请使用以下命令：

```
Router# show cdp entry neighbor's_name
```

这种方法与前面示例中的方法相比，其优点在于此命令仅列出指定邻居的信息。可以使用星号（*）作为通配符来显示所有邻居。以下是此命令的示例：

```
Router1# show cdp entry Router2
-----------------------------
Device ID: Router2
Entry address(es):
  IP address: 10.1.2.1
Platform: Cisco 2610, Capabilities: Router
Interface: Serial0/0, Port ID (outgoing port): Serial0/1
Holdtime: 125 sec
```

在此示例中，Router2的IP地址为10.1.2.1，Router1的serial0/0接口连接到Router2的serial0/1接口。

IOS设备还支持另一个CDP命令，可用于查看CDP流量统计信息：

```
IOS# show cdp traffic
Total packets output: 350, Input: 223
Hdr syntax: 0, Chksum error: 0, Encaps failed: 0
No memory: 0, Invalid: 0, Fragmented: 0
```

如果你正在接收CDP流量（每次执行命令时Input参数都会递增，每分钟执行一次），则说明数据链路层运行正常。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_443_0.png)

**考试提示** 请记住，CDP默认在所有思科设备上启用。CDP更新每60秒以组播方式生成一次，对于缺失的邻居，保持时间为180秒。**no cdp run**命令全局禁用CDP，而**no cdp enable**命令在接口上禁用CDP（应在连接到Internet或不可信设备的设备接口上执行此操作）。使用**show cdp neighbors**列出直连的思科邻居设备。添加**detail**参数将显示第3层寻址、设备型号、软件版本以及邻居上配置的其他信息。考试中要熟悉**show cdp**命令的输出。

## 使用LLDP

CDP的缺点之一是它是思科专有协议。如果你的环境中混合了思科设备和非思科设备，CDP将无法为你发现非思科设备。IEEE已为设备发现创建了行业标准协议，即链路层发现协议（LLDP），也称为IEEE 802.1ab标准。

当思科设备上运行LLDP时，你可以像使用CDP一样，用它来发现任何其他运行LLDP的设备。LLDP默认不在思科设备上启用，但可以通过以下命令启用：

```
Router(config)# lldp run
```

启用后，LLDP将应用于所有接口。可以使用全局配置命令`no lldp run`禁用LLDP。如果希望在特定接口上禁用LLDP，可以进入该接口并禁用LLDP的发送和接收：

```
Router(config-if)# no lldp transmit
Router(config-if)# no lldp receive
```

当然，你可以使用`lldp transmit`命令和`lldp receive`命令在接口上启用LLDP通信。但请记住，当你使用`lldp run`命令启用LLDP后，LLDP流量在所有接口上默认同时启用发送和接收。

你可以使用`show lldp`命令来验证思科设备上是否运行LLDP，如下例所示。请注意，可以看到LLDP已激活，并且默认每30秒发送一次通告消息。

```
Router# show lldp
Global LLDP Information:
  Status: ACTIVE
  LLDP advertisements are sent every 30 seconds
  LLDP hold time advertised is 120 seconds
  LLDP interface reinitialisation delay is 2 seconds
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_445_0.png)

**考试提示** 对于CCNA考试，请记住LLDP是用于多供应商环境的行业标准发现协议，而CDP是仅用于思科设备环境的思科专有协议。

## 第3层连通性测试

CDP在确定数据链路层是否与另一台直连的思科设备正常工作时非常有用，或者通常用于通过发现网络上的不同设备来创建网络拓扑图。你甚至可以看到邻居设备上配置的第3层地址，并使用它来测试第3层连通性。除了使用CDP之外，你还可以使用**show interfaces**命令进行数据链路层测试（参见第9章）。

然而，这两个工具的主要局限性在于它们不测试第3层问题，尽管思科确实提供了用于测试第3层连通性的工具。本节重点介绍两个命令：**ping**和**traceroute**。这两个命令都有两个版本：一个用于用户EXEC模式，一个用于特权EXEC模式。特权EXEC版本提供了额外的选项和参数，可帮助你进行故障排除。以下各节将更深入地介绍这些工具。

## 使用Ping

Ping（分组网间探测器）是一种命令实用程序，可用于验证思科设备上的第3层连通性。Ping使用ICMP作为状态报告协议，以识别系统是否已启动并运行。你可以通过在思科设备上输入`ping <ip_address>`来使用**ping**命令。

用户也可以从Windows桌面使用**ping**命令来检查连通性。以下是一些指示问题及可能原因的消息：

- **目标主机不可达** 连接到远程主机的路由器无法在连接的网段上联系到该主机。
- **目标网络不可达** 源和目标之间的某台路由器没有用于确定如何到达目标的路由表条目。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_446_0.png)

**考试提示** 请记住，对于Cisco CCT和CCNA考试，Cisco IOS的**ping**和**traceroute**命令都测试第3层连通性。

使用**ping**命令执行通信测试时，如果目标可达，目标会为源发送的每个回显请求回复一个回显应答消息。如果目标不可达，则中间路由器（如果存在）将回复目标网络不可达消息，指示问题开始的位置。如果路由器连接到目标主机所在的远程网段，而该路由器无法到达目标主机，则会导致“目标主机不可达”消息。另请注意，如果ICMP被防火墙阻止，你可能不会收到应答。

### 简单ping命令

要在用户EXEC模式或特权EXEC模式下执行简单的**ping**，请在CLI上输入**ping**命令，后面跟上目标的IP地址或主机名：

```
IOS> ping destination_IP_address_or_host_name
```

以下是使用此命令的简单示例：

