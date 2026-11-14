# 第 15 章. 追踪与错误管理

## 15.0 简介

如果一切不按计划进行怎么办？这是任何系统中错误管理的核心问题，在编写 PowerShell 脚本时也起着重要作用。

虽然这是许多系统的主要关注点，但 PowerShell 对错误管理的支持提供了几项独特功能，旨在使您的工作更轻松。主要优势在于区分终止性错误和非终止性错误。

运行复杂脚本或场景时，最不希望发生的情况是，脚本无法打开其操作的成千上万个文件中的一个，导致整个世界崩塌。虽然系统应让您知晓故障，但脚本仍应继续处理下一个文件。这就是非终止性错误的一个示例。但如果脚本在运行备份时耗尽磁盘空间呢？那绝对应该是一个导致脚本退出的错误——也称为终止性错误。

鉴于这一有用的区别，PowerShell 提供了多项功能，让您管理脚本和程序生成的错误，也允许您自行生成错误。

## 15.1 确定上一条命令的状态

### 问题

您想要获取有关您执行的上一条命令的状态信息，例如它是否成功。

### 解决方案

使用 PowerShell 提供的两个变量之一来确定您执行的上一条命令的状态：`$lastExitCode` 变量和 `$?` 变量。

`$lastExitCode`
: 一个数字，表示上一个退出的脚本或应用程序的退出代码/错误级别

`$?`（读作“dollar hook”）
: 一个布尔值，表示上一条命令的成功或失败

### 讨论

`$lastExitCode` PowerShell 变量类似于 DOS 中的 `%errorlevel%` 变量。它保存最后退出的应用程序的退出代码。这使您能继续与使用退出代码作为主要通信机制的传统可执行文件（如 `ping`、`findstr` 和 `choice`）进行交互。PowerShell 还扩展了该变量的含义，使其包含脚本的退出代码，脚本可使用 `exit` 语句设置其状态。示例 15-1 演示了这种交互。

#### 示例 15-1. 与 $lastExitCode 和 $? 变量交互

```powershell
PS > ping localhost

Pinging MyComputer [127.0.0.1] with 32 bytes of data:

Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128
Reply from 127.0.0.1: bytes=32 time<1ms TTL=128

Ping statistics for 127.0.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milliseconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
PS > $?
True
PS > $lastExitCode
0
PS > ping missing-host
Ping request could not find host missing-host. Please check the name and try again.
PS > $?
False
PS > $lastExitCode
1
```

`$?` 变量以更通用的方式描述上一个应用程序的退出状态。在以下错误条件下，PowerShell 将此变量设置为 `False`：

- 应用程序以非零退出代码退出。
- Cmdlet 或脚本向其错误流写入任何内容。
- Cmdlet 或脚本遇到终止性错误或异常。

对于不指示错误条件的命令，PowerShell 将 `$?` 变量设置为 `True`。

如果您希望根据管道中前一条命令的成功与否将多个命令链接在一起，可以使用 PowerShell 的管道链运算符。有关这些运算符的更多信息，请参阅 食谱 2.1。

### 另请参阅

- 食谱 2.1，“根据命令的成功或错误链接命令”

## 15.2 查看命令生成的错误

### 问题

您想要查看当前会话中生成的错误。

### 解决方案

要访问迄今为止生成的错误列表，请使用 `$error` 变量，如 示例 15-2 所示。

#### 示例 15-2. 查看 $error 变量中包含的错误



```
PS > 1/0
RuntimeException: Attempted to divide by zero.

PS > $error[0] | Format-List -Force

ErrorRecord                 : Attempted to divide by zero.
StackTrace                  :    at System.Management.Automation.Expressio
                              (...)
WasThrownFromThrowStatement : False
Message                     : Attempted to divide by zero.
Data                        : {}
InnerException              : System.DivideByZeroException: Attempted to
                              divide by zero.
                                 at System.Management.Automation.ParserOps
                              .PolyDiv(ExecutionContext context, Token op
                              Token, Object lval, Object rval)
TargetSite                  : System.Collections.ObjectModel.Collection`1[
                              System.Management.Automation.PSObject] Invoke
                              (System.Collections.IEnumerable)
