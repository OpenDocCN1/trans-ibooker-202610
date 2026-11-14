# 第 24 章 进程

## 24.0 简介

处理系统进程是系统管理的自然组成部分。这也是让系统管理员引以为豪的大多数正则表达式魔法的来源。毕竟，谁不会吹嘘这个用于停止所有使用超过 100 MB 内存的进程的 Unix 单行命令：

```
ps -el | awk '{ if ( $6 > (1024*100)) { print $3 } }' | grep -v PID | xargs kill
```

虽然有用，但它也演示了纯文本处理固有的脆弱性。要使此命令成功，它必须：

- 依赖 `ps` 命令在第 6 列显示内存使用情况
- 依赖 `ps` 命令输出的第 6 列以千字节表示内存使用情况
- 依赖 `ps` 命令输出的第 3 列表示进程 ID
- 从 `ps` 命令的输出中移除标题列

虽然 `ps` 命令具有简化部分工作的参数，但当操作仅生成文本的工具的输出时，这种“祈祷式解析”很常见。

由于 PowerShell 的 `Get-Process` cmdlet 以高度结构化的 .NET 对象形式返回信息，脆弱的文本解析已成为过去：

```
Get-Process | Where-Object { $_.WorkingSet -gt 100mb } | Stop-Process -WhatIf
```

如果简洁很重要，PowerShell 定义了别名以使大多数命令更易于输入：

```
gps | ? WS -gt 100mb | kill -WhatIf
```

除了简单的进程控制外，PowerShell 还提供用于启动进程、自定义其执行环境、等待进程退出等的命令。

## 24.1 列出当前正在运行的进程

### 问题

您想查看系统上正在运行的进程。

### 解决方案

要检索当前正在运行的进程列表，请使用 `Get-Process` cmdlet：

```
PS > Get-Process

Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)     Id ProcessName
-------  ------  -----  -----  -----  ------     -- -----------
    274       6   1328   3940     33           1084 alg
     85       4   3816   6656     57    5.67   3460 AutoHotkey
     50       2   2292   1980     14  384.25   1560 BrmfRsmg
     71       3   2520   4680     35    0.42   2592 cmd
    946       7   3676   6204     32            848 csrss
     84       4    732   2248     22           3144 csrss
     68       4    936   3364     30    0.38   3904 ctfmon
    243       7   3648   9324     48    2.02   2892 Ditto
(...)
```

### 讨论

`Get-Process` cmdlet 检索系统上所有正在运行的进程的信息。因为这些是丰富的 .NET 对象（类型为 `System.Diagnostics.Process`），高级筛选和操作比以往任何时候都更容易。

例如，要查找所有使用超过 100 MB 内存的进程：

```
PS > Get-Process | Where-Object { $_.WorkingSet -gt 100mb }

Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)    Id ProcessName
-------  ------  -----  -----  -----  ------    -- -----------
   1458      29  83468 105824    273  323.80  3992 BigBloatedApp
```

按公司对进程进行分组：

```
PS > Get-Process | Group-Object Company

Count Name                      Group
----- ----                      -----
   39                           {alg, csrss, csrss, dllhost...}
    4                           {AutoHotkey, Ditto, gnuserv, mafwTray}
    1 Brother Industries, Ltd.  {BrmfRsmg}
   19 Microsoft Corporation     {cmd, ctfmon, EXCEL, explorer...}
    1 Free Software Foundation  {emacs}
    1 Microsoft (R) Corporation {FwcMgmt}
(...)
```

或者按开始时间排序（最近的优先）：

```
PS > Get-Process | Sort-Object -Descending StartTime | Select-Object -First 10

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
   1810      39    53616      33964   193   318.02   1452 iTunes
    675       6    41472      50180   146    49.36    296 powershell
   1240      35    48220      58860   316   167.58   4012 OUTLOOK
    305       8     5736       2460   105    21.22   3384 WindowsSearch...
    464       7    29704      30920   153     6.00   3680 powershell
   1458      29    83468     105824   273   324.22   3992 iexplore
    478       6    24620      23688   143    17.83   3548 powershell
    222       8     8532      19084   144    20.69   3924 EXCEL
     14       2      396       1600    15     0.06   2900 logon.scr
    544      18    21336      50216   294   180.72   2660 WINWORD
```



