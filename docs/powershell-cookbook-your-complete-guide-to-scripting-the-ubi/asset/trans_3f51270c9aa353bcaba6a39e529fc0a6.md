
## 示例 11-9. 在模块作用域内调用命令

```
##############################################################################
##
## Enter-Module
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Lets you examine internal module state and functions by executing user
input in the scope of the supplied module.

.EXAMPLE

PS > Import-Module PersistentState
PS > Get-Module PersistentState

ModuleType Name                      ExportedCommands
---------- ----                      ----------------
Script     PersistentState           {Set-Memory, Get-Memory}


PS > "Hello World" | Set-Memory
PS > $m = Get-Module PersistentState
PS > Enter-Module $m
PersistentState: dir variable:\mem*

Name                           Value
----                           -----
memory                         {Hello World}

PersistentState: exit
PS >

#>

param(
    ## The module to examine
    [System.Management.Automation.PSModuleInfo] $Module
)

Set-StrictMode -Version 3

$userInput = Read-Host $($module.Name)
while($userInput -ne "exit")
{
    $scriptblock = [ScriptBlock]::Create($userInput)
    & $module $scriptblock

    $userInput = Read-Host $($module.Name)
}
```

## 讨论

PowerShell 模块是创建共享私有状态的相关命令集的有效方式。虽然模块中的命令可以在彼此间共享私有状态，但 PowerShell 可防止该状态意外影响 PowerShell 会话的其余部分。

不过，在开发模块时，您有时可能需要出于诊断目的与该内部状态进行交互。为了支持这一点，PowerShell 允许您使用调用运算符 (`&`) 以特定模块为目标：

```
PS > $m = Get-Module PersistentState
PS > & $m { dir variable:\mem* }

Name                           Value
----                           -----
memory                         {Hello World}
```

对于更详细的调查任务，此语法显得笨重，因此 `Enter-Module` 会为您自动完成提示和调用。

有关编写模块的更多信息，请参阅[食谱 11.6](#ch11.xhtml#place_common_functions_in_a_library)。

## 另请参考

[食谱 11.6，“在模块中打包通用命令”](#ch11.xhtml#place_common_functions_in_a_library)

## 11.10 处理模块移除时的清理任务

## 问题

您有一个模块，并希望在该模块被移除时执行某些操作（如清理任务）。

## 解决方案

将脚本块赋值给 `$MyInvocation.MyCommand.ScriptBlock.Module.OnRemove` 事件。将任何清理命令放在该脚本块中。请参阅[示例 11-10](#ch11.xhtml#ch11_id2304788)。

### 示例 11-10. 从模块内部处理清理任务

```
##############################################################################
##
## TidyModule.psm1
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS
Demonstrates how to handle cleanup tasks when a module is removed

.EXAMPLE

PS > Import-Module TidyModule
PS > $TidyModuleStatus
Initialized
PS > Remove-Module TidyModule
PS > $TidyModuleStatus
Cleaned Up

#>

## Perform some initialization tasks
$GLOBAL:TidyModuleStatus = "Initialized"

## Register for cleanup
$MyInvocation.MyCommand.ScriptBlock.Module.OnRemove = {
    $GLOBAL:TidyModuleStatus = "Cleaned Up"
}
```

## 讨论

PowerShell 模块有一种自然的方式来定义初始化要求（编写在模块主体中的任何脚本），但清理要求并不那么简单。

在模块创建期间，您可以使用 `$MyInvocation.MyCommand.ScriptBlock.Module` 属性访问您的模块。每个模块都有一个 `OnRemove` 事件，您可以通过向其分配一个脚本块来订阅该事件。当 PowerShell 卸载您的模块时，它会调用该脚本块。

请勿将此技术用于极其敏感的清理要求。如果用户只是关闭 PowerShell 窗口，则不会处理 `OnRemove` 事件。如果存在此顾虑，请注册 `PowerShell.Exiting` 引擎事件并从那里移除您的模块：

```
Register-EngineEvent PowerShell.Exiting { Remove-Module TidyModule }
```

这免去了用户必须记住调用 `Remove-Module` 的麻烦。

有关编写模块的更多信息，请参阅[食谱 11.6](#ch11.xhtml#place_common_functions_in_a_library)。有关 PowerShell 事件的更多信息，请参阅[食谱 31.2](#ch31.xhtml#respond_custom_events)。

## 另请参考

[食谱 11.6，“在模块中打包通用命令”](#ch11.xhtml#place_common_functions_in_a_library)

[食谱 31.2，“创建并响应自定义事件”](#ch31.xhtml#respond_custom_events)

## 11.11 访问脚本、函数或脚本块的参数

## 问题

您想要访问提供给脚本、函数或脚本块的参数。

## 解决方案

要按名称访问参数，请使用 `param` 语句：

```
param($firstNamedArgument, [int] $secondNamedArgument = 0)

"First named argument is: $firstNamedArgument"
"Second named argument is: $secondNamedArgument"
```

要按位置访问未命名参数，请使用 `$args` 数组：

```
"First positional argument is: " + $args[0]
"Second positional argument is: " + $args[1]
```

您可以在脚本、函数和脚本块中以完全相同的方式使用这些技术，如[示例 11-11](#ch11.xhtml#working_with_arguments_in_scripts_functi)所示。

### 示例 11-11. 在脚本、函数和脚本块中使用参数

```
##############################################################################
##
## Get-Arguments
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Uses command-line arguments

#>

param(
    ## The first named argument
    $FirstNamedArgument,

    ## The second named argument
    [int] $SecondNamedArgument = 0
)

Set-StrictMode -Version 3

## Display the arguments by name
"First named argument is: $firstNamedArgument"
"Second named argument is: $secondNamedArgument"

function GetArgumentsFunction
{
    ## We could use a param statement here, as well
    ## param($firstNamedArgument, [int] $secondNamedArgument = 0)

    ## Display the arguments by position
    "First positional function argument is: " + $args[0]
    "Second positional function argument is: " + $args[1]
}

GetArgumentsFunction One Two

$scriptBlock =
{
    param($firstNamedArgument, [int] $secondNamedArgument = 0)

    ## We could use $args here, as well
    "First named scriptblock argument is: $firstNamedArgument"
    "Second named scriptblock argument is: $secondNamedArgument"
}

& $scriptBlock -First One -Second 4.5
```

[示例 11-11](#ch11.xhtml#working_with_arguments_in_scripts_functi) 生成以下输出：

```
PS > Get-Arguments First 2
First named argument is: First
Second named argument is: 2
First positional function argument is: One
Second positional function argument is: Two
First named scriptblock argument is: One
Second named scriptblock argument is: 4
```

## 讨论

虽然 PowerShell 同时支持 `param` 关键字和 `$args` 数组，但您通常更倾向于使用 `param` 关键字来定义和访问脚本、函数和脚本块的参数。

### 注意

在大多数语言中，通过 `$args` 数组访问参数最常见的原因是确定当前正在运行的脚本的名称。有关如何在 PowerShell 中执行此操作的信息，请参阅[食谱 16.3](#ch16.xhtml#access_information_about_your_commands_i)。
