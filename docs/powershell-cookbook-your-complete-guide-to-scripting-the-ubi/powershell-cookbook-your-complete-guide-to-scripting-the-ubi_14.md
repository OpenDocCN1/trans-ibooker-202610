# 第 9 章. 简单文件

## 9.0 简介




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

对于极大文件，请小心使用小于 1 的 `ReadCount`。`Get-Content` cmdlet 的优势之一是其流式行为。无论文件多大，你仍能处理文件的每一行，而不会耗尽系统的所有内存。由于小于 1 的 `ReadCount` 会在返回任何结果前读取整个文件，大文件有可能耗尽系统内存。有关如何有效利用 PowerShell 流式处理能力的更多信息，请参阅 食谱 5.16。

如果性能是主要考量，来自 .NET Framework 的 `[System.IO.File]::ReadAllLines()` 方法会返回文件的所有行，但不会将附加的（有时有用的）属性附加到每一行。此方法在给你访问权限前也会将整个文件加载到内存中，因此可能不适合极大文件。

当你想一次性处理文件的全部内容（而不将其拆分为行）时，请使用 `Get-Content` cmdlet 的 `-Raw` 参数：

```
$rawContent = Get-Content c:\temp\file.txt -Raw
```

有关 `Get-Content` cmdlet 的更多信息，请输入 `Get-Help Get-Content`。有关如何处理更结构化的文件（如 XML 和 CSV）的信息，请参阅 第 10 章。有关如何处理二进制文件的更多信息，请参阅 食谱 9.6。

## 另请参阅

食谱 5.16，“生成大型报告和文本流”

食谱 9.6，“解析和管理二进制文件”

第 10 章

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

如果你想将命令的**对象**保存到文件中（而不是你在屏幕上看到的基于文本的表示形式），请参阅 食谱 10.5。

默认格式化宽度和默认输出编码是输出重定向的两个方面，有时会导致困难。

默认格式化宽度有时会导致问题，因为将 PowerShell 格式化的输出重定向到文件旨在模仿你在屏幕上看到的内容。如果你的屏幕宽 80 个字符，文件也将宽 80 个字符。PowerShell 格式化输出的示例包括目录列表（隐式地格式化为表）以及你使用一组 `Format-*` cmdlet 显式格式化的任何命令。如果这导致问题，你可以使用 `Out-File` cmdlet 上的 `-Width` 参数自定义文件的宽度。

默认输出编码有时会导致意外结果，因为 PowerShell 默认使用 UTF-16 Unicode 编码创建所有文件。这使得 PowerShell 能够完全支持整个范围的国际字符、cmdlet 和输出。虽然这比传统 Shell 有了巨大改进，但在对 ASCII 源代码文件运行大规模搜索和替换操作时，例如，可能会造成不必要的惊讶。要强制 PowerShell 以 ASCII 编码将输出发送到文件，请使用 `Out-File` cmdlet 上的 `-Encoding` 参数。

有关 `Out-File` cmdlet 的更多信息，请输入 `Get-Help Out-File`。有关支持的重定向操作符的完整列表，请参阅 “捕获输出”。

## 另请参阅

食谱 10.5，“轻松导入和导出结构化数据”

“捕获输出”

# 9.3 将信息添加到文件末尾

## 问题

你想将管道的输出重定向到文件中，但将信息添加到该文件的末尾。

## 解决方案

要将命令的输出重定向到文件并追加内容，请使用 `Out-File` cmdlet 的 `-Append` 参数或 “捕获输出” 中描述的追加重定向操作符。两者都支持将文本追加到文件末尾的选项。

`Out-File:`

```
Get-ChildItem | Out-File -Append files.txt
```

重定向操作符：

```
Get-ChildItem >> files.txt
```

## 讨论

`Out-File` cmdlet 和重定向操作符有许多共同之处。在大多数情况下，你可以使用任意一种。有关这两种方法的更详细比较，包括选择其中一种而非另一种的原因，请参阅 食谱 9.2 中的讨论。

