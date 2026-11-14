

幸运的是，PowerShell 让脚本处理文件和目录变得像在命令行下操作一样简单——这是许多经验丰富的程序员和脚本编写者常常忽视的一点。当你面对有限的磁盘空间，需要找出占用空间最大的文件时，这就是一个完美的例子。

典型的程序员可能会通过编写函数来扫描系统的特定目录来处理此任务。对于每个文件，他们检查该文件是否足够大以值得关注。如果是，则将其添加到列表中。对于原始目录中的每个子目录，程序员重复此过程（直到没有更多目录需要处理）。

俗话说，“你可以用任何编程语言写出 C 代码。”你带入一门语言的习惯和先入为主的观念，往往会直接影响你对该语言新特性的接受程度。

作为一个管理用的 Shell，PowerShell 直接支持诸如访问子目录中的所有文件，或将文件从一个目录移动到另一个目录等任务。那个复杂的面向程序员的脚本变成了一个单行命令：

```
Get-ChildItem -Recurse | Sort-Object -Descending Length | Select -First 10
```

在深入你最喜欢的程序员工具箱之前，先看看 PowerShell 在该领域支持什么。在许多情况下，它无需你的程序员技巧包就能处理任务。

# 20.1 确定并更改当前位置

## 问题

你想从脚本或命令中确定当前位置，或更改到不同的目录。

## 解决方案

要检索当前位置，请使用 `Get-Location` cmdlet。`Get-Location` cmdlet 提供驱动器和路径作为两个常用属性：

```
$currentLocation = (Get-Location).Path
```

作为 `(Get-Location).Path` 的简写形式，请使用 `$pwd` 自动变量。

要更改当前位置，请使用 `Set-Location` cmdlet。例如，要更改到你的主目录：

```
Set-Location ~
```

## 讨论

### 获取当前位置

`Get-Location` cmdlet 返回关于当前位置的信息。从它返回的信息中，你可以访问当前驱动器、提供程序和路径。

此当前位置会影响 PowerShell 命令以及从 PowerShell 启动的程序。但是，这不适用于与 .NET Framework 交互时。如果你需要调用与文件系统交互的 .NET 方法，请始终确保提供完全限定的路径：

```
[System.IO.File]::ReadAllText("c:\temp\file.txt")
```

如果你确定文件存在，`Resolve-Path` cmdlet 允许你将相对路径转换为绝对路径：

```
$filePath = (Resolve-Path file.txt).Path
[System.IO.File]::ReadAllText($filePath)
```

或者，

```
[System.IO.File]::ReadAllText("$pwd\file.txt")
```

如果文件不存在，请结合使用 `Join-Path` cmdlet 和 `Get-Location` cmdlet 来指定文件：

```
$filePath = Join-Path (Get-Location) file.txt
```

另一种结合了上述两种方法功能的替代方案稍微高级一些，但也允许你指定相对位置。它来自 PowerShell `$executionContext` 变量中的方法，该变量提供了通常由 cmdlet 和提供程序作者使用的功能：

```
$executionContext.SessionState.Path.`
    GetUnresolvedProviderPathFromPSPath("..\file.txt")
