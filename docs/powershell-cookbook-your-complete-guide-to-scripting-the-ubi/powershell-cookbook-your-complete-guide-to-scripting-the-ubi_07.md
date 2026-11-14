# 第 3 章：变量和对象

## 3.0 简介

正如 第 2 章 所提到的，PowerShell 通过将信息保持在其原生形式：*对象*，使生活变得轻松得多。用户在传统 Shell 中花费大部分精力，只是试图复活 Shell 将其从原生形式转换为纯文本后的信息。虽然已经出现了缓解纯文本处理负担的工具，但这项工作仍然比实际需要的要困难得多。

由于 PowerShell 构建在 Microsoft 的 .NET Framework 之上，原生信息以 .NET *对象* 的形式出现——这些是与该信息紧密相关的信息和功能的包。

假设你想获取系统上正在运行的进程列表。在其他 Shell 中，你的命令（如 `tlist.exe` 或 `/bin/ps`）会生成一份系统上运行进程的纯文本报告。要处理该输出，你需要将其通过一系列文本处理工具传递——前提是你有幸拥有这些工具。

PowerShell 的 `Get-Process` cmdlet 生成系统上正在运行的进程列表。然而，与其他 Shell 不同，这些是直接来自 .NET Framework 的全保真 `System.Diagnostics.Process` 对象。.NET Framework 文档将它们描述为“提供对本地和远程进程的访问，并允许你启动和停止本地系统进程”的对象。有了这些对象，PowerShell 使你能轻松访问对象的属性（如进程名称或内存使用量）以及对象上的功能（如停止它们、启动它们或等待它们退出）。

## 3.1 以列表形式显示项目的属性

### 问题




# 3.1 以列表形式显示项目的属性

## 问题

你有一个项目（例如，错误记录、目录项或 `.NET` 对象），并且想要以列表格式显示关于该对象的详细信息。

## 解决方案

要显示关于某个项目的详细信息，请将该项目传递给 `Format-List` cmdlet。例如，要以列表格式显示错误，请键入以下命令：

```
$currentError = $error[0]
$currentError | Format-List -Force
```

## 讨论

许多命令默认以表格格式显示其输出的摘要视图，例如 `Get-Process` cmdlet：

```
PS > Get-Process PowerShell

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    920      10    43808      48424   183     4.69   1928 powershell
    149       6    18228       8660   146     0.48   1940 powershell
    431      11    33308      19072   172            2816 powershell
```

在大多数情况下，输出实际上包含更多信息。你可以使用 `Format-List` cmdlet 来查看它：

```
PS > Get-Process pwsh | Format-List *

Name                       : pwsh
Id                         : 14820
PriorityClass              : Normal
FileVersion                : 7.1.0.0
HandleCount                : 940
TotalProcessorTime         : 00:00:25.7500000
VM                         : 2204249919488
WS                         : 81596416
Path                       : C:\Program Files\WindowsApps\...\pwsh.exe
CommandLine                : C:\Users\lee\AppData\Microsoft\WindowsApps\...\pwsh.exe
Parent                     : System.Diagnostics.Process (WindowsTerminal)
Company                    : Microsoft Corporation
CPU                        : 25.765625
ProductVersion             : 7.1.0 SHA: cbb7d40f684fdeb56cc276340b3b7435ac649d8f
Description                : pwsh
Product                    : PowerShell(...)
```

`Format-List` cmdlet 是四个 PowerShell 格式化 cmdlet 之一。这些 cmdlet 分别是 `Format-Table`、`Format-List`、`Format-Wide` 和 `Format-Custom`。`Format-List` cmdlet 接受输入并以列表形式显示关于该输入的信息。

默认情况下，PowerShell 从 PowerShell 安装目录中的 `*.format.ps1xml` 文件获取要显示的属性列表。在许多情况下，你只能得到一小部分属性：

```
PS > Get-Process pwsh | Format-List

Id      : 2816
Handles : 431
CPU     :
Name    : pwsh

Id      : 5244
Handles : 665
CPU     : 10.296875
Name    : pwsh
```

要显示项目的所有属性，请键入 `Format-List *`。如果你键入 `Format-List *` 但仍未获得项目属性列表，则说明该项目在 `*.format.ps1xml` 文件中已定义，但未为列表命令定义任何显示内容。在这种情况下，请键入 `Format-List -Force`。

PowerShell 格式化 cmdlet 的一个常见绊脚石是将它们放在脚本或管道的中间：

```
PS > Get-Process PowerShell | Format-List | Sort-Object Name
out-lineoutput : The object of type "Microsoft.PowerShell.Commands.Internal.Format.FormatEntryData" is not valid or not in the correct sequence. This is likely caused by a user-specified "format-*" command that is conflicting with the default formatting.
```

在内部，PowerShell 的格式化命令会生成一种新类型的对象：`Microsoft.PowerShell.Commands.Internal.Format.*`。当这些对象到达管道末尾时，PowerShell 会自动将它们发送给输出 cmdlet：默认为 `Out-Default`。这些 `Out-*` cmdlet 假设对象按特定顺序到达，因此对格式化命令的输出执行任何操作都会导致输出系统出错。

要解决此问题，请尝试避免在脚本或管道中间调用格式化 cmdlet。当你这样做时，脚本的输出将不再适合作为 PowerShell 代名词式的基于对象的操作。

如果你想直接使用格式化后的输出，请按照 配方 1.24 中的说明，通过 `Out-String` cmdlet 发送输出。

有关 `Format-List` cmdlet 的详细信息，请键入 `Get-Help Format-List`。

# 3.2 以表格形式显示项目的属性

## 问题

你有一组项目（例如，错误记录、目录项或 `.NET` 对象），并且想要以表格格式显示关于它们的摘要信息。

## 解决方案

要显示关于一组项目的摘要信息，请将这些项目传递给 `Format-Table` cmdlet。这是 PowerShell 中项目集合的默认格式化类型，并提供了几个有用的功能。

要使用 PowerShell 的默认格式化，请将 cmdlet（例如 `Get-Process` cmdlet）的输出通过管道传递给 `Format-Table` cmdlet：

```
Get-Process | Format-Table
```

要在表格格式化中显示特定属性（例如 `Name` 和 `WorkingSet`），请将这些属性名称作为参数提供给 `Format-Table` cmdlet：

```
Get-Process | Format-Table Name,WS
```

要指示 PowerShell 以最易读的方式格式化表格，请将 `-Auto` 标志提供给 `Format-Table` cmdlet。PowerShell 为进程定义 `WS` 作为 `WorkingSet` 属性的别名：

```
Get-Process | Format-Table Name,WS -Auto
```

要定义自定义列定义（例如以兆字节为单位的进程 `WorkingSet`），请向 `Format-Table` cmdlet 提供自定义格式化表达式：

```
$fields = "Name",@{ Label = "WS (MB)"; Expression = {$_.WS / 1mb}; Align = "Right"}
Get-Process | Format-Table $fields -Auto
```

## 讨论

`Format-Table` cmdlet 是四个 PowerShell 格式化 cmdlet 之一。这些 cmdlet 分别是 `Format-Table`、`Format-List`、`Format-Wide` 和 `Format-Custom`。`Format-Table` cmdlet 接受输入并以表格形式显示关于该输入的信息。默认情况下，PowerShell 从 PowerShell 安装目录中的 `*.format.ps1xml` 文件获取要显示的属性列表。如果你键入 `Format-Table *`，可以显示项目的所有属性，尽管这很少是有用的视图。

`Format-Table` 的 `-Auto` 参数是以最易读的方式自动格式化表格的有用方法。但这也是有代价的。为了找出最佳的表格布局，PowerShell 需要检查传入项目集合中的每个项目。对于小型项目集合，这没有太大区别，但对于大型集合（例如递归目录列表），区别就很明显了。如果没有 `-Auto` 参数，`Format-Table` cmdlet 一收到项目就可以显示它们。而带有 `-Auto` 标志时，该 cmdlet 只有在接收到所有输入后才会显示结果。

`Format-Table` cmdlet 或许最有趣的功能由最后一个示例说明：即定义完全自定义表格列的能力。你定义自定义表格列的方式类似于定义自定义列表列的方式。你不指定项目的现有属性，而是提供一个哈希表。该哈希表包含最多三个键：列标签、格式化表达式和对齐方式。`Format-Table` cmdlet 将标签显示为列标题，并使用你的表达式生成该列的数据。标签必须是字符串，表达式必须是脚本块，对齐方式必须是 `"Left"`、`"Center"` 或 `"Right"`。在表达式脚本块中，`$_`（或 `$PSItem`）变量代表正在被格式化的当前项目。

## 注意

