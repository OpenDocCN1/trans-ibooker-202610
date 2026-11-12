
## 练习 14-2：配置 OSPFv3 路由

在此练习中，你将删除上一练习中创建的 IPv6 静态路由，然后配置 OSPFv3 路由协议以在两台路由器之间共享 IPv6 路由表。

- 确保所有思科路由器都已启动并运行。
- 通过控制台登录 VAN-R1 并输入以下命令删除静态路由：
- 使用 do show ipv6 route 命令验证路由已被删除。
- 连接到 LA-R1 的控制台端口并使用以下命令删除静态路由：
- 从 LA-R1 路由器，尝试 ping 地址 2001:aaaa:bbbb:1::1。（你不应该能成功，因为提供路由器到达该系统知识的路由已消失。）
- 在 VAN-R1 上配置 OSPFv3，输入以下命令：

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#no ipv6 route 2001:aaaa:bbbb:3::/64
```

```
LA-R1#enable
LA-R1#config term
LA-R1(config)#no ipv6 route 2001:aaaa:bbbb:1::/64
```

- 为区域 0 配置接口，输入以下命令：

```
VAN-R1(config)#ipv6 unicast-routing
VAN-R1(config)#ipv6 router ospf 1
VAN-R1(config-rtr)#router-id 0.0.0.1
VAN-R1(config-rtr)#exit
VAN-R1(config)#interface g0/0
VAN-R1(config-if)#ipv6 ospf 1 area 0

