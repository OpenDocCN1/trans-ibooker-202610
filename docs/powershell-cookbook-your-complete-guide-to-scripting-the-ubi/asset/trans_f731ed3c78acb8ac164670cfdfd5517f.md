

再次切换到进程监视器窗口，然后按 `Ctrl+E`（或点击放大镜图标）。进程监视器现在不再捕获应用程序的活动。

### 查看捕获日志中的注册表修改

进程监视器窗口现在显示应用程序在应用其配置设置时交互过的所有注册表键。

按 `Ctrl+F`（或点击双筒望远镜图标），然后搜索 `RegSetValue`。进程监视器会高亮显示对注册表键的首次修改，如[图 21-4](#ch21.xhtml#process_monitors_registry_access_detail)所示。

![](Images/wps4_2104.png)

按 `Enter`（或双击高亮行）查看此特定注册表修改的详细信息。在本例中，我们可以看到 PowerShell 将 `ExecutionPolicy` 属性（位于 `HKLM:\Software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell` 下）的值更改为了 `RemoteSigned`。按 `F3` 查看对应注册表修改的下一条目。

### 自动化这些注册表写入

现在您已知晓应用程序在更新设置时执行的所有注册表写入操作，判断和实验将帮助您确定哪些修改实际代表了该设置。由于 PowerShell 仅执行了一次注册表写入（写入一个从名称上看似乎代表执行策略的键），因此在本例中选择非常明确。

一旦您发现了应用程序用于存储配置数据的注册表键、属性和值，就可以使用[配方 21.3](#ch21.xhtml#modify_or_remove_a_registry_key_value)中讨论的技术来自动化这些配置设置，如下例所示：

```
PS > $key = "HKLM:\Software\Microsoft\PowerShell\1\" +
    "ShellIds\Microsoft.PowerShell"

PS > Set-ItemProperty $key ExecutionPolicy AllSigned
PS > Get-ExecutionPolicy
AllSigned
PS > Set-ItemProperty $key ExecutionPolicy RemoteSigned
PS > Get-ExecutionPolicy
RemoteSigned
```

## 另请参阅

[配方 21.3，“修改或删除注册表键值”](#ch21.xhtml#modify_or_remove_a_registry_key_value)

# 第 22 章 比较数据

## 22.0 简介

在使用 PowerShell 时，通常需要处理对象集合。大多数 PowerShell 命令生成对象，.NET Framework 中使用的许多方法也是如此。为了帮助您处理这些对象集合，PowerShell 引入了 `Compare-Object` cmdlet。`Compare-Object` cmdlet 提供了类似于广为人知的 `diff` 命令的功能，但带有面向对象的特色。

## 22.1 比较两个命令的输出

### 问题

您想要比较两个命令的输出。

### 解决方案

要比较两个命令的输出，请将每个命令的输出存储在变量中，然后使用 `Compare-Object` cmdlet 来比较这些变量：

```
PS > notepad
PS > $processes = Get-Process
PS > Stop-Process -ProcessName Notepad
PS > $newProcesses = Get-Process
PS > Compare-Object $processes $newProcesses

InputObject                           SideIndicator
-----------                           -------------
System.Diagnostics.Process (notepad)  <=
```

### 讨论

解决方案展示了如何确定两次调用 `Get-Process` 之间退出了哪些进程。`SideIndicator` 为 `<=` 告诉我们该进程存在于左侧集合（`$processes`）中，但不存在于右侧集合（`$newProcesses`）中。要处理实际存在差异的对象，请访问 `InputObject` 属性：

```
PS > $diff = @(Compare-Object $processes $newProcesses)[0]
PS > $process = $diff.InputObject
PS > $process.Handles
55
```

默认情况下，`Compare-Object` cmdlet 使用内置于大多数 .NET 对象中的比较功能。大多数情况下这符合预期，但有时您可能想要覆盖该比较行为。例如，如果内存使用量发生变化，您可能希望将两个进程视为不同。在这种情况下，请使用 `-Property` 参数。

```
PS > Compare-Object $processes $newProcesses -Property Name,WS | Sort-Object Name

Name                                           WS SideIndicator
----                                           -- -------------
dwm                                      31358976 <=
dwm                                      29540352 =>
explorer                                 37969920 <=
explorer                                 38023168 =>
lsass                                     1548288 =>
lsass                                     1372160 <=
notepad                                   5701632 <=
notepad                                   2891776 =>
powershell                               44281856 =>
powershell                               44290048 <=
SearchIndexer                            13606912 =>
SearchIndexer                            13619200 <=
svchost                                  56061952 <=
svchost                                  43982848 <=
svchost                                  56037376 =>
svchost                                  44048384 =>
svchost                                  12193792 <=
svchost                                  12201984 =>
taskeng                                   9220096 <=
taskeng                                   9228288 =>
```

当您使用 `-Property` 参数时，`Compare-Object` cmdlet 输出的自定义对象仅包含您用于比较的属性。如果您仍想访问用于比较的原始对象，请同时使用 `-PassThru` 参数。在这种情况下，PowerShell 会将 `SideIndicator` 属性添加到原始对象中。

#### 注意

如果您要比较的对象已经按正确顺序排列（例如文件中的行），则可以通过使用 `-SyncWindow` 参数来提高比较过程的性能。例如，同步窗口设为 5 将仅在周围 5 个对象范围内查找差异。

有关 `Compare-Object` cmdlet 的更多信息，请输入 `Get-Help Compare-Object`。

## 22.2 确定两个文件之间的差异

### 问题

您想要确定两个文件之间的差异。

### 解决方案

要确定每个文件内容的简单差异，请将它们的内容存储在变量中，然后使用 `Compare-Object` cmdlet 来比较这些变量：

```
PS > "Hello World" > c:\temp\file1.txt
PS > "Hello World" > c:\temp\file2.txt
PS > "More Information" >> c:\temp\file2.txt
PS > $content1 = Get-Content c:\temp\file1.txt
PS > $content2 = Get-Content c:\temp\file2.txt
PS > Compare-Object $content1 $content2

InputObject                              SideIndicator
-----------                              -------------
More Information                         =>
```

### 讨论

`Compare-Object` cmdlet 的主要关注点是比较两个无序的对象集合。虽然这些对象集合可以是字符串（如两个文件的内容），但由于内容失去了顺序，`Compare-Object` 针对文件运行时的输出通常违反直觉。

在比较大文件（或顺序很重要的文件）时，您仍然可以使用传统的文件比较工具，如 `diff.exe` 或随 Windows 支持工具和 Visual Studio 一起提供的 `WinDiff` 应用程序。

有关 `Compare-Object` cmdlet 的更多信息，请输入 `Get-Help Compare-Object`。

# 第 23 章 事件日志

## 23.0 简介

事件日志构成了 Windows 上大多数监视和诊断活动的核心。为了支持此活动，PowerShell 提供了 `Get-WinEvent` cmdlet，允许您查询和处理系统上的事件日志数据。除了简单的事件日志检索外，PowerShell 还包含许多其他 cmdlet，用于创建、删除、自定义事件日志以及与之交互。

## 23.1 列出所有事件日志

### 问题

您想要确定系统上存在哪些事件日志。

### 解决方案