`Select-Object` cmdlet 支持类似的哈希表来添加计算属性，但使用 `Name`（而不是 `Label`）作为键来标识属性。在意识到这有多令人困惑后，PowerShell 团队更新了这两个 cmdlet，使它们都接受 `Name` 和 `Label`。

最后一个示例中显示的表达式获取当前项目的工作集，并将其除以 1 兆字节（1 MB）。





## 3.2 管道中的格式化 Cmdlet

在 PowerShell 的格式化 cmdlet 中，一个常见的绊脚石源于将它们置于脚本或管道的中间：

```
PS > Get-Process | Format-Table | Sort-Object Name
out-lineoutput : 类型为 "Microsoft.PowerShell.Commands.Internal.
Format.FormatEntryData" 的对象无效或序列不正确。这很可能是由用户指定的 "format-*"
命令与默认格式化发生冲突导致的。
```

在内部，PowerShell 的格式化命令会生成一种新类型的对象：`Microsoft.PowerShell.Commands.Internal.Format.*`。当这些对象到达管道末尾时，PowerShell 会自动将它们发送给一个输出 cmdlet：默认为 `Out-Default`。这些 `Out-*` cmdlet 假设对象按特定顺序到达，因此对格式化命令的输出执行任何操作都会导致输出系统报错。

要解决此问题，请尝试避免在脚本或管道中间调用格式化 cmdlet。如果这样做，脚本的输出将不再适合进行对象式操作，而这正是 PowerShell 的代名词。

如果您想直接使用格式化输请通过 `Out-String` cmdlet 发送输出，如 示例 1.24 所述。

有关 `Format-Table` cmdlet 的详细信息，请输入 `Get-Help Format-Table`。有关哈希表的详细信息，请参阅 示例 7.13。有关脚本块的详细信息，请参阅 示例 11.4。

### 另请参阅

示例 1.24，“程序：在格式化输出中搜索模式”

示例 7.13，“创建哈希表或关联数组”

示例 11.4，“编写脚本块”

## 3.3 在变量中存储信息

### 问题

您想要存储管道或命令的输出，以便稍后使用或更详细地处理它。

### 解决方案

要存储输出以供稍后使用，请将命令的输出存储在变量中。您稍后可以访问此信息，甚至可以将其传递到管道中，就像它是原始命令的输出一样：

```
PS > $result = 2 + 2
PS > $result
4

PS > $output = ipconfig
PS > $output | Select-String "Default Gateway" | Select -First 1

   Default Gateway . . . . . . . . . : 192.168.11.1

PS > $processes = Get-Process
PS > $processes.Count
85
PS > $processes | Where-Object { $_.ID -eq 0 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)    Id ProcessName
-------  ------    -----      ----- -----   -----     -- -----------
      0       0        0         16     0              0 Idle
```

### 讨论

PowerShell 中的变量（以及所有其他脚本和编程语言中的变量）允许您存储某物的输出，以便稍后使用。变量名以美元符号（`$`）开头，后面可以跟几乎任何字符。一小部分字符在 PowerShell 中具有特殊含义，因此 PowerShell 提供了一种方法来创建包含甚至这些特殊字符的变量名。

有关 PowerShell 变量语法和类型的详细信息，请参阅 “变量”。

您可以将任何管道或命令的结果存储在变量中以供稍后使用。如果该命令生成简单数据（如数字或字符串），则变量包含简单数据。如果该命令生成丰富数据（如 `Get-Process` cmdlet 返回的代表系统进程的对象），则变量包含该丰富数据列表。如果该命令生成纯文本（如传统可执行文件的输出），则变量包含纯文本。

#### 注意

如果您已将大量数据存储到变量中但不再需要该数据，请为该变量赋一个新值（如 `$null`）。这将允许 PowerShell 释放用于存储该数据的内存。

除了您创建的变量外，PowerShell 还会自动定义若干变量，它们代表诸如配置文件位置、PowerShell 进程 ID 等信息。要获取这些自动变量的完整列表，请输入 `Get-Help about_Automatic_Variables`。

### 另请参阅

“变量”

## 3.4 访问环境变量

### 问题

您想要在脚本或交互式会话中使用环境变量（如系统路径或当前用户名）。

### 解决方案

PowerShell 提供了多种访问环境变量的方法。

要列出所有环境变量，请列出 `env` 驱动器的子项：

```
Get-ChildItem env:
```

要使用更简洁的语法获取环境变量，请在其名称前加上 `$env:`

```
$env:variablename
```

（例如，`$env:username`。）

要使用其提供程序路径获取环境变量，请将 `env:` 或 `Environment::` 提供给 `Get-ChildItem` cmdlet：

```
Get-ChildItem env:variablename
Get-ChildItem Environment::variablename
```

### 讨论

PowerShell 通过其*环境提供程序*提供对环境变量的访问。提供程序让您处理数据存储（如注册表、环境变量和别名）的方式非常类似于访问文件系统。

默认情况下，PowerShell 会创建一个名为 `env` 的驱动器，它与*环境提供程序*配合工作，让您访问环境变量。环境提供程序允许您像访问其他驱动器一样访问 `env:` 驱动器中的项目：`dir env:\variablename` 或 `dir env:variablename`。如果您想直接访问提供程序（而不是通过其驱动器），也可以输入 `dir Environment::variablename`。

然而，处理环境变量最常用（也最简单）的方法是输入 `$env:variablename`。这适用于任何提供程序，但最典型的是用于环境变量。

这是因为环境提供程序与其他几个提供程序共享一项功能——即支持核心 cmdlet 的 `*-Content` 集合（请参阅 示例 3-1）。

#### 示例 3-1. 在不同提供程序上处理内容

```
PS > "hello world" > test
PS > Get-Content test
hello world

PS > Get-Content c:test
hello world

PS > Get-Content variable:ErrorActionPreference
Continue

PS > Get-Content function:prompt
"PS $($executionContext.SessionState.Path.CurrentLocation)
    $('>' * ($nestedPromptLevel + 1)) ";
(...)

PS > Get-Content env:systemroot
C:\WINDOWS
```

#### 示例 3-2. 使用 PowerShell 的特殊变量语法访问内容

```
PS > $function:prompt
"PS $($executionContext.SessionState.Path.CurrentLocation)
    $('>' * ($nestedPromptLevel + 1)) ";

PS > $variable:ErrorActionPreference
Continue
PS > $c:test
hello world
PS > $env:systemroot
C:\WINDOWS
```

这种用于内容管理的变量语法允许您同时获取和设置内容：

```
PS > $function:prompt = { "PS > " }
PS > $function:prompt
 "PS > "
```

现在，当使用此方法访问复杂的提供程序路径时，您会很快遇到命名问题（即使底层文件存在）：

```
PS > $c:\temp\test.txt
Unexpected token '\temp\test.txt' in expression or statement.
At line:1 char:17
+ $c:\temp\test.txt <<<<
```

解决方法在于 PowerShell 对复杂变量名的转义支持。要定义复杂变量名，请将其括在大括号中：

```
PS > ${1234123!@#$!@#$12$!@#$@!} = "Crazy Variable!"
PS > ${1234123!@#$!@#$12$!@#$@!}
Crazy Variable!
PS > dir variable:\1*

Name                              Value
----                              -----
1234123!@#$!@#$12$!@#$@!          Crazy Variable!
```




## 另请参阅

"变量"

# 3.5 程序：保留批处理文件设置的环境变量更改

当批处理文件修改环境变量时，`cmd.exe` 会保留此更改，即使在脚本退出后也是如此。这经常会导致问题，因为一个批处理文件可能会意外污染另一个批处理文件的环话。话虽如此，批处理文件作者有时会有意更改全局环境，以自定义路径和环境的其他方面，使其适合特定任务。

然而，环境变量是进程的私有细节，随进程退出而消失。这使得前面提到的环境自定义脚本在从 PowerShell 运行时停止工作——就像它们在从另一个 `cmd.exe` 运行时无法工作一样（例如，`cmd.exe /c MyEnvironmentCustomizer.cmd`）。

示例 3-3 中的脚本允许你运行修改环境的批处理文件，并在 `cmd.exe` 退出后保留其更改。它通过在批处理文件完成后将环境变量存储在文本文件中，然后在 PowerShell 会话中再次设置所有这些环境变量来实现这一点。

要运行此脚本，请输入 `Invoke-CmdScript Scriptname.cmd` 或 `Invoke-CmdScript Scriptname.bat`——取决于批处理文件使用的扩展名。

#### 注意

如果这是你第一次在 PowerShell 中运行脚本，你需要配置执行策略。有关选择执行策略的更多信息，请参阅 食谱 18.1。

请注意，此脚本对 cmdlet 使用全名：`Get-Content`、`ForEach-Object`、`Set-Content` 和 `Remove-Item`。这使脚本具有可读性，非常适合他人阅读的脚本。但这绝非强制要求。对于快速脚本和交互式使用，较短的别名（如 `gc`、`%`、`sc` 和 `ri`）能让你效率更高。

