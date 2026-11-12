

```
Router(config)# access-list 1 permit 192.168.1.1
Router(config)# access-list 1 deny 192.168.1.2
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 1 deny any
Router(config)# interface serial 0
Router(config-if)# ip access-group 1 in
```

在这个示例中，ACL #1 中的第一条 ACL 语句表示，要执行 permit 动作，IP 数据包的源地址必须是 192.168.1.1——如果不是，IOS 将处理第二条语句。请记住，如果在标准 ACL 中省略通配符掩码，它默认为 0.0.0.0——即 ACL 语句中对应地址的精确匹配。第二条 ACL 语句表示，要执行 deny 动作，IP 数据包的源地址必须是 192.168.1.2；如果不是，IOS 将处理第三条语句。第三条 ACL 语句表示，要执行 permit 动作，IP 数据包的源地址必须在 192.168.1.0 到 192.168.1.255 之间——如果不是，IOS 将处理第四条语句。第四条语句实际上并非必要：它会丢弃任何数据包。你不需要这条语句，因为每个 ACL 的末尾都会有一个不可见的隐式 deny any 语句。ACL 示例中的最后两条命令在流量进入接口时在串行接口 0 上激活 ACL #1。

实际上，你可以像下面这样编写前面的 ACL：

```
Router(config)# access-list 1 deny 192.168.1.2
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# interface serial 0
Router(config-if)# ip access-group 1 in
```

此示例将列表中的四条 ACL 语句减少到两条。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1005_0.png)

VIDEO 17.09. 随本书提供的数字资源包含在路由器上配置标准编号 ACL 的多媒体演示。

下面是标准 ACL 的另一个示例：

```
Router(config)# access-list 2 deny 192.168.1.0
Router(config)# access-list 2 deny 172.16.0.0
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 permit 0.0.0.0 255.255.255.255
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 2 out
```

此 ACL 示例存在一些问题。请检查它，看看是否能发现这些问题。

第一条 ACL 语句似乎拒绝来自 192.168.1.0/24 的所有流量。实际上，它不会产生任何效果。请记住，如果省略地址的通配符掩码，它默认为 0.0.0.0——即精确匹配。问题是，你永远不会有源地址为 192.168.1.0 的数据包，因为这是一个网络号，而不是主机地址。第二条语句存在同样的问题。第三条和第四条语句没有问题。

正如你所见，配置 ACL 可能比较棘手。对于前面的示例，下面是更新后的配置：

```
Router(config)# access-list 2 deny 192.168.1.0 0.0.0.255
Router(config)# access-list 2 deny 172.16.0.0 0.0.255.255
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 permit 0.0.0.0 255.255.255.255
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 1 out
```

在这个示例中，第一条语句现在表示，任何源地址来自网络 192.168.1.0/24 的数据包都应被丢弃。第二条语句将丢弃来自 B 类网络 172.16.0.0/16 的任何流量。第三条语句将允许来自 192.168.1.1 的流量。第四条语句将允许来自任何位置的流量。实际上，此配置仍有一个问题——请查看第一条和第三条语句。第三条语句会被执行吗？如果你回答“否”，那你是正确的。在这种情况下，你需要将更具体的条目放在较不具体的条目之前。另一个小点是，列表中的第四条语句可以使用关键字 any 来表示地址。下面是更新后的配置：

```
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 deny 192.168.1.0 0.0.0.255
Router(config)# access-list 2 deny 172.16.0.0 0.0.255.255
Router(config)# access-list 2 permit any
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 1 out
```

此 ACL 实际上还有一个问题。如果你猜测接口上使用的 ACL 编号不正确，那么你猜对了。请注意，创建的 ACL 编号为 2，而在接口上应用 ACL 时使用的编号是 1。要修复此问题，请使用以下配置：

```
Router(config)# interface ethernet 0
Router(config-if)# no ip access-group 1 out
Router(config-if)# ip access-group 2 out
```

请注意，必须先从接口中删除旧的 ACL，然后再应用新的 ACL。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1007_0.png)

