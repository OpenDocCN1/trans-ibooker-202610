# 第 2 章 管道

## 2.0 简介

Shell 中的基本概念之一称为 *管道*。它也是 PowerShell 最重要的进步之一的基础。管道是一个简单概念的大名——一系列命令，其中一个命令的输出成为下一个命令的输入。Shell 中的管道很像工厂里的装配线：它在各个阶段之间传递时逐步优化某些内容，如 示例 2-1 所示。

### 示例 2-1. PowerShell 管道

```powershell
Get-Process | Where-Object WorkingSet -gt 500kb | Sort-Object -Descending Name
```

在 PowerShell 中，你可以使用管道符 (`|`) 来分隔管道中的每个阶段。

在 示例 2-1 中，`Get-Process` cmdlet 生成代表系统上实际进程的对象。这些进程对象包含有关进程名称、内存使用量、进程 ID 等信息。当 `Get-Process` cmdlet 生成输出时，它会将输出传递下去。同时，`Where-Object` cmdlet 直接处理这些进程，轻松测试出内存使用量超过 500 KB 的进程。它在处理时立即将这些进程传递下去，使 `Sort-Object` cmdlet 也能直接处理这些进程，并按名称降序排序。

这个简短的例子说明了管道功能的重大进步：PowerShell 沿管道传递高保真对象，而不是它们的文本表示形式。

相比之下，所有其他 Shell 都在各阶段之间传递纯文本数据。从纯文本输出中提取有意义的信息，使得编写管道变成了一种黑魔法。在传统的基于 Unix 的 Shell 中表达前面的示例非常困难，在 `cmd.exe` 中几乎是不可能的。

传统的基于文本的 Shell 之所以使编写管道变得如此困难，是因为它们要求你深入了解管道中每个命令的输出格式特性，如 示例 2-2 所示。

### 示例 2-2. 传统的基于文本的管道

```bash
lee@ubuntu-20-04:~$ ps -F | awk '{ if($5 > 500) print }' | sort -r -k 64,70
UID        PID  PPID  C    SZ   RSS PSR STIME TTY             TIME CMD
lee       8175  7967  0   965  1036   0 21:51 pts/0       00:00:00 ps -F
lee       7967  7966  0  1173  2104   0 21:38 pts/0       00:00:00 -bash
```

在这个例子中，你必须知道，对于每一行，第 5 组代表内存使用量。你必须了解另一种语言（即 `awk` 工具的语言）来按该列进行筛选。最后，你必须知道包含进程名称的列范围（在此系统上为第 64 到 70 列），然后将其提供给 `sort` 命令。而这只是一个简单的例子。

基于对象的管道开启了巨大的可能性，使系统管理既更简单又更强大。

## 2.1 根据命令的成功或失败链接命令

### 问题

你希望根据管道中前一个命令的成功与否，将多个命令链接在一起。

### 解决方案

使用 `&&` 和 `||` 管道链接运算符：

```powershell
PS > Invoke-Command localhost { "Some output" } && "Connection successful!"
Some command output
Connection successful!

PS > Invoke-Command missing_computer { "Some output" } && "Connection successful!"
OpenError: [missing_computer] Connecting to remote server missing_computer failed...

PS > Invoke-Command missing_computer { "Some output" } || "Connection failed."
OpenError: [missing_computer] Connecting to remote server missing_computer failed...
Connection failed.
```

### 讨论

如果你希望根据管道中其他命令的成功与否来链接多个命令，可以使用 PowerShell 的管道链接运算符。`&&` 运算符仅在前一个命令成功时才执行下一个命令。`||` 运算符仅在前一个命令失败时才执行下一个命令。

对于管道链接运算符，命令的成功与否由 `$?`（“美元钩子”）自动变量决定。有关 `$?` 自动变量的更多信息，请参阅 配方 15.1。

### 另请参阅

- 配方 15.1，“确定最后一条命令的状态”



# 2.2 筛选列表或命令输出中的项目

## 问题

你想要筛选列表或命令输出中的项目。

## 解决方案

使用 `Where-Object` cmdlet 来选择列表（或命令输出）中与你提供的条件匹配的项目。`Where-Object` cmdlet 拥有标准别名 `where` 和 `?`。

要列出名称中包含 "search" 的所有正在运行的进程，请使用 `-like` 运算符与进程的 `Name` 属性进行比较：

```
Get-Process | Where-Object { $_.Name -like "*Search*" }
```