#### 示例 3-3. Invoke-CmdScript.ps1

```
##############################################################################
##
## Invoke-CmdScript
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

调用指定的批处理文件（及参数），但也将任何环境变量更改传播回调用它的 PowerShell 环境。

.EXAMPLE

PS > type foo-that-sets-the-FOO-env-variable.cmd
@set FOO=%*
echo FOO set to %FOO%.

PS > $env:FOO
PS > Invoke-CmdScript "foo-that-sets-the-FOO-env-variable.cmd" Test

C:\Temp>echo FOO set to Test.
FOO set to Test.

PS > $env:FOO
Test

#>

param(
    ## 要运行的脚本路径
    [Parameter(Mandatory = $true)]
    [string] $Path,

    ## 传递给脚本的参数
    [string] $ArgumentList
)

Set-StrictMode -Version 3

$tempFile = [IO.Path]::GetTempFileName()

## 存储 cmd.exe 的输出。我们还要求 cmd.exe 在批处理文件完成后输出环境变量表
cmd /c " `"$Path`" $argumentList && set > `"$tempFile`" "

## 遍历临时文件中的环境变量。对每个变量，在本地环境中设置该变量。
Get-Content $tempFile | Foreach-Object {
    if($_ -match "^(.*?)=(.*)$")
    {
        Set-Content "env:\$($matches[1])" $matches[2]
    }
}

Remove-Item $tempFile
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 18.1，“通过执行策略启用脚本”

# 3.6 控制变量和其他项目的访问权限与作用域

## 问题

你想要控制定义变量、别名、函数和驱动器（或与之交互）时的可见性。

## 解决方案

PowerShell 提供了多种访问变量的方式。

要创建具有特定作用域的变量，请在变量名前提供该作用域：

```
$SCOPE:variable = value
```

要访问特定作用域的变量，请在变量名前提供该作用域：

```
$SCOPE:variable
```

要创建一个即使在脚本退出后仍然保留的变量，请在 `GLOBAL` 作用域中创建它：

```
$GLOBAL:variable = value
```

要从函数内部更改脚本范围的变量，请将 `SCRIPT` 作为其作用域名称：

```
$SCRIPT:variable = value
```

## 讨论

PowerShell 通过一种称为*作用域*的机制来控制对变量、函数、别名和驱动器的访问。项目的*作用域*是其可见性的另一种说法。你始终处于一个作用域中（称为*当前*或*局部*作用域），但某些操作会改变其含义。

当你的代码进入嵌套提示符、脚本、函数或脚本块时，PowerShell 会创建一个新作用域。该作用域随即成为局部作用域。在此过程中，PowerShell 会记住旧作用域与新作用域之间的关系。从新作用域的视角来看，旧作用域称为*父作用域*。从旧作用域的视角来看，新作用域称为*子作用域*。子作用域可以访问父作用域中的所有变量，但在子作用域中更改这些变量不会改变父作用域中的版本。

#### 注意

尝试从函数中更改脚本范围的变量通常是一个“陷阱”，因为函数是一个新作用域。如前所述，在子作用域（函数）中更改某内容不会影响父作用域（脚本）。本讨论的其余部分描述了更改整个脚本值的方法。

当你的代码退出嵌套提示符、脚本、函数或脚本块时，会发生相反的过程。PowerShell 移除旧作用域，然后将局部作用域更改为最初创建它的作用域——即该旧作用域的父作用域。

有些作用域非常常见，以至于 PowerShell 给它们赋予了特殊名称：

全局
: 最外层的作用域。全局作用域中的项目从所有其他作用域均可见。

脚本
: 代表当前脚本的作用域。脚本作用域中的项目从脚本中的所有其他作用域均可见。

局部
: 当前作用域。

当你定义项目的作用域时，PowerShell 还支持两个更像选项的额外作用域名称：`Private` 和 `AllScope`。当你将项目定义为具有 `Private` 作用域时，PowerShell 不会让该项目直接对子作用域可用。不过，PowerShell 并不会*隐藏*它，因为子作用域仍可使用 `Get-Variable` cmdlet 的 `-Scope` 参数从父作用域获取变量。当你为项目指定 `AllScope` 选项（通过 `*-Variable`、`*-Alias` 或 `*-Drive` cmdlet 之一）时，更改该项目的子作用域也会影响父作用域中的值。

有了这些背景知识，PowerShell 提供了多种方法来控制变量和其他项目的访问权限与作用域。

### 变量

要在特定作用域定义变量（或访问特定作用域的变量），请在变量引用中使用其作用域名称。例如：

```
$SCRIPT:myVariable = value
```

如"变量"所示，`*-Variable` 这组 cmdlet 也允许通过其 `-Scope` 参数指定作用域名称。

### 函数




## 函数作用域定义

要在特定作用域定义函数（或在特定作用域访问函数），请在创建函数时使用其作用域名称。例如：

```
function GLOBAL:MyFunction { ... }
GLOBAL:MyFunction args
```

### 别名和驱动器

要在特定作用域定义别名或驱动器，请使用 `*-Alias` 和 `*-Drive` cmdlet 的 `Option` 参数。要在特定作用域访问别名或驱动器，请使用 `*-Alias` 和 `*-Drive` cmdlet 的 `Scope` 参数。

有关作用域的详细信息，请输入 `` `Get-Help About_Scopes` ``。

## 另请参阅

“变量”

# 3.7 程序：创建动态变量

在使用变量和命令时，某些概念感觉太微不足道，不值得创建一个全新的命令或函数，但如果没有它们，脚本的可读性就会受损。

这种情况明显的几个例子包括日期运算（*yesterday* 变为 `(Get-Date).AddDays(-1)`）和深度嵌套的变量（*windowTitle* 变为 `$host.UI.RawUI.WindowTitle`）。

## 注意

网上有一些创新解决方案，利用 PowerShell 的调试功能创建一个断点，每当你尝试读取变量时就更改其值。虽然独特，但此解决方案会导致 PowerShell 认为依赖该变量的任何脚本都处于调试模式。遗憾的是，这会阻止 PowerShell 在这些脚本中启用某些重要的性能优化。

虽然我们可以编写自己的扩展来使这些更容易访问，但 `Get-Yesterday`、`Get-WindowTitle` 和 `Set-WindowTitle` 显得太微不足道，不值得拥有自己的命令。

PowerShell 允许你通过扩展其 `PSVariable` 类来定义自己的变量类型，但该功能主要是为开发人员场景设计的，而非脚本场景。示例 3-4 通过使用 PowerShell 类创建一种新的变量类型 (`DynamicVariable`) 来解决这一困境，该类型支持在获取或设置变量值时执行动态脚本操作。

### 示例 3-4. New-DynamicVariable.ps1

```
##############################################################################
##
## New-DynamicVariable
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

创建一个支持在其 getter 和 setter 中执行脚本操作的变量

.EXAMPLE

PS > .\New-DynamicVariable GLOBAL:WindowTitle `
     -Getter { $host.UI.RawUI.WindowTitle } `
     -Setter { $host.UI.RawUI.WindowTitle = $args[0] }

PS > $windowTitle
Administrator: pwsh.exe
PS > $windowTitle = "Test"
PS > $windowTitle
Test

#>

using namespace System
using namespace System.Collections.ObjectModel
using namespace System.Management.Automation

param(
    ## 动态变量的名称
    [Parameter(Mandatory = $true)]
    $Name,

    ## 获取变量值时要调用的脚本块
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $Getter,

    ## 设置变量值时要调用的脚本块
    [ScriptBlock] $Setter
)

Set-StrictMode -Version Latest

class DynamicVariable : PSVariable
{
    DynamicVariable(
        [string] $Name,
        [ScriptBlock] $ScriptGetter,
        [ScriptBlock] $ScriptSetter)
            : base($Name, $null, "AllScope")
    {
        $this.getter = $scriptGetter
        $this.setter = $scriptSetter
    }
    hidden [ScriptBlock] $getter;
    hidden [ScriptBlock] $setter;

    [Object] get_Value()
    {
        if($this.getter -ne $null)
        {
            $results = $this.getter.Invoke()
            if($results.Count -eq 1)
            {
                return $results[0];
            }
            else
            {
                $returnResults = New-Object 'PSObject[]' $results.Count
                $results.CopyTo($returnResults, 0)
                return $returnResults;
            }
        }
        else { return $null; }
    }

    [void] set_Value([Object] $Value)
    {
        if($this.setter -ne $null) { $this.setter.Invoke($Value); }
    }
}

## 如果我们已经定义了该变量，则将其移除。
if(Test-Path variable:\$name)
{
    Remove-Item variable:\$name -Force
}

## 设置新变量及其 getter 和 setter。
$executioncontext.SessionState.PSVariable.Set(
    ([DynamicVariable]::New($name, $getter, $setter)))
```

