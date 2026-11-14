# 第 16 章 环境感知

## 16.0 简介

虽然您的许多脚本设计为独立工作，但您经常会发现，为脚本提供其执行环境的信息很有帮助：其名称、当前工作目录、环境变量、常见系统路径等。

PowerShell 提供了多种获取此信息的方式——从其 cmdlet 和内置变量到它从 .NET Framework 提供的功能。

## 16.1 查看和修改环境变量

### 问题

您想要与系统的环境变量进行交互。

### 解决方案

要与环境变量交互，请以几乎与访问常规 PowerShell 变量相同的方式访问它们。唯一的区别是您在美元符号 ($) 和变量名之间放置 `env:`：

```
PS > $env:Username
Lee
```

您也可以通过这种方式修改环境变量。例如，要临时将当前目录添加到路径中：

```
PS > Invoke-DemonstrationScript
Invoke-DemonstrationScript.ps1: The term 'Invoke-DemonstrationScript.ps1' is not
recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path
is correct and try again.

Suggestion [3,General]: The command Invoke-DemonstrationScript.ps1 was not found,
but does exist in the current location. PowerShell does not load commands from
the current location by default. If you trust this command, instead type:
".\Invoke-DemonstrationScript.ps1". See "get-help about_Command_Precedence"
for more details.

PS > $env:PATH = $env:PATH + ".;"
PS > Invoke-DemonstrationScript
The script ran!
```

### 讨论




# 16.1 查看和修改环境变量

在批处理文件中，环境变量是存储临时信息或在批处理文件之间传递信息的主要方式。PowerShell 变量和脚本参数是解决这些问题更有效的方法，但环境变量仍然提供了一种访问常见系统设置的有用途径，例如系统的路径、临时目录、域名、用户名等。

PowerShell 通过其 `environment provider`（环境提供程序）呈现环境变量：一个容器，让你像处理文件系统或注册表提供程序中的项目一样处理环境变量。默认情况下，PowerShell 定义了一个 `env:` 驱动器（很像 `c:` 或 `d:`），用于访问这些信息：

```
PS > dir env:

Name                          Value
----                          -----
Path                          c:\progra~1\ruby\bin;C:\WINDOWS\system32;C:\
TEMP                          C:\DOCUME~1\Lee\LOCALS~1\Temp
SESSIONNAME                   Console
PATHEXT                       .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;
(...)
```

既然它是一个常规的 PowerShell 驱动器，获取环境变量值的完整方法如下所示：

```
PS > Get-Content Env:\Username
Lee
```

不过，说到环境变量，由于 PowerShell 支持 `Get-Content` 和 `Set-Content` 变量语法，你几乎永远不需要使用那种语法，因为它可以将其简化为：

```
PS > $env:Username
Lee
```

此语法适用于所有驱动器，但最常用于访问环境变量。有关此语法的更多信息，请参阅 Recipe 16.3。

某些环境变量的值实际上来自两个位置的组合：机器范围的设置和当前用户设置。如果你想专门访问在机器或用户级别配置的环境变量值，请使用 `[Environment]::GetEnvironmentVariable()` 方法。例如，如果你已在路径中定义了一个 `tools` 目录，你可能会看到：

```
PS > [Environment]::GetEnvironmentVariable("Path", "User")
d:\lee\tools
```

要永久设置这些特定于机器或用户的环境变量，请使用 `[Environment]::SetEnvironmentVariable()` 方法：

```
[Environment]::SetEnvironmentVariable(<name>, <value>, <target>)
```

`target` 参数定义了该变量应存储在何处：`User` 表示当前用户，`Machine` 表示机器上的所有用户。例如，要永久将你的 `tools` 目录添加到你的路径中：

```
$pathElements = @([Environment]::GetEnvironmentVariable("Path", "User") -split ";")
$pathElements += "d:\tools"
$newPath = $pathElements -join ";"
[Environment]::SetEnvironmentVariable("Path", $newPath, "User")
```

有关修改系统路径的更多信息，请参阅 Recipe 16.2。

有关 `Get-Content` 和 `Set-Content` 变量语法的更多信息，请参阅 “Variables”。有关环境提供程序的更多信息，请输入 `Get-Help about_Environment_Provider`。

## 另请参阅

- Recipe 16.2, “Modify the User or System Path”
- Recipe 16.3, “Access Information About Your Command’s Invocation”
- “Variables”

# 16.2 修改用户或系统路径

## 问题

你想要更新你的（或系统的） `PATH` 变量。

## 解决方案

