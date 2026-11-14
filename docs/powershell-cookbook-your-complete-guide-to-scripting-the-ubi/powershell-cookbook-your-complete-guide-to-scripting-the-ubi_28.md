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
