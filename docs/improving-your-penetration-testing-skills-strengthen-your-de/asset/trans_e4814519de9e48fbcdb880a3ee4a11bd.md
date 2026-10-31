

创建初始数据库架构
# cowsay++
< metasploit >
-------------
    \  '.__'
     \ (oo)____
        (__)   )\ 
            ||--|| *

    =[ metasploit v4.16.8-dev- ]
+ -- --=[ 1683 exploits - 964 auxiliary - 299 post ]
+ -- --=[ 498 payloads - 40 encoders - 10 nops ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf >

## 还有更多...

另外，你也可以通过在终端窗口中输入 msfconsole 来启动 Metasploit Framework。

## 升级 Kali Linux

作为一个滚动发行版，升级 Kali Linux 很简单。建议定期升级 Kali Linux，以确保你能获得最新的安全更新。要升级，请使用 apt update 然后 apt upgrade；apt 会查找可以升级而无需删除任何软件包的已安装软件包，这样是最不具侵入性的方式。

对于主要版本升级和重要升级，请使用 apt full-upgrade；这将执行完整升级，并在必要时删除过时的软件包或安装新的依赖项。

## 设置渗透测试实验室

创建渗透测试实验室至关重要，它将允许你在安全环境中练习和测试新的技术和漏洞利用。使用虚拟机作为实验室环境将为你提供可移植性、灵活性和低维护性。你可以同时在多个操作系统上工作，设置复杂的网络场景，并在多个目标上执行渗透测试。因此，让我们快速了解一下如何使用虚拟化设置渗透测试实验室。

### 准备工作

对于你的实验室，你可以使用你选择的 hypervisor；最常见的 hypervisor 是 VirtualBox、VMware Workstation Pro、VMware Fusion Pro（适用于 Mac）、VMware ESXi 和 Microsoft Hyper-V。对于本书中使用的渗透测试实验室，我建议你使用 VirtualBox，因为它是一个开源 hypervisor，并且是构建其中一个虚拟机的要求。

![](img/6b6e557533100c0e47951d0e5e14c4ee_374_0.png)

虽然你需要使用 VirtualBox 构建虚拟机，但在构建机器后你可以将其导入到你喜欢的任何 hypervisor。

这是渗透测试实验室的网络图：

![](img/6b6e557533100c0e47951d0e5e14c4ee_374_1.png)

我们将使用四台虚拟机，分别是 Kali Linux、Linux 服务器、Windows 服务器和 Windows 10 客户端。在这个实验室中，我们有一个现代场景，这将允许我们测试和练习最新的技术和漏洞利用。

### 如何操作...

对于 Kali Linux 机器、Linux 服务器和 Windows 10 客户端，设置很简单。我们可以从官方网站 https://www.kali.org/downloads/ 下载 Kali Linux 虚拟机；对于 Linux 服务器，我们将使用 Metasploitable 2 机器，你可以从 SourceForge 下载 https://sourceforge.net/projects/metasploitable/files/Metasploitable2/；对于 Windows 10 客户端，我们可以从 Microsoft Developer 网站 https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/ 下载 90 天试用版。

对于最后一台机器，我们将使用 Metasploitable 3，这是一个我们将构建的 Windows 虚拟机，其中包含许多安全漏洞供我们测试。要构建 Metasploitable 3 机器，我们必须安装 Packer、Vagrant、Vagrant Reload 插件和 VirtualBox。构建脚本和文档以及最新的构建说明可以在官方 GitHub 仓库找到：https://github.com/rapid7/metasploitable3。要自动构建机器，请执行以下步骤：

- 1. 如果使用 Bash，请运行 `build_win2008.sh` 脚本，如果使用 Windows，请运行 `build_win2008.ps1`。
- 2. 成功完成后，运行 `vagrant up`。
- 3. 进程完成后，你应该能够在 VirtualBox 中打开 VM 并使用用户名 `vagrant` 和密码 `vagrant` 登录。

在启动虚拟机之前，有一个重要的配置你必须进行，以便为实验室设置网络通信：

- 1. 选择 Kali Linux 虚拟机并点击 **Settings**。然后，移动到 **Removable Devices**。在 **Network Adapter** 选项中，网络适配器应配置为使用 **Internet Sharing | Share with my Mac**，这将允许虚拟机访问互联网，共享主机机器的 IP 地址，因为它将为虚拟机到网络的流量提供 **Network Address Translation (NAT)**。

2. Metasploitable 3 虚拟机的网络适配器和 Metasploitable 2 虚拟机的第一个网络适配器也应配置为使用 NAT：

![](img/6b6e557533100c0e47951d0e5e14c4ee_376_0.png)

3. 在 VMware Fusion 中，转到 Preferences，选择 Network 选项卡，并创建一个自定义网络。选中通过 DHCP 在此网络上提供地址的框；使用 Subnet IP 为 10.0.0.0 和 Subnet Mask 为 255.255.255.0：

## Network

- Internet Sharing
- Share with my Mac
- Bridged Networking
- Autodetect
- Wi-Fi
- Bluetooth PAN
- Custom
- Private to my Mac
- vmnet2

使用此配置的虚拟机将使用自定义网络连接。

☑ Allow virtual machines on this network to connect to external networks (using NAT)

☐ Enable IPv6
IPv6 Prefix: fd15:4ba5:5a2b:1002::/64

## Port Forwarding

| Host Port | Type | VM IP Address | Description |
|---|---|---|---|

☑ Connect the host Mac to this network
☑ Provide addresses on this network via DHCP

Subnet IP: 10.0.0.0
Subnet Mask: 255.255.255.0

☑ Require authentication to enter promiscuous mode
Click the lock to prevent further changes.

General  Keyboard & Mouse  Display  Default Applications  Network  Feedback

4. 现在你已经创建了自定义网络，选择 Windows 10 虚拟机，点击 Settings，然后进入 Network Adapter 设置。选择 Custom network 并选择我们创建的自定义网络。对 Metasploitable 2 虚拟机的第二个 Network Adapter 重复此过程：

![](img/6b6e557533100c0e47951d0e5e14c4ee_378_0.png)

5. 要验证配置，请登录到 Metasploitable 2 机器并使用 ip a 命令。root 用户的默认用户名为 msfadmin，密码为 msfadmin：

![](img/6b6e557533100c0e47951d0e5e14c4ee_378_1.png)

### 它是如何工作的...

通过创建两个 NAT 网络，我们可以使用第一个 NAT 网络模拟面向互联网的服务器，并使用我们创建的自定义网络作为内部机器，从而提供更真实的场景，并让你有机会学习如何进行内部目标的侦察、枢轴和横向移动。

## 设置 SSH 连接

Secure Shell (SSH) 允许你通过不安全的网络安全地连接到远程主机。

### 准备工作

要配置 Kali Linux 机器以进行远程登录，我们将首先更改默认的 root 密码并生成新的 SSH 主机密钥。

### 如何操作...

要更改 root 密码，请按如下方式使用 passwd 命令：

```
root@kali:~# passwd
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

要生成新的 SSH 主机密钥，步骤也相对简单：删除当前的 SSH 主机密钥，使用 dpkg-reconfigure openssh-server 命令重新配置 OpenSSH 服务器，并生成新的 SSH 主机密钥：

```
root@kali:~# rm /etc/ssh/ssh_host_*
root@kali:~# dpkg-reconfigure openssh-server
Creating SSH2 RSA key; this may take some time ...
2048 SHA256:0k/J4YvIGYi eDI6YuOLDXADm5YUdrJSnzBKguuD9WWQ root@kali (RSA)
Creating SSH2 ECDSA key; this may take some time ...
256 SHA256:eYU5TtQVzFYQtjo6l yiVHku6SQWbgkMPMDtW8cgaAJ4 root@kali (ECDSA)
Creating SSH2 ED25519 key; this may take some time ...
256 SHA256:8nj2LMKQ NOLKS9S90sWcBArs1PgpFfD/5h4vNrwI4sA root@kali (ED25519)
```

出于实验室目的，我们将编辑 OpenSSH 服务器配置 `/etc/ssh/sshd_config` 文件以允许 `root` 登录，通过将行 `#PermitRootLogin without-password` 更改为 `PermitRootLogin yes`，如以下示例所示：

```
...
# Authentication:
#LoginGraceTime 2m
PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
...
```

要使 OpenSSH 服务在启动时自动运行，请运行 `systemctl enable ssh` 并通过使用 `systemctl restart ssh` 命令重新启动服务来完成配置，如下所示：

```
root@kali:~# systemctl enable ssh
Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
root@kali:~# systemctl restart ssh
root@kali:~#
```

这对于实验室环境来说是可以的，但在执行渗透测试时，请将 SSH 配置为使用加密密钥登录到 Kali Linux 机器。这比仅使用密码更安全。

## 使用 SSH 连接到 Kali

要连接到 Kali 机器，我们只需要一个 SSH 客户端。大多数 Unix、Linux 和 macOS 操作系统已经安装了 SSH 客户端；然而，如果你使用 Windows 连接到 Kali Linux 机器，你将需要安装一个客户端，例如 PuTTY，它是 Windows 上最受欢迎和免费的 SSH 客户端之一。

