
# 使用 `New-Event` 适配复杂事件

`New-Event` cmdlet 的一个主要用途是适配通过通用 WMI 和 .NET 事件 cmdlet 暴露的复杂事件。通过编写面向任务的命令来呈现这些适配后的数据，你可以提供并处理更易于使用的数据。

要实现此目标，请使用 `Register-ObjectEvent` 或 `Register-CimIndicationEvent` cmdlet 来注册其事件之一。在 `-Action` 脚本块中，使用 `New-Event` cmdlet 生成一个新的、更专用的事件。

在此场景中，直接与 .NET 或 WMI 交互的事件注册仅仅是“支持”事件，用户在使用 `Get-EventSubscriber` cmdlet 时不会期望看到它们。为了默认隐藏这些事件注册，`Register-ObjectEvent` 和 `Register-CimIndicationEvent` cmdlet 都提供了 `-SupportEvent` 参数。

## 示例：进程创建事件

以下是两个函数的示例，当新进程启动时它们会通知你：

### `Enable-ProcessCreationEvent` 函数

```
## Enable process creation events
function Enable-ProcessCreationEvent
{
    $identifier = "WMI.ProcessCreated"
    $query = "SELECT * FROM __instancecreationevent " +
                 "WITHIN 5 " +
                 "WHERE targetinstance isa 'win32_process'"
    Register-CimIndicationEvent -Query $query -SourceIdentifier $identifier `
        -SupportEvent -Action {
            [void] (New-Event "PowerShell.ProcessCreated" `
                -Sender $sender `
                -EventArguments $EventArgs.NewEvent.TargetInstance)
        }
}
```

### `Disable-ProcessCreationEvent` 函数

```
## Disable process creation events
function Disable-ProcessCreationEvent
{
   Unregister-Event -Force -SourceIdentifier "WMI.ProcessCreated"
}
```

## 在 Shell 中使用这些函数

在 Shell 中使用时，体验比直接处理 WMI 事件简单得多：

```
PS > Enable-ProcessCreationEvent
PS > calc
PS > Get-Event

ComputerName     :
RunspaceId       : feeda302-4386-4360-81d9-f5455d74950f
EventIdentifier  : 2
Sender           : System.Management.ManagementEventWatcher
SourceEventArgs  :
SourceArgs       : {calc.exe}
SourceIdentifier : PowerShell.ProcessCreated
TimeGenerated    : 2/21/2010 3:15:57 PM
MessageData      :

PS > (Get-Event).SourceArgs

(...)
Caption                    : calc.exe
CommandLine                : "C:\Windows\system32\calc.exe"
CreationClassName          : Win32_Process
CreationDate               : 20100221151553.574124-480
CSCreationClassName        : Win32_ComputerSystem
CSName                     : LEEHOLMES1C23
Description                : calc.exe
ExecutablePath             : C:\Windows\system32\calc.exe
(...)

PS > Disable-ProcessCreationEvent
PS > notepad
PS > Get-Event

ComputerName     :
RunspaceId       : feeda302-4386-4360-81d9-f5455d74950f
EventIdentifier  : 2
Sender           : System.Management.ManagementEventWatcher
SourceEventArgs  :
SourceArgs       : {calc.exe}
SourceIdentifier : PowerShell.ProcessCreated
TimeGenerated    : 2/21/2010 3:15:57 PM
MessageData      :
```

## 引擎事件

除了你创建的事件外，*引擎事件* 也表示由引擎本身生成的事件。PowerShell 支持三种此类事件：`PowerShell.Exiting` 允许你在 PowerShell 会话退出时执行一些工作，`PowerShell.OnIdle` 允许你协调 PowerShell 会话中的活动，`PowerShell.OnScriptBlockInvoke` 允许你在脚本块被调用前对其进行处理。

有关使用 `PowerShell.Exiting` 事件的示例，请参阅 [食谱 1.31](#ch01.xhtml#save-state-exiting)。

PowerShell 对待引擎事件的方式与其他任何类型的事件一样。你可以使用 `Register-EngineEvent` cmdlet 自动响应这些事件，就像你可以使用 `Register-ObjectEvent` 和 `Register-CimIndicationEvent` cmdlet 分别响应 .NET 和 WMI 事件一样。有关如何自动响应事件的信息，请参阅 [食谱 31.1](#ch31.xhtml#respond_generated_events)。

## 另请参阅

[食谱 1.31，“在会话间保存状态”](#ch01.xhtml#save-state-exiting)

[食谱 31.1，“响应自动生成的事件”](#ch31.xhtml#respond_generated_events)

# 31.3 创建临时事件订阅

## 问题

你希望在事件到达时自动执行某个操作，但在该事件触发一次后自动移除事件订阅。

## 解决方案

使用事件注册命令的 `-MaxTriggerCount` 参数将 PowerShell 触发事件的次数限制为一次：

```
PS > $timer = New-Object Timers.Timer
PS > $job = Register-ObjectEvent $timer Disposed -Action {
     [Console]::Beep(100,100) } -MaxTriggerCount 1