# 3.8 使用 .NET 对象

## 问题

你想要使用并交互 PowerShell 最强大的功能之一：其对 .NET 对象的内在支持。

## 解决方案

PowerShell 提供了访问方法（静态和实例）和属性的方式。

要调用类上的静态方法，请将类型名称放在方括号中，然后用两个冒号将类名与方法名分隔开：

```
[ClassName]::MethodName(parameter list)
```

要调用对象上的方法，请在代表该对象的变量与方法名之间放置一个点：

```
$objectReference.MethodName(parameter list)
```

要访问类上的静态属性，请将类型名称放在方括号中，然后用两个冒号将类名与属性名分隔开：

```
[ClassName]::PropertyName
```

要访问对象上的属性，请在代表该对象的变量与属性名之间放置一个点：

```
$objectReference.PropertyName
```

## 讨论

赋予 PowerShell 在系统管理和应用程序开发方面惊人触达能力的一个功能，是其利用 Microsoft 庞大而广泛的 .NET Framework 的能力。.NET Framework 是一个庞大的类集合。每个类体现一个特定概念，并将紧密相关的功能和信息分组。使用 .NET Framework 是 PowerShell 给管理 Shell 世界带来革命的一个方面。

.NET Framework 中的一个类示例是 `System.Diagnostics.Process`——该功能分组“提供对本地和远程进程的访问，并允许你启动和停止本地系统进程”。

## 注意

术语 *type* 和 *class* 经常互换使用。

类包含 *methods*（用于执行操作）和 *properties*（用于访问信息）。

例如，`Get-Process` cmdlet 生成 `System.Diagnostics.Process` 对象，而不是像传统 Shell 那样生成纯文本报告。管理这些进程变得非常容易，因为它们包含丰富的信息（属性）和操作（方法）组合。你不再需要解析文本流来获取进程的 ID；你可以直接询问对象！

```
PS > $process = Get-Process Notepad
PS > $process.Id
3872
```

### 静态方法

```
[ClassName]::MethodName(parameter list)
```

某些方法仅适用于类所代表的概念。例如，检索系统上所有正在运行的进程与进程的一般概念相关，而不是特定进程。适用于整个类/类型的方法称为 *static methods*。

例如：

```
PS > [System.Diagnostics.Process]::GetProcessById(0)
```

此特定任务最好由 `Get-Process` cmdlet 处理，但它演示了 PowerShell 调用 .NET 类方法的能力。它在 `System.Diagnostics.Process` 类上调用静态 `GetProcessById` 方法以获取 ID 为 0 的进程。这将生成以下输出：

```
Handles NPM(K) PM(K) WS(K) VM(M) CPU(s) Id ProcessName
------- ------ ----- ----- ----- ------ -- -----------
      0      0     0    16     0         0 Idle
```

### 实例方法

```
$objectReference.MethodName(parameter list)
```



## 使用 .NET 对象

某些方法仅与类的特定、有形实现（称为实例）相关。例如，停止实际在系统上运行的进程，而不是进程的一般概念。如果 `$objectReference` 指向特定的 `System.Diagnostics.Process`（例如由 `Get-Process` cmdlet 输出），则可以调用方法来启动它、停止它或等待它退出。作用于类的实例的方法称为*实例方法*。

> **注意**
>
> 术语*对象*通常与术语*实例*互换使用。

例如：

```powershell
PS > $process = Get-Process Notepad
PS > $process.WaitForExit()
```

将记事本进程存储到 `$process` 变量中。然后在该特定进程上调用 `WaitForExit()` 实例方法，暂停 PowerShell 直到进程退出。要了解给定方法支持的不同参数集（重载），请键入不带任何参数的方法名称：

```powershell
PS > $now = Get-Date
PS > $now.ToString

OverloadDefinitions
-------------------
string ToString()
string ToString(string format)
string ToString(System.IFormatProvider provider)
string ToString(string format, System.IFormatProvider provider)
string IFormattable.ToString(string format, System.IFormatProvider formatProvider)
string IConvertible.ToString(System.IFormatProvider provider)
```

如果你正在改编来自互联网的 C# 示例，而 PowerShell 找不到示例中使用的方法，则该方法可能是通过一种称为*显式接口实现*的相对罕见的技术添加的。如果是这种情况，可以在调用方法之前将对象转换为该接口：

```powershell
$sourceObject = 123
$result = ([IConvertible] $sourceObject).ToUint16($null)
```

## 静态属性

```powershell
[ClassName]::PropertyName
```

或：

```powershell
[ClassName]::PropertyName = value
```

与静态方法一样，某些属性仅与类所代表的概念的信息相关。例如，`System.DateTime` 类“表示一个时间点，通常以日期和时间表示。”它提供了一个返回当前时间的 `Now` 静态属性：

```powershell
PS > [System.DateTime]::Now
Monday, February 15, 2021 9:35:13 PM
```

此特定任务最好由 `Get-Date` cmdlet 处理，但它演示了 PowerShell 访问 .NET 对象属性的能力。

尽管它们相对较少见，但某些类型也允许你设置某些静态属性的值：例如 `[System.Environment]::CurrentDirectory` 属性。此属性表示进程的当前目录——即 PowerShell 的启动目录，而不是提示符中显示的路径。

## 实例属性

```powershell
$objectReference.PropertyName
```

或：

```powershell
$objectReference.PropertyName = value
```

与实例方法一样，某些属性仅与类的特定、有形实现（称为*实例*）相关。例如，实际时间点的日期，而不是日期和时间的一般概念。如果 `$objectReference` 指向特定的 `System.DateTime`（例如由 `Get-Date` cmdlet 或 `[System.DateTime]::Now` 输出），则可能需要检索其星期几、日期或月份。返回类实例信息的属性称为*实例属性*。

例如：

```powershell
PS > $today = Get-Date
PS > $today.DayOfWeek
Saturday
```

此示例将当前日期存储在 `$today` 变量中。然后调用 `DayOfWeek` 实例属性以检索该特定日期的星期几。

## 动态访问方法和属性

使用 .NET 类型时，可能会遇到一些高级场景：编写脚本时不知道方法或属性名称，但在运行时知道。即使在这些罕见情况下，PowerShell 仍允许你通过动态成员调用来访问这些成员。要使用动态名称访问属性或方法，只需将该名称存储在变量中，然后像访问其他方法或属性一样访问它：

```powershell
PS > $propertyName = "Length"
PS > "Hello World".$propertyName
11
PS > $methodName = "SubString"
PS > "Hello World".$methodName(6)
World
PS > $staticProperty = "OSVersion"
PS > [Environment]::$staticProperty

Platform ServicePack Version      VersionString
-------- ----------- -------      -------------
 Win32NT             10.0.19041.0 Microsoft Windows NT 10.0.19041.0
```

有了这些知识，接下来的问题是：“如何了解 .NET Framework 中可用的功能？”以及“如何了解对象的作用？”

关于第一个问题的答案，请参阅附录 F，其中列出了对系统管理员最有用的 .NET Framework 类。关于第二个问题的答案，请参阅配方 3.12 和 3.13。

## 另请参阅

配方 3.12，“了解类型和对象”

配方 3.13，“获取关于类型和对象的详细文档”

附录 F，*精选 .NET 类及其用途*

# 3.9 创建 .NET 对象的实例

## 问题

你想要创建一个 .NET 对象的实例，以便与其方法和属性进行交互。

## 解决方案

使用 `New-Object` cmdlet 创建对象的实例。

要使用默认构造函数创建对象的实例，请使用 `New-Object` cmdlet，并将类名作为其唯一参数：

```powershell
PS > $generator = New-Object System.Random
PS > $generator.NextDouble()
0.853699042859347
```

要创建一个接受构造函数参数的对象实例，请将这些参数提供给 `New-Object` cmdlet。在某些情况下，该类可能存在于 PowerShell 默认未加载的单独库中，例如 `System.Windows.Forms` 程序集。在这种情况下，必须首先加载包含该类的程序集：

```powershell
Add-Type -Assembly System.Windows.Forms
$image = New-Object System.Drawing.Bitmap "$pwd\source.gif"
$image.Save("$pwd\source_converted.jpg", "JPEG")
```

作为 `New-Object` cmdlet 的替代方案，你也可以使用 PowerShell 的 `new()` 方法：

```powershell
$image = [System.Drawing.Bitmap]::new("$pwd\source.gif")
```

要同时创建对象并使用它（而不保存以供后用），请将对 `New-Object` 的调用括在括号中：