```

有关 `Get-Location` cmdlet 的更多信息，请输入 `Get-Help Get-Location`。

### 设置当前位置

更改当前位置的 PowerShell 命令是 `Set-Location`，别名为 `cd`。它支持三种主要操作模式：

*   `Set-Location <path>`：将当前目录更改为指定的路径。这可以是绝对路径（如 `c:\users`）或相对路径（如 `..` 或 `..\..\system32`）。请参阅 [配方 1.11](#ch01.xhtml#override_command_resolution) 以了解添加对父目录更多层级支持的方法，例如 `Set-Location ....`。`<path>` 参数支持多种快捷方式：`~`（代表你的主目录），`$pshome` 代表 PowerShell 的安装目录，甚至复杂的通配符。例如，如果你不知道某软件安装在哪个 `Program Files` 目录下，你仍然可以通过以下命令快速导航到它：`cd c:\progra*\*steam*`。

*   `Set-Location -`：将当前目录更改为你上一次访问的目录，类似于 Web 浏览器中的 Alt+Back（后退）。

*   `Set-Location +`：将当前目录更改为目录历史记录中的下一个目录，类似于 Web 浏览器中的 Alt+Forward（前进）。

要添加热键支持以在目录历史记录中前后导航，请参阅 [配方 1.10](#ch01.xhtml#customize_user_input)。

## 另请参阅

[配方 1.11，“自定义 PowerShell 的命令解析行为”](#ch01.xhtml#override_command_resolution)

[配方 1.10，“自定义 PowerShell 的用户输入行为”](#ch01.xhtml#customize_user_input)

# 20.2 获取目录中的文件

## 问题

你想获取或列出目录中的文件。

## 解决方案

要检索目录中的文件列表，请使用 `Get-ChildItem` cmdlet。要获取特定项目，请使用 `Get-Item` cmdlet：

*   要列出当前目录中的所有项目，请使用 `Get-ChildItem` cmdlet：

    ```
    Get-ChildItem
    ```

*   要列出所有匹配通配符的项目，请向 `Get-ChildItem` cmdlet 提供通配符：

    ```
    Get-ChildItem *.txt
    ```

*   要列出当前目录（及其所有子目录）中匹配通配符的所有文件，请使用 `Get-ChildItem` cmdlet 的 `-Recurse` 参数：

    ```
    Get-ChildItem *.txt -Recurse
    ```

*   要列出当前目录中的所有目录，请使用 `-Attributes` 参数：

    ```
    Get-ChildItem -Attributes Directory
    dir -ad
    ```

*   要获取特定项目的信息，请使用 `Get-Item` cmdlet：

    ```
    Get-Item test.txt
    ```

## 讨论

虽然最常用于文件系统，但 `Get-ChildItem` 和 `Get-Item` cmdlet 实际上适用于所有 PowerShell 驱动器上的任何项目。除了 `A:` 到 `Z:`（标准文件系统驱动器）外，它们还适用于 `Alias:`、`Cert:`、`Env:`、`Function:`、`HKLM:`、`HKCU:` 和 `Variable:`。

虽然解决方案演示了 `Get-ChildItem` cmdlet 支持的一些简单通配符场景，但 PowerShell 实际上支持更多高级场景。有关这些场景的更多信息，请参阅 [配方 20.6](#ch20.xhtml#find_files_that_match_a_pattern)。一个特别值得注意的点是，解决方案中的第三个示例列出了目录及其所有子目录中匹配通配符的文件。该示例适用于任何 PowerShell 提供程序。但是，如果你使用特定于提供程序的筛选器，PowerShell 可以更快地检索结果，如 [配方 20.6](#ch20.xhtml#find_files_that_match_a_pattern) 所述。

当你在文件系统中工作时，`Get-ChildItem` cmdlet 暴露了几个参数（`-Directory`、`-File`、`-ReadOnly`、`-Hidden` 和 `-System`），使筛选变得尽可能简单。这些参数也有别名（例如 `-ad`), 使大多数常见任务变得轻而易举：

```
Get-ChildItem -Directory
dir -ad
```

对于不太常见的属性，`-Attributes` 参数支持针对所有其他文件和目录属性的强大筛选。在最基本的层面上，你可以提供任何标准文件属性。PowerShell 仅返回设置了该属性的文件：

```
Get-ChildItem -Attributes Compressed
```

要返回未设置某属性的项目（“非”场景），请使用感叹号：

```
Get-ChildItem -Attributes !Archive
```

要返回设置了*任意*多个属性的项目（“或”场景），请使用逗号：

```
Get-ChildItem -Attributes "Hidden, ReadOnly"
```

要返回设置了*所有*指定属性的项目（“与”场景），请使用加号：

```
Get-ChildItem -Attributes "ReadOnly + Hidden"
```

