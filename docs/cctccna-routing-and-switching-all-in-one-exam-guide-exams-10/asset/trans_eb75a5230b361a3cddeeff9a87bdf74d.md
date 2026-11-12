

## IPv6 静态路由

配置 IPv6 静态路由与配置 IPv4 静态路由类似。语法如下：

```
IOS(config)# ipv6 route prefix/bits IPv6_next_hop_address
    administrative_distance
```

*前缀* 是你想要到达的网络编号及其对应的网络位数。对于默认路由，请使用 ::/0 作为前缀。

要查看路由设备路由表中的 IPv6 路由，请使用 **show ipv6 route** 命令：

```
Router# show ipv6 route
IPv6 Routing Table - default - 19 entries
Codes:C - Connected, L - Local, S - Static, R - RIP, B - BGP
    U - Per-user Static route, I1 - ISIS L1, I2 - ISIS L2,
    IA - ISIS interarea, IS - ISIS summary, O - OSPF intra,
    OI - OSPF inter, OE1 - OSPF ext 1, OB2 - OSPF ext 2
IPv6 Routing Table - 8 entries
L   4000::2/128 [0/0]
    via ::, Ethernet1/0
C   4000::/64 [0/0]
    via ::, Ethernet1/0
LC  4001::1/128 [0/0]
    via ::, Loopback0
L   5000::2/128 [0/0]
    via ::, Serial6/0
C   5000::/64 [0/0]
    via ::, Serial6/0
S   5432::/48 [1/0]
    via 4000::1, Null0
L   FE80::/10 [0/0]
    via ::, Null0
L   FF00::/8 [0/0]
    via ::, Null0
```

输出格式与 IPv4 路由表略有不同。与 IPv4 路由表一样，**C** 表示直接连接的路由（网络），**S** 表示静态路由。**L** 是新增的——它是主机路由。这是连接到该接口的主机的 IPv6 地址。还要注意，你可以看到每条路由的管理距离和度量值（[X/Y]）。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_849_0.png)

**视频** 14.02. 随本书提供的数字资源包含在 Cisco 路由器上配置 IPv6 静态路由的多媒体演示。

## RIPng

启用 RIPng 的方式与启用 IPv4 的 RIP 略有不同。首先，使用 **ipv6 router rip tag** 命令全局启用 RIPng：

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router rip tag
```

如果尚未启用 IPv6 单播路由，则必须先执行此操作。执行 **ipv6 router rip** 命令后，将进入子命令模式，在该模式下可以修改 RIPng 的某些全局值，例如禁用水平分割、更改管理距离和调整定时器。**tag** 是一个仅在本地有意义的标识符，用于区分路由器上运行的多个 RIP 进程。

与 IPv4 的 RIP 不同，RIPng 没有用于包含接口的 **network** 命令。相反，必须使用 **ipv6 rip tag enable** 命令在每个接口上单独启用 RIPng：

```
IOS(config)# interface type [slot_#]/port_#
IOS(config-if)# ipv6 rip tag enable
```

**tag** 参数将接口与正确的 RIPng 路由进程关联。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_850_0.png)

**考试提示** 考试时，你将被要求知道如何使用这些命令。使用 ipv6 router rip tag 命令全局启用 RIPng，使用 ipv6 rip tag enable 命令在接口上启用它。

要查看路由协议配置，请使用 show ipv6 rip 命令：

```
Router# show ipv6 rip
RIP process "RIPPROC1", port 521, multicast-group FF02::9,
    pid 187
    Administrative distance is 120. Maximum paths is 16
    Updates every 30 seconds, expire after 180
    Holddown lasts 0 seconds, garbage collect after 120
    Split horizon is on; poison reverse is off
    Default routes are not generated
    Periodic updates 2, trigger updates 0
Interfaces:
    FastEthernet0/0
Redistribution:
    None