HelpLink                    :
Source                      : System.Management.Automation
```

## 讨论

PowerShell 的 `$error` 变量始终保存当前 Shell 会话中迄今产生的错误列表。此列表包含终止性错误和非终止性错误。

PowerShell 在遇到错误时会显示相当详细的信息：

```
PS > Stop-Process -name IDoNotExist
Stop-Process: Cannot find a process with the name "IDoNotExist".
Verify the process name and call the cmdlet again.
```

如果你想通过 `Format-Table` 或 `Format-List` cmdlet 以表格或列表形式查看错误，还必须指定 `-Force` 选项以覆盖这种自定义视图。

### 注意

有关错误的极其详细的信息，请参阅 食谱 15.4。

如果你想以更详细的方式显示错误，PowerShell 支持一个名为 `NormalView` 的额外视图（即早期 PowerShell 版本使用的视图），可通过 `$errorView` 首选项变量进行设置：

```
PS > Get-ChildItem IDoNotExist
Get-ChildItem: Cannot find path 'C:\IDoNotExist' because it does not exist.

PS > $errorView = "NormalView"
PS > Get-ChildItem IDoNotExist
Get-ChildItem : Cannot find path 'C:\IDoNotExist' because it does not exist.
At line:1 char:14
+ Get-ChildItem <<<<  IDoNotExist
    + CategoryInfo          : ObjectNotFound: (C:\IDoNotExist:String)
  [Get-ChildItem], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.
   GetChildItemCommand
```

在这个视图中，这些错误的一个独特特性是它们受益于一个多元化且国际化的 PowerShell 用户社区。请注意 `FullyQualifiedErrorId` 行：一个错误标识符，无论错误发生在哪种语言环境中，它都保持不变。当用户将此错误消息粘贴到互联网论坛、新闻组或博客时，这个完全限定的错误 ID 永远不会改变。英语用户随后可以从非英语 PowerShell 用户发布的错误中受益，反之亦然。

要清除错误列表，请在 `$error` 列表上调用 `Clear()` 方法：

```
PS > $error.Count
2
PS > $error.Clear()
PS > $error.Count
0
```

有关 PowerShell 首选项变量的更多信息，请输入 `Get-Help about_preference_variables`。如果你只想确定上一条命令的成功或失败，请参阅 食谱 15.1。

## 另请参阅

食谱 15.1，“确定上一条命令的状态”

食谱 15.4，“程序：解析错误”

# 15.3 管理命令的错误输出

## 问题

你想显示来自命令的错误的详细信息。

## 解决方案

要列出此会话中发生的所有错误（最多 `$MaximumErrorCount` 个），请访问 `$error` 数组：

```
$error
```

要列出此会话中发生的最后一个错误，请访问 `$error` 数组的第一个元素：

```
$error[0]
```

要获取此会话中发生的最后一个错误的详细信息，请使用 `Get-Error` cmdlet：

```
Get-Error
```

要列出错误的详细信息，请将错误通过管道传递给带有 `-Force` 参数的 `Format-List` cmdlet：

```
$currentError = $error[0]
$currentError | Format-List -Force
```

要列出导致错误的命令的详细信息，请访问其 `InvocationInfo` 属性：

```
$currentError = $error[0]
$currentError.InvocationInfo
```

要以更简洁的基于类别的视图显示错误，请将 `$errorView` 变量更改为 `"CategoryView"`：

```
$errorView = "CategoryView"
```

要清除 PowerShell 迄今收集的错误列表，请在 `$error` 变量上调用 `Clear()` 方法：

```
$error.Clear()
```

## 讨论

错误是管理世界生活中的简单事实。不过，并非所有错误都意味着灾难。因此，PowerShell 将错误分为两类：*非终止性* 和 *终止性*。

非终止性错误是最常见的错误类型。它们表明 cmdlet、脚本、函数或管道遇到了能够恢复或能够继续执行的错误。非终止性错误的一个例子来自 `Copy-Item` cmdlet。如果它无法将文件从一个位置复制到另一个位置，它仍可以继续处理指定的其余文件。

另一方面，终止性错误表明操作中存在更深层、更根本的错误。这同样可以来自 `Copy-Item` cmdlet，当你指定无效的命令行参数时。

深入挖掘错误（及其嵌套错误）可能很繁琐，因此对于自动执行此任务的脚本，请参阅 食谱 15.4。

## 另请参阅

食谱 15.4

# 15.4 程序：解析错误

分析错误通常需要几个不同的调查步骤：显示错误、探索其上下文以及分析其内部异常。在大多数场景中，使用 `Get-Error` cmdlet 查看你遇到的错误的扩展信息。

然而，有时你需要调查导致这些错误的错误（内部异常）、导致那些错误的错误，以及更多。示例 15-3 为你自动化了这些琐碎任务。

### 示例 15-3. Resolve-Error.ps1

```
#############################################################################
##
## Resolve-Error
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

