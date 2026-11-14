
# 11.15 支持 `-WhatIf` 和 `-Confirm` 参数

## 问题

你想要支持标准的 `-WhatIf` 和 `-Confirm` 参数，并访问 PowerShell 引擎中以 cmdlet 为中心的支持。

## 解决方案

确保你的脚本或函数声明了 `[CmdletBinding()]` 特性，然后通过 `$psCmdlet` 自动变量访问引擎功能。

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

## 讨论

当脚本或函数进阶为*高级函数*时，PowerShell 会定义一个额外的 `$psCmdlet` 自动变量。此自动变量公开了对 `-WhatIf` 和 `-Confirm` 自动参数的支持。如果你的命令定义了参数集，它还会根据用户选择的参数公开 PowerShell 选定的参数集名称。有关高级函数的更多信息，请参阅[食谱 11.11](#ch11.xhtml#access_arguments_of_a_script_function_or)。

要支持 `-WhatIf` 和 `-Confirm` 参数，请在脚本或函数内部添加 `[CmdletBinding(SupportsShouldProcess = $true)]` 特性。你应该在任何修改系统状态的脚本或函数上支持此特性，因为它们允许用户在实际执行前调查脚本将执行的操作。然后，你只需用 `if($psCmdlet.ShouldProcess(...) ) { }` 块包围更改系统的脚本部分即可。[示例 11-14](#ch11.xhtml#adding_support_whatif_confirm) 演示了此方法。

### 示例 11-14. 添加对 `-WhatIf` 和 `-Confirm` 的支持

```
function Invoke-MyAdvancedFunction
{
    [CmdletBinding(SupportsShouldProcess = $true)]
    param()

    if($psCmdlet.ShouldProcess("test.txt", "Remove Item"))
    {
        "Removing test.txt"
    }

    Write-Verbose "Verbose Message"
}
```

现在你的高级函数表现得和内置的 PowerShell cmdlet 一样规范！

```
PS > Invoke-MyAdvancedFunction -WhatIf
What if: Performing operation "Remove Item" on Target "test.txt".
```

如果你的命令会导致需要谨慎评估的高影响结果，请调用 `$psCmdlet.ShouldContinue()` 方法。这会为用户生成警告，但请务必支持一个 `-Force` 参数，让他们可以绕过此消息。

```
function Invoke-MyDangerousFunction
{
    [CmdletBinding()]
    param(
        [Switch] $Force
    )

    if($Force -or $psCmdlet.ShouldContinue(
        "Do you wish to invoke this dangerous operation?
         Changes can not be undone.",
        "Invoke dangerous action?"))
    {
        "Invoking dangerous action"
    }
}
```

这将生成标准的 PowerShell 确认消息：

```
PS > Invoke-MyDangerousFunction

Invoke dangerous action?
Do you wish to invoke this dangerous operation? Changes can not be undone.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Invoking dangerous action

PS > Invoke-MyDangerousFunction -Force
Invoking dangerous action
```

要进一步探索 `$psCmdlet` 自动变量，可以使用[示例 11-15](#ch11.xhtml#invoke-advancedfunctionps1)。此命令创建最基本的高级函数，然后在其中调用你提供的任何脚本块。

### 示例 11-15. Invoke-AdvancedFunction.ps1

```
param(
    [Parameter(Mandatory = $true)]
    [ScriptBlock] $Scriptblock
    )

## Invoke the script block supplied by the user.
& $scriptblock
```

为了开放式探索，请使用 `$host.EnterNestedPrompt()` 作为脚本块：

```
PS > Invoke-AdvancedFunction { $host.EnterNestedPrompt() }
PS > $psCmdlet | Get-Member


   TypeName: System.Management.Automation.PSScriptCmdlet

Name                                MemberType Definition
----                                ---------- ----------
(...)
WriteDebug                          Method     System.Void WriteDebug(s...
WriteError                          Method     System.Void WriteError(S...
WriteObject                         Method     System.Void WriteObject(...
WriteProgress                       Method     System.Void WriteProgres...
WriteVerbose                        Method     System.Void WriteVerbose...
WriteWarning                        Method     System.Void WriteWarning...
(...)
ParameterSetName                    Property   System.String ParameterS...

(Now at a nested prompt)
PS >> exit

(Now back to the regular prompt)
PS >
```

有关 PowerShell 引擎中 cmdlet 支持的更多信息，请参阅 [PowerShell 程序员指南](https://oreil.ly/jJrgp) 中的开发人员参考。

## 另请参阅

[食谱 11.11，「访问脚本、函数或脚本块的参数」](#ch11.xhtml#access_arguments_of_a_script_function_or)

# 11.16 为脚本或函数添加帮助

## 问题

你想要让你的命令和使用信息可通过 `Get-Help` 命令获取。

## 解决方案

在脚本开头添加描述性帮助注释（包含特定于帮助的特殊标记，如 `.SYNOPSIS`、`.EXAMPLE` 和 `.OUTPUTS`），用于提供摘要、说明、示例、注释等。在参数前添加描述性帮助注释以描述其含义和行为：

```
##############################################################################
##
## Measure-CommandPerformance
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Measures the average time of a command, accounting for natural variability by
automatically ignoring the top and bottom 10%.

.EXAMPLE

PS > Measure-CommandPerformance.ps1 { Start-Sleep -m 300 }

Count    : 30
Average  : 312.10155
(...)

#>

param(
    ## The command to measure
    [Scriptblock] $Scriptblock,

    ## The number of times to measure the command's performance
    [int] $Iterations = 30
)

Set-StrictMode -Version 3

## Figure out how many extra iterations we need to account for the outliers
$buffer = [int] ($iterations * 0.1)
$totalIterations = $iterations + (2 * $buffer)

## Get the results
$results = 1..$totalIterations |
    Foreach-Object { Measure-Command $scriptblock }

## Sort the results, and skip the outliers
$middleResults = $results | Sort TotalMilliseconds |
    Select -Skip $buffer -First $iterations

## Show the average
$middleResults | Measure-Object -Average TotalMilliseconds
```

## 讨论

正如[食谱 11.12](#ch11.xhtml#add_validation_to_parameters) 中讨论的参数验证一样，丰富的帮助传统上仅在最高端的命令中受支持。对于大多数命令，如果你能弄清楚如何获取某种形式的使用消息，那已经很幸运了。

与 PowerShell 易于定义的高级参数验证支持一样，为命令和函数添加帮助极其简单。尽管简单，但基于注释的帮助提供了你期望的完全成熟的 PowerShell 命令的所有功能：概述、说明、示例、特定于参数的详细信息等。

PowerShell 通过查看脚本或函数的注释来创建帮助。如果注释包含任何受支持的帮助标记（如 `.SYNOPSIS`、`.EXAMPLE`、`.OUTPUTS`），PowerShell 会将它们添加到命令的帮助中。

### 注意

为了加快这些帮助注释的处理速度，PowerShell 对其出现的位置施加了限制。此外，如果它遇到的注释*不是*基于帮助的注释，它将停止搜索该注释块中的帮助标记。如果你习惯在脚本开头放置标题或版权信息，这可能会让你感到惊讶。解决方案演示了如何通过将标题和基于注释的帮助放在单独的注释块中来避免此问题。有关这些准则的更多信息，请输入 `Get-Help about_Comment_Based_Help`。
