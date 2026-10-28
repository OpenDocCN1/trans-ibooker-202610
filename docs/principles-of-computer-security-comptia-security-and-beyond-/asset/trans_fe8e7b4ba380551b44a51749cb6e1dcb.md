

每个协议都由一个协议号标识。不要将协议号与端口号混淆。端口号标识程序和服务的连接端点，例如 53 代表 DNS。协议号标识封装在 IP 头中的协议。最常见的协议号是 1（ICMP）、6（TCP）和 17（UDP）。你可以在 https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml 查看完整列表。

在 Wireshark 中，找到你从 Kali Linux VM 发送的 ICMP 回显请求中协议号字段值为 1 的位置。

d. 输入以下命令以使用原始 IP 模式，这正是 -0 选项所指定的：

```
sudo hping3 -c 1 192.168.1.108 -e "Syracuse University" -0
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

与到目前为止 TCP、UDP 和 ICMP 的情况不同，你没有在 Layer 3 数据包内指定任何要包含的内容。相反，你直接将数据放入 IP 数据包中，而不考虑正确的头部和值。

在 Wireshark 中找到 “Syracuse University”。你会注意到，“Syracuse University” 正被误解为 TCP 头的字节，从源端口和目标端口字段开始。在 Wireshark 的数据包详情窗格（中间窗格）中单击 TCP 部分中的源端口字段，该字段显示源端口为 21369。你会注意到，在数据包字节窗格（底部窗格）中，左侧的十六进制转储突出显示了 53 79，右侧的 ASCII 部分突出显示了 Sy（来自 Syracuse 的有效载荷）。如 https://www.asciitable.com 所示，S 的基 16（十六进制）值为 53，y 的基 16（十六进制）值为 79。将基 16（十六进制）值 5379 转换为基 10（十进制）得到 21369，这就是 Wireshark 将 21369 显示为源端口的原因。同样的逻辑也适用于 ra（来自 Syracuse 的有效载荷），其十六进制为 7261，十进制为 29281，这正是目标端口字段显示的值。

Wireshark 会提示这是一个畸形数据包，因为 TCP 头的最后一个字段“紧急指针”（Urgent Pointer）无法由字符串 “Syracuse University” 的长度来表示。

如果你使用 “Syracuse University, New York,” 作为有效载荷，你将填满该头部，并在末尾多出 8 个字节，这些字节会被误认为是 TCP 选项，因此不会出现畸形数据包消息。但是，你会注意到紧急指针字段（现在出现在 Wireshark 的这个 TCP 头中）的值为 31020，这会导致 Wireshark 显示另一条消息：“78 bytes says option goes past end of options.”

e. 输入以下命令，发送比上一条命令更少的字节：

```
sudo hping3 -c 1 192.168.1.108 -e "Syracuse" -0
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

注意，在 Wireshark 中，当只发送 “Syracuse” 时，只会显示源端口和目标端口字段，因为这次 Wireshark 用来填充其余字段的字节更少（没有空白字符，十六进制数字 20 表示空白，也没有 University 这个词）。

f. 输入以下命令，在原始 IP 模式下使用 UDP：

```
sudo hping3 -c 1 192.168.1.108 -e "Nazareth College" -0 -H 17
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

以下内容来自 hping3 手册页：

- `-H --ipproto`  
  在原始 IP 模式下设置 IP 协议。

如前所述，17 是 UDP 的协议号。

Wireshark 仍然不满意，因为它期望的是一个 UDP 数据报及其相应的字段和值位于通常的位置。

在 Wireshark 中找到 “Nazareth College”。

如果仔细观察，在 Wireshark 中，“Nazareth” 的字节被解释为 8 字节的 UDP 头，而空白字符和 7

每个发送的数据包的编号。收到数据包时，序列号可计算为 *replies.dest.port - base.source.port*。默认基础源端口是随机的，使用此选项可以设置不同的数字。如果你希望源端口不随每个发送的数据包递增，请使用 *-k --keep* 选项。

在 Wireshark 中按 **tcp.port==536** 过滤。

d. 输入以下命令，使用 hping3 伪造源端口、目标端口和源 IP 地址：

```
sudo hping3 -c 1 192.168.1.108 -s 536 -p 9999 --rand-source
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

以下内容来自 hping3 手册页：

