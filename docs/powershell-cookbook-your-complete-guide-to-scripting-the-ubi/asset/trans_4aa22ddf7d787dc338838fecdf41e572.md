

在管理系统时，你自然会花费大量时间处理该系统上的文件。你想对这些文件执行的许多操作都很简单：获取其内容、搜索其中的模式，或替换其中的文本。

即使对于这些简单的操作，PowerShell 面向对象的特性也增加了几种独特且强大的变体。

# 9.1 获取文件内容

## 问题

你想获取文件的内容。

## 解决方案

将文件名作为参数提供给 `Get-Content` cmdlet：

```
PS > $content = Get-Content c:\temp\file.txt
```

将文件名放在 `${}` 部分中，以使用 `Get-Content` cmdlet 的变量语法：

```
PS > $content = ${c:\temp\file.txt}
```

将文件名作为参数提供给 `ReadAllLines()` 或 `ReadAllText()` 方法，以使用来自 .NET Framework 的 `System.IO.File` 类：

```
PS > $content = Get-Content c:\temp\file.txt -Raw
PS > $contentLines = [System.IO.File]::ReadAllLines("c:\temp\file.txt")
```

## 讨论

PowerShell 提供了三种主要方式来获取文件内容。第一种是 `Get-Content` cmdlet——专为此目的设计的 cmdlet。事实上，`Get-Content` cmdlet 适用于任何支持“具有内容的项”概念的 PowerShell 驱动器。这包括 `Alias:`、`Function:` 等。第二种和第三种方式分别是 `Get-Content` 变量语法和 `ReadAllText()` 方法。

在处理文件时，`Get-Content` cmdlet 逐行返回文件内容。当它这样做时，PowerShell 会提供关于该输出行的附加信息。这些信息作为属性附加到每个输出行上，包括该行来源的驱动器和路径等。

### 注意

如果你希望 PowerShell 根据你选择的字符串（而不是默认的换行符）来分割文件内容，`Get-Content` cmdlet 的 `-Delimiter` 参数允许你提供一个。

虽然有用，但当你不使用这些附加信息时，PowerShell 附加这些额外信息有时会拖慢处理大文件的脚本速度。如果你需要更快地处理大文件，`Get-Content` cmdlet 的 `ReadCount` 参数允许你控制 PowerShell 一次从文件中读取多少行。`ReadCount` 为 1（默认值）时，PowerShell 逐行返回。`ReadCount` 为 2 时，PowerShell 一次返回两行。`ReadCount` 小于 1 时，PowerShell 一次性返回文件中的所有行。

### 警告

