

# WMI 类

| 类 | 说明 |
|-------|-------------|
| `Win32_StartupCommand` | 表示用户登录到计算机系统时自动运行的命令。 |
| `Win32_SystemAccount` | 表示系统账户。操作系统以及在 Windows NT 下运行的服务使用系统账户。Windows NT 中有许多服务和进程需要内部登录的能力——例如，在 Windows NT 安装期间。系统账户就是为此目的而设计的。 |
| `Win32_SystemDriver` | 表示基础服务的系统驱动程序。 |
| `Win32_SystemEnclosure` | 表示与物理系统机箱关联的属性。 |
| `Win32_SystemSlot` | 表示物理连接点，包括端口、主板插槽和外设以及专有连接点。 |
| `Win32_TapeDrive` | 表示 Windows 计算机上的磁带驱动器。磁带驱动器主要通过只能按顺序访问这一事实来区分。 |
| `Win32_TemperatureProbe` | 表示温度传感器的属性（例如电子温度计）。 |
| `Win32_TimeZone` | 表示 Windows 系统的时区信息，其中包括夏令时转换所需的更改。 |
| `Win32_UserAccount` | 包含有关运行 Windows 操作系统的计算机上的用户账户的信息。由于 `Name` 和 `Domain` 都是关键属性，在大型网络上枚举 `Win32_UserAccount` 可能会对性能产生负面影响。调用 `GetObject` 或查询特定实例的影响较小。 |
| `Win32_VoltageProbe` | 表示电压传感器的属性（电子电压表）。 |
| `Win32_VolumeQuotaSetting` | 将磁盘配额设置与特定磁盘卷关联。Windows 2000/NT：此类不可用。 |
| `Win32_WMISetting` | 包含 WMI 服务的操作参数。此类只能有一个实例，该实例始终存在于每个 Windows 系统中且无法删除。无法创建其他实例。 |

# 附录 H. 选定的 COM 对象及其用途

作为扩展性和管理接口，许多应用程序通过 COM 对象公开有用的功能。尽管 PowerShell 直接处理许多此类任务，但许多 COM 对象仍然提供重要价值。

表 H-1. COM 标识符和说明

