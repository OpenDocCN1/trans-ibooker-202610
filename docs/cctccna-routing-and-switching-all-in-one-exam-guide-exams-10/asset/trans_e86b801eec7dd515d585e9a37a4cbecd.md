

| 协议 | 内部全局 | 内部本地 | 外部本地 | 外部全局 |
|------|----------|----------|----------|----------|
| tcp | 200.200.200.1:1080 | 192.168.1.1:1080 | 201.1.1.1:23 | 201.1.1.1:23 |
| tcp | 200.200.200.1:1081 | 192.168.1.2:1080 | 201.1.1.1:23 | 201.1.1.1:23 |

在此示例中，192.168.1.1 和 192.168.1.2 都正在使用 telnet 访问同一台外部设备（201.1.1.1）。请注意，两者还使用相同的源端口号（内部本地列下的 1080）。IOS 已注意到这一点，并将第二个连接的源端口号从 1080 更改为 1081，以区分两个连接。

```
Router# show ip nat statistics
Total translations: 2 (0 static, 2 dynamic; 0 extended)
Outside interfaces: Serial0
Inside interfaces: Ethernet0
Hits: 98 Misses: 4
Expired translations: 1
Dynamic mappings:
-- Inside Source
access-list 1 pool nat-pool refcount 2
pool nat-pool: netmask 255.255.255.255
start 200.200.200.10 end 200.200.200.254
type generic, total addresses 12, allocated 1 (9%), misses 0
```

在此示例中，翻译表中当前有两个动态翻译。Hits 是指 IOS 查找翻译表并找到匹配项（可用于数据包的现有翻译）的次数，而 Misses 是指 IOS 在表中查找翻译、未找到，并且不得不为数据包在翻译表中创建条目的次数。

对于翻译表中的动态条目，可以使用以下命令清除所有条目或特定条目：

```
Router# clear ip nat translation *
Router# clear ip nat translation inside
    global_IP_address local_IP_address
Router# clear ip nat translation outside
    global_IP_address local_IP_address
Router# clear ip nat translation protocol inside
    global_IP_address global_port
    local_IP_address local_port
```

第一个命令清除表中的所有动态条目。请注意，要清除静态条目，需要从配置模式中删除静态 NAT 配置命令。

除了 **show** 命令，还可以使用 **debug** 命令进行故障排除。例如，**debug ip nat** 命令将显示 IOS 对每个翻译数据包执行的翻译。这对于判断 IOS 是否正在正确翻译数据包和分段首址信息非常有用。请注意，在繁忙的网络上，此命令将占用路由器大量 CPU 周期。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_787_0.png)

**考试提示** 请记住这些命令以供考试使用。使用 **show ip nat translations** 命令显示路由器的翻译。使用 **clear ip nat translation** 命令从翻译表中清除动态翻译。**debug ip nat** 命令以实时方式显示路由器执行地址翻译的情况。

以下是 **debug ip nat** 命令的示例：

```
Router# debug ip nat
05:32:23: NAT: s=192.168.1.10->200.200.200.2, d=201.1.1.1 [70]
05:32:23: NAT*: s=201.1.1.1, d=200.200.200.2->192.168.1.10 [70]
```

在此示例的第一行中，一台内部机器（192.168.1.10），即数据包中的源地址（s=），正在将其地址翻译为 200.200.200.2，数据包将发送到目标 201.1.1.1（d=）。第二行显示来自 201.1.1.1 的返回流量以及从全局到内部本地地址的翻译。星号（*）表示数据包已快速交换。括号中的数字（[ ]）是数据包的标识号，可用于将此信息与使用外部协议分析器或嗅探器产品进行的数据包跟踪相关联。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_788_0.png)

**提示** 可以向 **debug ip nat** 命令添加 **detailed** 参数，以显示每个翻译候选数据包的描述，以及任何错误（例如，全局池中没有更多地址可分配给出站用户）。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_788_1.png)

**视频 13.05.** 随本书提供的数字资源包含一个多媒体演示，展示如何在路由器上监控 NAT。

## 练习 13-3：配置 NAT 过载

