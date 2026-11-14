
## 获取系统路径

虽然此方法提供对最常用的常见系统路径的访问，但它并不提供对所有路径的访问。对于 `[Environment]::GetFolderPath()` 方法不支持的路径，请使用 `WScript.Shell` COM 对象。`WScript.Shell` COM 对象支持以下路径：`AllUsersDesktop`、`AllUsersStartMenu`、`AllUsersPrograms`、`AllUsersStartup`、`Desktop`、`Favorites`、`Fonts`、`MyDocuments`、`NetHood`、`PrintHood`、`Programs`、`Recent`、`SendTo`、`StartMenu`、`Startup` 和 `Templates`。

如果能同时使用 `[Environment]::GetFolderPath()` 方法或 `WScript.Shell` COM 对象那将很不错，但正如我们所见，它们各自支持大量对方不支持的路径：

```powershell
PS > $shell = New-Object -Com WScript.Shell
PS > $shellPaths = $shell.SpecialFolders | Sort-Object
PS >
PS > $netFolders = [Enum]::GetValues([Environment+SpecialFolder])
PS > $netPaths = $netFolders |
    ForEach-Object { [Environment]::GetFolderPath($_) } | Sort-Object

PS > ## 查看仅 Shell 支持的路径
PS > Compare-Object $shellPaths $netPaths |
    Where-Object { $_.SideIndicator -eq "<=" }

InputObject                                                      SideIndicator
-----------                                                      -------------
C:\Users\lee\AppData\Roaming\Microsoft\Windows\Printer Shortcuts <=

PS > ## 查看仅 .NET 支持的路径
PS > Compare-Object $shellPaths $netPaths |
     Where-Object { $_.SideIndicator -eq "=>" }


InputObject                                                 SideIndicator
-----------                                                 -------------
C:\Program Files                                            =>
C:\Program Files (x86)                                      =>
C:\Program Files (x86)\Common Files                         =>
C:\Program Files\Common Files                               =>
C:\WINDOWS                                                  =>
C:\WINDOWS\resources                                        =>
C:\WINDOWS\system32                                         =>
C:\WINDOWS\SysWOW64                                         =>
D:\Lee                                                      =>
D:\Lee\My Music                                             =>
D:\Lee\My Pictures                                          =>
(...)
```

有关使用 .NET Framework 类的更多信息，请参阅 [配方 3.8](#ch03.xhtml#work_with_net_objects)。

## 另请参阅

[配方 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

# 16.8 获取当前位置

## 问题

你想要确定当前位置。

## 解决方案

要确定当前位置，请使用 `Get-Location` cmdlet：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > $currentLocation = (Get-Location).Path
PS > $currentLocation
C:\temp
```

此外，PowerShell 还通过 `$pwd` 自动变量提供对当前位置的访问：

```powershell
PS > $pwd

Path
----
C:\temp

PS > $currentLocation = $pwd.Path
PS > $currentLocation
C:\temp
```

## 讨论

有时影响使用 .NET Framework 的脚本的一个问题是，PowerShell 的“当前位置”概念并不总是与 `pwsh.exe` 进程的“当前目录”相同。例如：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > Get-Process | Export-CliXml processes.xml
PS > $reader = New-Object Xml.XmlTextReader processes.xml
PS > $reader.BaseURI
file:///C:/users/Lee/processes.xml
```

PowerShell 将这些概念分开，因为它支持多个执行管道。进程范围的当前目录会影响整个进程，因此如果在 Shell 中导航时更改了进程的当前目录，你将冒着破坏所有后台任务环境的风险。

当你在大多数 .NET 方法中使用文件名时，最佳实践是使用完全限定的路径名。`Resolve-Path` cmdlet 使这变得简单：

```powershell
PS > Get-Location

Path
----
C:\temp

PS > Get-Process | Export-CliXml processes.xml
PS > $reader = New-Object Xml.XmlTextReader (Resolve-Path processes.xml)
PS > $reader.BaseURI
file:///C:/temp/processes.xml
```

如果你想访问一个尚不存在的路径，请结合 `Get-Location` cmdlet 使用 `Join-Path` cmdlet：

```powershell
PS > Join-Path (Get-Location) newfile.txt
C:\temp\newfile.txt
```

有关 `Join-Path` cmdlet 的更多信息，请参阅 [配方 16.9](#ch16.xhtml#safely_build_file_paths_out_of_their_com)。

## 另请参阅

[配方 16.9](#ch16.xhtml#safely_build_file_paths_out_of_their_com)

# 16.9 安全地从组件构建文件路径

## 问题

你想要从子路径的组合中构建一个新路径。

## 解决方案

要将路径元素连接在一起，请使用 `Join-Path` cmdlet：

```powershell
PS > Join-Path (Get-Location) newfile.txt
C:\temp\newfile.txt
```

## 讨论

创建新路径的通常方法是将每个组件的字符串组合起来，并在它们之间放置路径分隔符：

```powershell
PS > "$(Get-Location)\newfile.txt"
C:\temp\newfile.txt
```

不幸的是，这种方法存在一些问题：

*   如果 `Get-Location` 返回的目录末尾已经有斜杠怎么办？
*   如果路径包含正斜杠而不是反斜杠怎么办？
*   如果我们讨论的是注册表路径而不是文件系统路径怎么办？

幸运的是，`Join-Path` cmdlet 解决了这些问题及更多问题。

有关 `Join-Path` cmdlet 的更多信息，请输入 `Get-Help Join-Path`。

# 16.10 与 PowerShell 的全局环境交互

## 问题

你想要在 PowerShell 环境中存储信息，以便其他脚本可以访问它。

## 解决方案

要使变量在整个 PowerShell 会话中可用，请在存储信息时使用 `$GLOBAL:` 前缀：

```powershell
## 如果 Web 服务缓存不存在，则创建它
if(-not (Test-Path Variable:\Lee.Holmes.WebServiceCache))
{
    ${GLOBAL:Lee.Holmes.WebServiceCache} = @{}
}
```

## 讨论

关于在会话的全局环境中存储信息，主要指导原则是尽可能避免这样做。在全局作用域存储信息的脚本容易破坏其他脚本，也容易被其他脚本破坏。

这在批处理文件编程中是常见做法，但脚本参数和返回值通常提供更简洁的替代方案。

大多数使用全局变量的脚本都是为了在调用之间保持状态。PowerShell 通过使用 `modules` 以更简洁的方式处理此问题。有关此技术的信息，请参阅 [配方 11.7](#ch11.xhtml#write_commands_maintain_global_state)。

如果你确实需要将变量写入全局作用域，请确保创建的名称足够唯一，以防止与其他脚本冲突，如解决方案所示。命名前缀的好选择包括脚本名称、作者姓名或公司名称。

有关在全局作用域（及其他作用域）设置变量的更多信息，请参阅 [配方 3.6](#ch03.xhtml#control_access_and_scope_of_variables_an)。

## 另请参阅

[配方 3.6，“控制变量和其他项目的访问与作用域”](#ch03.xhtml#control_access_and_scope_of_variables_an)

[配方 11.7，“编写维护状态的命令”](#ch11.xhtml#write_commands_maintain_global_state)

# 16.11 确定 PowerShell 版本信息

## 问题

你想要获取当前 PowerShell 版本、.NET CLR（公共语言运行时）版本、兼容的 PowerShell 版本等信息。

## 解决方案

访问 `$PSVersionTable` 自动变量：
