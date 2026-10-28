
ACK。之后，Kali Linux 虚拟机上的 Nmap 会发送一个 RST（同时也会设置 ACK 标志），但现在被探测机器的应用程序中会有一条关于已完成的连接的日志记录。

![](img/6e921d997751f0a0f87e5170ac9657a1_562_0.png)

图 16-9 端口 445 是开放的，但三次握手完成后，扫描机器被记录下来。

- e. 将过滤器改为 `tcp.port==21` 并按回车键。

与 SYN 扫描类似，使用连接扫描时，如图 16-10 所示，关闭的端口会对 SYN 响应一个 RST（在这种情况下还会伴随一个 ACK）。

图 16-10 端口 21 是关闭的。

### 3a–3k

### 第 3 步

根据 RFC 793 (https://tools.ietf.org/html/rfc793)，如果一个 TCP 段没有设置 SYN、ACK 或 RST 标志，那么当端口关闭时，会返回一个 RST；当端口开放时，则没有响应。其他三个标志——URG（紧急）、PSH（推送）和 FIN（finis——在 RFC 中如此拼写，引用拉丁语中意为“结束”的词）——的任意组合都会触发此行为。然而，有三种扫描——空扫描（Null scan）、FIN 扫描和 Xmas 扫描——被选中来利用这种行为。因此，Nmap 需要构建这些数据包，并且必须拥有 root 权限。

### 注意

URG 标志是过去遗留的产物，现代协议并不怎么使用它。它曾经是一种告诉目标系统对某一数据段中的数据进行优先处理的方式，优先处理的位置由 TCP 头中的紧急指针字段指定。

PSH 标志用于告诉发送系统立即将数据向下推送并发送出去，而无需等待缓冲区积累（在发送许多 TCP 段时，为了高效传输通常会这样等待），同时也用于告诉接收系统将接收到的数据向上推送给接收应用程序，而无需等待缓冲区积累（在接收许多 TCP 段时，为了高效传输通常会这样等待）。PSH 标志在 HTTP 或 TLS 会话结束时使用，因为此时没有更多数据需要发送或接收；在 SSH 会话中也会使用，因为按键需要立即发送到远程系统，此外还有其他场景。如果没有 PSH 标志，可能会出现显著的延迟，导致通信无法忍受。

FIN 标志用于拆除已建立的 TCP 连接，其方式与 SYN 标志用于建立连接类似。拆除过程使用四个步骤（两个独立的双向握手），不同于 TCP 三次握手的三个步骤。首先，发起 TCP 连接终止（也称为 TCP 拆除）的一方发送一个设置了 FIN 标志的 TCP 段。其次，另一方发送一个设置了 ACK 标志的 TCP 段。第三，在第二步中发送了带有 ACK 标志的段的那一方，现在发送另一个段，这次设置了 FIN 标志。第四，发起拆除的一方发送一个设置了 ACK 标志的段。

终止过程由四个步骤组成（两次双向握手），因为当一个系统收到第一个 FIN 时，它需要让其应用程序进程知道此事，然后等待响应。如果应用程序本身就准备好终止连接，则会发送 FIN。如果应用程序还有更多数据要发送，它可以继续在这个现在处于“半关闭”状态的连接中发送更多数据，并在所有数据发送并确认后最终发送 FIN（这会引发发送第一个 FIN 的一方回复 ACK）。

RFC 793 还说明，在连接建立后，后续的段中总是会设置 ACK 标志。这也是为什么设置 FIN 时，ACK 标志也会一并设置。然而，FIN-ACK 组合并不等同于 SYN-ACK 组合。在响应 FIN 而发送的每个 ACK 的 TCP 头中，确认号

无论端口是开放还是关闭，Windows 机器总是会对空扫描、FIN 扫描和 Xmas 扫描发送 RST 响应。

- d. 当您将 **N** 改为 **F** 进行 FIN 扫描时，会看到相同的结果：

```
nmap -sF -p 445 192.168.1.121
```

（请替换为 Windows 10 虚拟机的 IP 地址。）

- e. 当您使用 **X** 进行 Xmas 扫描时，也会看到相同的结果：

```
nmap -sX -p 445 192.168.1.121
```

（请替换为 Windows 10 虚拟机的 IP 地址。）

空扫描、FIN 扫描和 Xmas 扫描的输出见图 16-11。

图 16-13 再次展示空扫描、FIN 扫描和 Xmas 扫描

Wireshark 显示，每次扫描对每个端口发送两个探测包，如图 16-14 所示。这是因为 Nmap 不确定数据包是否丢失，或者端口是被过滤还是开放。第二次尝试验证了其中一种情况。如果收到了 RST，则说明第一个数据包丢失。如果仍然没有响应，则说明端口是开放的或被过滤。

![](img/6e921d997751f0a0f87e5170ac9657a1_571_0.png)

图 16-14 Wireshark 显示被探测机器开启防火墙时的空扫描、FIN 扫描和 Xmas 扫描

- h. 在 Kali Linux 虚拟机中，按 CTRL-ALT-T 打开一个新的终端标签页。在新终端中，输入 sudo wireshark 并按回车键启动 Wireshark。双击 eth0 接口开始嗅探。

在 Kali Linux 原来的终端标签页中（您可以通过顶部每个标签页的按钮在它们之间切换），使用 Xmas 扫描扫描您的路由器：

```
sudo nmap -sX 192.168.1.1
```

（请替换为您的默认网关的 IP 地址，如果不同的话。）

要查找路由器的 IP 地址，请在 Windows 10 虚拟机中打开命令提示符并输入 **ipconfig**。此处应使用默认网关 IP 地址。

根据您的路由器配置，您可能会看到端口 80 和 443 都显示为开放或被过滤，如图 16-15 顶部所示。

```
Kali 2020.2 (Weissman) - VMware Workstation 15 Player (Non-commercial use only)

jonathan@kali-weissman:~$
jonathan@kali-weissman:~$ sudo nmap -sX 192.168.1.1
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 23:15 EDT
Nmap scan report for 192.168.1.1
Host is up (0.013s latency).
Not shown: 998 closed ports
PORT     STATE         SERVICE
80/tcp   open|filtered http
443/tcp  open|filtered https
MAC Address: 58:6D:8F:1E:1E:E4 (Cisco-Linksys)

Nmap done: 1 IP address (1 host up) scanned in 12.41 seconds
jonathan@kali-weissman:~$ sudo nmap -sA 192.168.1.1
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-10 23:15 EDT
Nmap scan report for 192.168.1.1
Host is up (0.087s latency).
All 1000 scanned ports on 192.168.1.1 are unfiltered
MAC Address: 58:6D:8F:1E:1E:E4 (Cisco-Linksys)

Nmap done: 1 IP address (1 host up) scanned in 11.55 seconds
jonathan@kali-weissman:~$
```

图 16-16 Wireshark 显示端口 80 是开放的，ACK 扫描阐明了 Xmas 扫描的结果

- k. 现在将 Wireshark 中的显示过滤器更改为以下内容，以查看所有的 ACK 和 RST。Wireshark 按类型分组捕获它们（先是 ACK，然后是 RST）。

```
ip.addr==192.168.1.1 && tcp
```

（同样，请确保替换为您的默认网关的 IP 地址，如果不同的话。）

### 4a–4e

```
ip.addr==192.168.1.114 && udp.port==53
```

（请替换为 Kali Linux 虚拟机的 IP 地址。）

这将从显示中过滤掉与 Windows 主机之间的任何 DNS 流量，只关注与 Kali Linux 虚拟机之间的 DNS 流量。

- d. 在 Kali Linux 中，探测那个大家通常称为路由器的设备上使用 UDP 监听 53 端口的服务：

```
sudo nmap -sU -p 53 192.168.1.1
```

（请替换为您的默认网关的 IP 地址，如果不同的话。）

当然，我们说的是 DNS。

瞧，UDP 扫描识别出了路由器中的 DNS 服务器，如图 16-19 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_578_0.png)

图 16-19 UDP 扫描发现 DNS 服务器。