由于 PowerShell 为每个进程返回的丰富信息，这些高级任务变得非常简单。有关 `Get-Process` cmdlet 的更多信息，请输入 `Get-Help Get-Process`。有关 PowerShell 命令中的筛选、分组和排序的更多信息，请参阅 Recipe 2.2。

有关使用 .NET Framework 类的更多信息，请参阅 Recipe 3.8。

## 另请参阅

Recipe 2.2，“在列表或命令输出中筛选项”

Recipe 3.8，“处理 .NET 对象”

# 24.2 启动与文档关联的应用程序

## 问题

你想要启动与文档或其他 Shell 关联相关联的应用程序。

## 解决方案

使用 `Start-Process` cmdlet（或其 `start` 别名）来启动文档或位置：

```
PS > Start-Process https://devblogs.microsoft.com/powershell/
PS > start https://www.bing.com
PS > start c:\temp\output.csv
```

要启动文档的预定义操作之一（通常通过右键菜单公开），请使用 `-Verb` 参数：

```
start c:\documents\MyDoc.docx -Verb Print
```

## 讨论

`Start-Process` cmdlet 让你在启动应用程序方面拥有极大的灵活性。除了启动应用程序外，它还让你能够访问 Windows Shell 关联：与 URL 和文档相关的功能。

Windows 定义了许多 Shell 关联：用于 HTTP 网站、FTP 位置，甚至是资源管理器特定的行为。例如，要启动 Windows 控制面板的“所有任务”视图：

```
start 'shell:::{ED7BA470-8E54-465E-825C-99712043E01C}'
```

如果你要启动的文档定义了操作（如编辑或打印），可以使用 `-Verb` 参数来调用该操作。

有关 `Start-Process` cmdlet 和启动系统进程的更多信息，请参阅 Recipe 24.3。

## 另请参阅

Recipe 24.3，“启动进程”

# 24.3 启动进程

## 问题

你想要在系统上启动一个新进程，但也想要配置其启动环境。

## 解决方案

要启动新进程，请使用 `Start-Process` cmdlet。

```
Start-Process mmc -Verb RunAs -WindowStyle Maximized
```

对于 `Start-Process` cmdlet 未涵盖的高级任务，请调用 `[System.Diagnostics.Process]::Start()` 方法。要控制进程的启动环境，请向其提供一个你准备好的 `System.Diagnostics.ProcessStartInfo` 对象，如 示例 24-1 所示。

### 示例 24-1. 配置新进程的启动环境

```
$processname = "pwsh.exe"

## Prepare to invoke the process
$processStartInfo = New-Object System.Diagnostics.ProcessStartInfo
$processStartInfo.FileName = (Get-Command $processname).Definition
$processStartInfo.WorkingDirectory = (Get-Location).Path
if($argumentList) { $processStartInfo.Arguments = $argumentList }
$processStartInfo.UseShellExecute = $false

## Always redirect the input and output of the process.
## Sometimes we will capture it as binary, other times we will
## just treat it as strings.
$processStartInfo.RedirectStandardOutput = $true
$processStartInfo.RedirectStandardInput = $true

$process = [System.Diagnostics.Process]::Start($processStartInfo)
```

## 讨论

通常，在 PowerShell 中启动进程就像输入程序名称一样简单：

```
notepad c:\temp\test.txt
```

但是，有时你可能需要对进程详细信息进行详细控制，例如其凭据、工作目录、窗口样式等。在这种情况下，请使用 `Start-Process` cmdlet。它通过简单的参数公开了大多数常见配置选项。

**注意**

有关如何以另一用户身份启动进程（或作为提升的 PowerShell 命令）的示例，请参阅 Recipe 18.11。