要列出所有已停止的服务，请使用 `-eq` 运算符与服务的 `Status` 属性进行比较：

```
Get-Service | Where-Object { $_.Status -eq "Stopped" }
```

要列出所有无响应的进程，请测试 `Responding` 属性：

```
Get-Process | Where-Object { -not $_.Responding }
```

对于属性上的简单比较，你可以省略脚本块语法，直接使用 `Where-Object` 的比较参数：

```
Get-Process | Where-Object Name -like "*Search*"
```

## 讨论

对于其输入中的每个项目（即前一个命令的输出），`Where-Object` cmdlet 会根据你指定的脚本块对该输入进行求值。如果脚本块返回 `True`，则 `Where-Object` cmdlet 会将对象传递下去。否则，它不会传递。脚本块是由 `{` 和 `}` 字符括起来的一系列 PowerShell 命令。你可以在脚本块中编写任何 PowerShell 命令。在脚本块中，`$_`（或 `$PSItem`）变量代表当前输入对象。对于传入对象集合中的每个项目，PowerShell 都会将该项目赋值给 `$_`（或 `$PSItem`）变量，然后运行你的脚本块。在前面的示例中，这个传入对象代表前一个 cmdlet 生成的进程、文件或服务。

如果需要，此脚本块可以包含大量功能。它可以组合多个测试、比较等。有关脚本块的更多信息，请参阅 Recipe 11.4。有关可用比较类型的更多信息，请参阅 “比较运算符”。

对于简单的筛选，在 `Where-Object` cmdlet 中使用脚本块的语法有时可能显得过于繁琐。对于这些场景，`Where-Object` 提供了直接支持参数以应用简单比较（如 `-Eq`、`-Match`、`-In` 等）的参数。

除了 `Where-Object` cmdlet 提供的脚本块语法外，PowerShell 还提供了一个内置于语言本身的版本：`where()` 方法。对于非常大的数据集合，这稍微快一些，尽管收集这些项目所需的时间（例如获取目录中的文件列表）通常远大于筛选它们所需的时间。然而，`where()` 方法确实通过其第二个参数提供了几种额外的有用模式。

### 获取列表的第一部分

```
PS > (1..10).where( { $_ -eq 5 }, "Until" )
1
2
3
4
```

### 获取列表的第二部分

```
PS > (1..10).where( { $_ -eq 5 }, "SkipUntil" )
5
6
7
8
9
10
```

### 分割列表

```
PS > $even,$odd = (1..10).where( { $_ % 2 -eq 0 }, "Split" )
PS > $even -join ","
2,4,6,8,10
PS > $odd -join ","
1,3,5,7,9
```

对于复杂筛选（例如，你通常依靠鼠标在资源管理器窗口中对文件进行的那种筛选），编写脚本块来表达你的意图可能很困难，甚至不可行。如果是这种情况，Recipe 2.4 展示了一个可以使手动筛选更容易完成的脚本。

有关 `Where-Object` cmdlet 的更多信息，请输入 `Get-Help Where-Object`。有关 `where()` 方法的更多信息，请输入 `Get-Help about_Arrays`。

## 另请参阅

Recipe 2.4，“以交互方式筛选对象列表”

Recipe 11.4，“编写脚本块”

“比较运算符”

# 2.3 按名称对数据进行分组和透视

## 问题

你想要通过属性名称轻松访问列表中的项目。

## 解决方案

使用 `Group-Object` cmdlet（具有标准别名 `group`）及其 `-AsHash` 和 `-AsString` 参数。这将创建一个哈希表，其中选定的属性（或表达式）用作该哈希表中的键：

```
PS > $h = dir | group -AsHash -AsString Length
PS > $h

Name                           Value
----                           -----
746                            {ReplaceTest.ps1}
499                            {Format-String.ps1}
20494                          {test.dll}

PS > $h["499"]

    Directory: C:\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:57 PM        499 Format-String.ps1


PS > $h["746"]

    Directory: C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:51 PM        746 ReplaceTest.ps1
```

## 讨论

在某些情况下，你可能会发现自己反复调用 `Where-Object` cmdlet 来与同一个列表或输出进行交互：

```
PS > $processes = Get-Process
PS > $processes | Where-Object { $_.Id -eq 1216 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     62       3     1012       3132    50     0.20   1216 dwm


PS > $processes | Where-Object { $_.Id -eq 212 }

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    614      10    28444       5484   117     1.27    212 SearchIndexer
```

