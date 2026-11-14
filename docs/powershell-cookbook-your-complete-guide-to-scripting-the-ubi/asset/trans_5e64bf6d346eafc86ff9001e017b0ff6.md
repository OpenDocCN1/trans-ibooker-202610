
PS > `[ADSI] "WinNT://./Administrator"` | `Format-List *`

UserFlags                  : {66113}
MaxStorage                 : {-1}
PasswordAge                : {19550795}
PasswordExpired            : {0}
LoginHours                 : {255 255 255 255 255 255 255 255 255 255 255
                             255 255 255 255 255 255 255 255 255 255}
FullName                   : {}
Description                : {Built-in account for administering the computer/domain}
BadPasswordAttempts        : {0}
LastLogin                  : {5/21/2007 3:00:00 AM}
HomeDirectory              : {}
LoginScript                : {}
Profile                    : {}
HomeDirDrive               : {}
Parameters                 : {}
PrimaryGroupID             : {513}
Name                       : {Administrator}
MinPasswordLength          : {0}
MaxPasswordAge             : {3710851}
MinPasswordAge             : {0}
PasswordHistoryLength      : {0}
AutoUnlockInterval         : {1800}
LockoutObservationInterval : {1800}
MaxBadPasswordsAllowed     : {0}
RasPermissions             : {1}
objectSid                  : {1 5 0 0 0 0 0 5 21 0 0 0 121 227 252 83 122
                             130 50 34 67 23 10 50 244 1 0 0}

