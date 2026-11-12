

![](img/0eafdcebe219ef3756f0e57bcf1eb997_497_0.png)

注意：在较旧的 Cisco 交换机上，例如 2960，请使用 **show mac-address-table** 和 **clear mac-address-table** 命令。

## 静态 MAC 地址

除了让交换机动态学习 MAC 地址外，您还可以手动创建静态条目。静态条目的优点在于它们属于交换机配置的一部分，因此即使交换机重新启动，这些条目也会保留（而动态条目在重新启动后会丢失，需要重新学习）。

要在 MAC 地址表中添加静态条目，请使用以下语法：

```
Switch(config)# mac address-table static MAC_address
                vlan VLAN_#
                interface type module/port_#
```

例如，您可以使用以下命令为端口 8 添加静态条目：

```
Switch(config)# mac address-table static 0000.1111.2222 vlan 1 int g1/0/8
```

除了指定设备的 MAC 地址和设备所在的接口外，还必须指定设备所在的 VLAN（有关 VLAN 的更多信息，请参见第 10 章）。使用 **show mac-address-table** 命令查看新条目。要从 CAM 表中移除静态条目，请在命令前加上 **no** 参数：**no show mac-address-table**。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_498_0.png)

注意：在交换机上静态配置 MAC 地址当前并不常见。如果已配置，静态条目通常用于网络设备，例如服务器和路由器。如果您担心控制哪些设备连接到交换机的特定端口，请使用端口安全功能。

## 练习 9-3：使用 MAC 地址表

在本练习中，您将查看 Cisco 交换机上的 MAC 地址表（也称为 CAM 表），并向 MAC 地址表添加静态 MAC 地址。

1. 连接到 VAN-SW1 的控制台端口，并在 PuTTY 中按 ENTER 刷新提示符。
2. 要查看包含交换机已学习的所有地址列表的 MAC 地址表，请输入以下命令：

```
VAN-SW1>enable
VAN-SW1#show mac address-table
```

   请注意，交换机端口 1 上仅学习了一个地址（即连接到路由器的端口）。要验证交换机是否会动态学习 MAC 地址，请从 ComputerA 对路由器执行 ping 操作，然后再次检查 MAC 地址表。

3. 打开 ComputerA 上的命令提示符，并输入 **ping 12.0.0.1** 对路由器执行 ping 操作。您应该会收到响应。
4. 返回 VAN-SW1 交换机的控制台窗口。
5. 再次执行 **show mac address-table** 命令。注意连接到交换机端口 5 的设备的 MAC 地址。还请注意，这两个条目都是动态条目，因为它们是基于网络流量学习的。
6. 要创建静态条目并将其与端口 3 关联，请输入以下命令：

```
VAN-SW1#config term
VAN-SW1(config)# mac address-table static 0000.1111.2222 vlan 1 int g1/0/3
```

7. 查看 MAC 地址表，验证静态地址是否已添加。

## 接口配置

现在我们已经介绍了交换机的基本配置设置，接下来将介绍如何配置交换机上的端口。在以下部分中，您将学习如何在端口上配置基本设置，然后学习如何使用端口安全功能限制哪些系统可以连接到端口。

### 配置端口

您将使用几个常用命令来配置交换机上的端口。在这里，您将学习添加端口描述、配置交换机速率、配置双工设置、启用和禁用端口以及验证端口设置的命令。

### 添加端口描述

有时，您可能希望向端口添加描述，以指明端口的用途。例如，如果 Web 服务器连接到某个端口，您可能希望添加“Web Server”描述标签。以下代码示例将端口标记为“UpLink Port”，因为它连接到另一台交换机：

```
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line.  End with CNTL/Z.
VAN-SW1(config)#interface GigabitEthernet1/0/1
VAN-SW1(config-if)#description UpLink Port
```

向端口添加描述后，使用 **show interface g1/0/1** 命令查看接口状态时，您将看到该标签。

### 配置交换机速率

