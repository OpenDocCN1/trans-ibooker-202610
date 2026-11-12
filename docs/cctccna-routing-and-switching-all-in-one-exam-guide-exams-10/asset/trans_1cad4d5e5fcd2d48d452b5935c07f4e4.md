

IPv6 使用组播，但使用 FF02::A IPv6 组播链路本地地址。本节对 EIGRP for IPv6 的配置提供了扼要的介绍。

## EIGRP for IPv6 全局配置

以下是启用 EIGRP for IPv6 的基本全局命令：

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router eigrp autonomous_system
IOS(config-rtr)# eigrp router-id router_ID
IOS(config-rtr)# [no] shutdown
```

第一条命令在路由设备上启用 IPv6。第二条命令指定路由设备关联的自治系统。与 EIGRP for IPv4 类似，两台路由器之间的自治系统编号必须匹配，否则它们不会建立邻居关系。eigrp router-id 命令定义路由器的 32 位 ID。您可以将其表示为十进制或点分十进制数字，例如 10.1.1.1。shutdown 命令禁用或启用 EIGRP 进程。根据 IOS 版本，默认值可能不同（此内容已在第 11 章中讨论）。

## EIGRP for IPv6 接口配置

在配置好 EIGRP for IPv6 的全局属性后，必须将接口放入本地进程。与在 EIGRP 路由进程配置中使用 network 命令的 EIGRP for IPv4 不同，IPv6 的配置与 RIPng 和 OSPFv3 相似，即在接口下进行配置：

```
IOS(config)# interface type [slot_#]/port_#
IOS(config-if)# ipv6 eigrp autonomous_system
```

## EIGRP for IPv6 简单配置

以下是将两个接口置于自治系统 100 的配置示例：

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router eigrp 100
IOS(config-router)# eigrp router-id 100.1.1.1
IOS(config-router)# no shutdown
IOS(config)# interface g1/0/1
IOS(config-if)# ipv6 eigrp 100
IOS(config-if)# exit
IOS(config)# interface g1/0/2
IOS(config-if)# ipv6 eigrp 100
IOS(config-if)# exit
```

如您所见，配置相当简单，且与 OSPFv3 的配置非常相似。请记住，您的 Cisco 设备不需要在接口上配置全球 IPv6 地址即可参与 EIGRP for IPv6——它只需要一个链路本地地址，假设 IPv6 已全局启用且接口已启用，它将自动获取该地址。

## EIGRP for IPv6 验证

要验证 EIGRP for IPv6 的邻居关系，请使用 **显示 IPv6 EIGRP 邻居** 命令：

```
IOS# show ipv6 eigrp neighbors
IPv6-EIGRP neighbors for AS (100)
H Address          Intf   Hold   Uptime   SRTT  RTO  Q   Seq
                     (sec)  (ms)                Cnt  Num
0 Link-local address: Et0/0 14 00:00:13 11 200 0 2
FE80::A8BB:CCFF:FE00:200
```

在此示例中，一个邻居通过自治系统 100 中的接口 E0/0 连接。

要显示 EIGRP IPv6 拓扑表中的条目，请使用 **显示 IPv6 EIGRP 拓扑** 命令：

```
IOS# show ipv6 eigrp topology
IPv6-EIGRP Topology Table for AS(1)/ID(2001:0DB8:10::/64)
Codes: P - Passive, A - Active, U - Update, Q - Query, R - Reply,
r - reply Status, s - sia Status
P 2001:0DB8:3::/64, 1 successors, FD is 281600
  via Connected, Ethernet1/0
```

在此示例中，拓扑表中有一条继任者路由。

要查看路由表中的 EIGRP 路由，请使用 **显示 IPv6 EIGRP 路由** 命令。与 IPv4 类似，EIGRP for IPv6 路由以 **D** 标识。所有这些 **显示** 命令的输出与相应的 IPv4 命令非常相似。

## 练习 14-1：配置 IPv6 静态路由

在本练习中，您将在 VAN-R1 和 LA-R1 路由器上配置 IPv6 地址，然后在路由器上配置 IPv6 静态路由，以便数据可以通过网络。本章所有练习的网络图如下图所示：

![](img/0eafdcebe219ef3756f0e57bcf1eb997_856_0.png)

- 1. 确保所有网络设备已启动。
- 2. 连接到 VAN-R1 的控制台端口。
- 3. 在 VAN-R1 路由器上，通过查看其状态来验证 Gig0/0 和 Serial0/3/0 接口是否已启动。
- 4. 要在 VAN-R1 上启用 IPv6 路由，请键入以下命令：

```
VAN-R1>enable
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#ipv6 unicast-routing
```

