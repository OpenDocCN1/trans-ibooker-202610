

设备（如果已配置 Telnet 并且你知道密码）。这是你可以使用 CDP 记录网络拓扑的方法。

## 为 Cisco 设备授权

为许多 Cisco 平台上的 Cisco ISR G2 路由器授权变得越来越普遍；甚至 Cisco 的交换机也开始支持对功能和使用进行授权。

在 Cisco 路由器使用 Cisco IOS 15 之前的版本中，Cisco 针对特定设备型号和软件版本最多有 12 种不同的映像类型。为简化这一点，Cisco 现在只有四种映像类型：

- IP Base
- Data (MPLS 和 ATM)
- 统一通信 (VoIP 和 IP 电话)
- 安全 (防火墙、IPS 和 VPN)

你可以使用 **show license feature** 命令查看路由器支持的技术包许可证和功能许可证。以下是使用此命令的示例：

```
Router# show license feature
Feature name  Enforcement Evaluation Subscription Enabled RightToUse
ipbasek9      no          no          no           yes      no
securityk9    yes         yes         no           yes      yes
uck9          yes         yes         no           yes      yes
datak9        yes         yes         no           no       yes
LI            yes         no          no           no       no
ios-ips-update yes        yes         yes          no       yes
```

许可证强制执行从 Cisco ISR G2 路由器（1900、2900 和 3900 系列）开始。许可证解锁 IOS 代码中的功能。许可证有两种类型：

- **评估** 此许可证是一种临时许可证，在特定时期内有效，例如 60 天或一年。
- **永久** 此许可证在产品生命周期内有效。

许可证与 Cisco ISR G2 路由器的两个值相关联，称为通用设备标识符 (UDI)：

- 产品 ID（平台类型）
- 序列号（位于主板 ROM 中）

UDI 的重要性在于，你从 Cisco 获得的许可证与此值绑定。换句话说，你不能从一个路由器获取许可证并将其复制到另一个路由器：许可证与你的 Cisco ISR G2 路由器的 UDI 值绑定。以下是在 3925 ISR G2 路由器上查看用于许可证用途的 UDI 信息的示例：

```
Router# show license udi
Device#   PID            SN            UDI
------------------------------------------------------------
*0        C3900-SPE100/K9  FHH13030044   C3900-SPE100/K9:FHH13030044
```

使用 **show license feature** 命令查看安装在路由器上的包和功能许可证。

## 安装许可证

你的路由器随附大多数支持的包和功能的评估许可证（也称为临时许可证）。如果你想尝试新的软件包或功能，可以为该包或功能激活评估许可证。

要安装永久许可证，请使用以下特权 EXEC 命令：

```
Router# license install URL-location
```

许可证文件是你从 Cisco 下载的 XML 文件。通常你会将其放置在路由器的 flash 中，但也可以通过 TFTP 或 FTP 从远程服务器获取。以下是在 3950 路由器上安装许可证的示例：

```
Router# license install flash0:uck9-C3900-SPE150_K9-FHH12250057.xml
Installing licenses from "uck9-C3900-SPE150_K9-FHH12250057.xml"
Installing...Feature:uck9...Successful: Supported
1/1 licenses were successfully installed
0/1 licenses were existing licenses
0/1 licenses were failed to install
```

安装许可证后，在路由器重新启动之前不会使用它。

你可以执行 **reload** 命令来重新启动路由器。

要激活评估许可证以试用技术包，请使用此命令：

```
Router(config)# license boot module module_name
                technology-package package_name
```

*module_name* 是产品型号，例如 3950 路由器的 **c3900**。

技术 *package_name* 是四个包中的一个。例如，安全包参数是 **securityk9**。与永久许可证一样，你必须重新启动路由器才能使许可证生效。

## 许可证验证

你可以用来验证许可证的第一个命令之一是 **show version** 命令（在第 6 章中介绍）。以下是基于前面许可证安装示例的示例：

