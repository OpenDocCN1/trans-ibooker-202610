

**图 20-5** 使用 whois 网站查找公司信息

**mheducation.com**

更新于 4 天前

**域名信息**

| 字段 | 值 |
|---|---|
| 域名 | mheducation.com |
| 注册商 | CSC Corporate Domains, Inc. |
| 注册日期 | 2000-06-08 |
| 到期日期 | 2018-06-08 |
| 更新日期 | 2017-06-04 |
| 状态 | clientTransferProhibited |
| 名称服务器 | pdns85.ultradns.biz<br>pdns85.ultradns.com<br>pdns85.ultradns.net<br>pdns85.ultradns.org |

**注册人联系信息**

| 字段 | 值 |
|---|---|
| 姓名 | 域名管理员 |
| 组织 | McGraw-Hill Global Education Holdings, LLC |
| 街道 | 2 Penn Plaza |
| 城市 | 纽约 |
| 州/省 | NY |
| 邮政编码 | 10121 |
| 国家 | 美国 |
| 电话 | +1.6094265291 |
| 传真 | +1.6094265291 |
| 电子邮件 | hostmaster@mheducation.com |

请注意，黑客攻击过程的 85% 都花在信息收集阶段以及收集可用于攻击的信息上。

**考试提示**

在信息收集阶段执行的侦察被称为被动侦察，因为黑客不接触组织的系统——黑客是从公开来源提取信息。

**考试提示**

在扫描和枚举阶段执行的侦察被称为主动侦察，因为黑客仍在收集信息，但通过与组织的系统进行通信来完成。

## 扫描与枚举

在信息收集阶段收集到 IP 地址后，黑客进入扫描阶段。此时黑客仍在进行侦察，但被认为是主动侦察，因为他们实际上正在向组织的系统发送流量。

扫描阶段的目标是通过找出系统上开放的端口号来了解系统上运行哪些服务。一旦黑客找出开放的端口号，他们就会对系统进行横幅抓取（banner grab），该操作会报告该端口上运行的软件版本。黑客想知道软件版本的原因是他们可以随后研究如何利用该软件。

为了找出系统上运行的服务，黑客会对系统进行端口扫描，以报告哪些端口是开放的。要进行端口扫描，黑客会使用像 nmap（命令行）这样的程序。例如，你可以使用 nmap -sT 10.0.0.1 对 10.0.0.1 系统进行端口扫描。图 20-6 显示了 nmap 命令的输出。稍后你将在“用于评估安全的工具”一节中了解更多关于端口扫描的内容。

**图 20-6** 使用 nmap 对系统执行端口扫描

```
bt ~ # nmap -sT 10.0.0.1
Starting Nmap 4.60 ( http://nmap.org ) at 2011-03-22 19:30 GMT
Interesting ports on 10.0.0.1:
Not shown: 1698 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
88/tcp   open  kerberos-sec
110/tcp  open  pop3
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
1025/tcp open  NFS-or-IIS
1026/tcp open  LSA-or-nterm
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
MAC Address: 00:0C:29:4E:76:03 (VMware)
Nmap done: 1 IP address (1 host up) scanned in 15.442 seconds
bt ~ #
```

黑客完成端口扫描后，会进行横幅抓取。你可以使用 telnet 10.0.0.1 25 之类的命令，通过 telnet 连接到系统上的特定端口来进行横幅抓取。在此示例中，你连接到 10.0.0.1 系统上的 25 端口，系统会发回一条“欢迎”消息，显示该端口上运行的软件版本。请注意，在图 20-7 中，服务器在 TCP 80 端口上运行 IIS 6.0 版。

**图 20-7** 使用 nmap 查出端口上运行的软件版本