当今大多数交换机的端口都支持 10 Mbps（以太网）、100 Mbps（快速以太网）和 1000 Mbps（千兆以太网）。交换机会检测连接到它的设备的速率，并以该速率运行端口。例如，如果您连接一台支持 100 Mbps 的网络接口卡（NIC）的计算机，交换机会检测到这一点并将端口配置为 100 Mbps。这个 *自动检测功能* 设置了端口的速率。要手动配置端口速率，可以使用接口全局配置命令导航到该端口：

```
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-SW1(config)#interface g1/0/1
```

如果您不确定接口支持哪些速率，可以在 **speed** 命令上使用帮助功能，如下所示：

```
VAN-SW1(config-if)#speed ?
  10    Force 10 Mbps operation
  100   Force 100 Mbps operation
  1000  Force 1000 Mbps operation
  auto  Enable AUTO speed configuration
```

请注意，支持 10/100/1000 速率，如果您希望交换机自动检测速率（默认值），可以将速率设置为 auto。因此，要将端口配置为以 1000 Mbps 运行，请使用以下命令：

```
VAN-SW1(config-if)#speed 1000
```

要一次性在多个端口上配置这些设置，请使用 **interface range** 命令选择多个端口，然后更改设置。在本示例中，我设置了所有 24 个千兆端口的速率：

```
VAN-SW1(config)#interface range g1/0/1 - 24
VAN-SW1(config-if-range)#speed 1000
```

### 配置双工设置

您可以使用接口上的 **duplex** 命令配置端口的双工设置。此设置也具有自动检测功能，可检测连接设备的双工设置，并将端口的双工设置设置为匹配。请注意，您可以将双工设置设置为 **全双工** 以在端口上同时发送和接收数据，或将其设置为 **半双工** 以发送和接收数据，但不能同时进行：

```
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-SW1(config)#interface g1/0/1
VAN-SW1(config-if)#duplex ?
  auto  Enable AUTO duplex configuration
  full  Force full duplex operation
  half  Force half-duplex operation
VAN-SW1(config-if)#duplex full
```

### 启用和禁用端口

您会不时想要在交换机上禁用或启用端口。例如，您可能出于安全原因决定禁用端口，以便在启用它之前没有人可以使用该端口。本节将向您展示如何在交换机上禁用和启用端口。

### 禁用接口

要禁用端口，请导航到要禁用的端口的接口，然后使用 **shutdown** 命令。执行此命令后，您将收到反馈，指示端口的状态已更改为“administratively down”。这意味着管理员已禁用该端口；当端口显示状态为“down”时，表示端口因配置或协议错误而无法工作。以下是示例：

```
VAN-SW1>enable
VAN-SW1#config term
```

同样，请不要忘记，您可以使用 **interface range** 命令一次配置多个端口。在本示例中，我要禁用交换机上的千兆端口 1 和 2，因此可以先使用 **interface range** 命令选择它们，然后使用 **shutdown** 命令：

```
VAN-SW1(config-if)#interface range g1/0/1 - 2
VAN-SW1(config-if-range)#shutdown
%LINK-5-CHANGED: Interface GigabitEthernet1/0/1, changed state to administratively down
%LINK-5-CHANGED: Interface GigabitEthernet1/0/2, changed state to administratively down
```

### 启用接口

如果端口已被禁用（administratively down），可以使用 **no shutdown** 命令将其启用。请记住，在 Cisco 交换机上，通常通过在命令前放置 **no** 单词来撤销某项设置：

```
VAN-SW1(config-if)#interface g1/0/1
VAN-SW1(config-if)#no shutdown
```

同样，要启用多个接口，可以使用 **interface range** 命令选择接口，然后使用 **no shutdown** 命令：

```
VAN-SW1(config-if)#interface range g1/0/1 - 2
VAN-SW1(config-if-range)#no shutdown
%LINK-5-CHANGED: Interface GigabitEthernet1/0/1, changed state to up
%LINK-5-CHANGED: Interface GigabitEthernet1/0/2, changed state to up
```

### 验证端口设置

在接口上配置所需设置后，必须验证设置是否正确。在排查通信问题时，通常希望使用 **show interfaces** 命令检查接口状态，然后指定要检查的接口（否则，您将获得所有接口及其设置的列表）。以下是示例：