显示错误及其上下文的详细信息。

#>

param(
    ## 要解析的错误
    $ErrorRecord = ($error[0])
)

Set-StrictMode -Off

""
"如果这是你编写的脚本中的错误，请使用 Set-PsBreakpoint cmdlet"
"来诊断它。"
""

'错误详细信息 ($error[0] | Format-List * -Force)'
"-"*80
$errorRecord | Format-List * -Force

'导致此错误的命令的信息 ' +
    '($error[0].InvocationInfo | Format-List *)'
"-"*80
$errorRecord.InvocationInfo | Format-List *

'错误目标的信息 ' +
    '($error[0].TargetObject | Format-List *)'
"-"*80
$errorRecord.TargetObject | Format-List *

'异常详细信息 ($error[0].Exception | Format-List * -Force)'
"-"*80

$exception = $errorRecord.Exception

for ($i = 0; $exception; $i++, ($exception = $exception.InnerException))
{
    "$i" * 80
    $exception | Format-List * -Force
}
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

# 15.5 配置调试、详细和进度输出

## 问题



# 15.5 管理调试、详细和进度输出

## 问题

您想要管理由 cmdlet 和脚本生成的详细调试、详细和进度输出。

## 解决方案

要为生成调试输出的脚本和 cmdlet 启用调试输出：

```
$debugPreference = "Continue"
Start-DebugCommand
```

要为检查 `-Verbose` 参数的 cmdlet 启用详细模式：

```
Copy-Item c:\temp\*.txt c:\temp\backup\ -Verbose
```

要禁用从脚本或 cmdlet 生成的进度输出：

```
$progressPreference = "SilentlyContinue"
Get-Progress.ps1
```

## 讨论

除了错误输出（如 Recipe 15.3 所述）之外，许多脚本和 cmdlet 还会生成其他几种类型的输出。这些包括以下类型：

**调试输出**

: 帮助您诊断可能出现的问题，并可提供对命令内部工作原理的洞察。您可以使用 `Write-Debug` cmdlet 在脚本中生成此类输出，或使用 `WriteDebug()` 方法在 cmdlet 中生成此类输出。默认情况下，PowerShell 以黄色显示此输出，但您可以通过 `$host.PrivateData.Debug*` 颜色配置变量对其进行自定义。

**详细输出**

: 帮助您比默认级别更精细地监控命令的操作。您可以使用 `Write-Verbose` cmdlet 在脚本中生成此类输出，或使用 `WriteVerbose()` 方法在 cmdlet 中生成此类输出。默认情况下，PowerShell 以黄色显示此输出，但您可以通过 `$host.PrivateData.Verbose*` 颜色配置变量对其进行自定义。

**进度输出**

: 帮助您监控长时间运行命令的状态。您可以使用 `Write-Progress` cmdlet 在脚本中生成此类输出，或使用 `WriteProgress()` 方法在 cmdlet 中生成此类输出。默认情况下，PowerShell 以黄色显示此输出，但您可以通过 `$host.PrivateData.Progress*` 颜色配置变量自定义颜色。

某些 cmdlet 仅当您分别指定 `-Verbose` 和 `-Debug` 参数时才会生成详细和调试输出。

### 注意

正如 PowerShell 的*参数消歧义*支持允许您仅输入足以将参数与同一 cmdlet 的其他参数区分开的字符一样，当参数值限于一组特定值时，PowerShell 支持*枚举消歧义*。当您交互式运行已知会生成错误的命令时，这或许最有用：

```
PS > Get-ChildItem c:\windows -Recurse -ErrorAction Ignore
PS > dir c:\windows -rec -ea ig
```

要配置脚本或 cmdlet 的调试、详细和进度输出，请修改 `$debugPreference`、`$verbosePreference` 和 `$progressPreference` Shell 变量。这些变量可以接受以下值：

`Ignore`

: 不显示此输出，也不将其添加到 `$error` 集合中。仅当作为命令的 `ErrorAction` 参数提供时受支持。

`SilentlyContinue`

: 不显示此输出，但将其添加到 `$error` 集合中。

`Stop`

: 将此输出视为错误。

`Continue`

: 显示此输出。

`Inquire`

: 为此输出显示继续提示。

## 另请参阅

Recipe 15.3, “Manage the Error Output of Commands”

# 15.6 处理警告、错误和终止错误

## 问题

您想要处理您调用的脚本或其他工具生成的警告、错误和终止错误。