```
bt ~ # nmap -sV 10.0.0.1
Starting Nmap 4.60 ( http://nmap.org ) at 2011-03-22 19:33 GMT
Interesting ports on 10.0.0.1:
Not shown: 1697 closed ports
PORT     STATE SERVICE       VERSION
21/tcp   open  ftp           Microsoft ftpd
25/tcp   open  smtp          Microsoft ESMTP 6.0.3790.0
53/tcp   open  domain        Microsoft DNS
80/tcp   open  http          Microsoft IIS-webserver 6.0
88/tcp   open  kerberos-sec  Microsoft Windows kerberos-sec
110/tcp  open  pop3          Microsoft Windows 2003 POP3 Service 1.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn
389/tcp  open  ldap          Microsoft LDAP server
445/tcp  open  microsoft-ds  Microsoft Windows 2003 microsoft-ds
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft LDAP server
1025/tcp open  msrpc         Microsoft Windows RPC
1026/tcp open  msrpc         Microsoft Windows RPC
3268/tcp open  ldap          Microsoft LDAP server
3269/tcp open  ssl/ldap      Microsoft LDAP server
3389/tcp open  ms-term-serv?
MAC Address: 00:0C:29:4E:76:03 (VMware)
Service Info: Host: 2003SERVERA.certworld.loc; OSs: Windows, Windows 2000
```

在黑客攻击过程的这一阶段，黑客还可能会尝试枚举系统，即收集更多关于系统的信息。黑客可能会尝试识别计算机名、用户名，甚至从系统中获取组列表。诸如 nmap 之类的工具可用于枚举系统。

网络映射软件也可用于识别已连接到网络的无效系统。这被称为流氓系统检测（rogue system detection）。

网络应用程序（如 Web 服务器、FTP 服务器，甚至启用了远程桌面的系统）都使用客户端连接的端口号。作为安全专业人员，你可以通过对那些开放相应端口的系统进行端口扫描，在网络中定位这些系统。端口扫描的结果会告诉你哪些系统上开放了哪些端口，这应该有助于你评估黑客可能以何种方式危害系统，因为每个应用程序都有不同的漏洞利用方式。

多年来，已经开发出不同类型的端口扫描。以下列表列出了几种主要的端口扫描类型：

- **TCP 连接扫描** TCP 连接扫描会与每个端口执行三次握手，以确定该端口是否开放。以下 nmap 命令演示如何对整个 192.168.2.0 网络执行 TCP 连接扫描：

    ```
    nmap -sT 192.168.2.0/24
    ```

- **SYN 扫描（半开）** 由于 TCP 连接扫描会执行完整的三次握手，因此会产生比必要更多的流量。你可以执行 SYN 扫描，也称为半开扫描或隐蔽扫描，因为它不完成完整的三次握手。扫描软件向端口发送 SYN 消息，如果目标系统回复 SYN-ACK，则丢弃连接。其原理是，既然收到了回复，那么该端口很可能已开放，因此无需发送三次握手的最终 ACK 消息。这种扫描也被称为隐蔽扫描，因为它产生的流量更少，希望避免被检测到，并且你不会完全打开到该端口的连接。以下命令演示如何使用 nmap 执行 SYN 扫描：

    ```
    nmap -sS 192.168.2.0/24
    ```

- **反向扫描** 反向扫描因其以与你预期相反的方式回应而得名。对于反向扫描，如果端口开放，则不发送回复；但如果端口关闭，则发送重置（RST）包（见图 20-8）。反向扫描的不同类型包括：空扫描（null scan），向端口发送一个未设置任何标志的数据包；XMAS 扫描，发送一个设置了 PSH、URG 和 FIN 标志的数据包。FIN 扫描发送一个仅设置了 FIN 标志的数据包。请注意，扫描的名称来自数据包中设置的标志。以下命令展示了 nmap 使用的 FIN 扫描和 XMAS 扫描：

    ```
    nmap -sF 192.168.2.0/24
    nmap -sX 192.168.2.0/24
    ```

请注意，反向扫描在 Windows 系统上不适用，因此会响应为没有端口开放。通常，在 Windows 网络上你会使用 SYN 扫描或 TCP 连接扫描。

**图 20-8** 反向扫描以你预期相反的方式回应。

![](img/3e3af33e2c753e920c09ecd40b5a457a_836_0.png)

