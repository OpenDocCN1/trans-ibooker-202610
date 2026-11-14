

## 23.7 创建或删除事件日志

`Remove-EventLog` cmdlet 允许你同时删除事件日志和事件日志源。

### 警告

删除事件日志时请务必小心，因为如果你不小心删错了日志，要重新创建所有事件源是非常困难的。如果你删除了标准事件日志，几乎没有恢复的希望。

若要仅删除事件日志源，请使用 `-Source` 参数：

```
Remove-EventLog -Source PowerShellCookbook
```

若要彻底删除事件日志，请在 `-LogName` 参数中指定日志名称：

```
Remove-EventLog -LogName ScriptEvents
```

创建事件日志后，你可以使用 `Write-EventLog` cmdlet 来操作它。有关写入事件日志的详细信息，请参阅 [配方 23.8](#ch23.xhtml#write_to_an_event_log)。

## 另请参阅

[配方 23.8](#ch23.xhtml#write_to_an_event_log)

# 23.8 写入事件日志

## 问题

你想向事件日志添加一个条目。

## 解决方案

使用 `Write-EventLog` cmdlet 将事件写入事件日志：

```
PS > Write-EventLog -LogName ScriptEvents -Source PowerShellCookbook `
    -EventId 1234 -Message "Hello World"

PS > Get-EventLog ScriptEvents | Select EntryType,Source,InstanceId,Message

         EntryType Source                     InstanceId Message
         --------- ------                     ---------- -------
       Information PowerShellCookbook               1234 Hello World
```

## 讨论

`Write-EventLog` cmdlet 允许你将事件日志消息写入指定的事件日志。要写入事件日志消息，你必须提供有效的日志名称和已注册的事件日志源。如果你需要创建新的事件日志或注册新的事件源，请参阅 [配方 23.7](#ch23.xhtml#create_or_remove_an_event_log)。

除了日志名称和源之外，`Write-EventLog` cmdlet 还需要事件 ID 和消息。在一个事件日志和事件源中，每个事件 ID 应唯一标识正在记录的情况：例如，*登录失败* 或 *磁盘已满*。这使得脚本和其他管理任务能够轻松自动响应系统事件。事件消息应详细说明正在记录的情况（例如，用户名或驱动器号），但不应被用作识别其原因的必要条件。

虽然 PowerShell 还包含 `Write-WinEvent` cmdlet（`WinEvent` cmdlet 几乎是你唯一需要使用的事件日志 cmdlet），但 `Write-WinEvent` cmdlet 仅适用于你已定义并通过复杂的自定义清单注册的事件日志源——对于临时事件来说，这复杂得多，不值得使用。

## 另请参阅

[配方 23.7，“创建或删除事件日志”](#ch23.xhtml#create_or_remove_an_event_log)

# 23.9 为 Windows 事件日志条目运行 PowerShell 脚本

## 问题

当系统生成特定的事件日志条目时，你想运行一个 PowerShell 脚本。

## 解决方案

使用 `schtasks.exe` 工具定义一个对事件日志条目做出反应的新任务。作为其操作，使用禁用配置文件、自定义执行策略、隐藏窗口以及启动脚本的参数来调用 `powershell.exe`：

```
$cred = Get-Credential
$password = $cred.GetNetworkCredential().Password

## 定义任务计划程序在事件发生时应运行的命令
$command = "PowerShell -NoProfile -ExecutionPolicy RemoteSigned " +
    "-WindowStyle Hidden -File 'C:\Program Files\TaskScripts\ScriptEvents.ps1'"

## 创建一个新的计划任务
SCHTASKS /Create /TN "ScriptEvents Monitor" /TR $command /SC ONEVENT `
         /RL Highest /RU $cred.Username /RP $password `
         /EC ScriptEvents /MO *[System/EventID=1010]
```

## 讨论

Windows 事件日志允许你定义在生成事件时启动的自定义操作。虽然你可以使用 UI 来创建这些任务和筛选器，但 `schtasks.exe` 工具让你能从自动化友好的命令行创建所有这些内容。

作为实际操作的一个示例，想象一下试图在发生问题事件时捕获系统上正在运行的进程。该脚本可能如下所示：

```
$logTag = "{0:yyyyMMdd_HHmm}" -f (Get-Date)
$logPath = 'C:\Program Files\TaskScripts\ScriptEvents-{0}.txt' -f $logTag

Start-Transcript -Path $logPath

Get-CimInstance Win32_OperatingSystem | Format-List | Out-String
Get-Process | Format-Table | Out-String

Stop-Transcript
```

生成事件后，我们可以看到日志刚刚创建片刻后就出现了：

```
PS > dir

    Directory: C:\Program Files\TaskScripts

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         2/21/2020   8:38 PM        278 ScriptEvents.ps1

PS > Write-EventLog -LogName ScriptEvents -Source PowerShellCookbook `
    -EventId 1010 -Message "Hello World"

PS > dir

    Directory: C:\Program Files\TaskScripts

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         2/21/2020   9:50 PM      12766 ScriptEvents-20200221_2150.txt
-a---         2/21/2020   8:38 PM        278 ScriptEvents.ps1
```

定义任务时，我们使用 `/TN` 参数为任务定义名称。作为命令（由 `/TR` 参数指定），我们告诉 Windows 启动 `powershell.exe` 并使用多个参数来自定义其环境。我们使用 `/RL` 参数确保任务以提升的权限运行（因为它写入 `Program Files` 目录）。要定义实际的事件日志筛选器，我们使用 `/EC` 参数定义事件通道——在本例中为 `ScriptEvents` 日志。在 `/MO`（“修饰符”）参数中，我们指定匹配我们关心的事件所需的 XPath 筛选器。在本例中，我们搜索 EventId 1010。`System/` 前缀并不告诉 Windows 搜索 `System` 事件日志；它告诉 Windows 查看标准系统属性：`EventID`、`Level`、`Task`、`Keywords`、`Computer` 等。

有关事件查看器 XPath 语法的详细信息，请参阅 [配方 23.4](#ch23.xhtml#retrieve_a_specific_event_log_entry)。

## 另请参阅

[配方 1.17，“从 PowerShell 外部调用 PowerShell 命令或脚本”](#ch01.xhtml#invoke_a_powershell_script_from_outside_)

[配方 23.4，“检索和筛选事件日志条目”](#ch23.xhtml#retrieve_a_specific_event_log_entry)

# 23.10 清除或维护事件日志

## 问题

你想清除事件日志或管理其保留策略。

## 解决方案

要清除事件日志，请使用 `wevtutil.exe` 应用程序。例如，要清除 Microsoft Office 警报日志：

```
wevtutil cl 'OAlerts'
```

同样，要配置日志属性（例如增加保留限制）：

```
wevtutil sl Microsoft-Windows-PowerShell/Operational /ms:$(200mb)
```

## 讨论

大多数事件日志的默认策略在很大程度上是合理的。但是，你可能想要修改的事件日志是你的安全事件日志：操作系统默认的几十兆字节在发生安全事件时不太可能对你有帮助。

PowerShell 包含用于管理较旧的经典事件日志（应用程序、安全性等）的 cmdlet，但你需要对现代事件日志使用 `wevtutil.exe`。

要永久更改经典事件日志的策略，请使用 `Limit-EventLog` cmdlet。此 cmdlet 允许你限制应用它的事件日志的日志大小、最大事件期限和覆盖行为。虽然大小和期限限制是相当不言自明的参数，但配置溢出行为则更微妙。

`-OverflowAction` 参数支持三个选项之一。每个选项描述了 Windows 在写入已满的事件日志时采取的不同策略：

`DoNotOverwrite`

: 丢弃新条目。

`OverwriteAsNeeded`

: 覆盖最旧的条目。

`OverwriteOlder`

: 覆盖早于为事件日志指定的期限限制的条目（通过 `RetentionDays` 参数）。如果没有旧条目可供覆盖，Windows 将丢弃新条目。