使用 `[Environment]::SetEnvironmentVariable()` 方法来设置 `PATH` 环境变量：

```
$scope = "User"
$pathElements = @([Environment]::GetEnvironmentVariable("Path", $scope)
                -split ";")
$pathElements += "d:\tools"
$newPath = $pathElements -join ";"
[Environment]::SetEnvironmentVariable("Path", $newPath, $scope)
```

## 讨论

在 Windows 中，`PATH` 环境变量描述了应用程序在查找可执行命令时应搜索的目录列表。按照惯例，路径中的项目由分号字符分隔。

正如 Recipe 16.1 中所述，环境变量有两个作用域：系统范围的变量和每用户变量。当你输入 `$env:PATH` 时看到的 `PATH` 变量是这两者结合的结果。

当你想要修改路径时，你需要决定是希望路径更改应用于系统上的所有用户，还是仅应用于你自己。如果你希望更改应用于整个系统，请在解决方案提供的示例中使用 `Machine` 作用域。如果你希望它仅应用于你的用户帐户，请使用 `User` 作用域。

如前所述，路径中的元素由分号字符分隔。为了更新路径，解决方案首先使用 `-split` 运算符创建一个由分号分隔的单个目录列表。它向路径添加一个新元素，然后使用 `-join` 运算符用分号字符重新组合这些元素。这有助于防止出现双分号、缺少分号，或不必担心分号是在路径元素之前还是之后。

有关使用环境变量的更多信息，请参阅 Recipe 16.1。

## 另请参阅

- Recipe 16.1, “View and Modify Environment Variables”

# 16.3 访问有关命令调用的信息

## 问题

你想了解用户是如何调用你的脚本、函数或脚本块的。

## 解决方案

要访问有关用户如何调用你的命令的信息，请使用 `$PSScriptRoot`、`$PSCommandPath` 和 `$myInvocation` 变量：

```
"Script's path: $PSCommandPath"
"Script's location: $PSScriptRoot"
"You invoked this script by typing: " + $myInvocation.Line
```

## 讨论

`$PSScriptRoot` 和 `$PSCommandPath` 变量提供了对命令最常需要的关于自身信息的快速访问：其完整路径和位置。

此外，`$myInvocation` 变量提供了大量关于当前脚本、函数或脚本块的信息——以及调用它的上下文：

`MyCommand`
: 关于命令（脚本、函数或脚本块）本身的信息。

`ScriptLineNumber`
: 调用此命令的脚本中的行号。

`ScriptName`
: 在函数或脚本块中，调用此命令的脚本的名称。

`Line`
: 调用此命令的脚本行（或命令行）中使用的逐字文本。

`InvocationName`
: 用户提供的用于调用此命令的名称。如果用户为该命令定义了别名，这将不同于 `MyCommand` 给出的信息。

`PipelineLength`
: 调用此命令的管道中的命令数量。

`PipelinePosition`
: 此命令在调用它的管道中的位置。

关于使用 `$myInvocation` 变量的一个重要点是，它会根据你调用它的命令类型而变化。如果你从函数访问此信息，它将提供特定于该函数的信息——而不是调用它的脚本的信息。由于脚本、函数和脚本块相当独特，`$myInvocation.MyCommand` 变量中的信息在不同命令类型之间会略有变化。

### 脚本

`Definition` 和 `Path`
: 当前正在运行的脚本的完整路径

`Name`
: 当前正在运行的脚本的名称

`CommandType`
: 始终为 `ExternalScript`

### 函数

`Definition` 和 `ScriptBlock`
: 当前正在运行的函数的源代码

`Options`
: 适用于当前正在运行的函数的选项（`None`、`ReadOnly`、`Constant`、`Private`、`AllScope`）

`Name`
: 当前正在运行的函数的名称

`CommandType`
: 始终为 `Function`




### 脚本块

**定义和 ScriptBlock**：当前正在运行的脚本块的源代码

**名称**：空

**CommandType**：始终为 `Script`

# 16.4 程序：探究 InvocationInfo 变量

当你尝试通过 `$myInvocation` 变量获取可用信息时，查看这些信息在脚本、函数和脚本块之间如何变化会很有帮助。若想深入了解 `$myInvocation` 变量提供的资源，请查看 示例 16-1 的输出。

### 示例 16-1. Get-InvocationInfo.ps1