在这些情况下，你可以改用 `Group-Object` cmdlet 的 `-AsHash` 参数。当你使用此参数时，PowerShell 会创建一个哈希表来保存结果。这将在你感兴趣的属性及其代表的对象之间创建一个映射：

```
PS > $processes = Get-Process | Group-Object -AsHash Id
PS > $processes[1216]

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
     62       3     1012       3132    50     0.20   1216 dwm

PS > $processes[212]

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    610      10    28444       5488   117     1.27    212 SearchIndexer
```

对于简单类型的数据，这种方法效果很好。不过，根据你的数据，单独使用 `-AsHash` 参数可能会造成困难。

你可能遇到的第一个问题是当属性的值为 `$null` 时。PowerShell 中的哈希表（以及提供底层支持的 .NET Framework）不支持将 `$null` 作为值，因此你会得到一条误导性的错误消息：

```
PS > "Hello",(Get-Process -id $pid) | Group-Object -AsHash Id
Group-Object : The objects grouped by this property cannot be expanded
since there is a duplication of the key. Please give a valid property and try
again.
```

当更复杂的数据存储在哈希表中时，会出现第二个问题。遗憾的是，即使是*看起来*简单的数据也可能存在这种情况：

```
PS > $result = dir | Group-Object -AsHash Length
PS > $result

Name                           Value
----                           -----
746                            {ReplaceTest.ps1}
499                            {Format-String.ps1}
20494                          {test.dll}

PS > $result[746]
(Nothing appears)
```



这是由于哈希表中的信息与您输入的信息之间存在不兼容性导致的。在您自己创建的哈希表中，这通常不是问题，因为您提供了填充它们所需的所有信息。但在这种情况下，哈希表中存储的 `Length` 值来自目录列表，类型为 `Int64`。显式转换可以解决此问题，但需要大量的尝试和错误才能发现：

```
PS > $result[ [int64] 746 ]

    Directory: C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:51 PM        746 ReplaceTest.ps1
```

很难同时避免这两个问题，因此 `Group-Object` cmdlet 还提供了 `-AsString` 参数，将所有值转换为其字符串等效项。使用该参数，您始终可以假设这些值将被视为字符串（并可通过字符串访问）：

```
PS > $result = dir | Group-Object -AsHash -AsString Length
PS > $result["746"]

    Directory: C:\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        10/18/2009   9:51 PM        746 ReplaceTest.ps1
```

有关 `Group-Object` cmdlet 的更多信息，请输入 **`Get-Help Group-Object`**。有关 PowerShell 哈希表的更多信息，请参阅 配方 7.13。

## 另请参阅

- 配方 7.13，“创建哈希表或关联数组”
- “哈希表（关联数组）”

## 2.4 交互式筛选对象列表

有时 `Where-Object` cmdlet 的脚本块语法过于强大。在这种情况下，简化的属性访问参数提供了更简单的替代方案。有时 `Where-Object` cmdlet 又过于简单——当将选择逻辑表示为代码比手动选择更繁琐时。在这种情况下，交互式筛选会更有效。

PowerShell 通过 `Out-GridView` cmdlet 的 `-PassThru` 参数使这种交互式筛选变得非常容易。例如，您可以在尝试命令一段时间后使用此参数来创建一个简单的脚本。只需突出显示要保留的行，然后按“确定”即可：

```
PS > $script = Get-History | ForEach-Object CommandLine | Out-GridView -PassThru
PS > $script | Set-Content c:\temp\script.ps1
```

默认情况下，`Out-GridView` cmdlet 允许您在按“确定”之前一次选择多个项目。如果您想将选择限制为单个元素，请将 `Single` 用作 `-OutputMode` 参数的值。

有关运行脚本的更多信息，请参阅 配方 1.2。

### 另请参阅

- 配方 1.2，“运行程序、脚本和现有工具”
- 配方 2.2，“筛选列表或命令输出中的项目”

## 2.5 处理列表或命令输出中的每个项目

### 问题

您有一个项目列表，想要处理该列表中的每个项目。

### 解决方案

使用 `ForEach-Object` cmdlet（具有标准别名 `foreach` 和 `%`）来处理列表中的每个项目。

要对列表中的每个项目应用计算，请在脚本块参数中将 `$_`（或 `$PSItem`）变量作为计算的一部分使用：

```
PS > 1..10 | ForEach-Object { $_ * 2 }
2
4
6
8
10
12
14
16
18
20
```

要对目录中的每个文件运行程序，请在脚本块参数中将 `$_`（或 `$PSItem`）变量作为程序的参数使用：

