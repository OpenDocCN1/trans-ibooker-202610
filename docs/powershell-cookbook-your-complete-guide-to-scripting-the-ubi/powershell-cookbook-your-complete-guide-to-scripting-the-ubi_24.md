# 第 19 章 Visual Studio Code

## 19.0 简介

虽然文本模式的 PowerShell 以其高效性和自动化能力著称，但其用户界面 (UI) 却鲜有优点可言。就控制台应用程序而言（尤其是在 Windows Terminal 中），它表现惊艳。但与传统集成开发环境 (IDE) 提供的体验相比，它还有很长的路要走。

所有这些仅仅是 `pwsh.exe` 作为控制台应用程序的简单副作用。这些问题影响每个操作系统上的每个控制台应用程序，且可能永远都会存在。

除了 UI 的怪癖之外，控制台应用程序的致命缺陷在于其缺乏对 Unicode 标准的完整支持：即大多数国际语言表示其字母表的方式。虽然 Windows 控制台支持少量基本的非英语字符（如带重音符号的字母），但对其他字符几乎不提供完整支持。Windows Terminal 应用程序极大地改善了这一点，但其覆盖范围仍不完整。

这对全球管理员来说是个大问题！由于直接在命令行输入国际字符非常困难，许多国家的管理员被迫在记事本中编写脚本以获得完整的 Unicode 支持，然后使用 PowerShell 运行脚本，即使该命令最终只有一行。

PowerShell 通过提供与 Visual Studio Code 的全方位集成来解决这些问题。

Visual Studio Code 为 PowerShell 提供了现代应用程序应有的 UI，支持完整的 Unicode 输入和多标签页会话，并提供了极佳的交互式调试体验。

从概念上讲，Visual Studio Code 由两个主要组件组成（如 图 19-1 所示）：

**编辑器**

: 编辑器是 Visual Studio Code 的顶部窗格，旨在进行多行脚本编辑和创建。它提供行号和语法高亮，并支持极佳的调试体验。

脚本窗格的一个独特之处在于它支持*选择性执行*：即仅运行你所高亮的部分，而非你正在处理的整个脚本。这使得脚本编写变得轻而易举。当你开始编写脚本时，可以交互式地尝试命令，直到它们正确无误。一旦它们按预期工作，你可以保留它们，继续前进，然后继续逐块构建你的脚本。正如你从 PowerShell 的控制台 Shell 所期望的那样，脚本窗格中的脚本编辑支持命令、参数、路径等的 Tab 补全。

**控制台面板**

: 位于应用程序底部的控制台面板，是你在 Visual Studio Code 中进行大多数交互式会话的地方。就像 PowerShell 控制台中的命令提示符一样，控制台面板支持 Tab 补全。

与 Windows Terminal 一样，如果你发现命令变得太长，可以按 Shift-Enter 为当前命令启用多行编辑。

如果你希望 Visual Studio Code 看起来像 PowerShell 集成脚本环境 (ISE)，只需选择“查看”→“命令面板”，然后搜索“PowerShell: Enable ISE Mode”（启用 ISE 模式）。

除了这些功能外，Visual Studio Code 还提供广泛的自定义、脚本编写和远程支持。

![](img/wps4_1402.png)

## 19.1 调试脚本

### 问题

你想通过一个比其 `*-PsBreakpoint` cmdlet 更友好的界面来使用 PowerShell 的调试命令。

### 解决方案

使用 Visual Studio Code 中的“运行”菜单来添加和移除断点，并管理 PowerShell 遇到断点时的调试行为。

### 讨论

Visual Studio Code 为你提供了一组丰富的交互式图形调试命令，帮助你诊断脚本中的错误。它通过“运行”菜单暴露这些功能，其行为类似于你可能有经验的许多其他图形调试环境。图 19-2 显示了 Visual Studio Code 中可用的调试选项。

![](img/wps4_1902.png)