或者，正如[示例 P-7](#preface02.xhtml#working_with_com_objects_in_powershell)所示，你甚至可以使用 PowerShell 来编写传统 COM 对象的脚本。

### 示例 P-7. 在 PowerShell 中使用 COM 对象

```
PS > $firewall = New-Object -com HNetCfg.FwMgr
PS > $firewall.LocalPolicy.CurrentProfile

Type                                         : 1
FirewallEnabled                              : True
ExceptionsNotAllowed                         : False
NotificationsDisabled                        : False
UnicastResponsesToMulticastBroadcastDisabled : False
RemoteAdminSettings                          : System.__ComObject
IcmpSettings                                 : System.__ComObject
GloballyOpenPorts                            : {Media Center Extender Service,
                                               Remote Media Center Experience,
                                               Adam Test Instance, QWAVE...}
Services                                     : {File and Printer Sharing,
                                               UPnP Framework, Remote Desktop}
AuthorizedApplications                       : {Remote Assistance, Windows
                                               Messenger, Media Center, Trillian...}
```

# 通过提供程序导航命名空间

PowerShell 为系统操作提供的另一种途径是*提供程序*。PowerShell 提供程序让你能够使用处理文件系统的相同技术来导航和管理数据存储，如[示例 P-8](#preface02.xhtml#navigating_the_filesystem)所示。

### 示例 P-8. 导航文件系统

```
PS > Set-Location c:\
PS > Get-ChildItem

    Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----           5/8/2007   8:37 PM                Blurpark
d----           5/15/2016  4:32 PM                Chocolatey
d----            3/8/2020 12:45 PM                DXLab
d----           4/30/2020  7:00 AM                Go
d----            4/2/2016  3:05 PM                Intel
d-r--          12/15/2020  1:41 PM                Program Files
d-r--          11/28/2020  5:06 PM                Program Files (x86)
d----           5/12/2019  6:37 PM                Python27
d----           3/25/2018  1:11 PM                Strawberry
d----          12/16/2020  8:13 AM                temp
d-r--           8/11/2020  5:02 PM                Users
da---          12/16/2020 10:51 AM                Windows
```

这在注册表上同样适用，如[示例 P-9](#preface02.xhtml#navigating_the_registry)所示。

### 示例 P-9. 导航注册表

```
PS > Set-Location HKCU:\Software\Microsoft\Windows\
PS > Get-ChildItem

    Hive: HKEY_CURRENT_USER\Software\Microsoft\Windows

Name                           Property
----                           --------
CurrentVersion
DWM                            Composition                  : 1
                               ColorPrevalence              : 0
                               ColorizationColor            : 3290322719
                               EnableAeroPeek               : 1
                               AccentColor                  : 4280243998
                               EnableWindowColorization     : 1
Shell
TabletPC
Windows Error Reporting

PS > Set-Location CurrentVersion\Run
PS > Get-ItemProperty .

(...)
OneDrive            : "C:\Users\lee\AppData\Local\Microsoft\OneDrive\OneDrive.exe"
    /background
OpenDNS Updater     : "C:\Program Files (x86)\OpenDNS Updater\OpenDNSUpdater.exe"
    /autostart
Ditto               : C:\Program Files\Ditto\Ditto.exe
(...)
```

甚至在机器的证书存储上也适用，如[示例 P-10](#preface02.xhtml#navigating_the_certificate_store)所示。

### 示例 P-10. 导航证书存储

```
PS > Set-Location cert:\CurrentUser\Root
PS > Get-ChildItem

    Directory: Microsoft.PowerShell.Security\Certificate::CurrentUser\Root

Thumbprint                                Subject
----------                                -------
CDD4EEAE6000AC7F40C3802C171E30148030C072  CN=Microsoft Root Certificate...
BE36A4562FB2EE05DBB3D32323ADF445084ED656  CN=Thawte Timestamping CA, OU...
A43489159A520F0D93D032CCAF37E7FE20A8B419  CN=Microsoft Root Authority, ...
9FE47B4D05D46E8066BAB1D1BFC9E48F1DBE6B26  CN=PowerShell Local Certifica...
7F88CD7223F3C813818C994614A89C99FA3B5247  CN=Microsoft Authenticode(tm)...
245C97DF7514E7CF2DF8BE72AE957B9E04741E85  OU=Copyright (c) 1997 Microso...
(...)
```

# 更多内容

尽管这次导览令人振奋，但它仅触及了如何使用 PowerShell 提高生产力和系统管理技能的皮毛。有关 PowerShell 入门的更多信息，请参阅[第 1 章](#ch01.xhtml#the_windows_powershell_interactive_shell)。

# 第二部分 基础

- [第 1 章，*PowerShell 交互式 Shell*](#ch01.xhtml#the_windows_powershell_interactive_shell)
- [第 2 章，*管道*](#ch02.xhtml#pipelines)
- [第 3 章，*变量和对象*](#ch03.xhtml#variables_and_objects)
- [第 4 章，*循环和流程控制*](#ch04.xhtml#looping_and_flow_control)
- [第 5 章，*字符串和非结构化文本*](#ch05.xhtml#strings_and_unstructured_text)
- [第 6 章，*计算和数学*](#ch06.xhtml#calculations_and_math)
- [第 7 章，*列表、数组和哈希表*](#ch07.xhtml#lists_arrays_and_hashtables)
- [第 8 章，*实用任务*](#ch08.xhtml#utility_scripting)

# 第 1 章 PowerShell 交互式 Shell

## 1.0 简介

最重要的是，PowerShell 的设计将其作为高效且强大的交互式 Shell 的用途放在首位。即使是其脚本语言，也在这一努力中发挥着关键作用，因为它同样严重偏向交互式使用。

当人们首次启动 PowerShell 时，最令人惊讶的是它与长期作为 Windows 一部分的命令提示符的相似性。熟悉的工具继续运行。熟悉的命令继续运行。甚至熟悉的热键也相同。不过，支持这个熟悉 UI 的是一个强大的引擎，让你能够轻松完成曾经繁琐的管理和脚本任务。

本章从交互式 Shell 的角度介绍 PowerShell。

## 1.1 安装 PowerShell Core

### 问题

你想要在 Windows、Mac 或 Linux 系统上安装最新版本的 PowerShell。

### 解决方案

访问[微软网站](https://microsoft.com/PowerShell)以查找你想要安装的操作系统和平台的安装说明。对于最常见的情况：

#### Windows