```
Get-ChildItem *.txt | ForEach-Object { attrib -r $_ }
```

要访问列表中每个对象的方法或属性，请在脚本块参数中对 `$_`（或 `$PSItem`）变量访问该方法或属性。在本示例中，您获取名为 `notepad` 的正在运行的进程列表，然后等待每个进程退出：

```
$notepadProcesses = Get-Process notepad
$notepadProcesses | ForEach-Object { $_.WaitForExit() }
```

### 讨论

与 `Where-Object` cmdlet 一样，`ForEach-Object` cmdlet 会为输入中的每个项目运行您指定的脚本块。脚本块是由 `{` 和 `}` 字符括起来的一系列 PowerShell 命令。对于传入对象集合中的每个项目，PowerShell 会将该项目一次一个地赋值给 `$_`（或 `$PSItem`）变量。在解决方案给出的示例中，`$_`（或 `$PSItem`）变量表示前一个 cmdlet 生成的每个文件或进程。

> **注意**
>
> 解决方案中的第一个示例演示了一种生成数字范围的巧妙方法：`1..10`
>
> 这是 PowerShell 的数组范围语法，您可以在 配方 7.3 中了解更多相关信息。

如果需要，此脚本块可以包含大量功能。您可以组合多个测试、比较等。有关脚本块的更多信息，请参阅 配方 11.4。有关可用比较类型的更多信息，请参阅 “比较运算符”。

除了 `ForEach-Object` cmdlet 支持的用于处理管道中每个元素的脚本块外，它还支持在管道开始和结束时执行的脚本块。例如，考虑以下计算数组元素之和的代码：

```
$myArray = 1,2,3,4,5
$sum = 0
$myArray | ForEach-Object { $sum += $_ }
$sum
```

您可以将其简化为：

```
$myArray | ForEach-Object -Begin {
    $sum = 0 } -Process { $sum += $_ } -End { $sum }
```

由于您还可以按位置指定 `-Begin`、`-Process` 和 `-End` 参数，这可以进一步简化为：

```
$myArray | ForEach-Object { $sum = 0 } { $sum += $_ } { $sum }
```

对于简单场景（如仅检索单个属性），基于脚本块的语法可能会显得笨重：

```
Get-Process | ForEach-Object { $_.Name }
```

在 PowerShell 中，`ForEach-Object` cmdlet（及其 `%` 别名）还支持参数，以大幅简化属性和方法访问：

```
Get-Process | ForEach-Object Name
Get-Process | % Name | % ToUpper
```

与 `Where-Object` cmdlet 一样，PowerShell 在集合上提供了 `foreach()` 方法，让您执行许多相同的任务：

```
## 属性访问
(Get-Process).foreach("Name")

## 脚本块调用
$sum = 0
(1..5).foreach( { $sum += $_ } )

## 类型转换
$bytes = (1..5).foreach( [Byte] )
```

除了使用 `ForEach-Object` cmdlet 支持完整的成员调用外，PowerShell 语言还提供了一种快速简便的方法来枚举属性。正如您能够访问单个元素上的属性一样，PowerShell 允许您使用类似的语法来访问集合中每个项目的该属性：

```
PS > Start-Process PowerShell
PS > Start-Process PowerShell
PS > $processes = Get-Process -Name PowerShell
PS > $processes[0].Id
7928

PS > $processes.Id
7928
13120
```

在编写更高级的管道时，您有时可能会发现自己在一个已经在处理管道输入的脚本块中编写 `Where-Object` 或 `ForEach-Object` 脚本块。在这种情况下，您将失去对外部 `$_`（或 `$PSItem`）变量在内部脚本块中的访问：

## 获取所有进程
Get-Process | ForEach-Object {
    ## 获取它们的所有模块（加载的 DLL）
    $_.Modules | ForEach-Object {
        ## 如果 DLL 从 AppData 加载
        if($_.FileName -match 'AppData') {
            ## 期望行为：输出进程名称
            ## 实际行为：输出模块名称
            $_
        }
    }
}



## 2.5 PipelineVariable 参数

为了解决这个问题，PowerShell 支持 `-PipelineVariable` 参数。当你向命令添加此参数时，PowerShell 会将命令的当前管道输出保存到你指定的变量名中，除了 `$_` 变量之外。此时你可以在其他嵌套脚本块中自由使用它，而不必担心它被覆盖：