```
##############################################################################
##
## Get-InvocationInfo
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

显示 $myInvocation 变量提供的信息

#>

param(
    ## 开关，用于不再递归调用自身
    [switch] $PreventExpansion
)

Set-StrictMode -Version 3

## 定义一个辅助函数，以便查看调用它和点源引用它时 $myInvocation 如何变化
function HelperFunction
{
    "    来自函数的 MyInvocation:"
    "-"*50
    $myInvocation

    "    来自函数的 Command:"
    "-"*50
    $myInvocation.MyCommand
}

## 定义一个脚本块，以便查看调用它和点源引用它时 $myInvocation 如何变化
$myScriptBlock = {
    "    来自脚本块的 MyInvocation:"
    "-"*50
    $myInvocation

    "    来自脚本块的 Command:"
    "-"*50
    $myInvocation.MyCommand
}

## 定义一个辅助别名
Set-Alias gii .\Get-InvocationInfo

## 说明 $myInvocation.Line 如何返回用户输入的整行内容。
"You invoked this script by typing: " + $myInvocation.Line

## 显示脚本返回的 $myInvocation 信息
"MyInvocation from script:"
"-"*50
$myInvocation

"Command from script:"
"-"*50
$myInvocation.MyCommand

## 如果使用 -PreventExpansion 开关调用，则不再继续
if($preventExpansion)
{
    return
}

## 显示函数返回的 $myInvocation 信息
"Calling HelperFunction"
"-"*50
HelperFunction

## 显示点源引用函数返回的 $myInvocation 信息
"Dot-Sourcing HelperFunction"
"-"*50
. HelperFunction

## 显示别名脚本返回的 $myInvocation 信息
"Calling aliased script"
"-"*50
gii -PreventExpansion

## 显示脚本块返回的 $myInvocation 信息
"Calling script block"
"-"*50
& $myScriptBlock

## 显示点源引用脚本块返回的 $myInvocation 信息
"Dot-Sourcing script block"
"-"*50
. $myScriptBlock

## 显示别名脚本返回的 $myInvocation 信息
"Calling aliased script"
"-"*50
gii -PreventExpansion
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

# 16.5 查找脚本的名称

## 问题

你想知道当前正在运行的脚本的路径和名称。

## 解决方案

要确定当前正在执行的脚本的完整路径和文件名，请使用 `$PSCommandPath` 变量。要确定用户实际输入的用于调用脚本的文本（例如，在“用法”消息中），请使用 `$myInvocation.InvocationName` 变量。

## 讨论

因为非常常用，PowerShell 通过 `$PSCommandPath` 变量提供对脚本完整路径的访问。如果你只想知道脚本的名称（而不是其完整路径），请使用 `Split-Path` cmdlet：

```
$scriptName = Split-Path -Leaf $PSCommandPath
```

有关使用 `$myInvocation` 变量的更多信息，请参阅 食谱 16.3。

## 另请参阅

食谱 16.3，“访问有关命令调用的信息”

# 16.6 查找脚本的位置

## 问题

你想知道当前正在运行的脚本的位置。

## 解决方案

要确定当前正在执行的脚本的位置，请使用 `$PSScriptRoot` 变量。例如，要从与脚本相同的位置加载数据文件：

```
$dataPath = Join-Path $PSScriptRoot data.clixml
```

或者要从与脚本相同的位置运行命令：

```
$helperUtility = Join-Path $PSScriptRoot helper.exe
& $helperUtility
```

## 讨论

因为非常常用，PowerShell 通过 `$PSScriptRoot` 变量提供对脚本位置的访问。

一旦我们知道脚本路径的根目录，`Join-Path` cmdlet 就能轻松地基于该路径构建新路径。

有关 `Join-Path` cmdlet 的更多信息，请参阅 食谱 16.9。

## 另请参阅

食谱 16.3，“访问有关命令调用的信息”

食谱 16.5，“查找脚本的名称”

食谱 16.9，“安全地从组件构建文件路径”

# 16.7 查找常见系统路径的位置

## 问题

你想知道常见系统路径和特殊文件夹的位置，例如“我的文档”和“Program Files”。

## 解决方案

要确定常见系统路径和特殊文件夹的位置，请使用 `[Environment]::GetFolderPath()` 方法：

```
PS > [Environment]::GetFolderPath("System")
C:\WINDOWS\system32
```

对于此方法不支持的路径（例如“所有用户开始菜单”），请使用 `WScript.Shell` COM 对象：

```
$shell = New-Object -Com WScript.Shell
$allStartMenu = $shell.SpecialFolders.Item("AllUsersStartMenu")
```

## 讨论

`[Environment]::GetFolderPath()` 方法让你能够访问 Windows 中使用的许多常见位置。要使用它，请提供位置的短名称（例如 `System` 或 `Personal`）。你可能不会记住所有这些短名称，因此查看所有这些值的一种方法是使用 `[Enum]::GetValues()` 方法：

```
PS > [Enum]::GetValues([Environment+SpecialFolder])
Desktop
Programs
Personal
Favorites
Startup
Recent
SendTo
StartMenu
MyMusic
DesktopDirectory
MyComputer
Templates
ApplicationData
LocalApplicationData
InternetCache
Cookies
History
CommonApplicationData
System
ProgramFiles
MyPictures
CommonProgramFiles
(...)
```

不过，由于这是所有枚举常量的一项非常常见的任务，如果 PowerShell 无法转换你的输入，它实际上会在错误消息中提供可能的值：

```
PS > [Environment]::GetFolderPath("aouaoue")
MethodException: Cannot convert argument "folder", with value: "aouaoue", for
"GetFolderPath" to type "System.Environment+SpecialFolder": "Cannot convert
value "aouaoue" to type "System.Environment+SpecialFolder". Error: "Unable to
match the identifier name aouaoue to a valid enumerator name. Specify one of
the following enumerator names and try again:

