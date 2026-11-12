
你还可以让引导程序从 TFTP 服务器加载 IOS，但对于大镜像不推荐这样做，因为镜像是通过用户数据报协议（UDP）下载的，而 UDP 速度较慢。最后，你可以用 **boot system rom** 命令告诉引导程序从 ROM 中加载 Mini-IOS。要删除这些命令中的任何一个，只需在它们前面加上 **no** 参数即可。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_285_0.png)

**考试提示** **boot system** 命令可用于修改引导程序应从何处加载 IOS 的默认行为。当引导程序加载时，它会检查存储在 NVRAM 中的配置文件，查找 **boot system** 命令。如果找到这些命令，引导程序将使用这些命令来查找 IOS。如果未找到 **boot system** 命令，路由器将使用查找和加载 IOS 镜像的默认行为（首先是 flash 中的第一个镜像，然后向 TFTP 服务器广播，最后是 ROM 中的 IOS（如果存在））。当路由器启动时，如果你看到消息“boot: cannot open "flash:"”；这表明你错误配置了 **boot system** 命令，并且 flash 中对应的 IOS 镜像文件名不存在。

## 系统配置对话框

当路由器启动、运行硬件诊断并加载 IOS 软件后，IOS 随后会尝试在 NVRAM 中查找配置文件。如果找不到要加载的配置文件，IOS 就会运行系统配置对话框，通常称为 *设置* 模式，这是一个会提示你输入配置信息的脚本。该脚本的目的是向你提问，以便你能够在路由器上设置基本配置：它并非作为全功能配置工具而设计。换句话说，该脚本没有执行路由器所有配置任务的能力。相反，它被那些不太熟悉 IOS CLI 的新手使用。一旦你熟悉 CLI 以及路由器上的许多命令，你可能就再也不会使用此脚本了。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_286_0.png)

**考试提示** 请记住，如果在 NVRAM 中找不到启动配置，系统配置对话框脚本就会启动。

## 运行系统配置对话框

一种访问系统配置对话框的方法是在 NVRAM 中没有配置的情况下启动路由器。第二种方法是在特权 EXEC 模式下使用 **setup** 命令，如下所示：

```
Router# setup
    --- System Configuration Dialog ---
Continue with configuration dialog? [yes/no]: yes
At any point you may enter a question mark '?' for help.
Use ctrl-c to abort configuration dialog at any prompt.
Default settings are in square brackets '[ ]'.

Basic management setup configures only enough connectivity
for management of the system, extended setup will ask you
to configure each interface on the system

First, would you like to see the current interface summary? [yes]:
Interface IP-Address  OK? Method Status          Protocol
Ethernet0 unassigned  YES unset  administratively down down
Ethernet1 unassigned  YES unset  administratively down down
Serial0   unassigned  YES unset  administratively down down

Would you like to enter basic management setup? [yes/no]: no
Configuring global parameters:
  Enter hostname [Router]:
The enable secret is a password used to protect access to
privileged EXEC and configuration modes. This password, after
entered, becomes encrypted in the configuration.
Enter enable secret: dealgroup1
The enable password is used when you do not specify an
enable secret password, with some older software versions,
and some boot images.   Enter enable password: dealgroup2
The virtual terminal password is used to protect
access to the router over a network interface.
Enter virtual terminal password: cisco
Configure SNMP Network Management? [no]:
Configure LAT? [yes]: no
Configure AppleTalk? [no]:
Configure DECnet? [no]:
Configure IP? [yes]:
  Configure IGRP routing? [yes]: no
  Configure RIP routing? [no]:
.
.
.
Configuring interface parameters:
Do you want to configure Ethernet0 interface? [no]: yes
  Configure IP on this interface? [no]: yes
    IP address for this interface: 172.15.1.1
    Subnet mask for this interface [255.255.0.0] : 255.255.255.0
    Class B network is 172.15.0.0, 24 subnet bits; mask is /24
```

```
Do you want to configure Serial0 interface? [no]:
.
.
.
The following configuration command script was created:
hostname Router
enable secret 5 $1$/CCk$4r7zDwDNeqkxFO.kJxC3G0
enable password dealgroup2
line vty 0 4
  password cisco
.
.
end

[0] Go to IOS command prompt without saving this config.
[1] Return back to the setup without saving this config.
[2] Save this configuration to nvram and exit.
Enter your selection [2]: 2
```

方括号 ([ ]) 中包含的信息表示默认值——如果你按回车键，则使用方括号中的值。此脚本的一个问题是，如果你犯了错误，就无法返回到前一个问题。相反，你必须使用 ctrl-c 中断序列来中止脚本并重新开始。

以下各节将分解脚本的不同组件。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_288_0.png)

注 脚本向你提出的问题可能因路由器而异，具体取决于硬件型号、安装的接口以及运行在其上的软件。

## 状态和全局配置信息

脚本开始时，会询问你是否要继续。如果回答 yes，脚本将继续；否则，如果回答 no，脚本将被中止并返回到特权 EXEC 模式。接下来会询问你是否要查看路由器的接口状态。如果回答 yes，你将看到路由器上的所有接口、接口的 IP 地址以及接口的状态。

在状态信息之后，会显示实际配置。配置的第一部分处理路由器除接口之外的所有配置信息，接口是第二部分。在配置的这一部分中，会要求你提供诸如特权 EXEC 密码、虚拟类型终端（VTY）密码（telnet 和 SSH）、你希望全局激活的网络协议以及其他全局配置信息等内容。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_289_0.png)

**注** 请注意，脚本中会提示你输入两个特权 EXEC 密码：**enable secret** 和 **enable password**。尽管你通常只配置一个，但脚本要求你同时输入两者，并且要求两个密码不同。当设置了 secret 后，你需要使用 secret 进入特权 EXEC 模式。

## 协议和接口配置信息

在为路由器配置全局信息后，脚本会引导你回答有关要使用哪些接口以及应如何配置它们的问题。脚本足够智能，可以根据你对全局问题的回答来提问配置问题。例如，如果你激活了 IP，脚本会询问你每个已激活的接口是否要处理 IP，如果回答是，则询问该接口的 IP 地址信息。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_290_0.png)

**考试提示** 你应该记住，当路由器启动且 NVRAM 中没有配置时，或从特权 EXEC 模式使用 **setup** 命令时，系统配置对话框脚本会启动。此外，要了解 Setup 对话框脚本末尾的三个选项。并且请记住，你可以按 ctrl-c 来中止脚本。

## 退出设置模式

在你回答完脚本的所有配置问题后，会显示脚本根据你对脚本问题的回答创建的路由器配置。请注意，IOS 尚未激活配置文件。仔细检查配置，然后从表 6-4 中显示的三个选项中选择一个。此外，如果你输入 **1** 作为选项，当脚本重新开始时，你之前输入的信息会出现在方括号中，并在空行上按回车键时成为默认值。

| 选项 | 描述 |
|---|---|
| 0 | 丢弃脚本的配置并返回到特权 EXEC 模式。 |
| 1 | 返回脚本的开头。 |
| 2 | 激活脚本的配置，将配置保存到 NVRAM，并返回到特权 EXEC 模式。 |

表 6-4 系统配置对话框结束时的选项

![](img/0eafdcebe219ef3756f0e57bcf1eb997_290_1.png)

**视频 6.03.** 本书附带的数字资源包含使用系统配置对话框的
