
当你管理计算机的网络设置时，`Win32_NetworkAdapterConfiguration` WMI 类要求你知道网络适配器的描述。通过查看 `Get-CimInstance` `Win32_NetworkAdapterConfiguration` `-Computer` `ComputerName` 的输出来获取它：

```
PS > Get-CimInstance Win32_NetworkAdapterConfiguration -Computer LEE-DESK

(...)
DHCPEnabled      : True
IPAddress        : {192.168.1.100}
DefaultIPGateway : {192.168.1.1}
DNSDomain        : hsd1.wa.comcast.net.
ServiceName      : USB_RNDIS
Description      : Linksys Wireless-G USB Network Adapter with (...)
Index            : 13
(...)
```

知道要续订哪个适配器后，你现在可以像解决方案中所示那样调用该对象上的方法。要再次在适配器上启用 DHCP，请使用 `EnableDHCP()` 方法：

```
PS > $adapter | Invoke-CimMethod -Name EnableDHCP
```

有关在 PowerShell 中使用 WMI 的更多信息，请参阅 [Recipe 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

[Recipe 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

# 27.21 列出计算机的所有 IP 地址

## 问题

你想列出计算机的所有 IP 地址。

## 解决方案

要列出分配给计算机的 IP 地址，请使用 `ipconfig` 应用程序：

```
ipconfig
```

## 讨论

标准的 `ipconfig` 应用程序非常适合管理本地计算机上的网络配置选项。要查看远程计算机上的 IP 地址，你有两个选项。

### 使用 Win32_NetworkAdapterConfiguration WMI 类

要查看远程计算机的 IP 地址，请使用 `Win32_NetworkAdapterConfiguration` WMI 类。由于它列出所有网络适配器，请使用 `Where-Object` cmdlet 将结果限制为已分配 IP 地址的适配器：

```
PS > Get-CimInstance Win32_NetworkAdapterConfiguration -Computer LEE-DESK |
    Where-Object { $_.IpEnabled }


DHCPEnabled      : True
IPAddress        : {192.168.1.100}
DefaultIPGateway : {192.168.1.1}
DNSDomain        : hsd1.wa.comcast.net.
ServiceName      : USB_RNDIS
Description      : Linksys Wireless-G USB Network Adapter with SpeedBooste
                   r v2 - Packet Scheduler Miniport
Index            : 13
```

### 在远程计算机上运行 ipconfig

查看远程计算机 IP 地址的另一种方法是使用 PowerShell 远程处理或 [Recipe 29.8](#ch29.xhtml#program_invoke_a_powershell_expression_o) 提供的解决方案：

```
PS > Invoke-Command LEE-DESK { ipconfig }
PS > Invoke-RemoteExpression \\LEE-DESK { ipconfig }
```

有关在 PowerShell 中使用 WMI 的更多信息，请参阅 [Recipe 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

[Recipe 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

[Recipe 29.8，“程序：在远程计算机上调用 PowerShell 表达式”](#ch29.xhtml#program_invoke_a_powershell_expression_o)

# 27.22 列出网络适配器属性

## 问题

你想检索计算机上网络适配器的信息。

## 解决方案

要检索计算机上网络适配器的信息，请使用 `Win32_NetworkAdapterConfiguration` WMI 类：

```
Get-CimInstance Win32_NetworkAdapterConfiguration -Computer <ComputerName>
```

要仅列出已分配 IP 地址的适配器，请使用 `Where-Object` cmdlet 按 `IpEnabled` 属性进行筛选：

```
PS > Get-CimInstance Win32_NetworkAdapterConfiguration -Computer LEE-DESK |
    Where-Object { $_.IpEnabled }


DHCPEnabled      : True
IPAddress        : {192.168.1.100}
DefaultIPGateway : {192.168.1.1}
DNSDomain        : hsd1.wa.comcast.net.
ServiceName      : USB_RNDIS
Description      : Linksys Wireless-G USB Network Adapter with SpeedBooster
                   v2 - Packet Scheduler Miniport
Index            : 13
```

## 讨论

该解决方案使用 `Win32_NetworkAdapterConfiguration` WMI 类来检索给定系统上网络适配器的信息。默认情况下，PowerShell 仅显示关于网络适配器的最重要信息，但它提供了访问更多信息的途径。

要查看所有可用信息，请使用 `Format-List` cmdlet，如 [示例 27-6](#ch27.xhtml#using_the_format-list_cmdlet_to_see_deta) 所示。

### 示例 27-6. 使用 Format-List cmdlet 查看网络适配器的详细信息

```
PS > $adapter = Get-CimInstance Win32_NetworkAdapterConfiguration |
    Where-Object { $_.IpEnabled }

PS > $adapter
DHCPEnabled      : True
IPAddress        : {192.168.1.100}
DefaultIPGateway : {192.168.1.1}
DNSDomain        : hsd1.wa.comcast.net.
ServiceName      : USB_RNDIS
Description      : Linksys Wireless-G USB Network Adapter with SpeedBooster
                   v2 - Packet Scheduler Miniport
Index            : 13

PS > $adapter | Format-List *

DHCPLeaseExpires             : 20070521221927.000000-420
Index                        : 13
Description                  : Linksys Wireless-G USB Network Adapter with
                                SpeedBooster v2 - Packet Scheduler Miniport
DHCPEnabled                  : True
DHCPLeaseObtained            : 20070520221927.000000-420
DHCPServer                   : 192.168.1.1
DNSDomain                    : hsd1.wa.comcast.net.
DNSDomainSuffixSearchOrder   :
DNSEnabledForWINSResolution  : False
DNSHostName                  : Lee-Desk
DNSServerSearchOrder         : {68.87.69.146, 68.87.85.98}
DomainDNSRegistrationEnabled : False
FullDNSRegistrationEnabled   : True
IPAddress                    : {192.168.1.100}
IPConnectionMetric           : 25
IPEnabled                    : True
IPFilterSecurityEnabled      : False
WINSEnableLMHostsLookup      : True
(...)
```

要检索特定属性，像访问其他 PowerShell 对象的属性一样访问它们：

```
PS > $adapter.MacAddress
00:12:17:77:B4:EB
```

有关在 PowerShell 中使用 WMI 的更多信息，请参阅 [Recipe 28.1](#ch28.xhtml#access_windows_management_instrumentatio)。

## 另请参阅

[Recipe 28.1，“访问 Windows 管理规范和 CIM 数据”](#ch28.xhtml#access_windows_management_instrumentatio)

# 第 28 章. CIM 和 Windows 管理规范

## 28.0 简介

Windows 管理规范 (WMI) 长期以来一直是 Windows 的核心管理功能。它提供了惊人的广度、广泛的覆盖范围和无处不在的远程管理。

然而，WMI 过去缺乏一种好的访问方式。从图形界面来看，`wbemtest.exe` 实用程序允许你试验 WMI、其命名空间和类。但它确实是一个测试工具，因为其复杂的 UI 使其在大多数场景中不切实际（请参阅 [图 28-1](#ch28.xhtml#wbemtest_process)）。

一个更用户友好的替代方案是 `wmic.exe` 命令行工具。`WMIC` 工具允许你交互式地查询 WMI——但更重要的是，自动化其行为。与 PowerShell 一样，`WMIC` 中的结果保留了大量结构化信息，并允许你编写相当详细的查询：

```
PS > WMIC logicaldisk WHERE drivetype=3 `
    GET "name,freespace,SystemName,FileSystem,Size"

FileSystem  FreeSpace    Name  Size         SystemName
NTFS        10587656192  C:    34357637120  LEEHOLMES1C23
```

然而，该语言功能有限，一旦 `WMIC` 将输出转换为文本，所有数据的结构都会丢失。

到目前为止，WMI 最流行的 UI 是 `VBScript`，即管理员的传统脚本语言。`VBScript` 提供了比 `WMIC` 丰富得多的语言功能，并在脚本的整个运行期间保留 WMI 的结构化数据。

![](Images/wps4_2801.png)

然而，VBScript 有其自身的一类易用性困难。例如，生成计算机上运行进程的报告通常最终看起来像这样：
