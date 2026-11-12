
```
Router>enable
Router#show inventory
NAME: "2811 chassis", DESCR: "2811 chassis, Hw Serial#: FTX1022A1ZJ, Hw
Revision: 52.57"
PID: CISCO2811        , VID: V02 , SN: FTX1022A1ZJ

NAME: "c2811 Motherboard with 2FE and integrated VPN", DESCR: "c2811
Motherboard with 2FE and integrated VPN"
PID: CISCO2811        , VID: V02 , SN: FOC1020257J

NAME: "9 Port FE Switch", DESCR: "9 Port FE Switch"
PID: HWIC-D-9ESW      , VID: VN/A, SN: FOC100131K6

NAME: "WAN Interface Card - DSU/CSU T1 Fractional (V2)", DESCR: "WAN
Interface Card - DSU/CSU T1 Fractional (V2)"
PID:                  , VID: 1.2, SN: FOC10174ES5
```

请注意 **show inventory** 命令输出中，这是一台包含九个快速以太网端口的 2800 系列路由器。您还可以看到有一个 WAN 接口卡——一个内部 CSU/DSU 设备。要查看该卡是否创建了串行端口，请使用 **show ip interface brief** 命令，如下所示：

```
Router#show ip interface brief
Interface              IP-Address      OK? Method Status    Protocol
(output cut for briefness)
Serial0/2/0            13.0.0.1        YES manual up        up
Vlan1                  unassigned      YES NVRAM   up        down
```

注意 S0/2/0 的串行接口。如果您想查看该接口的详细信息，请使用 **show interface s0/2/0** 命令：

```
Router#show interface s0/2/0
Serial0/2/0 is up, line protocol is up
  Hardware is GT96K with integrated T1 CSU/DSU
  MTU 1500 bytes, BW 1544 Kbit, DLY 20000 usec,
    reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation HDLC, loopback not set
  Keepalive set (10 sec)
(output cut for briefness)
```

在此输出中，您可以看到第 1 层和第 2 层组件正常（它们处于开启状态）。串行端口由于集成的 CSU/DSU 而存在，并且封装协议为 HDLC。最后需要注意的是，回环未设置。要在此接口上执行回环测试，您需要启用回环。为此，请使用以下命令：

```
Router#config term
Router(config)#interface s0/2/0
Router(config-if)#loopback ?
    dte      Loopback on the router side of the CSU/DSU
    line     Loopback on the line side of the CSU/DSU
    remote   Loopback on the remote side of the CSU/DSU
```

要启用回环，首先导航到接口。然后您可以使用 loopback 命令并指定要执行回环测试的对象。请注意，如果您使用 dte 参数，您正在测试 CSU/DSU 的路由器侧，但如果您设置 line 参数，您正在测试 CSU/DSU 的提供商侧。如果您将参数设置为 remote，您正在测试 WAN 链路另一端的连通性。对于本次讨论，我们从路由器（DTE 设备）到 CSU/DSU 进行测试：

```
Router(config-if)#loopback dte
Loopback in progress
```

要验证回环是否已启用，请再次对串行端口使用 show interface 命令。您应该看到消息“loopback is set”而不是“loopback not set。”

现在您可以使用 Cisco 设备的扩展 ping 功能来测试连通性。扩展 ping 是一种特殊的 ping 功能，可让您控制 Cisco 设备发送的 ping 消息，并提供多种高级选项。要使用扩展 ping，请进入特权 EXEC 模式并使用不带参数的 ping 命令。Cisco 设备将提示您输入多个设置，例如让您设置 ping 消息的源地址和目标地址、要发送的 ping 消息数量以及消息的大小：

```
Router#ping
Protocol [ip]:
Target IP address: 13.0.0.1
Repeat count [5]: 50
Datagram size [100]: 1500
Timeout in seconds [2]:
Extended commands [n]: y
Source address or interface: 13.0.0.1
Type of service [0]:
Set DF bit in IP header? [no]:
Validate reply data? [no]:
Data pattern [0xABCD]:
Loose, Strict, Record, Timestamp, Verbose [none]:
Sweep range of sizes [n]:
Type escape sequence to abort.
Sending 50, 1500-byte ICMP Echos to 13.0.0.1, timeout is 2 seconds:
Packet sent with a source address of 13.0.0.1
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Success rate is 100 percent (50/50), round-trip min/avg/max = 31/39/47 ms
Router#
```

请注意，回环测试会干扰您的 WAN 链路，因此测试后应通过导航到接口并使用 **no loopback dte** 命令将其关闭。

## 练习 7-4：查看您的配置