```powershell
(New-Object Net.WebClient).DownloadString("http://live.com")
```

如果你计划使用同一 .NET 命名空间中的多个类，`using` 语句可以使你的代码更易于阅读和输入：

```powershell
using namespace System.Collections

$arrayList = New-Object ArrayList
$queue = [Queue]::new()
```

## 讨论

许多 cmdlet（如 `Get-Process` 和 `Get-ChildItem`）生成代表实际进程、文件和目录的实时 .NET 对象。然而，PowerShell 支持的 .NET Framework 内容远不止其 cmdlet 产生的对象。.NET Framework 的这些额外领域提供了大量功能，可用于你的脚本和常规系统管理任务中。

> **注意**
>
> 要创建泛型对象的实例，请参阅示例 3-5。



## 创建类的实例

在使用这些类中的大多数时，第一步通常是创建该类的实例，将该实例存储在变量中，然后处理该实例上的方法和属性。要创建类的实例，请使用 `New-Object` cmdlet。`New-Object` cmdlet 的第一个参数是类型名称，第二个参数是传递给构造函数的参数列表（如果构造函数接受参数）。`New-Object` cmdlet 支持 PowerShell 的 `type shortcuts`（类型快捷方式），因此你永远不必使用完全限定的类型名称。有关类型快捷方式的详细信息，请参阅 “类型快捷方式”。

除了 `New-Object` cmdlet，还可以使用 PowerShell 支持的 `new()` 方法，就像它是该类型上的静态方法一样：用方括号括起类型名称，添加两个冒号，然后使用任意参数调用该方法：

```
PS > [System.Drawing.Point]::new(10, 20)

IsEmpty  X  Y
-------  -  -
  False 10 20
```

大多数对象支持多个不同的构造函数，允许你以不同的方式创建对象。MSDN 上的官方文档通常是获取这些构造函数详细信息的最佳去处，但 PowerShell 提供了一个方便的快捷方式，即不带括号调用其 `new()` 方法（就像检查其他方法的重载一样）：

```
PS > [System.Drawing.Point]::New

OverloadDefinitions
-------------------
System.Drawing.Point new(int x, int y)
System.Drawing.Point new(System.Drawing.Size sz)
System.Drawing.Point new(int dw)
```

使用 .NET 对象时的一种常见模式是创建它们、设置一些属性，然后使用它们。`New-Object` cmdlet 的 `-Property` 参数允许你合并这些步骤：

```
$startInfo = New-Object Diagnostics.ProcessStartInfo -Property @{
    'Filename' = "pwsh.exe";
    'WorkingDirectory' = $pshome;
    'Verb' = "RunAs"
}
[Diagnostics.Process]::Start($startInfo)
```

或者通过 PowerShell 内置的类型转换，可以更简单地实现：

```
$startInfo = [Diagnostics.ProcessStartInfo] @{
    'Filename' = "pwsh.exe";
    'WorkingDirectory' = $pshome;
    'Verb' = "RunAs"
}
```

直接调用 `New-Object` cmdlet 时，如果尝试指定一个本身就是列表的参数，可能会遇到困难。假设 `$byte` 是一个字节数组：

```
PS > [byte[]] $bytes = 1..10
PS > $memoryStream = New-Object System.IO.MemoryStream $bytes
New-Object : Cannot find an overload for ".ctor" and the argument count: "10".
At line:1 char:27
+ $memoryStream = New-Object <<<< System.IO.MemoryStream $bytes
```

要解决此问题，请使用 `new()` 关键字创建对象：

```
[System.IO.MemoryStream]::New($bytes)
```

`New-Object` 的变通方法更复杂，但也有效：

```
PS > $parameters = ,$bytes
PS > $memoryStream = New-Object System.IO.MemoryStream $parameters
```

或：

```
PS > $memoryStream = New-Object System.IO.MemoryStream @(,$bytes)
```

## 从另一个程序集加载类型

PowerShell 默认提供大多数常用类型。但是，许多类型只有在加载定义它们的库（称为程序集）后才可用。类的 Microsoft 文档包含定义该类的程序集。有关从另一个程序集加载类型的详细信息，请参阅 食谱 17.8。

有关对系统管理员最有用的 .NET Framework 类的精选列表，请参阅 附录 F。要了解类支持的功能，请参阅 食谱 3.12。

有关 `New-Object` cmdlet 的详细信息，请输入 `Get-Help New-Object`。有关 `Add-Type` cmdlet 的详细信息，请输入 `Get-Help Add-Type`。

## 另请参阅

食谱 3.8，“使用 .NET 对象”

食谱 3.12，“了解类型和对象”

食谱 17.8，“访问 .NET SDK 库”

附录 F，*精选 .NET 类及其用途*

示例 3-5

## 3.10 创建泛型对象的实例

使用 .NET Framework 时，你经常会遇到主要负责管理其他对象的类。例如，`System.Collections.ArrayList` 类允许你管理对象的动态列表。你可以向 `ArrayList` 添加对象、从中移除对象、对内部对象进行排序，等等。这些对象可以是任何类型的对象：`String` 对象、整数、`DateTime` 对象等。但是，使用支持任意对象的类有时会有点别扭。一个例子是 `type safety`（类型安全）。如果你不小心将一个 `String` 添加到整数列表中，可能直到程序失败时才会发现。

虽然如果你仅在 PowerShell 中工作，这个问题基本上不复存在，但在强类型语言（如 C#）中，一个更常见的抱怨是，当你再次使用该对象时，必须提醒环境（通过显式转换）对象的类型：

```
// This is C# code
System.Collections.ArrayList list =
    new System.Collections.ArrayList();
list.Add("Hello World");

string result = (String) list[0];
```

为了解决这些问题，.NET Framework 包含一项称为 `generic types`（泛型）的功能：支持任意类型对象但允许你指定对象`哪种类型`的类。在这种情况下，是一个字符串集合：

```
// This is C# code
System.Collections.ObjectModel.Collection<String> list =
    new System.Collections.ObjectModel.Collection<String>();
list.Add("Hello World");

string result = list[0];
```

PowerShell 支持通过将泛型参数放在方括号之间来指定它们，如 示例 3-5 所示。

### 示例 3-5. 创建泛型对象

```
PS > $coll = New-Object System.Collections.ObjectModel.Collection[Int]
PS > $coll.Add(15)
PS > $coll.Add("Test")
MethodException: Cannot convert argument "item", with value: "Test", for "Add" to
type "System.Int32": "Cannot convert value "Test" to type "System.Int32".
Error: "Input string was not in a correct format.""
```

对于接受两个或更多参数的泛型类型，请提供用逗号分隔的类型列表，并用引号括起来（请参阅 示例 3-6）。

### 示例 3-6. 创建多参数泛型对象

```
PS > $map = New-Object "System.Collections.Generic.Dictionary[String,Int]"
PS > $map.Add("Test", 15)
PS > $map.Add("Test2", "Hello")
MethodException: Cannot convert argument "Hello", for "Add" to
type "System.Int32": "Cannot convert value "Test" to type "System.Int32".
Error: "Input string was not in a correct format.""
```

## 3.11 使用 COM 对象

### 问题

你想要创建一个 COM 对象以与其方法和属性进行交互。

### 解决方案

使用 `New-Object` cmdlet（带有 `-ComObject` 参数）通过其 `ProgID` 创建 COM 对象。然后，你可以像在 PowerShell 中操作任何其他对象一样，与 COM 对象的方法和属性进行交互。

```
$object = New-Object -ComObject ProgId
```

例如：

```
PS > $sapi = New-Object -Com Sapi.SpVoice
PS > $sapi.Speak("Hello World")
```

### 讨论

历史上，许多应用程序将其脚本和管理接口作为 COM 对象公开。虽然 .NET API（和 PowerShell cmdlet）是最常见的，但与 COM 对象交互仍然是一项常规的管理任务。

与 .NET Framework 中的类一样，很难知道可以使用哪些 COM 对象来帮助你完成系统管理任务。有关对系统管理员最有用的 COM 对象的精选列表，请参阅 附录 H。



# 另请参阅

有关 `New-Object` cmdlet 的详细信息，请键入 `Get-Help New-Object`。

附录 H，选定的 COM 对象及其用途

# 3.12 了解类型和对象

## 问题

你有一个对象实例，想要知道它支持哪些方法和属性。

## 解决方案

探索对象支持的方法和属性最常用的方法是通过 `Get-Member` cmdlet。

要获取存储在 `$object` 变量中的对象的实例成员，将其通过管道传递给 `Get-Member` cmdlet：

```
$object | Get-Member
Get-Member -InputObject $object
```

要获取存储在 `$object` 变量中的对象的静态成员，请向 `Get-Member` cmdlet 提供 `-Static` 标志：

