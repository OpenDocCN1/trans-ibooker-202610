
# Watch-Command

源自 *PowerShell Cookbook*（O'Reilly 出版），作者 Lee Holmes (http://www.leeholmes.com/guide)

```powershell
<#

.SYNOPSIS

监视命令调用的结果，当输出与指定字符串匹配、缺少指定字符串或仅仅是发生变化时发出警报。

.EXAMPLE

PS > Watch-Command { Get-Process -Name Notepad | Measure } -UntilChanged
监视 Notepad 进程，直到启动或停止一个进程。

.EXAMPLE

PS > Watch-Command { Get-Process -Name Notepad | Measure } -Until "Count    : 1"
监视 Notepad 进程，直到恰好有一个打开。

.EXAMPLE

PS > Watch-Command {
     Get-Process -Name Notepad | Measure } -While 'Count    : \d\s*\n'
监视 Notepad 进程，当打开的数量在 0 到 9 之间时持续监视
（冒号后的数字）。

#>

[CmdletBinding(DefaultParameterSetName = "Forever")]
param(
    ## 要在监视时调用的脚本块
    [Parameter(Mandatory = $true, Position = 0)]
    [ScriptBlock] $ScriptBlock,

    ## 监视尝试之间的延迟（以秒为单位）
    [Parameter()]
    [Double] $DelaySeconds = 1,

    ## 指定不播放警报声音
    [Parameter()]
    [Switch] $Quiet,

    ## 仅当命令输出保持不变时才继续监视。
    [Parameter(ParameterSetName = "UntilChanged", Mandatory = $false)]
    [Switch] $UntilChanged,

    ## 要搜索的正则表达式。监视将持续到找到此表达式为止。
    [Parameter(ParameterSetName = "Until", Mandatory = $false)]
    [String] $Until,

    ## 要搜索的正则表达式。监视将持续到未找到此表达式为止。
    [Parameter(ParameterSetName = "While", Mandatory = $false)]
    [String] $While
)

Set-StrictMode -Version 3

$initialOutput = ""
$lastCursorTop = 0
Clear-Host

## 开始持续循环
while($true)
{
    ## 运行提供的脚本块
    $r = & $ScriptBlock

    ## 清屏并显示结果
    $buffer = $ScriptBlock.ToString().Trim() + "`r`n"
    $buffer += "`r`n"
    $textOutput = $r | Out-String
    $buffer += $textOutput

    [Console]::SetCursorPosition(0, 0)
    [Console]::Write($buffer)

    $currentCursorTop = [Console]::CursorTop
    $linesToClear = $lastCursorTop - $currentCursorTop
    if($linesToClear -gt 0)
    {
        [Console]::Write((" " * [Console]::WindowWidth * $linesToClear))
    }

    $lastCursorTop = [Console]::CursorTop
    [Console]::SetCursorPosition(0, 0)

    ## 如果尚未存储初始输出，则记住它
    if(-not $initialOutput)
    {
        $initialOutput = $textOutput
    }

    ## 如果只是查找任何变化，
    ## 请检查文本是否已更改。
    if($UntilChanged)
    {
        if($initialOutput -ne $textOutput)
        {
            break
        }
    }

    ## 如果需要确保找到某些文本，
    ## 如果未找到则中断。
    if($While)
    {
        if($textOutput -notmatch $While)
        {
            break
        }
    }

    ## 如果需要等待找到某些文本，
    ## 如果找到则中断。
    if($Until)
    {
        if($textOutput -match $Until)
        {
            break
        }
    }

    ## 延迟
    Start-Sleep -Seconds $DelaySeconds
}

## 通知用户
if(-not $Quiet)
{
    [Console]::Beep(1000, 1000)
}
```

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

## 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

# 1.8 在作业完成时通知自己

## 问题

你希望在长时间运行的作业完成时收到通知。

## 解决方案

