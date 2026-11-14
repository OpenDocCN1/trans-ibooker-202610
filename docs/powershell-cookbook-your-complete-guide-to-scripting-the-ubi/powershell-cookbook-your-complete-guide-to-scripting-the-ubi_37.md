# 第 31 章 事件处理

## 31.0 简介

系统管理的很大一部分是反应性的：在系统服务关闭时、文件创建或删除时、Windows 注册表发生更改时，甚至在定时间隔到达时采取某些操作。

响应系统更改的最简单方法就是简单地*轮询*它们。如果你在等待文件创建，只需偶尔检查一次直到它出现。如果你在等待进程启动，只需不断调用 `Get-Process` cmdlet 直到它出现。

这种方法对于某些事件（如等待进程来或去）是可以接受的，但当你需要监视系统的大部分内容（如整个注册表或文件系统）时，它很快就会失效。

作为轮询系统更改的替代方案，许多技术支持自动通知——称为*事件*。当应用程序注册这些自动通知时，它可以在事件发生时立即响应，而不必轮询它们。

不幸的是，每种技术都提供自己的事件通知方法：.NET 定义了一种方法，WMI 定义了另一种。当你有一个想要生成自己事件的脚本时，这两种技术都不提供选项。

PowerShell 通过引入一组单一、一致的事件相关 cmdlet 来解决这种复杂性。这些 cmdlet 让你能够处理所有这些不同的事件源。当事件发生时，你可以让 PowerShell 在其事件队列中为你存储通知，或使用 `Action` 脚本块自动处理它。





# 31.1 响应自动生成的事件

## 问题

您想要自动响应 .NET、WMI 或引擎事件。

## 解决方案

使用 `Register-ObjectEvent`、`Register-CimIndicationEvent` 和 `Register-EngineEvent` cmdlet 的 `-Action` 参数，以便在事件到达时收到通知，并让 PowerShell 调用您提供的脚本块：

```
PS > $timer = New-Object Timers.Timer
PS > $timer.Interval = 1000
PS > Register-ObjectEvent $timer Elapsed -SourceIdentifier Timer.Elapsed `
    -Action { $GLOBAL:lastRandom = Get-Random }

Id              Name            State      HasMoreData     Location
--              ----            -----      -----------     --------
2               Timer.Elapsed   NotStarted False

PS > $timer.Enabled = $true
PS > $lastRandom
836077209
PS > $lastRandom
2030675971
PS > $lastRandom
1617766254
PS > Unregister-Event Timer.Elapsed
```

## 讨论

PowerShell 的事件注册 cmdlet 为您提供了一种一致的方式，来与许多不同的事件技术进行交互：.NET 事件、WMI 事件和 PowerShell 引擎事件。

默认情况下，当您注册一个事件时，PowerShell 会向称为 *event queue*（事件队列）的会话范围事件存储库添加一个新条目。您可以使用 `Get-Event` cmdlet 查看添加到此队列的事件，并使用 `Remove-Event` cmdlet 从此队列中移除事件。

除了支持手动处理事件外，您还可以将脚本块提供给事件注册 cmdlet 的 `-Action` 参数。当您向 `-Action` 参数提供脚本块时，PowerShell 会在事件到达时自动处理它们。

然而，同时做两件事意味着多线程。而多线程？风险极大！为了避免您处理多线程问题，PowerShell 严格控制这些脚本块的执行。当需要处理操作时，它会挂起当前的脚本或管道，执行该操作，然后从中断处继续。它一次只处理一个操作。

```
PS > $timer = New-Object Timers.Timer
PS > $timer.Interval = 1000
PS > Register-ObjectEvent $timer Elapsed -SourceIdentifier Timer.Elapsed `
    -Action { Write-Host "Processing event" }
$timer.Enabled = $true

PS > while($true) { Write-Host "Processing loop"; Sleep 1 }
Processing loop
Processing event
Processing loop
Processing event
Processing loop
Processing event
Processing loop
Processing event
Processing loop
(...)
```

在 `-Action` 脚本块内部，PowerShell 为您的脚本提供了对五个自动变量的访问权限：

`$eventSubscriber`
: 生成此事件的订阅者（事件注册）。

`$event`
: 事件本身的详细信息：`MessageData`、`TimeGenerated` 等。

`$args`
: 事件处理程序的参数和自变量。大多数事件将事件发送者和自定义事件信息作为前两个参数，但这取决于事件处理程序。

`$sender`
: 引发事件的对象（如果有）。

`$eventArgs`
: 事件定义的自定义事件信息（如果有）。例如，`Timers.Timer` 对象为此参数提供一个 `TimerElapsedEventArgs` 对象。该对象包含一个 `SignalTime` 参数，用于标识计时器触发的确切时间。同样，WMI 事件定义的对象将大部分信息放在 `$eventArgs.NewEvent` 属性中。