在本练习中，您将继续进行前几次练习的配置，并通过使用多个 **show** 命令来验证配置。请参阅图 7-5。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_372_0.png)

### 图 7-5 练习的网络拓扑

- 1. 确保您已通过控制台连接到 VAN-R1。
- 2. 要查看接口的配置及其状态，请使用 **show ip interface brief** 命令。查看输出。
- 3. 要查看 g0/0 接口的状态，包括其 IP 地址以及其在第 1 层和第 2 层是否正常运行，请使用 **show interfaces g0/0** 命令。查看输出。
- 4. 要查看您的串行端口是 DCE 还是 DTE 设备，请使用 **show controllers s0/3/0** 命令。查看输出。
- 5. 将路由器的主机名更改为 *VAN-TEMP*。
- 6. 要查看 RAM 中的当前配置，请使用 **show running-config** 命令。查看输出。您应该看到主机名 VAN-TEMP 行。
- 7. 要查看 NVRAM 的配置，请使用 **show startup-config** 命令。查看输出。您应该会看到此配置仍然具有主机名 VAN-R1。这是因为您尚未将更改保存到 NVRAM。

- 8. 使用 **write** 命令将更改保存到 NVRAM。
- 9. 再次查看您的运行配置和启动配置。它们都应具有主机名 VAN-TEMP。
- 10. 将主机名改回 *VAN-R1*。
- 11. 使用 **write** 命令将更改保存到 NVRAM。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_373_0.png)

**VIDEO 7.10.** 随附本书的数字资源包含使用 Cisco 设备 ping 的多媒体演示。

## 章节回顾

本章向您介绍了 IOS CLI 以及如何在 Cisco 的交换机和路由器上进行基本配置。您的 PC 上的终端仿真器需要为控制台连接配置为 9600 bps、8 个数据位、1 个停止位、无奇偶校验和无流量控制。

交换机接口的命名方式如下：*type slot_#/port_#*。接口类型是媒体类型，例如 **Ethernet**、**FastEthernet** 或 **GigabitEthernet**。接下来是插槽号。对于 2960 交换机上的所有固定接口，插槽号始终为 *0*。所有交换机端口号从 *1* 开始递增。路由器接口的命名方式为 *type slot_#/port_#* 或 *type port_#*。接口类型是媒体类型，例如 **Ethernet**、**FastEthernet**、**GigabitEthernet** 和 **Serial**，以及许多其他类型。接下来是模块化路由器的插槽号。插槽号从 *0* 开始递增。同一插槽和类型内的所有路由器端口号从 *0* 开始递增，这与 Catalyst 交换机不同。

IOS 提供了一个 CLI，具有许多功能，包括通过使用 **help** 或 **?** 访问的上下文相关帮助、命令行历史记录和高级编辑功能，这些功能在所有模式下均可用，包括用户 EXEC、特权 EXEC 和配置模式。在配置模式下有子配置模式。要访问特权 EXEC 模式，请使用 **enable** 命令，要访问配置模式，请使用 **configure terminal。**

您可以通过分配用户 EXEC 和特权 EXEC 密码来保护对 Cisco 设备的访问。使用线路子配置模式 **password** 命令以及两个相应级别的 **enable secret** 或 **enable password** 命令。**service password-encryption** 命令在 IOS 设备上加密明文密码。**banner motd** 命令在任何用户名或密码提示之前显示登录横幅。

要查看活动配置，请使用 **show running-config** 命令。在 IOS 设备上，您必须使用 **copy running-config startup-config** 命令手动将配置保存到 NVRAM。

交换机接口默认启用，但路由器接口需要使用 **no shutdown** 命令启用。对于 DCE 串行接口，路由器需要使用 **clock rate** 命令应用时钟速率。使用 **show interfaces** 命令查看接口的状态和配置。

## 快速回顾

- 在您的终端仿真软件包中，将速度设置为 9600 bps，数据位设置为 8，停止位设置为 1，奇偶校验设置为无，流量控制设置为无。
- 接口具有名称，可能的插槽号和端口号。2960 的插槽号为 0；端口号从 1 开始。路由器插槽号从 0 开始；端口号从 0 开始，并在每个插槽内的每种类型中重新开始。

## 命令行界面 (CLI)

- 存在四种 CLI 访问模式：用户 EXEC (**IOS>**)、特权 EXEC (**IOS#**)、配置 (**IOS(config)#**) 和 ROMMON 模式 (>).
- **enable** 命令将您从用户 EXEC 移动到特权 EXEC 模式。**exit** 命令将您从 IOS 设备注销。

## IOS 基础
