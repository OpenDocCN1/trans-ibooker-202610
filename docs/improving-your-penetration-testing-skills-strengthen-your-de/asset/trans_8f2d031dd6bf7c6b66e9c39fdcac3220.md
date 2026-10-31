

PORT STATE SERVICE
22/tcp open ssh
135/tcp open msrpc
139/tcp open netbios-ssn
....
49158/tcp open unknown
49159/tcp open unknown
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 3.25 seconds

正如我们所见，我们使用了 -sT 参数，表示我们要执行 TCP 连接扫描。TCP 连接扫描基于三次握手过程，因此，此次扫描返回的结果被认为准确。

在使用 Nmap 且未指定端口范围时，Nmap 会扫描每个协议中最常见的 1,000 个端口。

3. SYN 扫描 [-sS] 被认为是一种隐蔽扫描技术，因为它从未在目标和扫描器之间建立完整的连接。因此，它也被称为半开扫描。让我们分析一下对目标进行的 SYN 扫描：

```
msf > nmap -sS 192.168.216.10 -p 22-5000
[*] exec: nmap -sS 192.168.216.10 -p 22-5000

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:00 EDT
Nmap scan report for 192.168.216.10
Host is up (0.00063s latency).
Not shown: 4967 closed ports
PORT STATE SERVICE
22/tcp open ssh
135/tcp open msrpc
...
3920/tcp open exasoftport1
4848/tcp open appserv-http
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 685.27 seconds
msf >
```

-sS 参数将指示 Nmap 对目标机器执行 SYN 扫描。TCP 连接和 SYN 扫描的输出在大多数情况下是相似的，但唯一的区别在于 SYN 扫描更难被防火墙和 **入侵检测系统 (IDS)** 检测到。然而，现代防火墙也足以捕获 SYN 扫描。-p 参数显示我们想要扫描的端口号范围。使用 -p 0-65535，或简写为 -p -，将扫描所有可用端口。

4. UDP 扫描 [-sU] 是识别目标上开放 UDP 端口的扫描技术。向目标机器发送 0 字节的 UDP 数据包，接收到 ICMP 端口不可达消息表明端口已关闭；否则，认为端口是开放的。它可以按以下方式使用：

```
msf > nmap -sU 192.168.216.10
[*] exec: nmap -sU 192.168.216.10

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:09 EDT
Nmap scan report for 192.168.216.10
Host is up (0.00064s latency).
Not shown: 994 closed ports
PORT STATE SERVICE
137/udp open netbios-ns
138/udp open|filtered netbios-dgm
500/udp open|filtered isakmp
4500/udp open|filtered nat-t-ike
5353/udp open|filtered zeroconf
5355/udp open|filtered llmnr
MAC Address: 00:0C:29:38:B3:A9 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 319.56 seconds
msf >
```

之前的命令将检查 192.168.56.102 上 UDP 协议最常见的 1,000 个端口是否开放。

## How it works...

我们分析了三种不同类型的 Nmap 扫描，它们在渗透测试期间非常有帮助。Nmap 为扫描目标机器提供了许多不同的模式。在这里，我们将重点关注三种扫描类型，即 TCP 连接扫描、SYN 隐蔽扫描和 UDP 扫描。Nmap 的不同扫描选项也可以组合在一次扫描中，以对目标执行更高级、更复杂的扫描。让我们继续前进并开始扫描过程。

在渗透测试期间，扫描过程可以提供大量有用的结果。由于此处收集的信息将构成渗透测试的基础，因此强烈建议正确了解扫描类型。现在让我们更深入地了解我们刚刚学习的每种扫描技术。

TCP 连接扫描是最基本的扫描技术，其中与正在测试的端口建立了完整的连接。它使用操作系统的网络功能来建立连接。扫描器向目标机器发送 SYN 数据包。如果端口开放，它会向扫描器返回 ACK 消息。扫描器然后向目标发送 ACK 数据包，显示连接成功建立。这称为三次握手过程。连接在打开后立即终止。该技术有其优点，但很容易被防火墙和 IDS 追踪。

SYN 扫描是另一种类型的 TCP 扫描，但它从未与目标建立完整的连接。它不使用操作系统的网络功能；相反，它生成原始 IP 数据包并监控响应。如果端口开放，则目标将以 ACK 消息进行响应。扫描器然后发送重置连接 (RST) 消息并结束连接。因此，它也称为半开扫描。这被认为是一种隐蔽扫描技术，因为它可以避免在某些配置不当的防火墙和 IDS 中触发警报。

UDP 扫描是一种无连接扫描技术；因此，无论数据包是否已被目标接收，都不会向扫描器发送通知。如果端口关闭，则会向扫描器发送 ICMP 端口不可达消息。如果未收到消息，则端口被报告为开放。此方法可能会返回错误结果，因为防火墙可以阻止数据包，因此不会生成响应消息，扫描器将报告端口为开放。

## There's more...

让我们进一步了解 Nmap 扫描，并看看如何将不同类型的扫描组合在一起。

## Operating system and version detection

除了端口扫描之外，Nmap 还提供了一些高级选项。这些选项可以帮助我们获取有关目标的更多信息。其中最广泛使用的选项之一是 **操作系统识别 [-O]**。这可以帮助我们识别目标机器上运行的操作系统。

操作系统检测扫描输出如下所示：

```
msf > nmap -O 192.168.216.129
[*] exec: nmap -O 192.168.216.129

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:28 EDT
Nmap scan report for 192.168.216.129
Host is up (0.0012s latency).
Not shown: 977 closed ports
PORT STATE SERVICE
21/tcp open ftp
22/tcp open ssh
23/tcp open telnet
...

Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.91 seconds
msf >
```

正如我们所见，Nmap 已成功检测到目标机器的操作系统。这可以简化我们根据目标的操作系统寻找合适的漏洞利用的任务。

另一个广泛使用的 Nmap 选项是不同开放端口的 **版本检测 [-sV]**。它可以与我们之前看到的任何扫描类型混合使用，以添加有关目标开放端口上运行的服务版本的信息：

```
msf > nmap -sV 192.168.216.129
[*] exec: nmap -sV 192.168.216.129

Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-19 09:30 EDT
Nmap scan report for 192.168.216.129
Host is up (0.00049s latency).
Not shown: 977 closed ports
PORT STATE SERVICE VERSION
...

irc.Metasploitable.LAN; OSs: Unix, Linux; CPE:
cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.57 seconds
msf >
```

正如我们所见，我们的扫描输出中添加了一个额外的版本列，报告了目标机器上运行的不同服务的版本。

## Increasing anonymity

有时以匿名方式执行扫描是必不可少的。如果您在不使用安全措施的情况下执行扫描，防火墙和 IDS 日志可能会泄露您的 IP 地址。Nmap 中提供的一项功能称为 **诱饵 (-D)**。

诱饵选项并不能阻止您的 IP 地址被记录在防火墙和 IDS 的日志文件中，但它会使扫描看起来很可怕。它在日志文件中添加了其他流量，从而给人一种印象，即有多个其他攻击者同时扫描该机器。因此，如果您添加两个诱饵 IP 地址，日志文件将显示请求数据包来自三个不同的 IP 地址；一个是您的，另两个是您添加的假地址：

```
msf > nmap -sT 192.168.216.10 -D 192.168.216.13,192.168.216.25
```