除了您提供给 `-Action` 参数的脚本块外，您还可以在事件注册期间将任何您想要的对象提供给 `-MessageData` 参数。PowerShell 将此数据与为此事件注册生成的任何事件通知关联起来。

为了防止您的脚本块意外破坏它所中断的脚本的状态，PowerShell 将其置于一个高度隔离的环境中。主要地，PowerShell 通过其作业基础设施为您提供对事件操作的访问。与其他 PowerShell 作业一样，您可以使用 `Receive-Job` cmdlet 检索事件操作生成的任何输出：

```
PS > $timer = New-Object Timers.Timer
PS > $timer.Interval = 1000
PS > Register-ObjectEvent $timer Elapsed -SourceIdentifier Timer.Elapsed `
    -Action {
        $SCRIPT:triggerCount = 1 + $SCRIPT:triggerCount
        "Processing Event $triggerCount"
    }
PS > $timer.Enabled = $true

Id              Name            State      HasMoreData     Location
--              ----            -----      -----------     --------
1               Timer.Elapsed   Running    True

PS > Get-Job 1
Id              Name            State      HasMoreData     Location
--              ----            -----      -----------     --------
1               Timer.Elapsed   Running    True

PS > Receive-Job 1
Processing Event 1
Processing Event 2
Processing Event 3
(...)
```

有关使用 PowerShell 作业的更多信息，请参阅 食谱 1.6。

除了通过作业接口公开您的事件操作外，PowerShell 还使用模块来确保您的 `-Action` 脚本块不受系统上运行的其他脚本的影响（也不影响它们）。与所有模块一样，`$GLOBAL` 变量由整个会话共享。`$SCRIPT` 变量由脚本块的所有调用共享并持久化。所有其他变量仅在当前事件操作触发期间持久化。有关 PowerShell 模块的更多信息，请参阅 食谱 11.7。

有关有用的 .NET 和 WMI 事件的更多信息，请参阅 附录 I。

## 另请参阅

- 食谱 1.6，“调用长时间运行或后台命令”
- 食谱 11.7，“编写维护状态的命令”
- 附录 I，《选定事件及其用途*》

# 31.2 创建和响应自定义事件

## 问题

您想要为其他脚本创建新事件以供使用，或者希望在事件发生时自动响应。

## 解决方案

使用 `New-Event` cmdlet 生成自定义事件。使用 `Register-EngineEvent` cmdlet 的 `-Action` 参数自动响应该事件。

```
PS > Register-EngineEvent -SourceIdentifier Custom.Event `
    -Action { Write-Host "Received Event" }

PS > $null = New-Event Custom.Event
Received Event
```

## 讨论

`New-Event` cmdlet 允许您为其他脚本或事件注册创建新的自定义事件。当您调用 `New-Event` cmdlet 时，PowerShell 会向称为 *event queue*（事件队列）的会话范围事件存储库添加一个新条目。您可以使用 `Get-Event` cmdlet 查看添加到此队列的事件，或者使用 `Register-EngineEvent` cmdlet 让 PowerShell 自动响应。




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

有关使用 `PowerShell.Exiting` 事件的示例，请参阅 食谱 1.31。

PowerShell 对待引擎事件的方式与其他任何类型的事件一样。你可以使用 `Register-EngineEvent` cmdlet 自动响应这些事件，就像你可以使用 `Register-ObjectEvent` 和 `Register-CimIndicationEvent` cmdlet 分别响应 .NET 和 WMI 事件一样。有关如何自动响应事件的信息，请参阅 食谱 31.1。

## 另请参阅

食谱 1.31，“在会话间保存状态”

食谱 31.1，“响应自动生成的事件”

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

食谱 31.1，“响应自动生成的事件”

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



有关注册事件的详细信息，请参阅配方 31.1。有关 PowerShell 远程处理的详细信息，请参阅第 29 章。

## 另请参阅

配方 31.1，“响应自动生成的事件”

第 29 章

## 31.5 调查内部事件操作状态

### 问题

你想要调查事件订阅者操作的内部环境或状态。

### 解决方案

检索事件订阅器，然后与 `Subscriber.Action` 属性交互：

```
PS > $null = Register-EngineEvent -SourceIdentifier Custom.Event `
    -Action {
        "Hello World"

        Write-Error "Got an Error"

        $SCRIPT:privateVariable = 10
    }

PS > $null = New-Event Custom.Event
PS > $subscriber = Get-EventSubscriber Custom.Event
PS > $subscriber.Action | Format-List

Module        : __DynamicModule_f2b39042-e89a-49b1-b460-6211b9895acc
StatusMessage :
HasMoreData   : True
Location      :
Command       :
                        "Hello World"
                        Write-Error "Got an Error"
                        $SCRIPT:privateVariable = 10