对于极大文件，请小心使用小于 1 的 `ReadCount`。`Get-Content` cmdlet 的优势之一是其流式行为。无论文件多大，你仍能处理文件的每一行，而不会耗尽系统的所有内存。由于小于 1 的 `ReadCount` 会在返回任何结果前读取整个文件，大文件有可能耗尽系统内存。有关如何有效利用 PowerShell 流式处理能力的更多信息，请参阅 [食谱 5.16](#ch05.xhtml#generate_large_reports_and_text_streams)。

如果性能是主要考量，来自 .NET Framework 的 `[System.IO.File]::ReadAllLines()` 方法会返回文件的所有行，但不会将附加的（有时有用的）属性附加到每一行。此方法在给你访问权限前也会将整个文件加载到内存中，因此可能不适合极大文件。

当你想一次性处理文件的全部内容（而不将其拆分为行）时，请使用 `Get-Content` cmdlet 的 `-Raw` 参数：

```
$rawContent = Get-Content c:\temp\file.txt -Raw
```

有关 `Get-Content` cmdlet 的更多信息，请输入 `Get-Help Get-Content`。有关如何处理更结构化的文件（如 XML 和 CSV）的信息，请参阅 [第 10 章](#ch10.xhtml#structured_files)。有关如何处理二进制文件的更多信息，请参阅 [食谱 9.6](#ch09.xhtml#parse_and_manage_binary_files)。

## 另请参阅

[食谱 5.16，“生成大型报告和文本流”](#ch05.xhtml#generate_large_reports_and_text_streams)

[食谱 9.6，“解析和管理二进制文件”](#ch09.xhtml#parse_and_manage_binary_files)

[第 10 章](#ch10.xhtml#structured_files)

# 9.2 将命令输出存储到文件中

## 问题

你想将命令或管道的输出重定向到文件中。

## 解决方案

要将命令的输出重定向到文件，请使用 `Out-File` cmdlet 或重定向操作符之一。

`Out-File:`

```
Get-ChildItem | Out-File unicodeFile.txt
Get-Content filename.cs | Out-File -Encoding ASCII file.txt
Get-ChildItem | Out-File -Width 120 unicodeFile.cs
```

重定向操作符：

```
Get-ChildItem > files.txt
Get-ChildItem 2> errors.txt
Get-ChildItem n> otherStreams.txt
```

## 讨论

`Out-File` cmdlet 和重定向操作符有许多共同之处。在大多数情况下，你可以使用任意一种。重定向操作符的独特之处在于，它们能最大程度地控制单个流的重定向。`Out-File` cmdlet 的独特之处主要在于，它让你能轻松配置格式化宽度和编码。

### 注意

如果你想将命令的**对象**保存到文件中（而不是你在屏幕上看到的基于文本的表示形式），请参阅 [食谱 10.5](#ch10.xhtml#easily_import_and_export_your_structured)。

默认格式化宽度和默认输出编码是输出重定向的两个方面，有时会导致困难。

默认格式化宽度有时会导致问题，因为将 PowerShell 格式化的输出重定向到文件旨在模仿你在屏幕上看到的内容。如果你的屏幕宽 80 个字符，文件也将宽 80 个字符。PowerShell 格式化输出的示例包括目录列表（隐式地格式化为表）以及你使用一组 `Format-*` cmdlet 显式格式化的任何命令。如果这导致问题，你可以使用 `Out-File` cmdlet 上的 `-Width` 参数自定义文件的宽度。

默认输出编码有时会导致意外结果，因为 PowerShell 默认使用 UTF-16 Unicode 编码创建所有文件。这使得 PowerShell 能够完全支持整个范围的国际字符、cmdlet 和输出。虽然这比传统 Shell 有了巨大改进，但在对 ASCII 源代码文件运行大规模搜索和替换操作时，例如，可能会造成不必要的惊讶。要强制 PowerShell 以 ASCII 编码将输出发送到文件，请使用 `Out-File` cmdlet 上的 `-Encoding` 参数。

有关 `Out-File` cmdlet 的更多信息，请输入 `Get-Help Out-File`。有关支持的重定向操作符的完整列表，请参阅 [“捕获输出”](#app01.xhtml#capturing_output)。

## 另请参阅

[食谱 10.5，“轻松导入和导出结构化数据”](#ch10.xhtml#easily_import_and_export_your_structured)

[“捕获输出”](#app01.xhtml#capturing_output)

# 9.3 将信息添加到文件末尾

## 问题

你想将管道的输出重定向到文件中，但将信息添加到该文件的末尾。

## 解决方案

要将命令的输出重定向到文件并追加内容，请使用 `Out-File` cmdlet 的 `-Append` 参数或 [“捕获输出”](#app01.xhtml#capturing_output) 中描述的追加重定向操作符。两者都支持将文本追加到文件末尾的选项。

`Out-File:`

```
Get-ChildItem | Out-File -Append files.txt
```

重定向操作符：

```
Get-ChildItem >> files.txt
```

## 讨论

`Out-File` cmdlet 和重定向操作符有许多共同之处。在大多数情况下，你可以使用任意一种。有关这两种方法的更详细比较，包括选择其中一种而非另一种的原因，请参阅 [食谱 9.2](#ch09.xhtml#store_the_output_of_a_command_into_a_fil) 中的讨论。

## 另请参阅

