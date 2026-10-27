

## 认证目标 19.02

### 监控工具

除了使用入侵检测系统之外，你还可以使用工具对系统进行手动监控。随着你对系统接触的增多，以及应对或排查潜在安全问题的需要，这些工具将变得越来越重要。

### 有用的系统命令

每个操作系统中都有许多命令用于帮助排查问题和处理安全事件。你在本书前面已经接触过其中一些命令，但我希望在这里复习它们，并补充一些新命令。

### Windows 命令

你可以使用许多 Windows 命令来监控系统活动。以下是一些常用命令，在监控 Windows 系统时，你应该练习并熟练掌握它们。

#### netstat

你应该熟悉的第一个命令是 netstat 命令。netstat 命令用于显示所有协议连接信息。例如，你可以使用 netstat -n 以系统和端口的数字格式（而不是友好名称）查看到达系统的所有 TCP 连接。你还应该定期使用 netstat -na -o 命令查看系统上所有正在监听的端口列表（-o 用于显示进程 ID，以便你知道是哪个程序打开了该端口）。以下命令对于查看 TCP 相关连接非常有用：

```
netstat -n
netstat -na -o
```

#### net session

Windows 的 net session 命令可用于显示通过 Windows 文件共享连接到你的系统的计算机。会话列表会显示连接到系统的客户端的 IP 地址，以及它们用于向系统进行身份验证的用户名。在下面的代码清单中，你可以看到有两台计算机连接到我的系统（10.0.0.2 和 10.0.0.3），并且它们都使用管理员账户进行了身份验证：

```
C:\>net session
Computer              User name              Client Type      Opens Idle time
-------------------------------------------------------------------------------
\\10.0.0.2            ADMINISTRATOR                         1 00:00:00
\\10.0.0.3            ADMINISTRATOR                         1 00:01:46
The command completed successfully.
C:\>
```

#### tasklist

我最喜欢的命令之一 tasklist 用于监控系统上运行的进程。当你使用前面提到的带 -o 选项的 netstat 命令时，你会获得打开端口的程序的进程 ID。你可以将该信息与 tasklist 命令配合使用，以找出该进程 ID 对应的 EXE 文件名。以下是 tasklist 输出的部分显示：

```
C:\Users\Administrator>tasklist
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ===========
System Idle Process              0 Services                 0          24 K
System                           4 Services                 0          64 K
smss.exe                       308 Services                 0         968 K
csrss.exe                      500 Services                 0       4,460 K
wininit.exe                    576 Services                 0       4,924 K
csrss.exe                      584 Console                  1      10,996 K
services.exe                   640 Services                 0       9,376 K
winlogon.exe                   664 Console                  1       6,276 K
```

**实战经验**

> 几年前，在排查一台感染病毒的系统时，因为任务管理器程序已被病毒禁用，我不得不使用 tasklist 和 taskkill 命令来结束导致问题的进程。

#### taskkill

监控系统时，如果你注意到内存中运行着某个可能导致性能或安全问题的进程，可以使用 taskkill 命令结束该进程。下面展示了如何按进程 ID 号或可执行文件名终止进程：

```
C:\Users\Administrator>taskkill /IM notepad.exe /F
SUCCESS: The process "notepad.exe" with PID 3456 has been terminated.
```

如果不使用 /F 开关强制结束进程，系统会提示你保存应用程序中正在运行的文档所做的更改。如果你确定要结束进程，并且不想保存程序中可能存在的任何数据，可以使用 /F 开关强制结束程序。taskkill 命令中的 /IM 开关代表“映像名称”，也就是你想要终止的程序的名称。

#### whoami

如果你需要知道自己以什么身份登录到系统，可以使用 whoami 命令。该命令将显示当前登录的用户名。使用 whoami 命令时，可以使用 /groups 开关列出你所属的组，或者使用 /LOGONID 开关查看你账户的登录 ID；这两者在排查访问控制问题时都很有用。下面的代码清单显示我以 administrator 身份登录到名为 2008R2 的系统：

```
C:\>whoami
2008r2\administrator
C:\>whoami /LOGONID
S-1-5-5-0-3295383
```

#### net statistics

在 Windows 中，另一个很好的监控命令是 net statistics server 命令，你可以用它密切关注服务器上发生的情况。该命令将显示诸如已接受的会话数、密码违规次数（失败的登录尝试）、权限违规次数（因没有权限而访问失败）以及打印到系统的打印作业数等信息。

```
C:\>net statistics server
Server Statistics for \\2003SERVERA
Statistics since 3/26/2011 11:09 AM
Sessions accepted                 2
(Output cut for briefness)
System errors                     0
Permission violations             3
Password violations               16
Files accessed                    11
Communication devices accessed    0
Print jobs spooled                0
(Output cut for briefness)
The command completed successfully.
```

你还可以使用 net statistics workstation 命令查看有关系统上客户端软件的信息以及发出网络资源请求的次数。通过 net statistics workstation 命令，你可以查看网络上任何资源请求失败的信息。

**图 19-3** Linux 中的系统监视器

![](img/3e3af33e2c753e920c09ecd40b5a457a_784_0.png)

选择“系统”菜单 | “管理”，然后启动“系统监视器”。“系统监视器”有三个选项卡（见图 19-3）：

- **进程** “进程”选项卡显示正在运行的进程及其关联的进程 ID。你可以使用“结束进程”按钮终止进程。
- **资源** “资源”选项卡显示折线图，展示内存使用情况、处理器使用情况和网络活动的历史记录。
- **文件系统** “文件系统”选项卡显示可用的文件系统及相关设备。

### 协议分析器与嗅探器

协议分析器（或称网络嗅探器）是一种出色的网络监控工具，允许你捕获并检查网络流量。这两者通常是同一回事，只不过协议分析器可能针对特定协议，而网络嗅探器会分析所有网络流量，无论使用的是何种网络协议。

9. 在 Ethernet II 部分下面是协议信息。该网络流量使用的是什么第三层协议？__________________

10. 如果你在上一题中回答了“IP”，那就对了！如果你双击“Internet Protocol Version 4”部分，你将看到数据包源地址和目的地址中的第三层地址（IP 地址）是什么。

11. 填写以下信息：

    数据包发往哪里？__________________  
    数据包来自哪里？________________  

    *提示：查看源地址和目的地址。*

12. 你还可以看到 IP 用来传递此数据包的传输协议。在源 IP 地址上方三行，你可以看到 IP 使用 TCP（一种面向连接的第四层协议）来确保数据包到达目的地。

13. 如果你双击 IP 标题，将折叠 IP 的详细信息。我们来看一下此数据包的应用协议信息。你想查看在网页中输入的信用卡号。在“数据包详细信息”窗格中，双击“Hypertext Transfer Protocol”以展开详细的应用信息。

14. 选择 HTTP 的最后一条信息，即“File Data:”行。要查看你捕获的信用卡号，请查看屏幕底部的 ASCII 部分。找到 CCNumber 字段。注意信用卡号。

15. 信用卡号是多少？________________

16. 关闭 Wireshark。

![](img/3e3af33e2c753e920c09ecd40b5a457a_786_0.png)

### 了解 Syslog

Syslog 是一种行业标准协议，允许任何支持 syslog 的系统、设备和应用将日志消息发送到中央 syslog 服务器，以便你可以集中查看和管理日志。你可以将 Syslog 配置在交换机、路由器、服务器、防火墙和入侵检测系统（IDS）上，将记录的事件发送到 syslog 服务器。这是一个很棒的功能！