```powershell
## Get all processes
Get-Process -PipelineVariable currentProcess | ForEach-Object {
    ## Get all of their modules (loaded DLLs)
    $_.Modules | ForEach-Object {
        ## If the DLL is loaded from AppData
        if ($_.FileName -match 'AppData') {
            ## Output the process name
            $currentProcess
        }
    } | Select-Object -First 1
}
```

`ForEach-Object` Cmdlet 并非对列表中的项目执行操作的唯一方式。PowerShell 脚本语言还支持其他几个关键字，例如 `for`、（另一种）`foreach`、`do` 和 `while`。有关如何使用这些关键字的信息，请参阅 Recipe 4.4。

有关 `ForEach-Object` Cmdlet 的更多信息，请输入 `Get-Help ForEach-Object`。

有关在自己的脚本、函数和脚本块中处理管道输入的更多信息，请参阅 Recipe 11.18。

## 另请参阅

-   Recipe 4.4，“使用循环重复操作"
-   Recipe 7.3，“访问数组元素"
-   Recipe 11.4，“编写脚本块"
-   Recipe 11.18，“访问脚本的管道输入"
-   “比较运算符"

# 2.6 自动化数据密集型任务

## 问题

你想要在大量数据上调用一个简单任务。

## 解决方案

如果只有一项数据发生变化（例如服务器名称或用户名），请将数据存储在文本文件中。使用 `Get-Content` Cmdlet 检索项目，然后使用 `ForEach-Object` Cmdlet（具有标准别名 `foreach` 和 `%`）来处理列表中的每个项目。示例 2-3 演示了此技术。

### 示例 2-3. 使用文本文件中的信息自动化数据密集型任务

```powershell
PS > Get-Content servers.txt
SERVER1
SERVER2
PS > $computers = Get-Content servers.txt
PS > $computers | ForEach-Object {
     Get-CimInstance Win32_OperatingSystem -Computer $_ }

SystemDirectory : C:\WINDOWS\system32
Organization    :
BuildNumber     : 19041
Version         : 10.0.19041

SystemDirectory : C:\WINDOWS\system32
Organization    :
BuildNumber     : 19041
Version         : 10.0.19041
```

如果在 `ForEach-Object` Cmdlet 中包含操作变得繁琐（或不清晰），你也可以使用 `foreach` 脚本关键字，如 示例 2-4 所示。

### 示例 2-4. 使用 foreach 脚本关键字使循环语句更易读

```powershell
$computers = Get-Content servers.txt

foreach ($computer in $computers)
{
    ## Get the information about the operating system from WMI
    $system = Get-CimInstance Win32_OperatingSystem -Computer $computer

    ## Determine if it is running Windows XP
    if ($system.Version -match "¹⁰.")
    {
        "$computer is running Windows 10"
    }
}
```

如果每个任务有多个数据方面发生变化（例如，大型报告中计算机的 CIM 类和计算机名称均发生变化），请创建一个 CSV 文件，每个任务占一行。使用 `Import-Csv` Cmdlet 将该数据导入 PowerShell，然后使用生成对象的属性作为多个相关数据源。示例 2-5 演示了此技术。

### 示例 2-5. 使用 CSV 中的信息自动化数据密集型任务

```powershell
PS > Get-Content WmiReport.csv
ComputerName,Class
LEE-DESK,Win32_OperatingSystem
LEE-DESK,Win32_Bios

PS > $data = Import-Csv WmiReport.csv
PS > $data

ComputerName                          Class
------------                          -----
LEE-DESK                              Win32_OperatingSystem
LEE-DESK                              Win32_Bios

PS > $data |
    ForEach-Object { Get-CimInstance $_.Class -Computer $_.ComputerName }

SystemDirectory : C:\WINDOWS\system32
Organization    :
BuildNumber     : 2600
Version         : 5.1.2600

SMBIOSBIOSVersion : ASUS A7N8X Deluxe ACPI BIOS Rev 1009
Manufacturer      : Phoenix Technologies, LTD
Name              : Phoenix - AwardBIOS v6.00PG
SerialNumber      : xxxxxxxxxxx
Version           : Nvidia - 42302e31
```

## 讨论

PowerShell 的主要优势之一是其自动化重复任务的能力。有时这些重复任务是操作密集型的（例如通过注册表和文件清理进行系统维护），并由始终一起调用的复杂命令序列组成。在这种情况下，你可以编写脚本来组合这些操作，以节省时间并减少错误。

其他时候，你只需要完成单个任务（例如检索 WMI 查询结果），但需要为大量数据重复调用该任务。在这种情况下，PowerShell 的脚本语句、管道支持和数据管理 Cmdlet 有助于自动化这些任务。

