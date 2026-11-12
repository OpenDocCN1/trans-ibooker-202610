
默认情况下）。这是合理的，因为连接到SwitchB的PC-E和PC-F位于同一VLAN中。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_564_0.png)

图10-1 无VTP剪枝的中继

图10-1展示了一个包含两个成员的第二个VLAN：PC-C和PC-D。如果PC-C产生本地广播，SwitchA显然会将其发送到PC-D的端口。不合理的是，SwitchA会将该广播从中继端口泛洪到SwitchB，尽管SwitchB上没有设备属于该VLAN。这是浪费带宽和资源的一个例子。在示例中，单个广播不是什么大问题；然而，如果来自PC-A的视频组播流为5 Mbps，网络在中继上可能会遇到吞吐量问题，因为交换机对待组播就像对待广播一样——它会将其泛洪到与源端口VLAN关联的所有端口。

你可以使用两种方法之一来解决这个问题：静态VLAN剪枝或动态VLAN剪枝。通过静态配置，你可以手动将非活动VLAN从两台交换机的中继上剪除，如图10-2所示。请注意，在此图中，深色VLAN（用虚线表示）已从中继上被剪除。手动剪枝的问题在于，如果你向SwitchB添加一个深色VLAN成员，你将不得不登录到两台交换机，并手动将该被剪除的VLAN添加回中继。在具有多个VLAN的多交换机网络中，这可能会变得非常混乱，因为并非每个VLAN在每台交换机上都处于活动状态。很容易意外地从一个不应被剪除的中继上剪除某个VLAN，从而造成连通性问题。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_565_0.png)

图10-2 VLAN剪枝

## 带剪枝的中继

VTP剪枝功能使交换机能够共享额外的VLAN信息，并动态地从中继连接中剪除非活动VLAN。在这种情况下，交换机之间会共享哪些VLAN处于活动状态。例如，SwitchA告诉SwitchB它有两个活动VLAN（白色VLAN和深色VLAN）。另一方面，SwitchB只有一个活动VLAN，并将这一事实告知SwitchA。基于共享的信息，SwitchA和SwitchB都意识到深色VLAN在它们的中继连接上不活动，因此深色VLAN应被动态地从中继配置中移除。

此功能的好处在于，如果你碰巧通过将设备连接到SwitchB的某个端口并将该端口分配给深色VLAN来激活SwitchB上的深色VLAN，SwitchB将通知SwitchA这一新激活的VLAN，并且两台交换机会动态地将该VLAN添加回中继配置中。这将使PC-C、PC-D和新设备能够相互发送帧，如图10-3所示。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_566_0.png)

图10-3 VTP剪枝在中继上激活VLAN

![](img/0eafdcebe219ef3756f0e57bcf1eb997_567_0.png)

**注意** 默认情况下，所有VLAN都可以通过中继传输。VTP剪枝用于在中继连接上动态删除两台交换机之间不活动的VLAN。它必须在VTP服务器交换机上启用，并且其他交换机必须是服务器或客户端。

只有处于*服务器*模式的VTP交换机才能启用VTP剪枝，域中的其余交换机必须处于VTP服务器或客户端模式。如果你有透明模式交换机，则必须手动从其中继链路上剪除VLAN。

## 管理VLAN

与Cisco路由器不同，每台Cisco交换机都带有默认配置。例如，交换机上已经存在一些预配置的VLAN，包括VLAN 1。在配置过程中，所有VLAN命令都引用VLAN编号，尽管你可以为VLAN配置可选名称。默认情况下，交换机上的每个端口都与VLAN 1关联。并且来自交换机本身的所有通信——VTP消息、Cisco发现协议（CDP）组播以及交换机产生的其他流量——都发生在VLAN 1中。回顾第9章可知，Cisco交换机的IP配置基于你为其配置IP地址的VLAN接口。

VLAN 1有时被称为*管理VLAN*，尽管你也可以使用不同的VLAN。通常的做法是将所有管理设备——交换机和网管工作站——放在它们自己的VLAN中。如果你决定将交换机放在不同于VLAN 1的VLAN中，建议你在所有管理设备上更改此配置，以便更容易地保护它们，因为其他VLAN必须通过三层设备才能访问它们；并且在这个三层设备上，你可以设置访问控制列表来过滤不需要的流量。

所有交换机都应处于同一管理VLAN中，这一点很重要，因为交换机的许多管理协议（如CDP、VTP和动态中继协议（DTP））都发生在交换机的管理VLAN内。如果一台交换机的管理VLAN设置为1，而另一台连接的交换机的管理VLAN设置为2，则这两台交换机会失去很多交换机间功能。

## 配置VTP

无论是排查现有网络设置的问题，还是自行配置VTP，了解常见的VTP参数及其值都很重要。表10-2显示了Cisco交换机的默认VTP配置。以下各节将介绍这些VTP参数的配置。

| VTP组件 | VTP默认值 |
|---|---|
| 域名 | 无 |
| 模式 | 服务器 |
| 密码 | 无 |
| 剪枝 | 禁用 |
| 版本 | 1 |

表10-2 VTP默认配置值

你在Cisco交换机的全局配置模式下配置VTP：

```
switch(config)# vtp domain VTP_domain_name
switch(config)# vtp mode server|client|transparent
switch(config)# vtp password VTP_password
switch(config)# vtp pruning
```

vtp domain 命令为你的交换机定义域名。请记住，为了使交换机共享VTP信息，它们必须位于同一域中。从其他域接收的消息将被忽略。如果你没有配置域名，交换机将从服务器通告中学习到该域名。

配置中的其余命令是可选的。第二条 vtp 命令定义交换机的VTP模式。如果你不配置此命令，默认模式为服务器模式。你可以为交换机配置VTP MD5密码，该密码必须与域中每台交换机上配置的密码匹配。交换机将使用此密码来验证来自其他交换机的VTP消息；如果VTP消息中携带的哈希值（通过将VTP消息和密码运行MD5生成哈希签名）无法通过验证，交换机将忽略这些VTP消息。在大多数交换机上，剪枝默认是禁用的，但你可以使用**vtp pruning**命令来禁用或启用它。需要注意的是，如果在服务器交换机上启用了剪枝，该服务器交换机会将此信息传播到同一域中的所有其他服务器和客户端交换机。

完成VTP配置后，使用**show vtp status**命令检查你的配置：

```
switch# show vtp status
VTP Version : 1
Configuration Revision : 17
Maximum VLANs supported locally : 255
Number of existing VLANs : 7
VTP Operating Mode : Server
VTP Domain Name : dealgroup
VTP Pruning Mode : Enabled
VTP V2 Mode : Disabled
VTP Traps Generation : Disabled
MD5 digest : 0x95 0xAB 0x29 0x44 0x32 0xA1 0x2C 0x31
Configuration last modified by 0.0.0.0 at 3-1-03 15:18:37
Local updater ID is 192.168.1.4 on interface Vlan1
(lowest numbered VLAN interface found)
```

在此示例中，已经发生了17次配置更改（请参阅配置修订号字段）。该交换机正在dealgroup域中以服务器模式运行。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_569_0.png)

**注意** **vtp password**命令用于验证交换机之间的VTP消息。**show vtp status**命令将显示

