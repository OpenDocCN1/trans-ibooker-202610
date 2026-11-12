
如果在访问交换网络中的设备时遇到问题，可以查看几个选项。首先，您尝试访问的设备是否在同一 VLAN 中？如果是，请确保在 VLAN 中使用正确的 IP 寻址方案，并且尝试共享信息的两台设备的端口位于同一 VLAN 中。如果这两台设备是 Cisco 设备，可以使用 CDP 获取一些这些信息（例如 IP 地址），方法是使用 **show cdp** 命令（第 8 章中讨论）。交换机是否了解您网络中的设备？您可能需要检查 CAM 表，并确保端口安全违规没有导致连接问题。

有关 VLAN 信息，请在交换机上使用 **show** 命令检查 VLAN 配置。还要检查每个交换机上的 VLAN 配置，并确保使用 **show vlan** 命令配置的 VLAN 参数相同。如果端口 LED 为常亮琥珀色，问题可能是端口的 VLAN 被删除了且端口未重新分配。其他问题也会导致端口 LED 变为琥珀色，例如 STP 将端口置于阻塞状态，或端口安全因安全违规禁用端口。实际上，如果看到很多端口 LED 为琥珀色，那么被删除的 VLAN 很可能是问题所在。使用 **show interface switchport** 命令来确定是否是被删除的 VLAN 导致了问题：如果看到分配给端口的 VLAN 处于非活动状态，如下所示，那么您就找到了罪魁祸首：

```
switch# show interface fastEthernet0/1 switchport
Name: Fa0/1
Switchport: Enabled
Administrative mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: native
Negotiation of Trunking: Off
Access Mode VLAN: 5 (Inactive)
Trunking Native Mode VLAN: 1 (default)
.
.
.
```

注意在此示例中，VLAN 5 处于非活动状态，这表明接口已分配给该 VLAN，但 VLAN 已被删除，导致接口无法使用。

如果在交换机之间使用干线，请确保干线配置正确：**show interface trunk**。确保干线两端的本机 VLAN 编号匹配：如果不匹配，干线将不会形成。

还要通过执行 **show vtp** 命令检查 VTP（如果正在使用它）。确保交换机之间有干线连接，因为 VTP 消息仅通过干线传输。使用服务器/客户端实现时，请确保 VTP 交换机之间的域名以及（如果使用密码选项）密码匹配。此外，所有交换机必须运行相同的 VTP 版本（默认为版本 1）。

## VLAN 间连接问题

如果在访问其他 VLAN 中的设备时遇到问题，请首先确保能够 ping 通作为 VLAN 出口的默认网关（路由器）。用户 PC 上常见的配置错误是默认网关配置错误。如果无法 ping 通默认网关，请返回上一节并检查本地 VLAN 连接问题。如果可以 ping 通网关，请检查路由器的配置及其接口。还要确保路由器到目标 VLAN 有路由（**show ip route**）。如果确实有到达目标的路由，请确保目标 VLAN 配置正确，并且该 VLAN 中的默认网关能够访问目标设备。

## 练习 9-6：配置 VLAN

在此练习中，您将练习跨两台 Cisco 交换机配置 VLAN。创建 VLAN 后，需要创建干线端口，以便 VLAN 流量可以在两台交换机之间传输。本练习中，温哥华办公室的两台交换机配置如下图所示：

![](img/0eafdcebe219ef3756f0e57bcf1eb997_543_0.png)

请注意，ComputerA 连接到 VAN-SW1 的端口 5，而 ComputerB 连接到 VAN-SW1 的端口 15。ComputerC 连接到 VAN-SW2 的端口 5，而 ComputerD 连接到 VAN-SW2 的端口 15。

1. 确保两台交换机都已通电。

2. 确保 ComputerA 和 ComputerD 的 IP 地址分别为 12.0.0.5 和 12.0.0.15，ComputerB 和 ComputerC 的 IP 地址分别为 14.0.0.5 和 14.0.0.15。

3. 使用交叉电缆将 VAN-SW1 的端口 1 连接到 VAN-SW2 的端口 1。

4. 使用交叉电缆将 VAN-SW1 的端口 24 连接到 VAN-SW2 的端口 24。等待一分钟，直到绿色链路指示灯亮起。请注意，由于交换机的环路防止功能，一个链路指示灯将保持琥珀色（下一章将详细介绍）。

5. 在 VAN-SW1 上，创建 VLAN ID 为 2 的 MKT VLAN 和 VLAN ID 为 3 的 SALES VLAN。请使用以下命令作为指南：