如果你的需求比 `Start-Process` cmdlet 提供的功能更复杂，可以使用来自 .NET Framework 的 `[System.Diagnostics.Process]::Start()` 方法来提供额外功能。示例 24-1 摘自 Recipe 2.9，展示了此类高级需求的示例。

有关从 PowerShell 启动程序的更多信息，请参阅 Recipe 1.2。有关使用 .NET Framework 类的更多信息，请参阅 Recipe 3.8。

## 另请参阅

Recipe 1.2，“运行程序、脚本和现有工具”

Recipe 3.8，“处理 .NET 对象”

# 24.4 停止进程

## 问题

你想要停止系统上的进程。

## 解决方案

要停止进程，请使用 `Stop-Process` cmdlet，如 示例 24-2 所示。

### 示例 24-2. 使用 Stop-Process cmdlet 停止进程

```
PS > notepad
PS > Get-Process Notepad

Handles  NPM(K)  PM(K)  WS(K)  VM(M)  CPU(s)    Id ProcessName
-------  ------  -----  -----  -----  ------    -- -----------
     42       3   1276   3916     32    0.09  3520 notepad

PS > Stop-Process -ProcessName notepad
PS > Get-Process Notepad
Get-Process : Cannot find a process with the name 'Notepad'. Verify the
process name and call the cmdlet again.
At line:1 char:12
+ Get-Process <<<< Notepad
```

## 讨论

虽然 `Stop-Process` cmdlet 的参数本身很有用，但 PowerShell 的管道模型让你能更精确地操作。`Stop-Process` cmdlet 会停止通过管道传入的任何进程，因此由 `Get-Process` 生成的高级进程集会自动转变为供 `Stop-Process` cmdlet 操作的高级进程集：

```
PS > Get-Process | Where-Object { $_.WorkingSet -lt 10mb } |
    Sort-Object -Descending Name | Stop-Process -WhatIf

What if: Performing operation "Stop-Process" on Target "svchost (1368)".
What if: Performing operation "Stop-Process" on Target "sqlwriter (1772)".
What if: Performing operation "Stop-Process" on Target "qttask (3672)".
What if: Performing operation "Stop-Process" on Target "Ditto (2892)".
What if: Performing operation "Stop-Process" on Target "ctfmon (3904)".
What if: Performing operation "Stop-Process" on Target "csrss (848)".
What if: Performing operation "Stop-Process" on Target "BrmfRsmg (1560)".
What if: Performing operation "Stop-Process" on Target "AutoHotkey (3460)".
What if: Performing operation "Stop-Process" on Target "alg (1084)".
```

**注意**

请注意，此示例在 `Stop-Process` cmdlet 上使用了 `-WhatIf` 标志。此标志让你查看运行命令时会发生什么，但不会实际执行该操作。

停止进程时的另一个常见需求是等待进程退出。大多数脚本通过创建一个循环来处理此需求，该循环仅在 `Get-Process` cmdlet 对相关进程返回无结果时退出。PowerShell 通过提供 `Wait-Process` cmdlet 极大地简化了这一需求，该 cmdlet 让你暂停脚本直到指定进程退出。如果你在等待进程停止时仍希望保留一定的控制权，`-Timeout` 参数可让你控制 PowerShell 等待进程退出的时间。当超时到期时，PowerShell 将控制权返回给你的脚本——让你有机会继续等待、强制终止进程或执行任何其他操作。



# 24.5 获取进程的所有者

## 问题

您想要知道启动给定进程的用户账户。

## 解决方案

使用 `Get-Process` cmdlet 的 `-IncludeUserName` 参数。

```
PS > Get-Process -Name Notepad -IncludeUserName

Handles      WS(K)   CPU(s)     Id UserName               ProcessName
-------      -----   ------     -- --------               -----------
    245      15128     0.08  15084 LEE-DESK\lee           notepad
```

## 讨论

虽然 `Get-Process` 命令返回的输出包含大量信息，但默认情况下它不返回进程的所有者。为此，我们可以使用 `-IncludeUserName` 参数。