```
$object | Get-Member -Static
Get-Member -Static -InputObject $object
```

要获取特定类型的静态成员，将该类型通过管道传递给 `Get-Member` cmdlet，并同时指定 `-Static` 标志：

```
[Type] | Get-Member -Static
Get-Member -Static -InputObject [Type]
```

要从存储在 `$object` 变量中的对象获取指定成员类型（例如 `Method` 或 `Property`）的成员，请将该成员类型提供给 `-MemberType` 参数：

```
$object | Get-Member -MemberType MemberType
Get-Member -MemberType MemberType -InputObject $object
```

## 讨论

`Get-Member` cmdlet 是你在探索 PowerShell 时最常用的三个命令之一。另外两个命令是 `Get-Command` 和 `Get-Help`。

### 注意

要以交互方式探索对象的方法和属性，请参阅 食谱 1.26。

如果你通过管道将对象集合（如 `Array` 或 `ArrayList`）传递给 `Get-Member` cmdlet，PowerShell 会从集合中提取每个项目，然后逐个将它们传递给 `Get-Member` cmdlet。`Get-Member` cmdlet 随后返回它接收到的每种唯一类型的成员。尽管这在绝大多数情况下很有帮助，但当你想了解集合类本身的成员或属性时，这有时会造成困难。

如果你想查看集合的属性（而不是它包含的元素），请将集合提供给 `-InputObject` 参数。或者，你可以将集合包装在一个数组中（使用 PowerShell 的 `一元逗号运算符`），这样当 `Get-Member` cmdlet 展开外层数组时，集合类就会保留下来：

```
PS > $files = Get-ChildItem
PS > ,$files | Get-Member

   TypeName: System.Object[]

Name               MemberType     Definition
----               ----------     ----------
Count              AliasProperty  Count = Length
Address            Method         System.Object& Address(Int32)
(...)
```

要了解有关类型和对象的详细信息的另一种方法，请参阅 食谱 3.13。

有关 `Get-Member` cmdlet 的详细信息，请键入 `Get-Help Get-Member`。

## 另请参阅

食谱 1.26，“程序：以交互方式查看和探索对象”

食谱 3.13，“获取关于类型和对象的详细文档”

# 3.13 获取关于类型和对象的详细文档

## 问题

你有一种对象类型，想要知道它支持的方法和属性的详细信息。

## 解决方案