## 另请参阅





食谱 9.2，“将命令输出存储到文件中”

“捕获输出”

# 9.4 在文件中搜索文本或模式

## 问题

你想要在文件中查找字符串或正则表达式。

## 解决方案

要在文件中搜索精确匹配（但不区分大小写），请使用 `Select-String` cmdlet 的 `-Simple` 参数：

```
Select-String -Simple SearchText file.txt
```

要在文件中搜索正则表达式，请将该模式提供给 `Select-String` cmdlet：

```
Select-String "\(...\) ...-...." phone.txt
```

要递归搜索所有 `*.txt` 文件中的正则表达式，请将 `Get-ChildItem` 的结果通过管道传递给 `Select-String` cmdlet：

```
Get-ChildItem *.txt -Recurse | Select-String pattern
```

或者，使用内置别名：

```
dir *.txt -rec | sls pattern
```

## 讨论

`Select-String` cmdlet 是在文件中搜索模式或特定字符串的最简单方法。与支持相同类型功能的传统文本匹配实用程序（如 `grep`）不同，`Select-String` cmdlet 返回的匹配项包含关于匹配项本身的详细信息：

```
PS > $matches = Select-String "output file" transcript.txt
PS > $matches | Select LineNumber,Line

                        LineNumber Line
                        ---------- ----
                                 7 Transcript started, output file...
```

使用正则表达式匹配时，你通常想要准确找出正则表达式匹配的文本。PowerShell 在结果的 `Matches` 属性中捕获此信息。对于每个匹配项，`Value` 属性代表你的模式所匹配的文本：

```
PS > Select-String "\(...\) ...-...." phone.txt | Select -Expand Matches

...
Value    : (425) 555-1212

...
Value    : (416) 556-1213
```

如果你的正则表达式定义了组（括在括号中的模式部分），你可以通过 `Groups` 属性访问这些组匹配的文本。第一组（`Group[0]`）代表你的模式匹配的所有文本。其他组（1 及以上）代表你定义的组。在这种情况下，我们在区号周围添加额外的括号来捕获它：

```
PS > Select-String "\((...)\) ...-...." phone.txt |
    Select -Expand Matches | Foreach { $_.Groups[1] }

Success  : True
Captures : {425}
Index    : 1
Length   : 3
Value    : 425

Success  : True
Captures : {416}
Index    : 1
Length   : 3
Value    : 416
```

如果你的正则表达式定义了*命名捕获*（在组的开头带有文本 `?<Name>`），`Groups` 集合允许你按名称访问它们。在此示例中，我们使用 `AreaCode` 作为捕获名称来捕获区号：

```
PS > Select-String "\((?<AreaCode>...)\) ...-...." phone.txt |
     Select -Expand Matches | Foreach { $_.Groups["AreaCode"] }

Success  : True
Captures : {425}
Index    : 1
Length   : 3
Value    : 425

Success  : True
Captures : {416}
Index    : 1
Length   : 3
Value    : 416
```

默认情况下，`Select-String` cmdlet 每行输入仅捕获第一个匹配项。如果输入每行可能有多个匹配项，请使用 `-AllMatches` 参数：

```
PS > Get-Content phone.txt
(425) 555-1212
(416) 556-1213 (416) 557-1214

PS > Select-String "\((...)\) ...-...." phone.txt |
    Select -Expand Matches | Select -Expand Value

(425) 555-1212
(416) 556-1213

PS > Select-String "\((...)\) ...-...." phone.txt -AllMatches |
    Select -Expand Matches | Select -Expand Value

(425) 555-1212
(416) 556-1213
(416) 557-1214
```

有关捕获、命名捕获和正则表达式其他方面的更多信息，请参阅 附录 B。