使用 [食谱 31.3](#ch31.xhtml#create-temporary-event-subscription) 中提供的 `Register-TemporaryEvent` 命令来注册事件的 `StateChanged` 事件：

```powershell
PS > $job = Start-Job -Name TenSecondSleep { Start-Sleep 10 }
PS > Register-TemporaryEvent $job StateChanged -Action {
     [Console]::Beep(100,100)
     Write-Host "Job #$($sender.Id) ($($sender.Name)) complete."
}

PS > Job #6 (TenSecondSleep) complete.
PS >
```

## 讨论

当作业完成时，它会引发 `StateChanged` 事件以通知订阅者其状态已更改。我们可以使用 PowerShell 的事件处理 cmdlet 来注册关于此事件的通知，但它们不适合这种一次性事件处理。为了解决这个问题，我们使用 [食谱 31.3](#ch31.xhtml#create-temporary-event-subscription) 中提供的 `Register-TemporaryEvent` 命令。

在解决方案中的示例操作块里，我们只是发出一声蜂鸣并写入一条消息，表明作业已完成。

作为另一种选择，你也可以更新 `prompt` 函数，以突出显示那些已完成但仍有未处理输出的作业：

```powershell
$psJobs = @(Get-Job -State Completed | ? { $_.HasMoreData })
if($psJobs.Count -gt 0) {
    ($psJobs | Out-String).Trim() | Write-Host -Fore Yellow }
```

有关事件和这种自动事件处理的更多信息，请参阅 [第 31 章](#ch31.xhtml#event_handling)。

## 另请参阅

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)
- [第 31 章](#ch31.xhtml#event_handling)

# 1.9 自定义 Shell、配置文件和提示符

## 问题

你想要通过个性化的提示符、别名等来自定义 PowerShell 的交互体验。

## 解决方案

当你想要自定义 PowerShell 的各个方面时，请将这些自定义内容放在个人配置文件脚本中。PowerShell 通过将配置文件的位置存储在 `$profile` 变量中，提供了对该配置文件脚本的便捷访问。

> **注意**
>
> 默认情况下，PowerShell 的安全策略会阻止脚本（包括配置文件）运行。不过，一旦你开始编写脚本，就应该将此策略配置为限制较少的设置。有关如何配置执行策略的信息，请参阅 [食谱 18.1](#ch18.xhtml#enable_scripting_through_an_execution_po)。

要创建新的配置文件（如果已存在则覆盖）：

```powershell
New-Item -type file -force $profile
```

要编辑配置文件（如果已安装 Visual Studio Code，则在其中编辑）：

```powershell
code $profile
```

要查看配置文件：

```powershell
Get-ChildItem $profile
```

创建配置文件脚本后，你可以添加一个名为 `prompt` 的函数来返回字符串。PowerShell 将此函数的输出显示为命令行提示符：

```powershell
function prompt
{
    "PS [$env:COMPUTERNAME] >"
}
```

此示例提示符显示计算机名称，外观类似 `PS [LEE-DESK] >`。

你可能还会发现向配置文件添加别名很有帮助。别名让你可以用自己选择的名称来引用常用命令。个人配置文件脚本让你能够自动定义别名、函数、变量，或任何你可能从 PowerShell 提示符交互式设置的其他自定义项。别名是最常见的自定义项之一，因为它们让你可以用更容易输入的名称来引用 PowerShell 命令（以及你自己的脚本）。

> **注意**
>
> 如果你想为命令定义别名，但也需要修改该命令的参数，则应改为定义函数。有关更多信息，请参阅 [食谱 11.14](#ch11.xhtml#dynamically_compose_command_parameters)。

例如：

```powershell
Set-Alias new New-Object
Set-Alias browse 'C:\Users\lee\AppData\Local\Microsoft\*\MicrosoftEdge.exe'
```

保存配置文件并重启 PowerShell 后，更改将生效。或者，你可以运行此命令立即重新加载配置文件：

```powershell
. $profile
```

函数也是非常常见的自定义项，其中最流行的是 `prompt` 函数。

## 讨论