要设置断点，请先保存脚本。然后，选择“切换断点”菜单项，或点击你想要停止的那一行旁边、行号左侧的位置。一旦 PowerShell 在你的脚本中命中断点，它会暂停执行，让你检查变量、脚本状态以及其他你感兴趣的内容。要控制执行流程，可以使用单步执行命令：“单步跳过”、“单步进入”和“单步跳出”。

“单步跳过”继续执行脚本的下一行，执行（但不调试进入）遇到的任何函数调用。“单步进入”继续执行脚本的下一行，调试进入遇到的任何函数调用。如果你正在一个函数中，“单步跳出”命令让 PowerShell 完成该函数的执行，并在函数完成后恢复调试。

在 Visual Studio Code 中调试的一个独特之处在于，它完全建立在 第 14 章 中讨论的核心调试 cmdlet 之上。你从调试菜单所做的更改（如添加断点）会立即反映在 cmdlet 中（如列出断点）。同样，你从集成命令行添加或修改的断点会显示在 UI 中，就如同你是从“调试”菜单本身创建的一样。

#### 注意

事实上，PowerShell 断点 cmdlet 暴露的功能在许多情况下超过了 Visual Studio Code 调试菜单暴露的功能。例如，`Set-PsDebug` cmdlet 支持命令断点、条件断点、变量断点等。有关 `Set-PsDebug` cmdlet 的更多信息，请参阅 食谱 14.4。

与大多数图形调试环境不同，Visual Studio Code 在调试脚本时，使得调查脚本的动态状态变得难以置信地简单。有关如何在调试时调查脚本状态的更多信息，请参阅 食谱 14.7。

### 另请参阅

食谱 14.7，“调试时调查系统状态”

第 14 章




## 19.2 连接到远程计算机

### 问题

您想要通过 Visual Studio Code 与远程 PowerShell 会话及其文件进行交互。

### 解决方案

从终端窗口，使用 `Enter-PSSession` cmdlet。

### 讨论

当您在 Visual Studio Code 中创建远程 PowerShell 会话时，PowerShell 扩展会自动识别您已建立连接，并为您的编辑器会话提供额外的远程支持。

一旦您连接了远程会话，在终端窗口中与该远程系统交互就像与本地系统交互一样。来自远程系统的提示显示得就像来自本地系统的提示一样，进度条、凭据请求和 PowerShell 的其他反馈机制也是如此。

如果您的远程脚本命中调试断点，Visual Studio Code 会自动将该脚本的副本拉取到您的本地计算机，并为您提供交互式调试体验，就好像您在自己的计算机上拥有该脚本一样。您可以检查系统状态、单步进入和跳出函数，甚至编辑脚本本身。有关 PowerShell 对远程调试的支持的更多信息，请参阅 配方 14.8。

如果您想要编辑远程系统上的文件，可以使用 Visual Studio Code 的 `psedit` 命令。例如：

```
[localhost]: PS D:\Lee> psedit c:\temp\scratch.txt
(scratch.txt opens in your local Visual Studio Code editor)
```

Visual Studio Code 会自动将您对该文件所做的任何更改镜像回远程系统。

有关 PowerShell 远程处理的更多信息，请参阅 第 29 章。

### 另请参阅

- 配方 14.8，“在远程计算机上调试脚本”
- 第 29 章

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
Tokens        Property   System.Collections.Generic.IReadOnlyListSystem.Manage...
Uri           Property   uri Uri {get;}
WorkspacePath Property   string WorkspacePath {get;}
```

通过利用对象模型，您可以编写工具来自动处理脚本（例如，注释和取消注释脚本区域、处理脚本输出等）。

有关使用 .NET 对象的更多信息，请参阅 [配方 3.8。

### 另请参阅

- 配方 3.8，“使用 .NET 对象”

## 19.4 快速插入脚本片段

### 问题

您想要快速插入常见的 PowerShell 脚本片段。

### 解决方案

按 `Ctrl+Alt+J` 打开 Visual Studio Code 片段菜单（请参阅 图 19-3）。

![](img/wps4_1903.png)

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