## 解决方案

要控制脚本如何响应警告消息，请设置 `$warningPreference` 变量。在此示例中，将其忽略：

```
$warningPreference = "SilentlyContinue"
```

要控制脚本如何响应非终止错误，请设置 `$errorActionPreference` 变量。在此示例中，将其忽略：

```
$errorActionPreference = "SilentlyContinue"
```

要控制脚本如何响应终止错误，您可以使用 `try`/`catch`/`finally` 语句或 `trap` 语句。在此示例中，我们输出一条消息并继续执行脚本：

```
try
{
    1 / $null
}
catch [DivideByZeroException]
{
    "Don't divide by zero: $_"
}
finally
{
    "Script that will be executed even if errors occur in the try statement"
}
```

如果您希望其错误处理应用于整个作用域，请使用 `trap` 语句：

```
trap [DivideByZeroException] { "Don't divide by zero!"; continue }
1 / $null
```

## 讨论

PowerShell 定义了若干首选项变量，可帮助您控制脚本对警告、错误和终止错误的反应。作为这些错误管理技术的示例，请考虑以下脚本。

```
##############################################################################
##
## Get-WarningsAndErrors
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

演示 Write-Warning、Write-Error 和 throw 语句的功能

#>

Set-StrictMode -Version 3

Write-Warning "Warning: About to generate an error"
Write-Error "Error: You are running this script"
throw "Could not complete operation."
```

有关运行脚本的更多信息，请参阅 Recipe 1.2。

现在您可以看到脚本如何管理那些单独的错误类型：

```
PS > $warningPreference = "Continue"
PS > Get-WarningsAndErrors
WARNING: Warning: About to generate an error
Exception: C:\scripts\Get-WarningsAndErrors.ps1:23
Line |
  23 |  throw "Could not complete operation."
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Could not complete operation.
```

修改警告首选项后，原始警告消息被抑制。当您预期会发生某种错误时，`SilentlyContinue` 值很有用。

```
PS > $warningPreference = "SilentlyContinue"
PS > Get-WarningsAndErrors
Write-Error: Error: You are running this script
Exception: C:\scripts\Get-WarningsAndErrors.ps1:23
Line |
  23 |  throw "Could not complete operation."
     |  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Could not complete operation.
```

当您修改错误首选项时，错误和异常也会被抑制：

```
PS > $errorActionPreference = "SilentlyContinue"
PS > Get-WarningsAndErrors
PS >
```

除了 `$errorActionPreference` 变量之外，所有 cmdlet 还允许您在单个调用期间指定您的首选项。使用 `SilentlyContinue` 错误操作首选项时，PowerShell 不会显示错误或对其做出反应。但是，它仍会将错误添加到 `$error` 集合中以供进一步处理。如果您想连同那一起抑制，请使用 `Ignore` 错误操作首选项。

```
PS > $errorActionPreference = "Continue"
PS > Get-ChildItem IDoNotExist
Get-ChildItem : Cannot find path '...\IDoNotExist' because it does not exist.
At line:1 char:14
+ Get-ChildItem  <<<< IDoNotExist
PS > Get-ChildItem IDoNotExist -ErrorAction SilentlyContinue
PS >
```

如果您将错误首选项重置为 `Continue`，则可以看到 `try`/`catch`/`finally` 语句的影响。来自 `Write-Error` 调用的消息会通过，但异常不会：

```
PS > $errorActionPreference = "Continue"
PS > try { Get-WarningsAndErrors } catch { "Caught an error" }
WARNING: Warning: About to generate an error
Get-WarningsAndErrors: Error: You are running this script
Caught an error
```

`try`/`catch`/`finally` 语句的作用类似于其他编程语言中的类似语句。首先，它执行其脚本块内的代码。如果遇到终止错误，它将执行 `catch` 脚本块内的代码。无论发生什么，它都会执行 `finally` 语句中的代码——这对于清理或错误恢复代码而言是一个特别有用的功能。

一种类似的技术是 `trap` 语句：



```
PS > $errorActionPreference = "Continue"
PS > trap { "Caught an error"; continue }; Get-WarningsAndErrors
WARNING: Warning: About to generate an error
Get-WarningsAndErrors: Error: You are running this script
Caught an error
```

在 `catch` 块或 `trap` 语句中，`$_`（或 `$PSItem`）变量代表正在处理的当前异常或错误。

与 `try` 语句不同，`trap` 语句处理定义它的作用域中发生的任何终止错误。有关作用域的更多信息，请参阅 食谱 3.6。