Desktop, Programs, MyDocuments, Personal, Favorites, Startup, Recent, SendTo,
StartMenu, MyMusic, MyVideos, DesktopDirectory, MyComputer, NetworkShortcuts,
Fonts, Templates, CommonStartMenu, CommonPrograms, CommonStartup,
CommonDesktopDirectory, ApplicationData, PrinterShortcuts, LocalApplicationData,
InternetCache, Cookies, History, CommonApplicationData, Windows, System,
ProgramFiles, MyPictures, UserProfile, SystemX86, ProgramFilesX86,
CommonProgramFiles, CommonProgramFilesX86, CommonTemplates, CommonDocuments,
CommonAdminTools, AdminTools, CommonMusic, CommonPictures, CommonVideos,
Resources, LocalizedResources, CommonOemLinks, CDBurning""
```



## 获取系统路径

虽然此方法提供对最常用的常见系统路径的访问，但它并不提供对所有路径的访问。对于 `[Environment]::GetFolderPath()` 方法不支持的路径，请使用 `WScript.Shell` COM 对象。`WScript.Shell` COM 对象支持以下路径：`AllUsersDesktop`、`AllUsersStartMenu`、`AllUsersPrograms`、`AllUsersStartup`、`Desktop`、`Favorites`、`Fonts`、`MyDocuments`、`NetHood`、`PrintHood`、`Programs`、`Recent`、`SendTo`、`StartMenu`、`Startup` 和 `Templates`。

如果能同时使用 `[Environment]::GetFolderPath()` 方法或 `WScript.Shell` COM 对象那将很不错，但正如我们所见，它们各自支持大量对方不支持的路径：

```powershell
PS > $shell = New-Object -Com WScript.Shell
PS > $shellPaths = $shell.SpecialFolders | Sort-Object
PS >
PS > $netFolders = [Enum]::GetValues([Environment+SpecialFolder])
PS > $netPaths = $netFolders |
    ForEach-Object { [Environment]::GetFolderPath($_) } | Sort-Object

PS > ## 查看仅 Shell 支持的路径
PS > Compare-Object $shellPaths $netPaths |
    Where-Object { $_.SideIndicator -eq "<=" }

InputObject                                                      SideIndicator
-----------                                                      -------------
C:\Users\lee\AppData\Roaming\Microsoft\Windows\Printer Shortcuts <=

PS > ## 查看仅 .NET 支持的路径
PS > Compare-Object $shellPaths $netPaths |
     Where-Object { $_.SideIndicator -eq "=>" }


InputObject                                                 SideIndicator
-----------                                                 -------------
C:\Program Files                                            =>
C:\Program Files (x86)                                      =>
C:\Program Files (x86)\Common Files                         =>
C:\Program Files\Common Files                               =>
C:\WINDOWS                                                  =>
C:\WINDOWS\resources                                        =>
C:\WINDOWS\system32                                         =>
C:\WINDOWS\SysWOW64                                         =>
D:\Lee                                                      =>
D:\Lee\My Music                                             =>
D:\Lee\My Pictures                                          =>
(...)
```

有关使用 .NET Framework 类的更多信息，请参阅 配方 3.8。

## 另请参阅

配方 3.8，“使用 .NET 对象”

# 16.8 获取当前位置

## 问题

你想要确定当前位置。

## 解决方案

要确定当前位置，请使用 `Get-Location` cmdlet：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > $currentLocation = (Get-Location).Path
PS > $currentLocation
C:\temp
```