然而，有一个主要注意事项。在 Windows 上，查看进程的所有者（当该进程不是您自己的进程时）是一项仅限管理员的功能。因此，如果您尝试以非管理员身份使用此参数，PowerShell 将生成错误。

虽然有其他非管理员方式可以找出启动给定进程的用户账户（例如通过 WMI 或 `tasklist.exe`），但如果您尝试以非管理员身份运行它们，它们都会针对非您自己的进程返回空白信息。

## 另请参阅

配方 28.3，“在 WMI 实例或类上调用方法”

# 24.6 获取进程的父进程

## 问题

您想要知道启动给定进程的进程。

## 解决方案

使用 `Get-Process` cmdlet 返回的进程对象的 `Parent` 属性。

```
PS > $process = Get-Process -Name Notepad
PS > $process.Parent

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    529      31    72196      87928   659     9.86   3376 powershell
```

## 讨论

确定哪个进程启动了给定进程通常比听起来更复杂。

Windows 在进程启动时记录父进程 ID，但无法保证父进程在启动相关进程后没有退出。由于 Windows 会回收进程 ID，此属性有时可能会显示不正确的结果。您可以通过 WMI 的进程管理类亲自验证这一点：

```
PS > $null = Start-Process (Get-Command pwsh).Path -ArgumentList "-Command notepad"
PS > Get-Process -Name notepad

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    251      14     3264      15160       0.06  19952   1 notepad


PS > (Get-CimInstance Win32_Process -Filter "ProcessId = 19952").ParentProcessId
50560
PS > Get-Process -Id 50560
Get-Process : Cannot find a process with the process identifier 50560.
```

在示例的第一行中，PowerShell 启动记事本然后退出。记事本保持运行。我们可以看到系统仍然知道记事本的原始进程 ID，但父进程已退出。

在某个时刻，另一个进程将获得进程 ID 50560，大多数脚本会做出错误反应。正确获取此信息的关键步骤是确保父进程在相关进程之前启动（通过 `StartTime` 属性），但 PowerShell 会为您处理此问题。如果父进程已退出，`Get-Process` 输出的 `Parent` 属性将为空。

如果您仍然需要访问历史父进程 ID，可以直接使用 `Win32_Process` CIM 实例。有关使用 CIM 和 WMI 的更多信息，请参阅 配方 28.1。

## 另请参阅

配方 28.1，“访问 Windows 管理规范和 CIM 数据”

# 24.7 调试进程

## 问题

您想要将调试器附加到系统上正在运行的进程。

## 解决方案

要调试进程，请使用 `Debug-Process` cmdlet。

## 讨论

如果您的计算机上安装了软件调试器（如 Visual Studio 或 Windows 调试工具），`Debug-Process` cmdlet 允许您从 PowerShell 命令行启动调试会话。它并非设计用于在启动后自动化调试工具，但它确实提供了一个有用的快捷方式。

### 注意

要调试 PowerShell 脚本，请参阅 第 14 章。

`Debug-Process` cmdlet 启动系统范围的调试器，该调试器在 `HKLM:\Software\Microsoft\WindowsNT\CurrentVersion\AeDebug` 注册表键中配置。要更改此 cmdlet（以及启动默认调试器的其他工具）启动的调试器，请更改 `Debugger` 属性：

```
PS > Get-Location

Path
----
HKLM:\Software\Microsoft\Windows NT\CurrentVersion\AeDebug


PS > Get-ItemProperty .


PSPath             : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE
                     \Software\Microsoft\Windows NT\CurrentVersion\AeDebug
PSParentPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE
                     \Software\Microsoft\Windows NT\CurrentVersion
PSChildName        : AeDebug
PSDrive            : HKLM
PSProvider         : Microsoft.PowerShell.Core\Registry
UserDebuggerHotKey : 0
Debugger           : "c:\Windows\system32\vsjitdebugger.exe" -p %ld -e %ld
```

有关 `Debug-Process` cmdlet 的更多信息，请输入 `Get-Help Debug-Process`。

## 另请参阅

第 14 章