解决方案中给出的选项之一是 `Import-Csv` Cmdlet。`Import-Csv` Cmdlet 读取 CSV 文件，并为每一行自动创建一个对象，其属性对应于列名。示例 2-6 显示了包含 `ComputerName` 和 `Class` 标题的 CSV 的结果。

### 示例 2-6. Import-Csv Cmdlet 创建具有 ComputerName 和 Class 属性的对象

```powershell
PS > $data = Import-Csv WmiReport.csv
PS > $data

ComputerName                         Class
------------                         -----
LEE-DESK                             Win32_OperatingSystem
LEE-DESK                             Win32_Bios

PS > $data[0].ComputerName
LEE-DESK
```

正如解决方案所示，你可以使用 `ForEach-Object` Cmdlet 将这些对象中的数据提供给重复的 Cmdlet 调用。它通过指定每个参数名称，后跟适用于该参数的数据（取自当前 CSV 对象的属性）来实现这一点。

> **注意**
>
> 如果你已经在变量中拥有逗号分隔的值（而不是文件），可以使用 `ConvertFrom-Csv` Cmdlet 将这些值转换为对象。

虽然这是最通用的解决方案，但许多 Cmdlet 参数可以自动从传入对象中检索其值，前提是该对象的任何属性具有相同的名称。这使你可以完全省略 `ForEach-Object` 和属性映射步骤。支持此功能的参数被称为支持 *按属性名从管道获取值*。`Move-Item` Cmdlet 就是一个支持此功能的参数示例，正如 示例 2-7 中的 `Accept pipeline input?` 行所示。

### 示例 2-7. Move-Item Cmdlet 的帮助内容，显示接受按属性名从管道获取值的参数



## PowerShell Move-Item 帮助信息

```
PS > Get-Help Move-Item -Full
(...)
PARAMETERS

    -path <string[]>
        Specifies the path to the current location of the items. The default
        is the current directory. Wildcards are permitted.

        Required?                    true
        Position?                    1
        Default value                <current location>
        Accept pipeline input?       true (ByValue, ByPropertyName)
        Accept wildcard characters?  true

    -destination <string>
        Specifies the path to the location where the items are being moved.
        The default is the current directory. Wildcards are permitted, but
        the result must specify a single location.

        To rename the item being moved, specify a new name in the value of
        Destination.

        Required?                    false
        Position?                    2
        Default value                <current location>
        Accept pipeline input?       true (ByPropertyName)
        Accept wildcard characters?  True
        (...)
```

如果你有意将 CSV 中的列命名为与接受管道按属性名传值的参数相对应，PowerShell 可以为你完成部分（或全部）参数映射。示例 2-8 演示了一个批量移动项目的 CSV 文件。

### 示例 2-8. 使用 `Import-Csv` cmdlet 自动化接受管道按属性名传值的 cmdlet

```
PS > Get-Content ItemMoves.csv
Path,Destination
test.txt,Test1Directory
test2.txt,Test2Directory

PS > dir test.txt,test2.txt | Select Name

Name
----
test.txt
test2.txt

PS > Import-Csv ItemMoves.csv | Move-Item
PS > dir Test1Directory | Select Name

Name
----
test.txt

PS > dir Test2Directory | Select Name

Name
----
test2.txt
```

有关 `ForEach-Object` cmdlet 和 `foreach` 脚本关键字的更多信息，请参阅配方 2.5。有关处理 CSV 文件的更多信息，请参阅配方 10.7。有关使用 WMI 的更多信息，请参阅第 28 章。

## 另请参阅

- 配方 2.5，“处理列表或命令输出中的每个项目”
- 配方 10.7，“从文件导入 CSV 和分隔数据”
- 第 28 章

## 2.7 拦截管道阶段

### 问题

你想要在 PowerShell 管道的不同阶段拦截或执行某些操作。

### 解决方案

使用配方 11.23 中提供的 `New-CommandWrapper` 脚本来包装 `Out-Default` 命令，并将你的自定义功能放入其中。

### 讨论

对于任何管道，PowerShell 都会在末尾添加一个对 `Out-Default` cmdlet 的隐式调用。通过在此函数上添加命令包装器，我们可以大量自定义管道处理行为。

