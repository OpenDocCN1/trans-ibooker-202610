
```
PS > Get-Process | Get-PropertyValue Name
audiodg
avgamsvr
avgemc
avgrssvc
avgrssvc
avgupsvc
(...)
```

有关 cmdlet 风格关键字的更多信息，请参阅 [Recipe 11.19](#ch11.xhtml#write_pipeline-oriented_scripts_with_cmd)。

## 另请参阅

[Recipe 11.19，“使用 Cmdlet 关键字编写面向管道的脚本”](#ch11.xhtml#write_pipeline-oriented_scripts_with_cmd)

# 11.21 组织脚本以提高可读性

## 问题

你有一个包含辅助函数的长脚本，但这些辅助函数掩盖了脚本的主要意图。

## 解决方案

将脚本的主要逻辑放在一个名为 `Main` 的函数中，并将该函数放在脚本的顶部。在脚本的底部（在所有辅助函数也定义之后），点源 `Main` 函数：

```
## LongScript.ps1

function Main
{
    "Invoking the main logic of the script"
    CallHelperFunction1
    CallHelperFunction2
}

function CallHelperFunction1
{
    "Calling the first helper function"
}

function CallHelperFunction2
{
    "Calling the second helper function"
}

. Main
```

## 讨论

当 PowerShell 调用脚本时，它会从头到尾按顺序执行。就像在控制台中输入命令一样，如果你尝试调用一个尚未定义的函数，PowerShell 会生成错误。

编写包含大量辅助函数的长脚本时，这通常会导致这些辅助函数移至脚本顶部，以便在主逻辑最终执行它们时它们都已定义。因此，在阅读脚本时，你不得不翻阅大量看似无关的辅助函数，才能到达脚本的主要逻辑。

###### 注意

你可能会想知道为什么 PowerShell 要求函数定义及其调用有这种严格的顺序。毕竟，脚本是自包含的，PowerShell 完全可以在调用脚本之前处理所有函数定义。

原因是为了与交互式环境保持一致。将脚本粘贴到控制台窗口是一种常见的诊断或实验技术，在 Visual Studio Code 中高亮脚本部分并选择“运行选择内容”也是如此。如果 PowerShell 在假想的*脚本模式*中做一些特殊处理，这些技术就无法实现。

要解决此问题，可以将主脚本逻辑放在其自己的函数中。名称无关紧要，但 `Main` 是一个传统名称。如果将此函数放在脚本顶部，主逻辑将立即可见。

函数不会自动执行，因此最后一步是调用 `Main` 函数。将此调用放在脚本末尾，可以确保所有必需的辅助函数都已定义。点源此函数可确保它在*脚本作用域*中处理，而不是在通常为其创建的隔离函数作用域中。

有关点源和脚本作用域的更多信息，请参阅 [Recipe 3.6](#ch03.xhtml#control_access_and_scope_of_variables_an)。

## 另请参阅

[Recipe 3.6，“控制变量和其他项的访问与作用域”](#ch03.xhtml#control_access_and_scope_of_variables_an)

# 11.22 调用动态命名的命令

## 问题

你想根据命令名称的*模式*采取行动，而不是根据命令名称本身。

## 解决方案

添加一个 `$executionContext.SessionState.InvokeCommand.CommandNotFoundAction`，它会拦截 PowerShell 的 `CommandNotFound` 错误，并根据未找到的 `CommandName` 采取行动。

[示例 11-20](#ch11.xhtml#add-relativepathcapture.ps1) 通过支持相对路径导航（无需显式调用 `Set-Location`）演示了此技术。

##### 示例 11-20. Add-RelativePathCapture.ps1

```
##############################################################################
##
## Add-RelativePathCapture
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

添加一个新的 CommandNotFound 处理程序，无需显式调用 'Set-Location' 即可捕获相对路径导航

.EXAMPLE

PS C:\Users\Lee\Documents>..
PS C:\Users\Lee>...
PS C:\>

#>

Set-StrictMode -Version 3

$executionContext.SessionState.InvokeCommand.CommandNotFoundAction = {
    param($CommandName, $CommandLookupEventArgs)

    ## 如果命令仅由点组成
    if($CommandName -match '^\.+$')
    {
        ## 关联一个应改为调用的新命令
        $CommandLookupEventArgs.CommandScriptBlock = {

            ## 计算点的数量，并运行 "Set-Location .." 比点数少一次。
            for($counter = 0; $counter -lt $CommandName.Length - 1; $counter++)
            {
                Set-Location ..
            }

        ## 我们调用 GetNewClosure()，以便在新命令中可以使用对 $CommandName 的引用。
        }.GetNewClosure()

        ## 停止命令解析过程。这在 CommandNotFoundAction 中并非严格必需。
        $CommandLookupEventArgs.StopSearch = $true
    }
}
```

## 讨论

PowerShell 支持多种有用的命名命令形式（cmdlet、函数和别名），但你可能会想编写扩展，根据名称的*形式*而不是传递给它的参数来改变其行为。例如，你可能只想通过输入 URL 就自动启动它们，或通过输入相对路径位置来浏览提供程序。

虽然相对路径导航不是 PowerShell 的内置功能，但通过自定义 PowerShell 的 `CommandNotFoundAction` 可以获得一个非常合理的替代方案。有关自定义 PowerShell 命令解析行为的更多信息，请参阅 [Recipe 1.11](#ch01.xhtml#override_command_resolution)。

## 另请参阅

[Recipe 1.11，“自定义 PowerShell 的命令解析行为”](#ch01.xhtml#override_command_resolution)

# 11.23 程序：增强或扩展现有 Cmdlet

虽然 PowerShell 的内置命令很有用，但有时你可能希望它们包含额外的参数或支持其功能的微小更改。这通常是一个困难的提议：除了解析参数并仅传递正确参数的复杂性外，包装命令还应能从 PowerShell 管道的流式特性中受益。

PowerShell 通过结合三个功能显著改善了这一情况：

`*可步进管道*`

: 

给定一个包含单个管道的脚本块，`GetSteppablePipeline()` 方法返回一个 `SteppablePipeline` 对象，使你能够控制管道的 `Begin`、`Process` 和 `End` 阶段。

`*参数展开*`

: 

给定一个包含名称和值的哈希表，PowerShell 允许你将整个哈希表传递给命令。如果你使用 `@` 符号标识哈希表变量名（而不是 `$` 符号），PowerShell 会将哈希表的每个元素视为命令的参数。

`*代理命令 API*`

: 

凭借对可步进管道、展开和参数验证的足够了解，你可以编写自己的函数来有效包装另一个命令。代理命令 API 通过自动生成大量所需的样板脚本，使这变得容易得多。

这三个功能最终实现了强大的命令扩展，但将它们组合在一起仍需要相当多的技术专长。为了简化操作，请使用 `New-CommandWrapper` 脚本（[示例 11-21](#ch11.xhtml#New-CommandWrapper.ps1)）轻松创建包装（并扩展）现有命令的命令。

##### 示例 11-21. New-CommandWrapper.ps1