- `-p --destport [+] [+] dest port`  
  设置目标端口，默认为 0。如果目标端口号前有 ‘+’ 字符（例如 +1024），则每收到一个回复，目标端口就会递增。如果目标端口号前有两个 ‘+’（例如 ++1024），则每发送一个数据包，目标端口就会递增。默认情况下，可以使用 **CTRL+z** 交互式地修改目标端口。

在 Wireshark 中按 **tcp.port==536** 过滤。

这对于通过某个端口上的易受攻击服务向目标机器发送流量非常有用，且不会留下任何关于源 IP 地址或源端口的归属信息。

30 分钟

## 实验 16.04：使用 Scapy 进行数据包构造

如果你觉得用 hping3 构造数据包很巧妙，那么等你看完 Scapy 能做什么再说吧！与 hping3 由在终端输入的命令组成不同，Scapy 使用 Python 接口来构造和发送数据包。Scapy 允许你输入单个命令，甚至可以编写脚本。在 https://scapy.net/ 上了解更多关于 Scapy 的信息。

同样的主题仍在继续：这个工具既可以用于正当目的，也可以用于恶意目的。

### 学习目标

在本实验中，你将使用 Scapy 构造数据包。完成本实验后，你将能够：

- 使用 Scapy 构造并发送包含各种协议的数据包
- 执行 SYN 洪泛攻击，并理解攻击者如何利用 TCP 发起拒绝服务（DoS）攻击
- 使用 Wireshark 解读结果

### 实验材料和设置

本实验所需的材料有：

- *《计算机安全原理：CompTIA Security+ 及进阶》* 教材
- 你在第 1 章创建的 Kali Linux VM
- 你在第 1 章创建的 Windows 10 VM
- 可连接互联网的 Web 浏览器

### 让我们开始吧！

在 Kali Linux VM 上，打开一个终端。

在 Windows 10 VM 上，关闭 Windows Defender 防火墙。

在 Windows 10 VM 上，在 Wireshark 中使用显示过滤器 **icmp** 开始抓包。定位并分析相关流量。

确保你的语法正确。缺少一个括号或斜杠（或多加一个）都可能导致错误。Python 同样区分大小写。要退出 Scapy 中的任何操作，请按 CTRL-C。在每条命令或函数调用后按 ENTER 键。

![](img/6e921d997751f0a0f87e5170ac9657a1_605_0.png)

### 步骤 1：使用 Scapy 发送自定义 ICMP 回显请求

a. 按如下方式启动 Scapy：

```
sudo scapy
```

如果出现 “Can’t import PyX. Won’t be able to use psdump() or pdfdump()” 消息，请忽略它。

b. 调用 **exit()** 函数退出 Scapy：

```
exit()
```

c. 在 Kali Linux VM 上，再次启动 Scapy。

d. 在 Scapy 提示符下，输入以下命令，使用 **send()** 函数发送一个 IP 数据包，该数据包由 **IP()** 函数创建，内部包含一个由 **ICMP()** 函数创建的 ICMP 回显请求数据包：

```
send(IP(dst="192.168.1.108")/ICMP()/"Staten Island, NY")
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

唯一指定的值是：IP 数据包的目标 IP 地址应为 Windows 10 VM 的 IP 地址，ICMP 有效载荷应为 “Staten Island”。

你将在 Scapy 中看到已发送一个数据包的确认信息，如图 16-32 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_606_0.png)

图 16-32 Scapy 发送了一个数据包。

在 Wireshark 中，在 ICMP 回显请求和随后的 ICMP 回显回复中找到 “Staten Island” 的有效载荷。

e. 在 Scapy 提示符下，一次一条地输入以下两条命令：

```
send(IP(src="1.9.9.7",
dst="192.168.1.108")/ICMP()/"College of Staten Island")
```

```
send(IP(src="2.0.0.5",
dst="192.168.1.108")/ICMP()/"Brooklyn College")
```

（将 IP 地址替换为 Windows 10 VM 的 IP 地址。）

这些命令发送带有自定义有效载荷和伪造 IP 地址的 ICMP 回显请求。对于每条命令，你都会在 Scapy 中看到 “Sent 1 packets.”，如图 16-33 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_607_0.png)

图 16-33 Scapy 又发送了两个数据包。

在 Wireshark 中找到相应的有效载荷。

2a–2g

### 步骤 2：使用 Scapy 发送自定义 TCP 段