PS > Get-EventSubscriber
PS > $timer.Dispose()
PS > Get-EventSubscriber
PS > Remove-Job $job
```

## 讨论

当你为 `Register-ObjectEvent` 的 `-Action` 参数提供脚本块时，PowerShell 会创建一个事件订阅者来表示该订阅，同时也会创建一个作业，让你能够与该操作的环境和结果进行交互。如果事件注册真的是一种“一次性”注册，即事件生成后你不再需要它，那么事后清理可能会很复杂。

幸运的是，PowerShell 支持 `-MaxTriggerCount` 参数，允许你配置事件触发的次数上限。一旦事件订阅达到该限制，PowerShell 会自动注销该事件订阅者。

在处理临时事件订阅时，如果你的事件订阅定义了 `-Action` 脚本块，解决方案演示了一个额外的步骤。PowerShell 不会自动移除与该操作关联的作业，因为它可能保存着重要的结果。如果你不需要这些结果，请务必同时调用 `Remove-Job` cmdlet。

有关结合这两个步骤的脚本，请参阅本书示例中包含的 `Register-TemporaryEvent` 脚本。

## 另请参阅

[食谱 31.1，“响应自动生成的事件”](#ch31.xhtml#respond_generated_events)

# 31.4 从远程计算机转发事件

## 问题

你有一个通过 PowerShell 远程处理连接到远程计算机的客户端，并且你希望在该计算机上发生事件时收到通知。

## 解决方案

使用 PowerShell 的任意事件注册 cmdlet 订阅远程计算机上的事件。然后，使用 `-Forward` 参数告诉 PowerShell 在事件到达时转发这些事件：

```
PS > Get-Event
PS > $session = New-PSSession leeholmes1c23
PS > Enter-PSSession $session

[leeholmes1c23]: PS C:\> $timer = New-Object Timers.Timer
[leeholmes1c23]: PS C:\> $timer.Interval = 1000
[leeholmes1c23]: PS C:\> $timer.AutoReset = $false
[leeholmes1c23]: PS C:\> Register-ObjectEvent $timer Elapsed `
    -SourceIdentifier Timer.Elapsed -Forward
[leeholmes1c23]: PS C:\> $timer.Enabled = $true
[leeholmes1c23]: PS C:\> Exit-PSSession

PS > Get-Event

ComputerName     : leeholmes1c23
RunspaceId       : 053e6232-528a-4626-9b86-c50b8b762440
EventIdentifier  : 1
Sender           : System.Timers.Timer
SourceEventArgs  : System.Management.Automation.ForwardedEventArgs
SourceArgs       : {System.Timers.Timer, System.Timers.ElapsedEventArgs}
SourceIdentifier : Timer.Elapsed
TimeGenerated    : 2/21/2010 11:01:54 PM
MessageData      :
```

## 讨论

PowerShell 的事件基础设施允许你在注册事件时定义三种可能的操作之一：

*   将事件通知添加到事件队列。
*   使用 `-Action` 脚本块自动处理事件通知。
*   将事件通知转发到客户端计算机。

所有事件注册 cmdlet 上的 `-Forward` 参数启用了第三种选项。当你连接到一台在事件注册上启用了此类行为的远程计算机时，PowerShell 会自动将那些事件通知转发到你的客户端计算机。使用此技术，你可以轻松监视多台远程计算机上你感兴趣的系统更改。