VAN-R1(config-if)#interface s0/3/0
VAN-R1(config-if)#ipv6 ospf 1 area 0
VAN-R1(config-if)#exit
VAN-R1(config)#exit
```

- 在 LA-R1 上配置 OSPFv3，输入以下命令：

```
LA-R1>enable
LA-R1#config term
LA-R1(config)#ipv6 unicast-routing
LA-R1(config)#ipv6 router ospf 1
%OSPFv3-4-NORTRID: OSPFv3 process 1 could not pick a router-id, please configure manually
LA-R1(config-rtr)#router-id 0.0.0.2
LA-R1(config-rtr)#exit
LA-R1(config)#interface g0/0
LA-R1(config-if)#ipv6 ospf 1 area 0
LA-R1(config-if)#interface s0/3/0
LA-R1(config-if)#ipv6 ospf 1 area 0
LA-R1(config-if)#exit
LA-R1(config)#exit
LA-R1#
01:51:40: %OSPFv3-5-ADJCHG: Process 1, Nbr 0.0.0.1 on Serial0/3/0 from LOADING to FULL, Loading Done
LA-R1#
```

- 从 LA-R1 输入 ping 2001:aaaa:bbbb:1::1 以测试到 :1 子网的连通性。应该会成功，因为 OSPF 已在两台路由器之间共享了路由表。

- 使用以下命令验证两台路由器上 OSPFv3 的配置：

```
show ipv6 interface brief
show ipv6 route
show ipv6 ospf
```

## 章节复习

本章重点介绍了 IPv6。由于 IPv4 剩余地址数量有限，IPv6 的设计旨在将互联网带入下一代。IPv6 地址使用八组四位十六进制地址（每组 16 位），用冒号 (:) 分隔，格式如下：xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx。如果 IPv6 地址中有连续的零字段，可以使用两个冒号 (::) 表示。但是，这种简写在一个地址中只能使用一次。

IPv6 地址有三种基本类型：任播、多播和单播。任播地址与 IPv4 广播非常不同：它表示一对最近接口，许多接口可以共享相同地址。多播地址类似于 IPv4 中的多播：一对多。单播地址表示单个接口。全球单播地址是 2000::/3。环回地址是 ::1。站点本地私有地址范围从 FEC:: 到 FFF::，链路本地地址范围从 FE8:: 到 FEB::。

IP 标头中的许多 IPv4 字段已被弃用，不再使用。IPv6 标头已被简化和精简，以使其对当今网络更加高效。一些协议，例如 ARP 和 IGMP，也已被弃用。它们的功能现在由 ICMPv6 执行，ICMPv6 还具有新功能，包括路由器、DNS 服务器和邻居发现。

为接口分配静态地址最常用的方法是使用 EUI-64 方法。DHCPv6 和无状态自动配置使设备能够动态获取地址。无状态自动配置通过请求路由器提供子网 ID，并让设备使用 EUI-64 动态获取接口 ID 来实现这一点。

IPv6 支持的路由协议包括静态、RIPng、OSPFv3、IPv6 的 IS-IS、MP-BGP4 和 IPv6 的 EIGRP。RIPng 使用 UDP 端口 521 进行连接。其目标多播地址是 FF02::9。从 IPv4 过渡到 IPv6 最常用的方法包括双栈和 6to4 隧道。在双栈中，设备同时运行两种协议栈。在 6to4 隧道中，IPv6 数据包被封装在 IPv4 数据包中，以便通过 IPv4 骨干网到达另一个 IPv6 网络。

必须先执行 ipv6 unicast-routing 命令来启用 IPv6。每个接口必须分配一个地址，通常使用 EUI-64 方法，才能处理 IPv6 数据包。RIPng 必须全局配置，然后在每个接口上启用。标记用于指定接口所属的 RIPng 路由进程。与 RIPng 类似，OSPFv3 和 EIGRP 必须全局配置，然后在每个接口上启用。在 OSPFv3 全局配置中，你定义路由器 ID 和区域编号；然后将接口与 OSPF 进程的区域关联起来。对于 IPv6 的 EIGRP，全局配置要求你配置自治系统编号，并可能使用 no shutdown 命令启动路由进程。

## 快速复习

### IPv6 必要性

- IPv6 地址长度为 128 位。
- IPsec 内置于 IPv6 协议中，允许设备在不丢失连接的情况下漫游。

### IPv6 寻址

- IPv6 地址使用八组四位十六进制地址（每组 16 位），用冒号 (:) 分隔，格式如下：xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx。
- 如果 IPv6 地址中有连续的零字段，可以使用两个冒号 (::) 表示，但这种表示法在地址中只能使用一次。
- 任播地址表示设备的最近接口，许多设备可以共享任播地址。多播地址以 FF 开头。
- 全球单播地址是公共地址，用途类似于 IPv4 公共地址。全球单播地址以 2 或 3 开头，例如 2000 或 2001。
- 站点本地/唯一本地地址用途类似于 IPv4 私有地址，始终以 FEC:: 到 FFF:: 开头。常见的站点本地/唯一本地地址引用为 FEC0。
- 链路本地地址用途类似于 IPv4 中的 APIPA 地址，以 FE80 开头。
- 子网 ID 是前 64 位，接口 ID 是最后 64 位。EUI-64 允许使用接口上的 MAC 地址动态创建接口 ID 部分。

### IPv6 增强功能

- IPv6 标头已更新并简化，因为 IPv4 标头中的一些字段今天已不相关。
- ICMPv6 已增强。ICMPv6 接管了 IPv4 ARP 和 IGMP 协议的职责，还执行邻居、路由器和 DNS 服务器发现服务。

### 地址分配

- DHCPv6 和无状态自动配置使接口能够动态获取其地址。
- 无状态自动配置使接口能够从路由器学习子网 ID，并使用 EUI-64 方法动态创建接口 ID。

### 路由与 IPv6

- IPv6 支持的路由协议包括静态、RIPng、OSPFv3、IPv6 的 IS-IS、MP-BGP4 和 IPv6 的 EIGRP。
- RIPng 基于 RIPv2。所有 RIP 路由器的多播地址是 FF02::9，并通过 UDP 发送到端口 521。
- 在双栈中，设备同时运行 IPv4 和 IPv6 协议栈。6to4 隧道将 IPv6 数据包隧道传输在 IPv4 有效载荷中，以通过中间 IPv4 网络连接两个 IPv6 网络。

### IPv6 配置

- **ipv6 unicast-routing** 命令全局启用 IPv6，并且必须是路由器上执行的第一个 IPv6 命令。**ipv6 address** 命令分配前缀、长度以及使用 EUI-64 分配接口 ID。
- **ipv6 router rip tag** 命令在路由器上启用 RIPng 路由进程。**ipv6 rip tag enable** 命令将接口与特定的 RIPng 路由进程关联。**show ipv6 rip** 命令显示 RIPng 路由进程的配置。**show ipv6 route rip** 命令显示 IPv6 路由表中的 RIPng 路由。