> **注意**
>
> 如果你需要的信息位于与包含匹配项不同的行上，请使用 `-Context` 参数，以便在 `Select-String` 的输出中包含该行。PowerShell 将结果放在 `Select-String` 输出的 `Context.PreContext` 和 `Context.PostContext` 属性中。
>
> 如果你想搜索特定扩展名的多个文件，`Select-String` cmdlet 允许你在文件名上使用通配符（如 `*.txt`）。对于更复杂的文件列表（包括搜索目录中的所有文件），通常最好使用 `Get-ChildItem` cmdlet 生成文件列表，如前面的解决方案所示。
>
> 由于 `Select-String` cmdlet 为找到的每个匹配项输出文件名、行号和匹配行，此输出有时可能包含过多细节。一个完美的例子是当你搜索包含特定字符串的二进制文件时。二进制文件（如 DLL 或 EXE）作为文本显示时很少有意义，因此你的屏幕会迅速充满明显的乱码。
>
> 解决此问题的方法来自 `Select-String` 的 `-Quiet` 开关。它仅根据文件是否包含该字符串返回 `true` 或 `false`。因此，要查找当前目录中包含文本“Debug”的 DLL 或 EXE：
>
> ```
> Get-ChildItem | Where { $_ | Select-String "Debug" -Quiet }
> ```
>
> 搜索文件文本的另外两个常用工具是 `-match` 运算符和带有 `-file` 选项的 `switch` 语句。有关这些的更多信息，请参阅 食谱 5.7 和 食谱 4.3。有关 `Select-String` cmdlet 的更多信息，请输入 `Get-Help Select-String`。

## 另请参阅

食谱 4.3，“使用 Switch 管理大型条件语句”

食谱 5.7，“在字符串中搜索文本或模式”

附录 B，*正则表达式参考*

# 9.5 解析和管理基于文本的日志文件

## 问题

你想要使用 PowerShell 的标准基于对象的命令来解析和分析基于文本的日志文件。

## 解决方案

使用 食谱 5.15 中描述的 `ConvertFrom-String` cmdlet 来处理基于文本的日志文件。在你的协助下，它将文本流转换为对象流，然后你可以使用 PowerShell 的标准命令轻松地处理它们。

## 讨论

当你解析日志文件时，`ConvertFrom-String` 脚本主要接受两个参数：

*   一个描述如何将传入文本分割成组的正则表达式
*   一个属性名称列表，脚本随后将这些名称分配给那些文本组

正如示例 9-1 所示，你可以使用 Windows 目录中的防火墙日志。如果启用，这些日志会跟踪计算机上的入站和出站网络连接。

### 示例 9-1. 检查 Windows 防火墙日志

```
PS C:\WINDOWS\system32> Get-Content .\Logfiles\Firewall\pfirewall.log -Head 10
#Version: 1.5
#Software: Microsoft Windows Firewall
#Time Format: Local
#Fields: date time action protocol src-ip dst-ip src-port dst-port size tcpflags tcpsyn

2020-12-22 15:49:56 ALLOW UDP 192.168.1.132 208.67.222.222 51411 53 0 - - - - SEND
2020-12-22 15:49:57 ALLOW TCP 192.168.1.251 192.168.1.132 43223 32400 0 - 0 0 RECEIVE
2020-12-22 15:50:00 ALLOW TCP 192.168.1.251 192.168.1.132 43231 32400 0 - 0 0 RECEIVE
2020-12-22 15:50:01 ALLOW UDP 192.168.1.132 208.67.222.222 49998 53 0 - - - - SEND
2020-12-22 15:50:02 ALLOW TCP 192.168.1.132 168.62.58.130 58406 443 0 - 0 0 0 SEND
(...)
```

像大多数日志文件一样，文本的格式非常规则但难以管理。在本例中，你有 10 个似乎已填充的字段，还有一些未填充的。

幸运的是，此日志记录了其字段，因此我们可以将它们存储到一个数组中：

```
$fields = -split ("date time action protocol src-ip dst-ip src-port dst-port size " +
    "tcpflags tcpsyn tcpack tcpwin icmptype icmpcode info path")
```





### 另请参阅

食谱 5.15，“将文本流转换为对象”

附录 B，`正则表达式参考`

