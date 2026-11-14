
```
PS > $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.1.0
PSEdition                      Core
GitCommitId                    7.1.0
OS                             Linux 4.19.128-microsoft-standard #1 SMP
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

## 讨论

`$PSVersionTable` 自动变量保存着 PowerShell 所有组件的版本信息：PowerShell 版本、其版本类型（`Core` 或 `Desktop`）、运行所在的操作系统（包括 Mac 和 Linux！），以及更多内容。

然而，这种技术并不完全足以编写在所有 PowerShell 版本中都能工作的脚本。如果你的脚本使用了较新版本 PowerShell 引入的语言特性（例如新关键字），该脚本将无法在早期版本中加载。

如果在多个 PowerShell 版本中运行脚本是一项强需求，最好的方法就是简单地编写一个适用于你需要支持的最旧 PowerShell 版本的脚本。它将自动在较新版本中工作。

# 16.12 测试管理权限

## 问题

你有一个脚本，如果不在管理会话中运行就会失败，并且希望在脚本启动时尽快检测到这种情况。

## 解决方案

在脚本的第一行，作为 `#requires` 语句的一部分，指定 `-RunAsAdministrator` 参数：

```
#requires -RunAsAdministrator

## 此处为一些管理任务
Get-Process -IncludeUserName
```

## 讨论

测试管理权限，虽然看似简单，但比预期的要棘手得多。

在 PowerShell 出现之前，许多批处理文件尝试直接向操作系统的安装目录写入文件。如果成功，你就是管理员，于是可以清理并继续。如果失败，则生成错误。但如果你使用 `C:\Windows` 作为路当有人将操作系统安装在不同驱动器时，你的脚本就会失败。如果你使用 `%SYSTEMROOT%` 环境变量，仍可能触发防病毒程序的怀疑。

作为对该技术的改进，一些批处理文件尝试解析 `NET LOCALGROUP Administrators` 命令的输出。遗憾的是，这在非英语机器上会失败，因为组名可能是 `Administratoren`。最重要的是，它只能检测用户是否属于 Administrators 组，而无法检测其当前 Shell 是否提升权限并能以管理员身份行事。

幸运的是，PowerShell 通过其 `#requires` 语句解决了这些问题。该语句允许你声明脚本运行所需的条件，PowerShell 会自动代表你验证这些条件。`#requires` 语句支持多种测试，你可以根据需要将它们组合到一条 `#requires` 语句中：

* `#requires -shellid <shellID>`
* `#requires -version <major.minor>`
* `#requires -psedition <edition>`
* `#requires -pssnapin <psSnapInName>`
* `#requires -version <major.minor>`
* `#requires -modules <ModuleSpecification>`
* `#requires -RunAsAdministrator`

如果你在脚本中指定 `#requires -RunAsAdministrator`，而用户尝试从非提升的 Shell 运行它，PowerShell 将显示以下消息：

```
./example.ps1: The script 'example.ps1' cannot be run because it contains a
"#requires" statement for running as Administrator. The current PowerShell
session is not running as Administrator. Start PowerShell by using the Run
as Administrator option, and then try running the script again.
```

## 另请参阅

[食谱 13.6，“编写区分区域性的脚本”](#ch13.xhtml#write_culture-aware_scripts)

# 第 17 章 扩展 PowerShell 的触达范围

## 17.0 简介

PowerShell 环境极其全面。它提供了大量的 Cmdlet 来帮助你管理系统，提供了优秀的脚本语言让你自动化这些任务，并提供了对你已知的所有实用工具和工具的直接访问。

然而，Cmdlet、脚本语言和预先存在的工具只是让 PowerShell 如此全面的部分原因。除了这些功能，PowerShell 还提供了对少数技术的访问，这些技术能极大地增强其能力：.NET Framework、WMI、COM 自动化对象、原生 Windows API 调用等。

PowerShell 不仅让你能访问这些技术，还以一致的方式让你访问它们。你用来与 PowerShell 对象的属性和方法交互的技术，与你用来与 .NET 对象的属性和方法交互的技术相同。反过来，这些又是你用来处理 WMI 和 COM 对象的相同技术。

使用这些技术和技术提供了另一个巨大的好处——所获得的知识可以轻松迁移到使用 C# 等 .NET 编程语言进行开发中。

## 17.1 使用 COM 脚本接口自动化程序

### 问题

你想通过程序的 COM 自动化接口来自动化该程序或系统任务。

### 解决方案

要实例化和使用 COM 对象，请使用 `New-Object` Cmdlet 的 `-ComObject` 参数。

```
$shell = New-Object -ComObject "Shell.Application"
$shell.Windows() | Format-Table LocationName,LocationUrl
```

### 讨论

与 WMI 一样，COM 自动化接口长期以来一直是脚本编写和系统管理的标准工具。当应用程序公开管理或自动化任务时，COM 对象是最常见的接口（仅次于自定义命令行工具）。

PowerShell 像暴露系统中大多数其他管理对象一样暴露 COM 对象。一旦你获得了对 COM 对象的访问权限，你就可以用与处理 PowerShell 中其他对象的方法和属性相同的方式来处理其属性和方法。

#### 注意

某些 COM 对象需要一种称为*多线程单元*（MTA）的特殊交互模式才能正确工作。有关如何与需要 MTA 交互的组件进行交互的信息，请参阅[食谱 13.12](#ch13.xhtml#interact_ui_frameworks)。

除了自动化任务，许多 COM 对象的存在完全是为了改善 VBScript 等语言的脚本编写体验。两个例子是处理文件和对数组进行排序。

在 PowerShell 中，这些 COM 对象大多已过时，因为 PowerShell 通常提供了比它们更好的替代方案！在许多情况下，PowerShell 的 Cmdlet、脚本语言或对 .NET Framework 的访问提供了与你习惯的 COM 对象相同或相似的功能。

有关使用 COM 对象的更多信息，请参阅[食谱 3.11](#ch03.xhtml#use_a_com_object)。有关最有用的 COM 对象列表，请参阅[附录 H](#app08.xhtml#selected_com_objects_and_their_uses)。

### 另请参阅

[食谱 3.11，“使用 COM 对象”](#ch03.xhtml#use_a_com_object)

[附录 H，《精选 COM 对象及其用途*](#app08.xhtml#selected_com_objects_and_their_uses)

## 17.2 程序：查询 SQL 数据源

对 SQL Server、Access 数据库甚至 Excel 电子表格等数据源执行临时查询和命令通常很有帮助。当你想将数据从一个系统移至另一个系统，或者想将数据引入 PowerShell 环境进行详细的交互式操作或处理时，这一点尤其正确。

虽然你可以直接在 PowerShell 中访问这些数据源（通过其对 .NET Framework 的支持），但每个数据源都需要独特且难以记忆的语法。[示例 17-1](#ch17.xhtml#invoke-sqlcommandps1) 使得处理这些基于 SQL 的数据源既一致又强大。

### 示例 17-1. Invoke-SqlCommand.ps1