当 PowerShell 创建管道时，它首先调用管道中每个命令的 `BeginProcessing()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `Begin` 块。如果你想在管道开始时执行任何操作，请将自定义代码放在该块中。

对于管道发出的每个对象，PowerShell 将该对象发送给管道中下一个命令的 `ProcessRecord()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `Process` 块。如果你想为管道中的每个元素执行任何操作，请将自定义代码放在该块中。

最后，当 PowerShell 处理完管道中的所有项目时，它会调用管道中每个命令的 `EndProcessing()` 方法。对于高级函数（由 `New-CommandWrapper` 脚本创建的类型），PowerShell 会调用 `End` 块。如果你想在管道结束时执行任何操作，请将自定义代码放在该块中。

有关此方法的两个示例，请参阅配方 2.8 和配方 11.22。

有关运行脚本的更多信息，请参阅配方 1.2。

### 另请参阅

- 配方 1.2，“运行程序、脚本和现有工具”
- 配方 2.8，“自动捕获管道输出”
- 配方 11.22，“调用动态命名的命令”
- 配方 11.23，“程序：增强或扩展现有 Cmdlet”

## 2.8 自动捕获管道输出

### 问题

你想要自动捕获最后一条命令的输出，而无需显式将其输出存储在变量中。

### 解决方案

使用 `$PSDefaultParameterValues` 自动变量将 `Out-Default` 命令的 `-OutVariable` 参数值设置为你选择的变量名：

```
$PSDefaultParameterValues["Out-Default:OutVariable"] = "__"
```

### 讨论

一旦命令中的每个对象都通过了管道，它最终会到达末端。如果你的脚本没有捕获此输出，PowerShell 会将其提供给 `Out-Default` cmdlet，后者负责确定如何格式化和显示输出。

与所有 cmdlet 一样，`Out-Default` cmdlet 支持 `-OutVariable` 参数，允许你将其输出存储到变量中：

```
PS > 1..3 | Out-Default -OutVariable myOutput
1
2
3

PS > $myOutput
1
2
3
```

了解这一点后，我们可以利用 PowerShell 的 `$PSDefaultParameterValues` 基础设施，让 `Out-Default` 每次都执行此操作。解决方案使用两个下划线字符作为变量名，以类似于 PowerShell 中表示当前管道输入的单下划线，但你可以使用任何想要的变量名：

```
PS > $PSDefaultParameterValues["Out-Default:OutVariable"] = "lastOutput"

PS > 1..3
1
2
3

PS > $lastOutput
1
2
3
```

有关为 cmdlet 参数提供默认值的更多信息，请参阅配方 1.5。

### 另请参阅

- 配方 1.5，“为参数提供默认值”
- 配方 2.7，“拦截 PowerShell 管道阶段”
- 配方 11.23，“程序：增强或扩展现有 Cmdlet”

## 2.9 捕获和重定向二进制进程输出

### 问题

你想要运行在它们之间传输复杂二进制数据的程序。

### 解决方案

使用 `Invoke-BinaryProcess` 脚本来调用该程序，如示例 2-9 所示。如果它是二进制数据的来源，请使用 `-RedirectOutput` 参数。如果它消费二进制数据，请使用 `-RedirectInput` 参数。

### 示例 2-9. Invoke-BinaryProcess.ps1



# Invoke-BinaryProcess

