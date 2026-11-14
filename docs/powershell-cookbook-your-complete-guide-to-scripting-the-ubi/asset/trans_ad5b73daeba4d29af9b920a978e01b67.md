

[食谱 9.2，“将命令输出存储到文件中”](#ch09.xhtml#store_the_output_of_a_command_into_a_fil)

[“捕获输出”](#app01.xhtml#capturing_output)

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

有关捕获、命名捕获和正则表达式其他方面的更多信息，请参阅 [附录 B](#app02.xhtml#regular_expression_reference)。

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
> 搜索文件文本的另外两个常用工具是 `-match` 运算符和带有 `-file` 选项的 `switch` 语句。有关这些的更多信息，请参阅 [食谱 5.7](#ch05.xhtml#search_a_string_for_text_or_a_pattern) 和 [食谱 4.3](#ch04.xhtml#manage_large_conditional_statements_with)。有关 `Select-String` cmdlet 的更多信息，请输入 `Get-Help Select-String`。

## 另请参阅

[食谱 4.3，“使用 Switch 管理大型条件语句”](#ch04.xhtml#manage_large_conditional_statements_with)

[食谱 5.7，“在字符串中搜索文本或模式”](#ch05.xhtml#search_a_string_for_text_or_a_pattern)

[附录 B，*正则表达式参考*](#app02.xhtml#regular_expression_reference)

# 9.5 解析和管理基于文本的日志文件

## 问题

你想要使用 PowerShell 的标准基于对象的命令来解析和分析基于文本的日志文件。

## 解决方案

使用 [食谱 5.15](#ch05.xhtml#convert_text_streams_to_objects) 中描述的 `ConvertFrom-String` cmdlet 来处理基于文本的日志文件。在你的协助下，它将文本流转换为对象流，然后你可以使用 PowerShell 的标准命令轻松地处理它们。

## 讨论

当你解析日志文件时，`ConvertFrom-String` 脚本主要接受两个参数：

*   一个描述如何将传入文本分割成组的正则表达式
*   一个属性名称列表，脚本随后将这些名称分配给那些文本组

正如[示例 9-1](#ch09.xhtml#examining_firewall_log) 所示，你可以使用 Windows 目录中的防火墙日志。如果启用，这些日志会跟踪计算机上的入站和出站网络连接。

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