## 9.6 解析和管理二进制文件

### 问题

您想要处理文件中的二进制数据。

### 解决方案

在文件中处理二进制数据时，主要有两种技术。第一种是使用 `Byte` 编码读取文件，这样 PowerShell 就不会将内容视为文本。第二种是使用 `BitConverter` 类将这些字节来回转换为您更常关心的数字。

示例 9-2 显示 Windows 可执行文件的 `可移植可执行 (PE) 头`。任何可执行文件（`.dll`、`.exe` 或其他几种）的开头部分都以一个称为 `可移植可执行 (PE) 头` 的二进制部分开始——其中包含通用对象文件格式 (COFF) 头。此头的一部分包含关于该文件的特征，例如该文件是否为 DLL。

有关 PE 头格式的更多信息，请参阅 [PE 头格式规范](https://oreil.ly/u833N)。

#### 示例 9-2. Get-Characteristics.ps1

```
##############################################################################
##
## Get-Characteristics
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

获取 PE 可执行文件格式中文件的文件特征。

.EXAMPLE

PS > Get-Characteristics $env:WINDIR\notepad.exe
IMAGE_FILE_LOCAL_SYMS_STRIPPED
IMAGE_FILE_RELOCS_STRIPPED
IMAGE_FILE_EXECUTABLE_IMAGE
IMAGE_FILE_32BIT_MACHINE
IMAGE_FILE_LINE_NUMS_STRIPPED

#>

param(
    ## 要检查的文件路径
    [Parameter(Mandatory = $true)]
    [string] $Path
)

Set-StrictMode -Version 3

## 定义 PE 文件头中使用的特征。
## 取自：
## http://www.microsoft.com/whdc/system/platform/firmware/PECOFF.mspx
$characteristics = @{}
$characteristics["IMAGE_FILE_RELOCS_STRIPPED"] = 0x0001
$characteristics["IMAGE_FILE_EXECUTABLE_IMAGE"] = 0x0002
$characteristics["IMAGE_FILE_LINE_NUMS_STRIPPED"] = 0x0004
$characteristics["IMAGE_FILE_LOCAL_SYMS_STRIPPED"] = 0x0008
$characteristics["IMAGE_FILE_AGGRESSIVE_WS_TRIM"] = 0x0010
$characteristics["IMAGE_FILE_LARGE_ADDRESS_AWARE"] = 0x0020
$characteristics["RESERVED"] = 0x0040
$characteristics["IMAGE_FILE_BYTES_REVERSED_LO"] = 0x0080
$characteristics["IMAGE_FILE_32BIT_MACHINE"] = 0x0100
$characteristics["IMAGE_FILE_DEBUG_STRIPPED"] = 0x0200
$characteristics["IMAGE_FILE_REMOVABLE_RUN_FROM_SWAP"] = 0x0400
$characteristics["IMAGE_FILE_NET_RUN_FROM_SWAP"] = 0x0800
$characteristics["IMAGE_FILE_SYSTEM"] = 0x1000
$characteristics["IMAGE_FILE_DLL"] = 0x2000
$characteristics["IMAGE_FILE_UP_SYSTEM_ONLY"] = 0x4000
$characteristics["IMAGE_FILE_BYTES_REVERSED_HI"] = 0x8000

## 获取文件内容，作为字节数组
$fileBytes = Get-Content $path -ReadCount 0 -AsByteStream

## 文件中签名的偏移量存储在位置 0x3c 处。
$signatureOffset = [BitConverter]::ToUint32($fileBytes, 0x3c)

## 确保它是 PE 文件
$signature = [char[]] $fileBytes[$signatureOffset..($signatureOffset + 3)]
if(($signature -join '') -ne "PE`0`0")
{
    throw "此文件不符合 PE 规范。"
}

## COFF 头的位置在签名之后 4 字节处
$coffHeader = $signatureOffset + 4

## 特征数据在 COFF 头之后 18 字节处。BitConverter 类管理将 4 字节转换为整数。
$characteristicsData = [BitConverter]::ToInt32($fileBytes, $coffHeader + 18)

## 遍历每个特征。如果文件中的数据设置了该标志，则输出该特征。
foreach($key in $characteristics.Keys)
{
    $flag = $characteristics[$key]
    if(($characteristicsData -band $flag) -eq $flag)
    {
        $key
    }
}
```

### 讨论

对于大多数文件，这种技术是处理二进制数据最简单的方法。如果您确实修改了二进制数据，在将其写回磁盘时，您也需要使用 `Byte` 编码：

```
$fileBytes | Set-Content modified.exe -AsByteStream
```

不过，对于极大的文件，将整个文件加载到内存中进行处理可能会慢到无法接受。如果您开始遇到此限制，解决方案是使用 .NET Framework 中的文件管理类。这些类包括 `BinaryReader`、`StreamReader` 等。有关使用 .NET Framework 类的更多信息，请参阅 食谱 3.8。有关运行脚本的更多信息，请参阅 食谱 1.2。

### 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 3.8，“使用 .NET 对象”

## 9.7 创建和管理临时文件

### 问题

您想要为临时目的创建文件或管理已存在的临时文件。

### 解决方案




# 创建临时文件

使用 `New-TemporaryFile` cmdlet 创建临时文件：

```
$file = New-TemporaryFile
$file | Set-Content "Some temporary content"
(... use the file ...)
$file | Remove-Item
```

使用 `temp:` PowerShell 驱动器管理已存在的临时文件。查找所有超过 30 天的临时文件：

```
dir temp: | Where-Object LastWriteTime -lt ((Get-Date).AddDays(-30))
```

## 讨论

创建临时文件用于临时用途是很常见的。例如，你可能想要在文件中搜索并替换文本。对大文件执行此操作需要临时文件（参见配方 9.8）。另一个例子是 配方 2.4 中使用的临时文件。

通常，人们会在能想到的任何地方创建临时文件：在 `C:\\`、脚本的当前位置或其他许多地方。虽然这在作者的系统上可能工作，但在其他地方很少能良好运行。例如，如果用户不使用管理员账户进行日常任务，你的脚本将无法访问 `C:\\` 并会失败。

另一个困难来自于尝试为临时文件创建唯一名称。如果脚本只是硬编码一个名称（无论包含多少随机字符），同时运行两个副本时就会失败。你甚至可能编写一个足够智能的脚本来搜索不存在的文件名、创建它并使用它。不幸的是，如果另一个脚本副本在你看到文件不存在后但在你实际创建文件前创建了该文件，这仍然会中断。

最后，如果脚本将临时文件写入其他用户可读或可写的位置，可能会引入多个安全漏洞。

幸运的是，`New-TemporaryFile` cmdlet 为你解决了这些问题。它以可靠且安全的方式在可靠位置创建唯一文件名。该方法返回一个文件对象，你可以按需使用。

> **注意**
>
> 当脚本不再需要该文件时，请记得删除它；否则，脚本会浪费磁盘空间并在用户系统上造成不必要的混乱。记住：你的脚本应该解决管理员的问题，而不是制造问题！

默认情况下，`New-TemporaryFile` cmdlet 返回一个带有 `.tmp` 扩展名的文件。对于大多数用途，文件扩展名无关紧要，这工作良好。在极少数需要创建特定扩展名文件的情况下，使用 `Rename-Item` cmdlet 重命名临时文件。以下示例创建一个使用 `.cs` 扩展名的新临时文件：

```
$file = New-TemporaryFile
$file = $file | Rename-Item -NewName { $_.Name + ".cs" } -PassThru

(... use the file ...)

$file | Remove-Item
```

当你想要在系统通用临时文件位置管理临时文件时，可以使用 `temp:` PowerShell 驱动器。在 Windows 上，这等同于 `$env:TEMP` 位置。在 Linux 机器上，这是 `/tmp`。有关在自定义 PowerShell 驱动器中处理文件的更多信息，请参见 配方 20.15。

## 另请参阅

- 配方 2.4，“以交互方式筛选对象列表”
- 配方 9.8，“在文件中搜索和替换文本”
- 配方 20.15，“创建和映射 PowerShell 驱动器”

# 9.8 在文件中搜索和替换文本

## 问题

你想要在文件中搜索文本并将该文本替换为新内容。

## 解决方案

要在文件中搜索和替换文本，首先将文件内容存储在变量中，然后将替换后的文本存回该文件，如示例 9-3 所示。

### 示例 9-3. 在文件中替换文本

```
PS > $filename = "file.txt"
PS > $match = "source text"
PS > $replacement = "replacement text"
PS >
PS > $content = Get-Content $filename
PS > $content
This is some source text that we want
to replace. One of the things you may need
to be careful about with Source
Text is when it spans multiple lines,
and may have different Source Text
capitalization.
PS >
PS > $content = $content -creplace $match,$replacement
PS > $content
This is some replacement text that we want
to replace. One of the things you may need
to be careful about with Source
Text is when it spans multiple lines,
and may have different Source Text
capitalization.
PS > $content | Set-Content $filename
```

## 讨论

使用 PowerShell 在文件（或多个文件！）中搜索和替换文本是使用工具自动化重复任务的最佳示例之一。手工操作可能需要几个月的时间，但可以缩短到几分钟（最多几小时）。

> **注意**
>
> 请注意，解决方案使用 `-creplace` 运算符以区分大小写的方式替换文本。这几乎总是你想要的，因为替换文本使用你提供的确切大小写。如果要替换的文本以多种不同方式大写（如解决方案中的 `Source Text`），则使用不同的可能大小写多次搜索和替换。

示例 9-3 说明了也许是最简单（但实际最常见）的场景：

- 你处理 ASCII 文本文件。
- 你用字面文本替换替换某些字面文本。
- 你不担心文本匹配可能跨越多行。
- 你的文本文件相对较小。

如果这些假设中的某些不成立，本讨论将向你展示如何调整在此文件中搜索和替换的方式。

### 处理采用 Unicode 或其他 (OEM) 代码页编码的文件

默认情况下，`Set-Content` cmdlet 假设你希望输出文件包含纯 ASCII 文本。如果你处理其他编码的文件（例如 Unicode 或 OEM 代码页如 Cyrillic），请使用 `Out-File` cmdlet 的 `-Encoding` 参数指定：

```
$content | Out-File -Encoding Unicode $filename
$content | Out-File -Encoding OEM $filename
```

### 使用模式而非纯文本替换文本

虽然最常见的是用另一个字面字符串替换一个字面字符串，但在某些高级场景中，你可能想要根据模式替换文本。一个例子可能是交换名和姓。PowerShell 通过其替换运算符对正则表达式的支持来支持这种类型的替换：

```
PS > $content = Get-Content names.txt
PS > $content
John Doe
Mary Smith
PS > $content -replace '(.*) (.*)','$2, $1'
Doe, John
Smith, Mary
```

### 替换跨越多行的文本

解决方案中使用的 `Get-Content` cmdlet 从文件检索行列表。当你对该数组使用 `-replace` 运算符时，它会单独替换每行中的文本。如果你的匹配跨越多行，如 示例 9-3 第 3 行和第 4 行之间所示，`-replace` 运算符将无法感知匹配且不会执行替换。

如果你想要替换跨越多行的文本，则有必要停止将输入文本视为行集合。一旦停止将输入视为行集合，使用能够忽略换行符的替换表达式也很重要，如 示例 9-4 所示。

### 示例 9-4. 在文件中跨多行替换文本

```
$singleLine = Get-Content file.txt -Raw
$content = $singleLine -creplace "(?s)Source(\s*)Text",'Replacement$1Text'
```



示例 9-4 的前两行将文件的整个内容作为单个字符串读取。它们通过使用 `Get-Content` cmdlet 的 `-Raw` 参数来实现这一点，因为 `Get-Content` cmdlet 默认会将文件内容拆分为单独的行。

该解决方案的第三行使用正则表达式模式替换文本。`Source(\s*)Text` 部分扫描单词 `Source`，后面可选地跟随一些空白字符，然后是单词 `Text`。由于正则表达式的空白字符部分被括号括起来，我们想要记住该空白字符的确切内容。默认情况下，正则表达式不允许换行符算作空白字符，因此正则表达式的第一部分使用了*单行选项* (`?`s) 来允许换行符算作空白字符。`-replace` 运算符的替换部分将该匹配项替换为 `Replacement`，后跟我们捕获的匹配项中的精确空白字符 (`$1`)，后跟 `Text`。有关详细信息，请参阅“简单运算符”。

### 在大文件中替换文本

迄今为止使用的方法在替换文本时将文件的全部内容存储在内存中。一旦我们在内存中完成替换，就会将更新后的内容写回磁盘。这在替换小型、中型甚至中大型文件中的文本时效果很好。对于极大文件（例如超过几百兆字节），使用这么多内存可能会给系统带来负担并拖慢脚本速度。要解决该问题，可以逐行处理文件，而不是一次性处理整个文件。

由于你是逐行处理文件，当你尝试将替换文本写回其中时，该文件仍将处于使用中。如果你将替换文本写入临时文件，直到完成对主文件的处理，则可以避免此问题。一旦你扫描完文件，就可以删除它并用临时文件替换它。

```
$filename = "file.txt"
$temporaryFile = [System.IO.Path]::GetTempFileName()

$match = "source text"
$replacement = "replacement text"

Get-Content $filename |
    ForEach-Object { $_ -creplace $match,$replacement } |
    Add-Content $temporaryFile

Remove-Item $filename
Move-Item $temporaryFile $filename
```

## 另请参阅

“简单运算符”

# 9.9 程序：获取文件的编码

PowerShell 和 .NET Framework 都做了大量工作来向你隐藏文件编码的复杂性。`Get-Content` cmdlet 会自动检测文件的编码，然后在将内容返回给你之前处理所有编码问题。不过，当你确实需要知道文件的编码时，解决方案需要一点工作量。

示例 9-5 通过为你完成艰苦工作来解决此问题。具有非标准编码的文件应该（且几乎总是）有一个*字节顺序标记*来标识编码。在字节顺序标记之后是实际内容。如果文件缺少字节顺序标记（无论内容如何编码），`Get-FileEncoding` 将假定为 .NET Framework 的默认编码 UTF-7。如果内容实际上并未按字节顺序标记定义的方式编码，`Get-FileEncoding` 仍会输出声明的编码。

#### 示例 9-5. Get-FileEncoding.ps1

```
##############################################################################
##
## Get-FileEncoding
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

获取文件的编码

.EXAMPLE

Get-FileEncoding.ps1 .\UnicodeScript.ps1

BodyName          : unicodeFFFE
EncodingName      : Unicode (Big-Endian)
HeaderName        : unicodeFFFE
WebName           : unicodeFFFE
WindowsCodePage   : 1200
IsBrowserDisplay  : False
IsBrowserSave     : False
IsMailNewsDisplay : False
IsMailNewsSave    : False
IsSingleByte      : False
EncoderFallback   : System.Text.EncoderReplacementFallback
DecoderFallback   : System.Text.DecoderReplacementFallback
IsReadOnly        : True
CodePage          : 1201

#>

param(
    ## 要获取编码的文件的路径。
    $Path
)

Set-StrictMode -Version 3

## 首先，检查文件是否为二进制文件。即前 5 行是否包含任何不可打印字符。
$nonPrintable = [char[]] (0..8 + 10..31 + 127 + 129 + 141 + 143 + 144 + 157)
$lines = Get-Content $Path -ErrorAction Ignore -TotalCount 5
$result = @($lines | Where-Object { $_.IndexOfAny($nonPrintable) -ge 0 })
if($result.Count -gt 0)
{
    "Binary"
    return
}

## 接下来，检查是否匹配已知编码。

## 用于存储编码字节到其名称的映射的哈希表。例如，“255-254 = Unicode”
$encodings = @{}

## 查找 .NET Framework 理解的所有编码。对于每个编码，
## 确定文件开头的字节（前导码），.NET Framework 用它来识别该编码。
foreach($encoding in [System.Text.Encoding]::GetEncodings())
{
    $preamble = $encoding.GetEncoding().GetPreamble()
    if($preamble)
    {
        $encodingBytes = $preamble -join '-'
        $encodings[$encodingBytes] = $encoding.GetEncoding()
    }
}

## 找出所有前导码的长度。
$encodingLengths = $encodings.Keys | Where-Object { $_ } |
    Foreach-Object { ($_ -split "-").Count }

## 默认假定编码为 UTF7
$result = [System.Text.Encoding]::UTF7

## 遍历每个可能的前导码长度，从文件中读取那么多字节，
## 然后查看是否匹配我们已知的编码之一。
foreach($encodingLength in $encodingLengths | Sort -Descending)
{
    $bytes = Get-Content -AsByteStream -readcount $encodingLength $path | Select -First 1
    $encoding = $encodings[$bytes -join '-']

    ## 如果找到具有相同前导码字节的编码，
    ## 保存该输出并中断。
    if($encoding)
    {
        $result = $encoding
        break
    }
}

## 最后，输出编码。
$result
```

有关运行脚本的详细信息，请参阅食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

# 9.10 查看内容的十六进制表示

## 问题

你想查看文件内容中的字节和特殊字符。

## 解决方案

使用 `Format-Hex` cmdlet 显示文件内容：

```
PS > "Hello World" | Out-File unicode.txt -Encoding unicode
PS > Format-Hex unicode.txt

   Label: C:\scripts\unicode.txt

          Offset Bytes                                           Ascii
                 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
          ------ ----------------------------------------------- -----
0000000000000000 FF FE 48 00 65 00 6C 00 6C 00 6F 00 20 00 57 00 ÿþH e l l o   W
0000000000000010 6F 00 72 00 6C 00 64 00 0D 00 0A 00             o r l d

PS > "Hello World" | Out-File ascii.txt -Encoding ASCII
PS > Format-Hex ascii.txt

   Label: C:\scripts\ascii.txt

          Offset Bytes                                           Ascii
                 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
          ------ ----------------------------------------------- -----
0000000000000000 48 65 6C 6C 6F 20 57 6F 72 6C 64 0D 0A          Hello World
```

## 讨论

处理二进制数据时，查看该二进制数据中实际使用的字节值通常很有用。除了数据的值之外，找到其在文件或内容中的偏移量通常也很重要。



## Format-Hex Cmdlet

PowerShell 的 `Format-Hex` cmdlet 通过在报表中显示内容来启用这两种方案，该报表显示所有这些信息。最左侧的列显示内容中的偏移量，每次增加 16 字节。中间的 16 列显示该位置字节在内容中的十六进制表示。每列的标题显示该字符在 16 字节块中的偏移量。最右侧的列显示该行字符的 ASCII 表示。

要确定输入中字节的位置，将行最左侧的数字加上该字符所在列顶部的数字。例如，`0000000000000010`（显示在最左侧）+ `0A`（显示在列顶部）= `000000000000001A`。因此，此示例中的字节位于内容中的偏移量 `1A` 处。

除了这种基于文本的视图外，PowerShell 的 `Format-Hex` cmdlet 还返回结构丰富的对象。例如，要查看输出第二行（集合中的索引 1）中字节的十六进制表示，可以输入：

```
PS > $output = Format-Hex unicode.txt
PS > $output[1].HexBytes
6F 00 72 00 6C 00 64 00 0D 00 0A 00
```

有关与二进制数据交互的详细信息，请参阅 Recipe 9.6。

## 另请参阅

Recipe 9.6，“解析和管理二进制文件”