JobStateInfo  : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : b3fcceae-d878-4c8b-a53e-01873f2cfbea
Id            : 1
Name          : Custom.Event
ChildJobs     : {}
Output        : {Hello World}
Error         : {Got an Error}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
State         : Running

PS > $subscriber.Action.Error

Write-Error:
Line |
   2 |      -Action {
     |              ~
     | Got an Error
```

### 讨论

当你向任何事件注册 cmdlet 提供 `-Action` 脚本块时，PowerShell 会创建一个 PowerShell 作业，让你与该操作进行交互。在与该作业交互时，你可以访问作业的输出、错误、进度、详细输出、调试输出和警告。

有关使用 PowerShell 作业的详细信息，请参阅配方 1.6。

除了作业接口外，PowerShell 的事件系统还会生成一个模块，将你的脚本块与系统的其余部分隔离开来——这对你和系统都有好处。

当你想要调查操作的内部状态时，PowerShell 会通过操作的 `Module` 属性呈现此状态。通过将模块传递给调用运算符，你可以从该模块内部调用命令：

```
PS > $module = $subscriber.Action.Module
PS > & $module { dir variable:\privateVariable }

Name                           Value
----                           -----
privateVariable                10
```

为了使此操作更加容易，你可以使用配方 11.9 提供的 `Enter-Module` 脚本。

### 另请参阅

配方 1.6，“调用长时间运行或后台命令”

配方 11.9，“诊断并与内部模块状态交互”

配方 31.1，“响应自动生成的事件”

## 31.6 将脚本块用作 .NET 委托或事件处理程序

### 问题

你想要使用 PowerShell 脚本块直接处理 .NET 事件或委托。

### 解决方案

对于支持 .NET 委托的对象，只需将脚本块分配给该委托：

```
$replacer = {
    param($match)

    $chars = $match.Groups[0].Value.ToCharArray()
    [Array]::Reverse($chars)
    $chars -join ''
}

PS > $regex = [Regex] "\w+"
PS > $regex.Replace("Hello World", $replacer)
olleH dlroW
```

要让脚本块直接处理 .NET 事件，请调用该对象的 `Add_Event()` 方法：

```
$form.Add_Shown( { $form.Activate(); $textbox.Focus() } )
```

### 讨论

在使用某些 .NET 开发人员 API 时，你可能会遇到接受委托作为其参数之一的方法。.NET 中的委托充当一种向接受它们的 .NET 方法提供自定义逻辑的方式。例如，解决方案向正则表达式 `Replace()` 方法提供了一个自定义委托，以反转匹配项中的字符——这是正则表达式完全不支持的功能。

作为另一个示例，许多数组类支持用于搜索、排序、筛选等的自定义委托。在本示例中，我们创建一个自定义排序器，按元素长度对数组进行排序：

```
PS > $list = New-Object System.Collections.Generic.List[String]
PS > $list.Add("1")
PS > $list.Add("22")
PS > $list.Add("3333")
PS > $list.Add("444")
PS > $list.Add("5")
PS > $list.Sort( { $args[0].Length - $args[1].Length } )
PS > $list
5
1
22
444
3333
```

也许按字符计算最有用的委托是自定义 .NET Framework 在 Web 网络连接中遇到无效证书时的行为的能力。例如，当你尝试连接到具有过期 SSL 证书的网站时，就会发生这种情况。.NET Framework 允许你通过向 `System.Net.ServicePointManager` 类中的 `ServerCertificateValidationCallback` 属性提供委托来重写此行为。如果应接受证书，你的委托应返回 `$true`，否则返回 `$false`。要在开发会话期间接受所有证书，只需运行以下语句：

```
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = { $true }
```

除了委托外，你还可以将 PowerShell 脚本块直接分配给 .NET 对象上的事件。

通常，你会想要使用 PowerShell 事件处理来支持此场景。PowerShell 事件处理提供了一组非常丰富的 cmdlet，让你能够与来自许多技术的事件进行交互：.NET、WMI 和 PowerShell 引擎本身。当你使用 PowerShell 事件处理来处理 .NET 事件时，PowerShell 会保护你免受多个脚本块同时运行的危险，并防止它们干扰 PowerShell 会话的其余部分。

但是，当你编写一个使用事件来处理 WinForms 应用程序中事件的独立脚本时，直接将脚本块分配给这些事件可以提供更轻量级的开发体验。有关此方法的示例，请参阅配方 13.10。

有关 PowerShell 事件处理的详细信息，请参阅配方 31.1。

### 另请参阅

配方 13.10，“为脚本添加图形用户界面”

配方 31.1，“响应自动生成的事件”