### 注意

处理错误后，您还可以通过输入 `$error.RemoveAt(0)` 将其从系统的错误集合中移除。

有关 PowerShell 自动变量的更多信息，请输入 `Get-Help about_automatic_variables`。有关 PowerShell 中错误管理的更多信息，请参阅 “管理错误”。有关这些首选项变量有效设置的更详细信息，请参阅 附录 A。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 3.6，“控制变量和其他项目的访问权限和作用域”

“管理错误”

附录 A，*PowerShell 语言和环境*

# 15.7 输出警告、错误和终止错误

## 问题

您希望脚本通知其调用者有关警告、错误或终止错误的信息。

## 解决方案

要输出警告和错误，请分别使用 `Write-Warning` 和 `Write-Error` cmdlet。使用 `throw` 语句生成终止错误。

## 讨论

当您需要通知脚本调用者有关异常情况时，`Write-Warning`、`Write-Error` 和 `throw` 语句是实现此目的的方法。如果您的用户应将该消息视为警告，请使用 `Write-Warning` cmdlet。如果您的脚本遇到错误（但可以合理地继续执行），请使用 `Write-Error` cmdlet。如果错误是致命的，且您的脚本根本无法继续，请使用 `throw` 语句。

有关生成这些错误以及处理其他脚本抛出的错误的更多信息，请参阅 食谱 15.6。有关 PowerShell 中错误管理的更多信息，请参阅 “管理错误”。有关运行脚本的更多信息，请参阅 食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 15.6，“处理警告、错误和终止错误”

“管理错误”

# 15.8 分析脚本的性能配置文件

## 问题

您有一个交互式或耗时的脚本，想要查看哪些部分拖慢了它的速度。

## 解决方案

从 PowerShell 库安装 `PSProfiler` 模块，并使用 `Measure-Script` 命令查看脚本中消耗时间最多的行。在本示例中，我们正在调查 `burn-console-original.ps1`，这是一个有趣的小脚本，可在 PowerShell 控制台中显示类似演示场景的火焰效果：

```
PS > Install-Module -Name PSProfiler -Scope CurrentUser
PS > $profileData = Measure-Script (Get-Command burn-console-original.ps1).Path
PS > $profileData | Sort-Object ExecutionTime -Desc | Select -First 18

 Count  Line       Time Taken Statement
 -----  ----       ---------- ---------
     1   252    00:12.7965373 . main
     1    57    00:12.6452424 $totalTime = Measure-Command {
    26    60    00:11.1477132 updateBuffer
    26    61    00:01.4885363 updateScreen
111020   146    00:00.4423771 $nextScreen[$row, $column] = `
104312   122    00:00.3652381 $tempWorkingBuffer[$baseOffset - $windowWidth] = `
104312   110    00:00.3542900 $colour += $screenBuffer[$baseOffset + $windowWidth]
104312   103    00:00.3332991 $baseOffset = ($windowWidth * $row) + $column
104312   108    00:00.3325509 $colour += $screenBuffer[$baseOffset - 1]
104312   107    00:00.3277796 $colour = $screenBuffer[$baseOffset]
104312   109    00:00.3223024 $colour += $screenBuffer[$baseOffset + 1]
104312   111    00:00.2604536 $colour /= 4.0
102983   116    00:00.2573369 if($colour -le 70) { $colour -= 3 }
101458   117    00:00.2492673 if($colour -lt 20) { $colour -= 1 }
100910   118    00:00.2466116 if($colour -lt 0) { $colour = 0 }
    26   152    00:00.0921347 $host.UI.RawUI.SetBufferContents($origin, $nextScreen)
     1    46    00:00.0609062 clear-host
     1    49    00:00.0515547 generatePalette
```

## 讨论

当您编写的脚本导致用户或其他系统等待结果时，您有时会觉得脚本的性能可以得到改进。

解决性能问题的第一条规则是测量问题。除非您能用硬性的性能数据指导优化工作，否则您几乎肯定是在错误的地方浪费精力。随意的微小性能改进会迅速将您的代码变成难以阅读的混乱局面，通常却没有明显的性能提升！低级优化有其用武之地，但应始终由支持它的硬性数据指导。

获取硬性性能数据的方法是使用分析器。PowerShell 不附带脚本分析器，但它附带了让社区编写分析器的功能！`PSProfiler` 模块是由 PowerShell 团队的几位成员合作编写的，是您工具箱中的绝佳补充。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 1.29，“查找并安装其他 PowerShell 脚本和模块”