**考试技巧** 对于 CCNA 考试，你需要能够通过检查列表中语句的顺序来排查 ACL 配置问题。前面带有配置错误 ACL 语句的示例是配置 ACL 时需要查找的问题的绝佳示例。

## 限制对路由器的 VTY 访问

除了使用标准 IP ACL 过滤进入和/或离开接口的流量外，你还可以使用它们来限制对路由器的 VTY 访问（Telnet 和 SSH）。你可能希望这样做，以便只允许网络管理员远程访问 IOS 设备的 CLI。此设置与在接口上设置受限访问几乎相同。

首先，你需要创建一个标准 ACL，其中包含一组 **permit** 语句，以允许相应的网络管理员进行远程访问；在列表中包括他们的 PC 的 IP 地址。接下来，你需要激活 ACL。但是，你不会在路由器的任何接口上执行此操作。如果在接口上激活此 ACL，它将允许来自管理员的任何类型流量，但丢弃所有其他流量。请记住，当有人通过 Telnet 或 SSH 访问你的路由器时，路由器会将此连接与 VTY 线路关联。因此，你会将标准 ACL 应用到 VTY 上，如下所示：

```
Router(config)# line vty 0 4
Router(config-line)# access-class standard_ACL_# in|out
```

请记住，你的路由器默认支持五个 Telnet 会话（0–4），某些 IOS 设备支持更多。你可以在 vty 参数后指定起始和结束行号，以同时配置所有 VTY。如果你不对所有 VTY 应用限制，就会为路由器留下后门，这可能会导致安全问题。

还要注意将 ACL 应用到线路时使用的命令：access-class。这与在路由器接口上激活 ACL 不同。如果使用 in 参数，你限制的是对路由器本身的 Telnet 和 SSH 访问。out 参数比较特殊。使用此参数，你限制的是当有人使用 telnet、connect 或 ssh 命令时，该路由器可以 Telnet 或 SSH 到达的目标地址。这会对标准 ACL 产生例外，并使路由器将 ACL 语句中的地址视为目标地址；它会导致路由器在允许路由器上的用户 Telnet 到指定目标之前，将该地址与 Telnet 命令中的地址进行比较。

下面是使用标准 ACL 过滤发送到路由器的 Telnet 流量的简单示例：

```
Router(config)# access-list 99 permit 192.168.1.0 0.0.0.255
Router(config)# line vty 0 4
Router(config-line)# access-class 99 in
```

在这个示例中，只有来自 192.168.1.0/24 的流量被允许 Telnet 或 SSH 登录此路由器。由于访问列表 99 末尾存在隐式 deny，因此到达此路由器（通过 VTY）的所有其他连接都将被丢弃。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1008_0.png)

**VIDEO 17.10.** 随本书提供的数字资源包含配置标准编号 ACL 以限制对路由器远程访问的多媒体演示。

正如你在下一节中将看到的，你也可以使用扩展 ACL 来限制对 IOS 设备的访问，但此配置要复杂得多。其次，扩展 ACL 应用于接口，因此无法限制从路由器到远程目标的 Telnet 访问。第三，每当你在路由器接口上应用 ACL 时，都会影响该接口上路由器的性能。取决于路由器型号、IOS 版本和你启用的功能，性能下降程度会有所不同（当今最新的 IOS 版本在接口上使用 ACL 时性能损耗非常小）。因此，如果你只想限制到或来自路由器的 Telnet 或 SSH 访问，在 VTY 上使用标准 ACL 和 **access-class** 语句是最佳方法。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1009_0.png)

**考试技巧** 对于 CCNA 考试，请记住，你可以通过要求用户登录、仅使用 SSH 以及在 VTY 线路上应用标准 ACL 来限制对 IOS 设备的远程访问。你需要在 VTY 线路上使用 **access-class** 模式命令应用 ACL。请注意，你也可以使用扩展 ACL 来实现此目的，但这需要更多的配置工作。

## 练习 17-5：配置标准编号 ACL

在本练习中，你将在 VAN-R1 路由器上创建两个标准访问列表。第一个 ACL 将阻止来自 14.0.0.0 网络的所有流量，但一个 IP 地址的流量除外，第二个 ACL 将用于