出自 *PowerShell Cookbook* (O'Reilly) 作者 Lee Holmes (http://www.leeholmes.com/guide)

## 概要

调用一个发出或消费二进制数据的进程。

## 示例

```powershell
PS > Invoke-BinaryProcess binaryProcess.exe -RedirectOutput -ArgumentList "-emit" |
       Invoke-BinaryProcess binaryProcess.exe -RedirectInput -ArgumentList "-consume"
```

## 参数

| 参数 | 别名 | 类型 | 说明 |
|-----------|-------|------|-------------|
| `ProcessName` | | `[string]` | 要调用的进程名称 |
| `RedirectInput` | `Input` | `[switch]` | 指定进程的输入应被视为二进制 |
| `RedirectOutput` | `Output` | `[switch]` | 指定进程的输出应被视为二进制 |
| `ArgumentList` | | `[string]` | 指定进程的参数 |

## 脚本主体

```powershell
Set-StrictMode -Version 3

## 准备调用进程
$processStartInfo = New-Object System.Diagnostics.ProcessStartInfo
$processStartInfo.FileName = (Get-Command $processname).Definition
$processStartInfo.WorkingDirectory = (Get-Location).Path
if($argumentList) { $processStartInfo.Arguments = $argumentList }
$processStartInfo.UseShellExecute = $false

## 始终重定向进程的输入和输出。
## 有时我们将其作为二进制捕获，其他时候则
## 仅将其视为字符串处理。
$processStartInfo.RedirectStandardOutput = $true
$processStartInfo.RedirectStandardInput = $true

$process = [System.Diagnostics.Process]::Start($processStartInfo)

## 如果要求重定向输入，将其视为字节处理。
## 否则，将任何输入作为字符串写入进程。
if($redirectInput)
{
    $inputBytes = @($input)
    $process.StandardInput.BaseStream.Write($inputBytes, 0, $inputBytes.Count)
    $process.StandardInput.Close()
}
else
{
    $input | % { $process.StandardInput.WriteLine($_) }
    $process.StandardInput.Close()
}

## 如果要求重定向输出，将其视为字节处理。
## 否则，从进程读取任何输出作为字符串。
if($redirectOutput)
{
    $byteRead = -1
    do
    {
        $byteRead = $process.StandardOutput.BaseStream.ReadByte()
        if($byteRead -ge 0) { $byteRead }
    } while($byteRead -ge 0)
}
else
{
    $process.StandardOutput.ReadToEnd()
}
```

## 讨论

当 PowerShell 启动本机应用程序时，它提供的好处之一是允许你使用 PowerShell 命令来处理输出。例如：

```powershell
PS > (ipconfig)[7]
   Link-local IPv6 Address . . . . . : fe80::20f9:871:8365:f368%8
PS > (ipconfig)[8]
   IPv4 Address. . . . . . . . . . . : 10.211.55.3
```

PowerShell 通过在程序的换行符处拆分输出，然后将每一行独立地传递到管道中来实现这一点。这包括使用 Unix 换行符 (`\n`) 以及 Windows 换行符 (`\r\n`) 的程序。

然而，如果程序输出二进制数据，这种重新解释会在数据被重定向到另一个进程或文件时损坏数据。例如，某些程序通过复杂的二进制数据结构相互通信，这些结构在传输过程中不能被修改。这在某些图像编辑工具和其他为管道数据操作设计的非 PowerShell 工具中很常见。

我们可以通过一个示例 *BinaryProcess.exe* 应用程序来观察这一点，该应用程序要么发出二进制数据，要么消费它。以下是 *BinaryProcess.exe* 应用程序的 C# 源代码：

```csharp
using System;
using System.IO;

public class BinaryProcess
{
    public static void Main(string[] args)
    {
        if(args[0] == "-consume")
        {
            using(Stream inputStream = Console.OpenStandardInput())
            {
                for(byte counter = 0; counter < 255; counter++)
                {
                    byte received = (byte) inputStream.ReadByte();
                    if(received != counter)
                    {
                        Console.WriteLine(
                            "Got an invalid byte: {0}, expected {1}.",
                            received, counter);
                        return;
                    }
                    else
                    {
                        Console.WriteLine(
                            "Properly received byte: {0}.", received, counter);
                    }
                }
            }
        }

        if(args[0] == "-emit")
        {
            using(Stream outputStream = Console.OpenStandardOutput())
            {
                for(byte counter = 0; counter < 255; counter++)
                {
                    outputStream.WriteByte(counter);
                }
            }
        }
    }
}
```

当我们使用 `-emit` 参数运行它时，PowerShell 将输出拆分为三个对象：

```powershell
PS > $output = .\binaryprocess.exe -emit
PS > $output.Count
3
```

我们期望此输出包含 0 到 254 的数字，但我们看到并非如此：

```powershell
PS > $output | ForEach-Object { "------------";
    $_.ToCharArray() | ForEach-Object { [int] $_ } }
------------
0
1
2
3
4
5
6
7
8
9
------------
11
12
------------
14
15
16
17
18
19
20
21
22
(...)
255
214
220
162
163
165
8359
402
225
```

在数字 10 处，PowerShell 将该字节解释为行尾，并用它将输出拆分为一个新元素。对于数字 13 也是如此。当我们到达较大数字且 PowerShell 开始将字节组合解释为另一种语言的 Unicode 字符时，情况似乎变得更加奇怪。

该解决方案通过直接管理二进制进程的输出来解决此行为。如果你提供 `-RedirectInput` 参数，脚本会假设传入的是二进制数据流并将其直接传递给程序。如果你提供 `-RedirectOutput` 参数，脚本会假设输出是二进制数据，同样直接从进程读取它。

## 另请参阅

食谱 1.2, "运行程序、脚本和现有工具"