```
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-SW1(config)#vlan 2
VAN-SW1(config-vlan)#name MKT
VAN-SW1(config-vlan)#vlan 3
VAN-SW1(config-vlan)#name SALES
VAN-SW1(config-vlan)#exit
VAN-SW1(config)#exit
VAN-SW1#
```

6. 使用 show vlan 命令查看 VLAN 列表。请注意，VLAN 2 和 VLAN 3 现在已存在，但未向 VLAN 分配任何端口。

7. 使用以下命令将 ComputerA（端口 5）放入 MKT VLAN：

```
VAN-SW1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-SW1(config)#interface g1/0/5
VAN-SW1(config-if)#switchport mode access
VAN-SW1(config-if)#switchport access vlan 2
```

8. 使用以下命令将 ComputerB（端口 15）放入 SALES VLAN：

```
VAN-SW1(config-if)#interface g1/0/15
VAN-SW1(config-if)#switchport mode access
VAN-SW1(config-if)#switchport access vlan 3
VAN-SW1(config-if)#exit
VAN-SW1(config)#exit
VAN-SW1#
```

9. 查看 VLAN 列表，并注意 VLAN 2 和 VLAN 3 现在已有端口。

10. 连接到 VAN-SW2 的控制台端口，并在 PuTTY 中按 Enter 刷新屏幕。

11. 在 VAN-SW2 上，执行相同的任务。创建 VLAN ID 为 2 的 MKT VLAN，并创建 VLAN ID 为 3 的 SALES VLAN。

12. 在 VAN-SW2 上，将端口 15 放入 MKT VLAN，将端口 5 放入 SALES VLAN。

13. 查看 VLAN，并确保端口 15 在 MKT VLAN 中，端口 5 在 SALES VLAN 中。

14. 转到 ComputerA 的命令提示符。您可以 ping 通 12.0.0.15 (ComputerD) 吗？________
    您不应该能够 ping 通 ComputerD，因为需要将两台交换机之间的链路设置为干线端口，以便它们在两台交换机之间传输所有 VLAN 流量。

15. 在 VAN-SW1 上，使用以下命令将端口 1 配置为干线端口：

```
VAN-SW1>enable
VAN-SW1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-SW1(config)#interface g1/0/1
VAN-SW1(config-if)#switchport trunk encapsulation dot1q
VAN-SW1(config-if)#switchport mode trunk
```

16. 在 VAN-SW1 上，还要将端口 24 配置为干线端口：

```
VAN-SW1(config-if)#interface g1/0/24
VAN-SW1(config-if)#switchport trunk encapsulation dot1q
VAN-SW1(config-if)#switchport mode trunk
```

17. 要查看干线端口的状态，请使用 show interfaces trunk 命令并回答以下问题：
    端口 1 和端口 24 的状态是什么？________________
    哪些 VLAN 流量被允许通过干线端口传输？________________

18. 关闭 VAN-SW1 选项卡。

19. 转到 ComputerA 并 ping 12.0.0.15 (ComputerD)。您能够 ping 通吗？________
    现在允许 VLAN 流量穿越干线端口，因此 ping 消息应发送到另一台交换机上的 ComputerD。

20. 从 ComputerA，您能够 ping 通 14.0.0.15 (ComputerB) 吗？________
    请注意，ComputerB 位于同一交换机上，但您无法 ping 通它，因为它位于不同的 VLAN。VLAN 是一个通信边界。

## 章节回顾

本章侧重于特定于 Cisco 交换机的基本配置任务。回顾了交换机的物理特征，包括 LED 的含义和 Mode 按钮的使用。

您了解了交换机的核心功能和交换机操作模式。然后，您了解了交换机上的基本配置设置，例如更改交换机名称以及在交换机上配置 IP 地址和默认网关。随后是交换机端口的配置。

然后，您了解了端口安全功能，这是一种防止未经授权的设备连接到网络的方法。您了解了不同的端口违规模式以及配置端口安全的命令。

作为本章的最后一个主题，您了解了 VLAN 作为在设备之间创建广播域和通信边界的方法。您了解到 VLAN 可以跨连接的交换机，并且需要配置干线端口来在交换机之间传输 VLAN 流量。

## 快速回顾

### 了解交换机功能

- 交换机有三个核心功能：地址学习、过滤/转发和环路避免。
- 二层交换机有三种操作模式：存储转发、直通和无碎片转发。Cisco 交换机使用无碎片转发。

### 识别交换机组件

- 如果交换机在启动期间发生故障，系统 LED 将显示琥珀色。
- 使用 Mode 按钮在交换机上切换显示模式。
- 在 STAT 显示模式下，如果端口 LED 为琥珀色，则端口已因关闭、STP 阻塞或端口安全而被禁用。