```

在此示例中，tag 为 RIPPROC1，即 RIPng 路由进程的名称，且 RIPng 已在 FastEthernet0/0 上启用。要查看 RIPng 的 IPv6 路由表，请使用 show ipv6 route 或 show ipv6 route rip 命令。

## OSPFv3

为 IPv6 启用 OSPFv3 与为 IPv4 启用 OSPFv2 略有不同。以下部分将简要介绍基本的 OSPFv3 配置和验证。

### OSPFv3 全局配置

与 OSPFv2 一样，在 OSPFv3 中，你可以全局启用一个进程，并在该进程内执行某些功能，例如分配路由器 ID 和定义区域：

```
IOS(config)# ipv6 router process_ID
IOS(config-router)# router-id router_ID
IOS(config-router)# area area_number
```

进程 ID（与 OSPFv2 相同）用于唯一标识路由设备上本地运行的 OSPF 进程（它仅在本地有意义，不与其他 OSPFv3 路由设备共享）。路由器 ID 是一个 32 位数字，通常使用点十进制格式表示。区域号也是一个 32 位数字，可以用十进制（12）或点十进制（0.0.0.12）格式表示。以下是一个简单示例：

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router process_ID
IOS(config-router)# router-id router_ID
IOS(config-router)# area area_number
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_851_0.png)

**提示** 通常，在创建路由器 ID 时，我使用第一个数字来表示路由器所在的区域。对于区域边界路由器（ABR），第一个数字将是 0，第二个数字（可能还有第三个数字）将表示设备所在的区域。我保留最后一个八位组用于区域内路由器的标识。例如，如果我有一个所有接口都在区域 0 内的骨干路由器，其路由器 ID 可能是 0.0.0.1。如果我有一个连接到区域 1 的 ABR，其路由器 ID 可能是 0.1.0.5。这有助于我快速识别路由器的身份及其角色。在 OSPFv3 中，你必须硬编码路由器 ID，而在 OSPFv2 中则是可选的。但是，即使在 OSPFv2 中，我也有时会为某些客户使用此方法。

### OSPFv3 接口配置

配置好 OSPFv3 的全局属性后，必须将接口置于本地进程中。与 OSPFv2 不同，在 OSPFv2 中，你在 OSPFv2 路由进程配置中使用 **network** 命令，而 IPv6 的配置类似于 RIPng，即在接口下进行配置：

```
IOS(config)# interface type [slot_#]/port_#
IOS(config-if)# ipv6 ospf process_ID area area_number
```

### 简单 OSPFv3 配置示例

以下是将两个接口放置在区域 0 中的示例配置：

```
IOS(config)# ipv6 unicast-routing
IOS(config)# ipv6 router ospf 1
IOS(config-router)# router-id 0.0.0.1
IOS(config-router)# exit
IOS(config)# interface g1/0/1
IOS(config-if)# ipv6 ospf 1 area 0
IOS(config-if)# exit
IOS(config)# interface g1/0/2
IOS(config-if)# ipv6 ospf 1 area 0
IOS(config-if)# exit
```

如你所见，配置相当简单。请记住，你的 Cisco 设备接口不需要全局 IPv6 地址即可参与 OSPFv3——它只需要一个链路本地地址，前提是已全局启用 IPv6 且接口已启用，设备会自动获取该地址。

### OSPFv3 验证

配置 OSPFv3 后，可以使用类似于 OSPFv2 中使用的 **show** 命令来验证 OSPFv3 的运行情况。要显示 OSPFv3 路由进程的一般信息，请使用 **show ipv6 ospf** 命令。以下是一个示例：

```
IOS# show ipv6 ospf
Routing Process "ospfv3 1" with ID 10.10.10.1
  SPF schedule delay 5 secs, Hold time between two SPFs 10 secs
  Minimum LSA interval 5 secs. Minimum LSA arrival 1 secs
  LSA group pacing timer 240 secs
  Interface flood pacing timer 33 msecs
  Retransmission pacing timer 66 msecs
  Number of external LSA 0. Checksum Sum 0x000000
  Number of areas in this device is 1. 1 normal 0 stub 0 nssa
    Area BACKBONE(0)
      Number of interfaces in this area is 2
      MD5 Authentication, SPI 1000
      SPF algorithm executed 2 times
      Number of LSA 5. Checksum Sum 0x02A005
      Number of DCbitless LSA 0
      Number of indication LSA 0
      Number of DoNotAge LSA 0
      Flood list length 0
```

在此示例中，路由器的 ID 是 10.10.10.1，两个接口连接到区域 0。

要显示 IPv6 邻居发现（ND）缓存信息（实际邻居），请使用 show ipv6 neighbors 命令。你还可以使用 show ipv6 ospf neighbor 命令专门显示 OSPFv3 的邻居信息，如下所示：

```
Router# show ipv6 ospf neighbors
Neighbor ID  Pri  State       Dead Time  Interface ID  Interface
172.16.4.4    1    FULL/ -     00:00:31   14            Serial1/0/1
172.16.3.3    1    FULL/BDR    00:00:30   3             FastEthernet0/0
172.16.5.5    1    FULL/ -     00:00:33   13            Serial1/0/2
```

使用 show ipv6 route 命令查看路由表。要仅查看 OSPF 路由，请添加 ospf 参数：show ipv6 route ospf。

## IPv6 的 EIGRP

IPv4 的 EIGRP 中可用的可扩展性功能（包括邻居发现、DUAL 算法、度量值、负载均衡、组播和增量更新）也可在 IPv6 的 EIGRP 中使用。与 IPv4 的 EIGRP 类似，IPv6 的 EIGRP 使用 hello 数据包来发现并与其他本地 IPv6 EIGRP 路由器成为邻居。同样，与 IPv4 的 EIGRP 类似，IPv6 的 EIGRP for