```
Router# show version
Cisco IOS Software, C3900 Software (C3900-UNIVERSALK9-M), Version 12.4(24.6)
P11k P11 ENGINEERING WEEKLY BUILD, synced to V124_24_6_T9
.
.
.
Cisco C3945 (revision 1.0) with 2025472K/71680K bytes of memory.
Processor board ID FHH1226P01E
3 Gigabit Ethernet interfaces
4 Serial(sync/async) interfaces
2 ISDN Basic Rate interfaces
1 ATM interface
25 terminal lines
1 Virtual Private Network (VPN) Module
DRAM configuration is 72 bits wide with parity enabled.
255K bytes of non-volatile configuration memory.
2000880K bytes of ATA System CompactFlash 0 (Read/Write)
License Info:
License UDI:
---------------------------------------------
Device#    PID                 SN
---------------------------------------------
*0         C3900-SPE150/K9     FHH12250057
Technology Package License Information for Module:'c3900'
------------------------------------------------------------
Technology    Technology-package    Technology-package
              Current              Type               Next reboot
------------------------------------------------------------
ipbase        ipbasek9            Permanent          ipbasek9
security      None                None               None
uc            uck9                Permanent          uck9
data          None                None               None
```

请注意，在此示例中，路由器已安装 IP Base 和 统一通信 (UC) 永久许可证。

你也可以使用 **show license** 命令查看已安装的许可证，如以下示例所示：

```
Router# show license
Index 1 Feature: ipbasek9
  Period left: Life time
  License Type: Permanent
  License State: Active, In Use
  License Count: Non-Counted
  License Priority: Medium
Index 2 Feature: securityk9
  Period left: 8 weeks, 3 days
  Period Used: 15 minute 38 second
  License Type: EvalRightToUse
  License State: Active, In Use
  License Count: Non-Counted
  License Priority: Low
Index 3 Feature: uck9
  Period left: Not Activated
  Period Used: 0 minute 0 second
  License Type: EvalRightToUse
  License State: Not in Use, EULA not accepted
  License Count: Non-Counted
  License Priority: None
.
.
.
```

在此示例中，IP Base 包已安装永久许可证，安全包已安装激活的评估许可证，而统一通信包未安装许可证。

## 管理许可证

本节将介绍如何备份许可证以及如何卸载许可证。要备份许可证，请使用 license save 命令：

```
Router# license save URL
```

你可以将许可证文件保存到本地文件系统或远程服务器。以下是使用此命令的示例：

```
Router# license save flash:/licenses.lic
license lines saved.....to flash:licenses.lic
```

要从路由器卸载（删除）许可证，需要执行两个操作：

1. 禁用技术包。
2. 清除许可证。

要禁用技术包，请使用 **license boot module** 命令。以下是在 3900 系列路由器上禁用统一通信包的示例：

```
Router(config)# license boot module c3900 technology-package uck9 disable
Router(config)# exit
Router# copy running-config startup-config
Router# reload
```

路由器重新启动后，你需要使用 **license clear** 命令清除许可证。以下是示例：

```
Router# license clear uck9
*Jul  7 00:34:23.691: %SYS-5-CONFIG_I: Configured from console by console
clear uck9
Feature: uck9
  1   License Type: Permanent
      License State: Active, Not in Use
      License Addition: Exclusive
      License Count: Non-Counted
      Comment:
      Store Index: 15
      Store Name: Primary License Storage
Are you sure you want to clear? (yes/[no]) : yes
*Jul  7 00:34:31.223: %LICENSE-6-REMOVE: Feature uck9 1.0 was removed
from this device.
UDI=C3900-SPB150/K9:FHH12250057; StoreIndex=15:Primary License Storage
```

完成后，你可以重新启用技术包：

```
Router(config)# no license boot module c3900 technology uck9 disable
Router(config)# exit
Router# reload
```

重新启动后，使用 **show version** 命令验证许可证更改。