此外，PowerShell 还通过 `$pwd` 自动变量提供对当前位置的访问：

```powershell
PS > $pwd

Path
----
C:\temp

PS > $currentLocation = $pwd.Path
PS > $currentLocation
C:\temp
```

## 讨论

有时影响使用 .NET Framework 的脚本的一个问题是，PowerShell 的“当前位置”概念并不总是与 `pwsh.exe` 进程的“当前目录”相同。例如：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > Get-Process | Export-CliXml processes.xml
PS > $reader = New-Object Xml.XmlTextReader processes.xml
PS > $reader.BaseURI
file:///C:/users/Lee/processes.xml
```

PowerShell 将这些概念分开，因为它支持多个执行管道。进程范围的当前目录会影响整个进程，因此如果在 Shell 中导航时更改了进程的当前目录，你将冒着破坏所有后台任务环境的风险。

当你在大多数 .NET 方法中使用文件名时，最佳实践是使用完全限定的路径名。`Resolve-Path` cmdlet 使这变得简单：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > Get-Process | Export-CliXml processes.xml
PS > $reader = New-Object Xml.XmlTextReader (Resolve-Path processes.xml)
PS > $reader.BaseURI
file:///C:/temp/processes.xml
```

如果你想访问一个尚不存在的路径，请结合 `Get-Location` cmdlet 使用 `Join-Path` cmdlet：

```powershell
PS > Join-Path (Get-Location) newfile.txt
C:\temp\newfile.txt
```

有关 `Join-Path` cmdlet 的更多信息，请参阅 配方 16.9。

## 另请参阅

配方 16.9

# 16.9 安全地从组件构建文件路径

## 问题

你想要从子路径的组合中构建一个新路径。

## 解决方案

要将路径元素连接在一起，请使用 `Join-Path` cmdlet：

```powershell
PS > Join-Path (Get-Location) newfile.txt
C:\temp\newfile.txt
```

## 讨论

创建新路径的通常方法是将每个组件的字符串组合起来，并在它们之间放置路径分隔符：

```powershell
PS > "$(Get-Location)\newfile.txt"
C:\temp\newfile.txt
```

不幸的是，这种方法存在一些问题：

*   如果 `Get-Location` 返回的目录末尾已经有斜杠怎么办？
*   如果路径包含正斜杠而不是反斜杠怎么办？
*   如果我们讨论的是注册表路径而不是文件系统路径怎么办？

幸运的是，`Join-Path` cmdlet 解决了这些问题及更多问题。

有关 `Join-Path` cmdlet 的更多信息，请输入 `Get-Help Join-Path`。

# 16.10 与 PowerShell 的全局环境交互

## 问题

你想要在 PowerShell 环境中存储信息，以便其他脚本可以访问它。

## 解决方案

要使变量在整个 PowerShell 会话中可用，请在存储信息时使用 `$GLOBAL:` 前缀：

```powershell
## 如果 Web 服务缓存不存在，则创建它
if(-not (Test-Path Variable:\Lee.Holmes.WebServiceCache))
{
    ${GLOBAL:Lee.Holmes.WebServiceCache} = @{}
}
```

## 讨论

关于在会话的全局环境中存储信息，主要指导原则是尽可能避免这样做。在全局作用域存储信息的脚本容易破坏其他脚本，也容易被其他脚本破坏。

这在批处理文件编程中是常见做法，但脚本参数和返回值通常提供更简洁的替代方案。

大多数使用全局变量的脚本都是为了在调用之间保持状态。PowerShell 通过使用 `modules` 以更简洁的方式处理此问题。有关此技术的信息，请参阅 配方 11.7。

如果你确实需要将变量写入全局作用域，请确保创建的名称足够唯一，以防止与其他脚本冲突，如解决方案所示。命名前缀的好选择包括脚本名称、作者姓名或公司名称。

有关在全局作用域（及其他作用域）设置变量的更多信息，请参阅 配方 3.6。

## 另请参阅

配方 3.6，“控制变量和其他项目的访问与作用域”

配方 11.7，“编写维护状态的命令”

# 16.11 确定 PowerShell 版本信息

## 问题

你想要获取当前 PowerShell 版本、.NET CLR（公共语言运行时）版本、兼容的 PowerShell 版本等信息。

## 解决方案

访问 `$PSVersionTable` 自动变量：



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

食谱 13.6，“编写区分区域性的脚本”
