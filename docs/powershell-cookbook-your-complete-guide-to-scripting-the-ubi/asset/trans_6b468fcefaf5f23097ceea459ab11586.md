
## 19.2 连接到远程计算机

### 问题

您想要通过 Visual Studio Code 与远程 PowerShell 会话及其文件进行交互。

### 解决方案

从终端窗口，使用 `Enter-PSSession` cmdlet。

### 讨论

当您在 Visual Studio Code 中创建远程 PowerShell 会话时，PowerShell 扩展会自动识别您已建立连接，并为您的编辑器会话提供额外的远程支持。

一旦您连接了远程会话，在终端窗口中与该远程系统交互就像与本地系统交互一样。来自远程系统的提示显示得就像来自本地系统的提示一样，进度条、凭据请求和 PowerShell 的其他反馈机制也是如此。

如果您的远程脚本命中调试断点，Visual Studio Code 会自动将该脚本的副本拉取到您的本地计算机，并为您提供交互式调试体验，就好像您在自己的计算机上拥有该脚本一样。您可以检查系统状态、单步进入和跳出函数，甚至编辑脚本本身。有关 PowerShell 对远程调试的支持的更多信息，请参阅 [配方 14.8](#ch14.xhtml#debug_script_remote_machine)。

如果您想要编辑远程系统上的文件，可以使用 Visual Studio Code 的 `psedit` 命令。例如：

```
[localhost]: PS D:\Lee> psedit c:\temp\scratch.txt
(scratch.txt opens in your local Visual Studio Code editor)
```

Visual Studio Code 会自动将您对该文件所做的任何更改镜像回远程系统。

有关 PowerShell 远程处理的更多信息，请参阅 [第 29 章](#ch29.xhtml#chapter_remoting)。

### 另请参阅

- [配方 14.8，“在远程计算机上调试脚本”](#ch14.xhtml#debug_script_remote_machine)
- [第 29 章](#ch29.xhtml#chapter_remoting)

## 19.3 通过其对象模型与 Visual Studio Code 交互

### 问题

您想要与 Visual Studio Code 对象模型交互以实现高级功能和特性。

### 解决方案

探索并修改 `$psEditor` 自动变量的属性以与 PowerShell 编辑器服务对象模型交互。例如，要清理您当前正在编辑的脚本中的尾随空格，请使用以下命令：

```
$currentFile = $psEditor.GetEditorContext().CurrentFile
$currentText = $currentFile.GetText()
$currentText = $currentText -replace '(?m)\s+$',''
$endLine = $currentFile.FileRange.End.Line
$endColumn = $currentFile.FileRange.End.Column
$currentFile.InsertText($currentText, 1, 1, $endLine, $endColumn)
```

### 讨论

除了已有的功能外，Visual Studio Code 还通过其*对象模型*提供了许多额外的自动化机会。对象模型暴露了您创建自己功能所需的核心细节，并通过 `$psEditor` 自动变量使其可用。

与其他 .NET 对象模型一样，`Get-Member` 和 `Format-List` cmdlet 是探索 PowerShell 编辑器服务对象模型的关键。在第一层，对象模型为您提供对工作区和窗口的访问：

```
PS > $psEditor | Format-List

EditorServicesVersion : 2.2.0.0
Workspace             : Microsoft.PowerShell.EditorServices...EditorWorkspace
Window                : Microsoft.PowerShell.EditorServices...EditorWindow
```

随着您深入探索，您会发现许多有趣的功能。例如，`$psEditor.GetEditorContext().CurrentFile` 变量提供对当前文件的文本和行为的编程访问：

```
PS > $psEditor.GetEditorContext().CurrentFile | Get-Member

   TypeName: Microsoft.PowerShell.EditorServices.Extensions.FileContext

Name          MemberType Definition
----          ---------- ----------
Equals        Method     bool Equals(System.Object obj)
GetHashCode   Method     int GetHashCode()
GetText       Method     string GetText(), string GetText(Microsoft.PowerShell...
GetTextLines  Method     string[] GetTextLines(), string[] GetTextLines(Microso...
GetType       Method     type GetType()
InsertText    Method     void InsertText(string textToInsert), void InsertText(...
Save          Method     void Save()
SaveAs        Method     void SaveAs(string newFilePath)
ToString      Method     string ToString()
Ast           Property   System.Management.Automation.Language.Ast Ast {get;}
FileRange     Property   Microsoft.PowerShell.EditorServices.Extensions.IFileRa...
Language      Property   string Language {get;}
Path          Property   string Path {get;}
Tokens        Property   System.Collections.Generic.IReadOnlyList[System.Manage...
Uri           Property   uri Uri {get;}
WorkspacePath Property   string WorkspacePath {get;}
```

通过利用对象模型，您可以编写工具来自动处理脚本（例如，注释和取消注释脚本区域、处理脚本输出等）。

有关使用 .NET 对象的更多信息，请参阅 [配方 3.8](#ch03.xhtml#work_with_net_objects)。

### 另请参阅

- [配方 3.8，“使用 .NET 对象”](#ch03.xhtml#work_with_net_objects)

## 19.4 快速插入脚本片段

### 问题

您想要快速插入常见的 PowerShell 脚本片段。

### 解决方案

按 `Ctrl+Alt+J` 打开 Visual Studio Code 片段菜单（请参阅 [图 19-3](#ch19.xhtml#ch19_insert_snippet)）。

![](Images/wps4_1903.png)

### 讨论

即使有所有优秀的资源可用，我们有时也会忘记脚本构造（如 `switch` 语句和基于注释的帮助）的语法。为了帮助解决此问题，Visual Studio Code 包含对*片段*的支持——您只需按 `Ctrl+Alt+J` 即可插入的小段 PowerShell 脚本。输入时会自动搜索与您键入的文本匹配的片段。

默认情况下，Visual Studio Code 包含 PowerShell 语言大多数部分的片段。您也可以使用自己的片段扩展此菜单。

如果您有想要添加的多行片段，请打开**文件 → 首选项 → 用户片段**。在弹出的菜单中选择 **PowerShell**，Visual Studio Code 将打开您的自定义片段文件。下面是一个片段示例，当您在片段菜单中键入“Header”时，它允许您添加标准脚本标头：

```
"Standard script header": {
    "prefix": "header",
    "body": [
        "################################################",
        "##",
        "## My Standard Script Header",
        "##",
        "################################################"
    ],
    "description": "Insert a standard script header"
}
```

# 第 IV 部分：管理员任务

- [第 20 章，*文件和目录*](#ch20.xhtml#files_and_directories)
- [第 21 章，*Windows 注册表*](#ch21.xhtml#the_windows_registry)
- [第 22 章，*比较数据*](#ch22.xhtml#comparing_data)
- [第 23 章，*事件日志*](#ch23.xhtml#event_logs)
- [第 24 章，*进程*](#ch24.xhtml#processes)
- [第 25 章，*系统服务*](#ch25.xhtml#system_services)
- [第 26 章，*Active Directory*](#ch26.xhtml#active_directory)
- [第 27 章，*企业计算机管理*](#ch27.xhtml#enterprise_computer_management)
- [第 28 章，*CIM 和 Windows 管理规范*](#ch28.xhtml#windows_management_instrumentation)
- [第 29 章，*远程处理*](#ch29.xhtml#chapter_remoting)
- [第 30 章，*事务*](#ch30.xhtml#chapter_transactions)
- [第 31 章，*事件处理*](#ch31.xhtml#event_handling)

# 第 20 章：文件和目录

## 20.0 简介

管理系统时最常见的任务之一是处理其文件和目录。无论您是在命令行下管理计算机，还是编写脚本来自动管理它，都是如此。