| 标识符 | 说明 |
|------------|-------------|
| `Access.Application` | 允许与 Microsoft Access 进行交互和自动化。 |
| `Agent.Control` | 允许控制 Microsoft Agent 3D 动画角色。 |
| `AutoItX3.Control` | (非默认) 通过 `AutoIt` 管理工具提供对 Windows 自动化的访问。 |
| `CEnroll.CEnroll` | 提供对证书注册服务的访问。 |
| `CertificateAuthority.Request` | 提供对证书颁发机构请求的访问。 |
| `COMAdmin.COMAdminCatalog` | 提供对 Windows COM+ 目录的访问和管理。 |
| `Excel.Application` | 允许与 Microsoft Excel 进行交互和自动化。 |
| `Excel.Sheet` | 允许与 Microsoft Excel 工作表进行交互。 |
| `HNetCfg.FwMgr` | 提供对 Windows 防火墙管理功能的访问。 |
| `HNetCfg.HNetShare` | 提供对 Windows 连接共享管理功能的访问。 |
| `HTMLFile` | 允许与新的 Internet Explorer 文档进行交互和创作。 |
| `InfoPath.Application` | 允许与 Microsoft InfoPath 进行交互和自动化。 |
| `InternetExplorer.Application` | 允许与 Internet Explorer 进行交互和自动化。 |
| `IXSSO.Query` | 允许与 Microsoft Index Server 进行交互。 |
| `IXSSO.Util` | 提供与 `IXSSO.Query` 对象一起使用的实用程序的访问。 |
| `LegitCheckControl.LegitCheck` | 提供对当前计算机上 Windows 正版优势状态信息的访问。 |
| `MakeCab.MakeCab` | 提供创建和管理 Cabinet (*.cab*) 文件的功能。 |
| `MAPI.Session` | 提供对消息应用程序编程接口 (MAPI) 会话的访问，如文件夹、消息和通讯簿。 |
| `Messenger.MessengerApp` | 允许与 Messenger 进行交互和自动化。 |
| `Microsoft.FeedsManager` | 允许与 Microsoft RSS 订阅平台进行交互。 |
| `Microsoft.ISAdm` | 提供对 Microsoft Index Server 的管理。 |
| `Microsoft.Update.AutoUpdate` | 提供对 Microsoft Update 自动更新计划的管理。 |
| `Microsoft.Update.Installer` | 允许从 Microsoft Update 安装更新。 |
| `Microsoft.Update.Searcher` | 提供对来自 Microsoft Update 的更新的搜索功能。 |
| `Microsoft.Update.Session` | 提供对 Microsoft Update 历史记录本地信息的访问。 |
| `Microsoft.Update.SystemInfo` | 提供对当前系统与 Microsoft Update 相关信息的访问。 |
| `MMC20.Application` | 允许与 Microsoft 管理控制台 (MMC) 进行交互和自动化。 |
| `MSScriptControl.ScriptControl` | 允许对 WSH 脚本进行评估和控制。 |
| `Msxml2.XSLTemplate` | 允许处理 XSL 转换。 |
| `Outlook.Application` | 允许通过 Microsoft Outlook 与您的电子邮件、日历、联系人、任务等进行交互和自动化。 |
| `OutlookExpress.MessageList` | 允许通过 Microsoft Outlook Express 与您的电子邮件进行交互和自动化。 |
| `PowerPoint.Application` | 允许与 Microsoft PowerPoint 进行交互和自动化。 |
| `Publisher.Application` | 允许与 Microsoft Publisher 进行交互和自动化。 |
| `RDS.DataSpace` | 提供对远程 DataSpace 业务对象代理的访问。 |
| `SAPI.SpVoice` | 提供对 Microsoft 语音 API 的访问。 |
| `Scripting.FileSystemObject` | 提供对计算机文件系统的访问。大多数功能可通过 PowerShell 或 PowerShell 对 .NET Framework 的支持更直接地获得。 |
| `Scripting.Signer` | 提供对 WSH 文件数字签名的管理。 |
| `Scriptlet.TypeLib` | 允许动态创建脚本类型库 (*.tlb*) 文件。 |
| `ScriptPW.Password` | 允许以掩码方式输入明文密码。如果可能，应避免使用此方法，而应首选带有 `-AsSecureString` 参数的 `Read-Host` cmdlet。 |
| `SharePoint.OpenDocuments` | 允许与 Microsoft SharePoint Services 进行交互。 |
| `Shell.Application` | 提供对 Windows 资源管理器 Shell 应用程序各方面的访问，如管理窗口、文件和文件夹以及当前会话。 |
| `Shell.LocalMachine` | 提供与 Windows Shell 相关的当前计算机信息的访问。 |
| `Shell.User` | 提供对当前用户的 Windows 会话和配置文件各方面的访问。 |
| `SQLDMO.SQLServer` | 提供对 Microsoft SQL Server 管理功能的访问。 |
| `Vim.Application` | (非默认) 允许与 VIM 编辑器进行交互和自动化。 |
| `WIA.CommonDialog` | 通过 Windows 图像获取功能提供对图像捕获的访问。 |
| `WMPlayer.OCX` | 允许与 Windows Media Player 进行交互和自动化。 |
| `Word.Application` | 允许与 Microsoft Word 进行交互和自动化。 |
| `Word.Document` | 允许与 Microsoft Word 文档进行交互。 |
| `WScript.Network` | 提供对网络 Windows 环境各方面的访问，如打印机和网络驱动器，以及计算机和域信息。 |
| `WScript.Shell` | 提供对 Windows Shell 各方面的访问，如应用程序、快捷方式、环境变量、注册表和操作环境。 |