在此练习中，您将使用不同的网络拓扑来练习在 Cisco 路由器上配置 NAT 过载。此实验练习使用以下网络拓扑：

![](img/0eafdcebe219ef3756f0e57bcf1eb997_789_0.png)

- 1. 确保所有物理组件（如路由器和交换机）已根据网络图连接，然后为设备通电。
- 2. 将工作站连接到 VAN-R1 的控制台端口。然后启动 PuTTY，选择串行连接，并设置正确的 COM 端口。然后单击“打开”。
- 3. 在 PuTTY 窗口中按 ENTER 键以在 Cisco 设备上获取提示符。
- 4. 为 VAN-R1 上的每个接口配置适当的 IP 地址。
- 5. 创建一个访问控制列表，其中包含 10.1.0.0/16 网络上所有将被允许使用 NAT 的 IP 地址：

```
VAN-R1(config)#access-list 1 permit 10.1.0.0 0.0.255.255
```

- 6. 启用 NAT 并应用访问控制列表。对连接到 Internet 的接口进行过载（在此示例中为 g0/1）：
  `VAN-R1(config)#ip nat inside source list 1 interface g0/1 overload`
- 7. 配置内部接口：
  ```
  VAN-R1(config)#interface g0/0
  VAN-R1(config-if)#ip nat inside
  ```
- 8. 配置外部接口：
  ```
  VAN-R1(config-if)#interface g0/1
  VAN-R1(config-if)#ip nat outside
  ```
- 9. 确保客户端系统正在使用 10.1.0.0/16 网络中的 IP 地址，并将 10.1.0.1 设置为默认网关。
- 10. 从客户端系统输入 **ping 8.8.8.8**。
- 11. 要查看 VAN-R1 上的翻译，请使用 **show ip nat translations** 命令。

## 地址翻译故障排除

排除路由器流量问题不一定是一件容易的事。这可能会因您启用的功能而变得复杂，如您从前两节中所见。在排除您认为与地址翻译相关的问题时，请遵循以下步骤：

- 1. 验证 NAT 命令，以确保配置正确，包括 ACL，并使用指定应动态翻译的流量的 **permit** 语句。
- 2. 检查路由器是否使用 **show ip nat translations** 命令实际正在为用户翻译数据包。
- 3. 使用 **show ip nat statistics** 命令查看是否实际发生了翻译——请记住，输出是静态的，您需要多次重新执行它以更新统计信息。例如，通过路由器发送五个与 NAT 翻译规则匹配的 ICMP 回显，您应该看到命中计数增加 5。
- 4. 使用 **debug ip nat [detailed]** 命令查看是否正在发生翻译；执行动态 NAT 时，请确保相应的全局 NAT 池中有足够的地址可用。
- 5. 验证是否已使用 **ip nat inside** 或 **ip nat outside** 接口子配置模式命令正确配置了路由器的内部或外部 NAT 接口。
- 6. 使用 **show access-lists** 命令查看应翻译的数据包的 **permit** 语句上是否出现命中计数。
- 7. 通过使用 **show ip route** 查看路由表，确保路由器可以将数据包路由到目标。
- 8. 如果数据包正在被翻译，但您没有收到用户流量的任何回复（例如使用 ping 时的回显回复），请检查路由器上的任何 ACL，以使用 **show access-lists** 命令确保流量可以通过路由器返回：查找 **deny** 语句上递增的命中计数。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_791_0.png)

**考试提示** 对此处列出的排除地址翻译问题的故障排除步骤要熟练掌握。

## 服务质量

当今网络需要具备承载不同类型流量（如常规数据、语音和视频）的能力，并必须以某种方式确保所有数据都能及时到达目的地。网络管理员必须确保为关键业务应用程序或需要更多带宽的应用程序（如语音和视频应用程序）提供足够的带宽。

*服务质量*（QoS）具有一组机制，使网络设备能够做出与带宽和性能相关的决策，以优化网络流量。在图 13-8 中，您可以看到一台连接到 LAN 的三个千兆接口的路由器。假设每个 LAN 上都有许多主机需要通过快速以太网接口向 Internet 发送大量流量。问题是，所有这些来自三个千兆接口的流量都试图通过较低的