- 5. 要为 Gig0/0 和 Serial0/3/0 分配 IPv6 地址，请使用以下命令：

```
VAN-R1(config)#interface g0/0
VAN-R1(config-if)#ipv6 address 2001:aaaa:bbbb:1::1/64
VAN-R1(config-if)#no shutdown
VAN-R1(config-if)#interface s0/3/0
VAN-R1(config-if)#ipv6 address 2001:aaaa:bbbb:2::1/64
VAN-R1(config-if)#no shutdown
```

请记下每个接口的 IP 地址。

- 6. 要验证每个接口上的 IPv6 地址，请使用 show ipv6 interface brief 命令：

```
VAN-R1(config-if)#exit
VAN-R1(config)#exit
VAN-R1#show ipv6 interface brief
GigabitEthernet0/0                 [up/up]
  FE80::209:7CFF:FE04:AB01
  2001:AAAA:BBBB:1::1
Serial0/3/0                        [up/up]
  FE80::209:7CFF:FE04:AB01
  2001:AAAA:BBBB:2::1
```

请记下分配给每个接口的全球单播地址和链路本地地址。

- 7. 切换到 LA-R1 路由器的控制台端口并导航到全局配置模式。
- 8. 在 LA-R1 路由器上启用 IPv6 路由。
- 9. 在 LA-R1 路由器的串行接口上配置 IPv6 地址 2001:aaaa:bbbb:2::2/64。
- 10. 在 LA-R1 路由器的千兆接口上配置 IPv6 地址 2001:aaaa:bbbb:3::1/64。
- 11. 以下汇总了您应使用的所有命令：

```
LA-R1>enable
LA-R1#config term
LA-R1(config)#ipv6 unicast-routing
LA-R1(config)#interface s0/3/0
LA-R1(config-if)#ipv6 address 2001:aaaa:bbbb:2::2/64
LA-R1(config-if)#no shutdown
LA-R1(config-if)#interface g0/0
LA-R1(config-if)#ipv6 address 2001:aaaa:bbbb:3::1/64
LA-R1(config-if)#no shutdown
LA-R1(config-if)#exit
LA-R1(config)#exit
LA-R1#
```

通过输入 **显示 IPv6 接口简要信息** 命令验证 LA-R1 上的 IPv6 地址。

- 12. 通过运行 **显示 IPv6 路由** 命令验证 LA-R1 上的 IPv6 路由表。
- 13. 要测试 LA-R1 和 VAN-R1 路由器之间 WAN 链路的连通性，请使用以下 **ping** 命令，该命令应成功：

```
LA-R1#ping 2001:aaaa:bbbb:2::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:aaaa:bbbb:2::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/3/12 ms
```

- 14. 要通过 ping 千兆接口来测试 LA-R1 与温哥华网络之间的连通性，请运行以下命令（请注意，ping 不应成功，因为 LA-R1 不知道如何到达 :1 子网）：

```
LA-R1#ping 2001:aaaa:bbbb:1::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:aaaa:bbbb:1::1, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
```

- 15. 要能够 ping 通每个网络，您需要将静态 IPv6 路由添加到路由表中。在 LA-R1 上，通过输入以下命令向 :1 子网添加静态路由：

```
LA-R1>enable
LA-R1#config term
LA-R1(config)#ipv6 route 2001:aaaa:bbbb:1::/64 2001:aaaa:bbbb:2::1
```

请注意，此语句的意思是，“如果有针对 2001:aaaa:bbbb:1 子网的任何数据，则将其发送到 2001:aaaa:bbbb:2::1 地址”，即 VAN-R1 的 WAN 接口。

- 16. 查看 LA-R1 上的 IPv6 路由表，您应该能看到 :1 子网的条目。
- 17. 现在您需要向 VAN-R1 路由器添加一条静态路由，以便它知道如何到达 :3 子网。转到 VAN-R1 的 CLI。
- 18. 输入以下命令以添加到达 :3 子网的 IPv6 路由：

```
VAN-R1>enable
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#ipv6 route 2001:aaaa:bbbb:3::/64 2001:aaaa:bbbb:2::2
```

- 19. 使用 show ipv6 route 命令验证 :3 子网的路由是否已添加。
- 20. 切换回 LA-R1 路由器，尝试通过其 IPv6 地址 ping VAN-R1 的 WAN 接口：

```
LA-R1#ping 2001:aaaa:bbbb:2::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:aaaa:bbbb:2::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/3/12 ms
```

- 21. 现在尝试从 LA-R1 ping VAN-R1 上千兆端口的 IPv6 地址。（这次应该成功，因为我们已配置 IPv6 静态路由。）

```
LA-R1#ping 2001:aaaa:bbbb:1::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:aaaa:bbbb:1::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/4/17 ms
```