[.NET Framework 文档](https://docs.microsoft.com/dotnet)是获取对象支持的方法和属性详细文档的最佳途径。这种探索通常分两个阶段进行：

1. 找到对象的类型。

   要确定对象的类型，你可以使用 `Get-Member` cmdlet 显示的类型名称（如 食谱 3.12 所述），或者调用对象的 `GetType()` 方法（如果你有一个实例）：

   ```
   PS > $date = Get-Date
   PS > $date.GetType().ToString()
   System.DateTime
   ```

2. 将该类型名称输入搜索框。

## 讨论

当 `Get-Member` cmdlet 无法提供你需要的信息时，该类型的 Microsoft 文档是一个很好的替代方案。它提供的信息比 `Get-Member` cmdlet 提供的帮助详细得多——通常包括详细描述、相关信息，甚至代码示例。Microsoft 文档侧重于开发人员通过 C# 等语言使用这些类型，因此你最初可能会发现将这些信息解读为 PowerShell 用法有点困难。

通常，类的文档首先以总体概述开始，然后提供指向该类成员（它支持的方法和属性列表）的超链接。

### 注意

要快速访问成员的文档，请通过将“members”一词添加到搜索词中来更明确地搜索它们：`typename` members。

类成员的文档列出了方法和属性，就像 `Get-Member` cmdlet 的输出一样。S 图标代表静态方法和属性。单击成员名称可获取有关该方法或属性的更多信息。

### 公共构造函数

此部分列出了该类型的构造函数。当你通过 `New-Object` cmdlet 创建该类型时，会使用构造函数。当你单击某个构造函数时，文档会提供创建该对象的所有不同方式，包括你将与 `New-Object` cmdlet 一起使用的参数列表。

### 公共字段/公共属性

此部分列出了对象的字段和属性的名称。S 图标代表静态字段或属性。当你单击某个字段或属性时，文档还会提供该字段或属性返回的类型。

例如，你可能会在 `System.DateTime.Now` 的定义中看到以下内容：

```
public static DateTime Now { get; }
```

`Public` 意味着 `Now` 属性是公共的——你可以从 PowerShell 访问它。`Static` 意味着该属性是静态的（如 食谱 3.8 所述）。`DateTime` 意味着调用该属性时会返回一个 `DateTime` 对象。`get;` 意味着你可以从此属性获取信息，但不能设置信息。许多属性也支持 `set;`（例如 `System.IO.FileInfo` 上的 `IsReadOnly` 属性），这意味着你可以更改其值。

### 公共方法

此部分列出了对象的方法名称。S 图标代表静态方法。当你单击某个方法时，文档会提供调用该方法的所有不同方式，包括你将在 PowerShell 中用来调用该方法的参数列表。

例如，你可能会在 `System.DateTime.AddDays()` 的定义中看到以下内容：

```
C#
public DateTime AddDays (
    double value
)
```

`Public` 意味着 `AddDays` 方法是公共的——你可以从 PowerShell 访问它。`DateTime` 意味着调用该方法时会返回一个 `DateTime` 对象。文本 `double value` 意味着此方法需要一个参数（类型为 `double`）。在本例中，该参数决定了要向你调用该方法的 `DateTime` 对象添加多少天。

## 另请参阅

食谱 3.8，“使用 .NET 对象”

食谱 3.12，“了解类型和对象”

# 3.14 向对象添加自定义方法和属性

## 问题

你有一个对象，想要向该对象添加你自己的自定义属性或方法（`成员`）。

## 解决方案

使用 `Add-Member` cmdlet 向对象添加自定义成员。

## 讨论



`Add-Member` cmdlet 在帮助您向单个对象添加自定义成员方面非常有用。例如，假设您想根据当前目录中的文件创建一份报表，且该报表应包含每个文件的所有者。`Get-ChildItem` 生成的对象上没有标准的 `Owner` 属性，但您可以编写一个小脚本来添加它们，如 示例 3-7 所示。

### 示例 3-7. 向其文件对象输出添加自定义属性的脚本

```
##############################################################################
##
## Get-OwnerReport
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

获取当前目录中的文件列表，但将所有者添加到生成的对象中。

.EXAMPLE

PS > Get-OwnerReport | Format-Table Name,LastWriteTime,Owner
检索当前目录中的所有文件，并显示
名称、最后写入时间和所有者

#>

Set-StrictMode -Version 3

$files = Get-ChildItem
foreach($file in $files)
{
    $owner = (Get-Acl $file).Owner
    $file | Add-Member NoteProperty Owner $owner
    $file
}
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

要添加到对象的最常见信息类型是 `NoteProperty` 中的静态信息。如果您省略它，`Add-Member` 甚至将其用作默认值：

```
PS > $item = Get-Item C:\
PS > $item | Add-Member VolumeName "Operating System"
PS > $item.VolumeName
Operating System
```

除了 note 属性外，`Add-Member` cmdlet 还支持其他几种属性和方法类型，包括 `AliasProperty`、`ScriptProperty`、`CodeProperty`、`CodeMethod` 和 `ScriptMethod`。有关这些其他属性类型的更详细说明，请参阅 “使用 .NET Framework” 以及 `Add-Member` cmdlet 的帮助文档。

#### 注意

要创建全新的对象（而不是向现有对象添加信息），请参阅 食谱 3.15。

虽然 `Add-Member` cmdlet 允许您自定义特定对象，但它不允许您自定义该类型的所有对象。有关如何执行此操作的信息，请参阅 食谱 3.16。

### 计算属性

`Calculated` 属性是向输出对象添加信息的另一种有用方法。如果您的脚本或命令使用 `Format-Table` 或 `Select-Object` 命令生成输出，则可以通过提供生成其值的表达式来创建其他属性。例如：

```
Get-ChildItem |
  Select-Object Name,
      @{Name="Size (MB)"; Expression={ "{0,8:0.00}" -f ($_.Length / 1MB) } }
```

在此命令中，我们获取目录中的文件列表。我们使用 `Select-Object` 命令检索其名称和一个名为 `Size (MB)` 的计算属性。此计算属性以兆字节为单位返回文件大小，而不是默认值（字节）。

#### 注意

`Format-Table` cmdlet 支持类似的哈希表来添加计算属性，但使用 `Label`（而不是 `Name`）作为键来标识属性。为了消除由此产生的混淆，PowerShell 团队更新了这两个 cmdlet，使它们同时接受 `Name` 和 `Label`。

有关 `Add-Member` cmdlet 的更多信息，请键入 `Get-Help Add-Member`。

有关添加计算属性的更多信息，请键入 `Get-Help Select-Object` 或 `Get-Help Format-Table`。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 3.15，“创建并初始化自定义对象”

食谱 3.16，“向类型添加自定义方法和属性”

“使用 .NET Framework”

# 3.15 创建并初始化自定义对象

## 问题

您希望从命令返回结构化结果，以便用户可以轻松地对它们进行排序、分组和筛选。

## 解决方案

使用 `[PSCustomObject]` 类型转换创建一个新的 `PSCustomObject`，并提供一个包含自定义信息的哈希表作为其值，如 示例 3-8 所示。

### 示例 3-8. 创建自定义对象

```
$output = [PSCustomObject] @{
    'User' = 'DOMAIN\User';
    'Quota' = 100MB;
    'ReportDate' = Get-Date;
}
```

如果您想创建一个带有关联功能的自定义对象，请在模块中编写一个 PowerShell 类，并创建该类的实例：

```
using module c:\modules\PlottingObject.psm1

$obj = [PlottingObject]::new()
$obj.Move(10,10)
$obj.Points = SineWave
while($true) { $obj.Rotate(10); $obj.Draw(); Sleep -m 20 }
```

## 讨论

当您的脚本向用户输出信息时，始终应优先考虑结构丰富的数据，而非手工格式化的报表。通过输出自定义对象，您赋予最终用户对脚本输出的控制权，就像 PowerShell 赋予您对其自身命令输出的控制权一样。

尽管自定义对象输出功能强大，但用户编写的脚本经常继续生成纯文本输出。部分原因要归咎于 PowerShell 以前对创建和初始化自定义对象的繁琐支持，如 示例 3-9 所示。

### 示例 3-9. 在 PowerShell 版本 1 中创建自定义对象

```
$output = New-Object PsObject
Add-Member -InputObject $output NoteProperty User 'DOMAIN\user'
Add-Member -InputObject $output NoteProperty Quota 100MB
Add-Member -InputObject $output NoteProperty ReportDate (Get-Date)

$output
```

在 PowerShell 版本 1 中，创建自定义对象需要创建一个新对象（类型为 `PsObject`），然后多次调用 `Add-Member` cmdlet 以添加所需的属性。PowerShell 版本 2 通过向 `New-Object` cmdlet 添加 `-Property` 参数，使这变得容易得多，该参数也适用于 `PSObject` 类型。PowerShell 版本 3 通过直接支持 `[PSCustomObject]` 类型转换，使这变得尽可能简单。

虽然创建 `PSCustomObject` 使创建以数据为中心的对象（通常称为 `property bags`）变得容易，但它不允许您向这些对象添加功能。当您也需要功能时，下一步是创建一个 PowerShell 类（请参阅 示例 3-10）。与许多其他语言一样，PowerShell 类支持构造函数、公共属性和公共方法，以及内部辅助变量和方法。

### 示例 3-10. 创建导出自定义类的模块



# PlottingObject.psm1

源自 *PowerShell Cookbook* (O'Reilly)，作者 Lee Holmes (http://www.leeholmes.com/guide)

## 示例

```powershell
function SineWave { -15..15 | % { ,($_,(10 * [Math]::Sin($_ / 3))) } }
function Box { -5..5 | % { ($_,-5),($_,5),(-5,$_),(5,$_) } }

using module PlottingObject

$obj = [PlottingObject]::New(@())
$obj.Points = Box
$obj.Move(10,10)
while($true) { $obj.Rotate(10); $obj.Draw(); Start-Sleep -m 20 }

$obj = [PlottingObject]::New((SineWave))
while($true) { $obj.Rotate(10); $obj.Draw(); Start-Sleep -m 20 }
```

## 类定义

```powershell
class PlottingObject
{
    ## Constructors: one with no arguments and another that takes a
    ## set of initial points.
    PlottingObject()
    {
        $this.Points = @()
    }

    PlottingObject($initialPoints)
    {
        $this.Points = $initialPoints
    }

    ## An external property holding the points to plot
    $Points = @()

    ## Internal variables
    hidden $x = 0
    hidden $y = 0
    hidden $angle = 0
    hidden $xScale = -50,50
    hidden $yScale = -50,50
    hidden $windowWidth = [Console]::WindowWidth
    hidden $windowHeight = [Console]::WindowHeight

    ## A public method to rotate the points by a certain amount
    [void] Rotate([int] $angle)
    {
        $this.angle += $angle
    }

    ## A public method to move the points by a certain amount
    [void] Move([int] $xDelta, [int] $yDelta)
    {
        $this.x += $xDelta
        $this.y += $yDelta
    }

    ## A public method to draw the given points
    [void] Draw()
    {
        $degToRad = 180 * [Math]::Pi

        ## Go through each of the supplied points,
        ## move them the amount specified, and then rotate them
        ## by the angle specified
        $frame = foreach($point in $this.Points)
        {
            $pointX,$pointY = $point
            $pointX = $pointX + $this.x
            $pointY = $pointY + $this.y

            $newX = $pointX * [Math]::Cos($this.angle / $degToRad ) -
                $pointY * [Math]::Sin($this.angle / $degToRad )
            $newY = $pointY * [Math]::Cos($this.angle / $degToRad ) +
                $pointX * [Math]::Sin($this.angle / $degToRad )

            $this.PutPixel($newX, $newY, 'O')
        }

        ## Draw the origin
        $frame += $this.PutPixel(0, 0, '+')

        Clear-Host
        Write-Host "`e[?25l" -NoNewline
        Write-Host $frame -NoNewline
   }

    ## A helper function to draw a pixel on the screen
    hidden [string] PutPixel([int] $x, [int] $y, [char] $character)
    {
        $scaledX = ($x - $this.xScale[0]) / ($this.xScale[1] - $this.xScale[0])
        $scaledX = [int] ($scaledX * $this.windowHeight * 2.38)

        $scaledY = (($y * 4 / 3) - $this.yScale[0]) / ($this.yScale[1] - $this.yScale[0])
        $scaledY = [int] ($scaledY * $this.windowHeight)

        return "`e$scaledY;${scaledX}H$character"
    }
}
```

有关创建模块的详细信息，请参阅 [食谱 11.6。有关 PowerShell 类语法的详细信息，请参阅 “类”。

## 另请参阅

- 食谱 7.13，“创建哈希表或关联数组”
- 食谱 11.6，“在模块中打包通用命令”
- “类”

# 3.16 向类型添加自定义方法和属性

## 问题

您想要向特定类型的所有对象添加自定义属性或方法。

## 解决方案

使用 `Update-TypeData` cmdlet 向类型的所有对象添加自定义成员。

```powershell
Update-TypeData -TypeName AddressRecord `
    -MemberType AliasProperty -Membername Cell -Value Phone
```

或者，使用自定义类型扩展文件。

## 讨论

虽然 `Add-Member` cmdlet 在帮助您向单个对象添加自定义成员方面非常有用，但它要求您向要交互的每个对象添加成员。它不允许您自动将它们添加到该类型的所有对象。为此，PowerShell 支持另一种机制——*自定义类型扩展*。

向类型的所有实例添加成员最简单、最常用的方法是通过 `Update-TypeData` cmdlet。此 cmdlet 支持别名、注释、脚本方法等：

```powershell
$r = [PSCustomObject] @{
    Name = "Lee";
    Phone = "555-1212";
    SSN = "123-12-1212"
}
$r.PSTypeNames.Add("AddressRecord")
Update-TypeData -TypeName AddressRecord `
    -MemberType AliasProperty -Membername Cell -Value Phone
```

自定义类型扩展让您能够轻松地向系统公开的任何类型添加自己的功能。如果您编写的代码（例如脚本或函数）主要与单一类型的对象交互，那么该代码可能更适合作为该类型的扩展。

例如，设想一个返回指定驱动器可用磁盘空间的脚本。虽然作为脚本很有帮助，但您可能会发现，让 PowerShell 的 `PSDrive` 对象直接告诉您剩余多少可用空间会更方便。

除了 `Update-TypeData` 方法外，PowerShell 还支持基于 XML 的类型扩展文件。由于类型扩展文件是 XML 文件，请确保您的自定义内容正确编码了在 XML 文件中具有特殊含义的字符，例如 `<`、`>` 和 `&`。

有关这些格式化 XML 文件支持的功能的详细信息，请键入 `Get-Help about_format.ps1xml`。

### 入门

如果您尚未这样做，创建类型扩展文件的第一步是创建一个空文件。最佳位置可能是在您的自定义配置文件所在的同一目录中，文件名为 *Types.Custom.ps1xml*，如 示例 3-11 所示。

#### 示例 3-11. Types.Custom.ps1xml 示例文件

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Types>
</Types>
```

接下来，向 PowerShell 配置文件添加几行，以便 PowerShell 在启动时加载您的类型扩展：

```powershell
$typeFile = (Join-Path (Split-Path $profile) "Types.Custom.ps1xml")
Update-TypeData -PrependPath $typeFile
```

默认情况下，PowerShell 会从其内部配置存储加载多个类型扩展。`Update-TypeData` cmdlet 告诉 PowerShell 也在您的 *Types.Custom.ps1xml* 文件中查找扩展。`-PrependPath` 参数使 PowerShell 在发生冲突时优先使用您的扩展而非内置扩展。

拥有自定义类型文件后，添加功能就变得相对简单了。作为主题，这些示例完全实现了我们之前提到的内容：向 PowerShell 的 `PSDrive` 类型添加功能。

###### 注意

PowerShell 会自动执行此操作。键入 `Get-PSDrive` 查看结果。

为了支持这一点，您需要扩展自定义类型文件，使其定义对 `System.Management.Automation.PSDriveInfo` 类型的添加，如 示例 3-12 所示。`System.Management.Automation.PSDriveInfo` 是 `Get-PSDrive` cmdlet 生成的类型。

#### 示例 3-12. 自定义类型文件更改模板

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Types>
  <Type>
    <Name>System.Management.Automation.PSDriveInfo</Name>
    <Members>
        add members such as <ScriptProperty> here
    <Members>
  </Type>
</Types>
```

### 添加 ScriptProperty

`ScriptProperty` 允许您使用 PowerShell 脚本作为扩展语言向类型添加属性（用于获取和设置信息）。它由三个子元素组成：属性的 `Name`、属性的 *获取器*（通过 `GetScriptBlock` 子元素）以及属性的 *设置器*（通过 `SetScriptBlock` 子元素）。



# 3.16 向类型添加自定义方法和属性

## 添加 ScriptProperty

在 `GetScriptBlock` 和 `SetScriptBlock` 两个部分中，`$this` 变量指代正在被扩展的当前对象。在 `SetScriptBlock` 部分中，`$args[0]` 变量表示用户作为赋值语句右侧提供的值。

示例 3-13 向 `PSDriveInfo` 添加了一个 `AvailableFreeSpace` `ScriptProperty`，应放置在 示例 3-12 给出的模板的 members 部分中。当你访问该属性时，它返回驱动器上剩余的可用空间量。当你设置该属性时，它输出为获得该可用空间量必须进行的更改。

### 示例 3-13. PSDriveInfo 类型的 ScriptProperty

```xml
<ScriptProperty>
  <Name>AvailableFreeSpace</Name>
  <GetScriptBlock>
    ## Ensure that this is a FileSystem drive
    if($this.Provider.ImplementingType -eq
       [Microsoft.PowerShell.Commands.FileSystemProvider])
    {
       ## Also ensure that it is a local drive
       $driveRoot = $this.Root
       $fileZone = [System.Security.Policy.Zone]::CreateFromUrl(
                 $driveRoot).SecurityZone
       if($fileZone -eq "MyComputer")
       {
          $drive = New-Object System.IO.DriveInfo $driveRoot
          $drive.AvailableFreeSpace
       }
    }
  </GetScriptBlock>
  <SetScriptBlock>
   ## Get the available free space
   $availableFreeSpace = $this.AvailableFreeSpace

   ## Find out the difference between what is available, and what they
   ## asked for.
   $spaceDifference = (([long] $args[0]) - $availableFreeSpace) / 1MB

   ## If they want more free space than they have, give that message
   if($spaceDifference -gt 0)
   {
       $message = "To obtain $args bytes of free space, " +
          " free $spaceDifference megabytes."
       Write-Host $message
   }
   ## If they want less free space than they have, give that message
   else
   {
       $spaceDifference = $spaceDifference * -1
       $message = "To obtain $args bytes of free space, " +
           " use up $spaceDifference more megabytes."
        Write-Host $message
   }
  </SetScriptBlock>
</ScriptProperty>
```

## 添加 AliasProperty

`AliasProperty` 为属性提供一个替代名称（别名）。当 PowerShell 处理你的类型扩展文件时，被引用的属性不需要存在，因为你（或另一个脚本）稍后可能会通过 `Add-Member` cmdlet 等机制添加该属性。

示例 3-14 向 `PSDriveInfo` 添加了一个 `Free` `AliasProperty`，也应放置在 示例 3-12 给出的模板的 members 部分中。当你访问该属性时，它返回 `AvailableFreeSpace` 属性的值。当你设置该属性时，它设置 `AvailableFreeSpace` 属性的值。

### 示例 3-14. PSDriveInfo 类型的 AliasProperty

```xml
<AliasProperty>
  <Name>Free</Name>
  <ReferencedMemberName>AvailableFreeSpace</ReferencedMemberName>
</AliasProperty>
```

## 添加 ScriptMethod

`ScriptMethod` 允许你使用 PowerShell 脚本作为扩展语言，在对象上定义一个操作。它包含两个子元素：属性的 `Name` 和 `Script`。

在 script 元素中，`$this` 变量指代你正在扩展的当前对象。与独立脚本一样，`$args` 变量表示传递给方法的参数。与独立脚本不同，`ScriptMethod` 不支持用于参数的 `param` 语句。

示例 3-15 向 `PSDriveInfo` 添加了一个 `Remove` `ScriptMethod`。与其他添加一样，将这些自定义项放置在 示例 3-12 给出的模板的 members 部分中。当你不带参数调用此方法时，该方法模拟移除驱动器（通过 `Remove-PSDrive` 的 `-WhatIf` 选项）。如果你以 `$true` 作为第一个参数调用此方法，它将实际从 PowerShell 会话中移除该驱动器。

### 示例 3-15. PSDriveInfo 类型的 ScriptMethod

```xml
<ScriptMethod>
  <Name>Remove</Name>
  <Script>
    $force = [bool] $args[0]
    ## Remove the drive if they use $true as the first parameter
    if($force)
    {
       $this | Remove-PSDrive
    }
    ## Otherwise, simulate the drive removal
    else
    {
       $this | Remove-PSDrive -WhatIf
    }
  </Script>
</ScriptMethod>
```

## 添加其他扩展点

PowerShell 在类型扩展文件中支持其他几个功能，包括 `CodeProperty`、`NoteProperty`、`CodeMethod` 和 `MemberSet`。虽然这些功能通常对最终用户没什么用处，但 PowerShell 提供程序和 cmdlet 的开发者会发现它们很有帮助。有关这些附加功能的更多信息，请参阅 PowerShell 软件开发工具包 (SDK) 或 Microsoft 文档。

# 3.17 为类型定义自定义格式

## 问题

你希望从脚本中输出自定义对象，并以特定方式格式化它们。

## 解决方案

使用自定义格式扩展文件为该类型定义格式，然后调用 `Update-FormatData` cmdlet 将其加载到你的会话中：

```powershell
$formatFile = Join-Path (Split-Path $profile) "Format.Custom.Ps1Xml"
Update-FormatData -PrependPath $typesFile
```

如果基于文件的方法不可行，请使用 `[Runspace]::DefaultRunspace.InitialSessionState` 类型的 `Formats` 属性为自定义类型添加新的格式定义。

## 讨论

当 PowerShell 命令产生输出时，输出以丰富结构化对象的形式出现，而不是基本的文本流。不过，这些丰富结构化对象一旦显示在屏幕上就不再有用，因此 PowerShell 在将它们显示在屏幕上之前，会引导它们经历最后一个阶段：*格式化* 和 *输出*。

格式化和输出系统基于*视图*的概念。视图可以采取多种形式：表视图、列表视图、复杂视图等。最常见的视图类型是表视图。当你在命令中使用 `Format-Table`，或者对象具有四个或更少属性时，就会看到这种形式。

与 食谱 3.16 中描述的自定义类型扩展一样，PowerShell 支持基于文件和基于内存的类型格式定义更新。

为类型定义格式最简单且最常用的方法是通过 `Update-FormatData` cmdlet，如解决方案所示。`Update-FormatData` cmdlet 接受 `Format.ps1xml` 文件的路径作为输入。PowerShell 安装目录中有许多格式定义示例可供使用。要创建自己的格式自定义，请将这些文件作为示例来源，但不要直接修改它们。相反，请创建一个新文件，并使用 `Update-FormatData` cmdlet 加载你的自定义项。

有关这些格式化 XML 文件支持的功能的更多信息，请输入 `Get-Help about_format.ps1xml`。

除了基于文件的格式化，PowerShell 还使从头创建格式定义成为可能（虽然不容易）。示例 3-16 提供了一个脚本来简化此过程。

### 示例 3-16. Add-FormatData.ps1



