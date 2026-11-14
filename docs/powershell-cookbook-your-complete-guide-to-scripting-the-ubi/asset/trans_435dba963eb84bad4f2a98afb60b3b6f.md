
# PowerShell 字符串大小写转换与文化敏感性

由于 PowerShell 字符串是功能完备的 .NET 对象，它们直接支持许多面向字符串的操作。`ToUpper()` 和 `ToLower()` 方法是 `String` 类支持的众多功能中的两个示例。要了解 `String` 类支持的其他功能，请参阅 [Recipe 3.12](#ch03.xhtml#learn_about_types_and_objects)。

PowerShell 和 .NET `String` 类的方法都不直接支持仅将单词的首字母大写。如果你只想将单词或句子的首字符大写，请尝试以下命令：

```
PS > $text = "hello"
PS > $newText = $text.Substring(0,1).ToUpper() + $text.Substring(1)
PS > $newText
Hello
```

你也可以使用高级正则表达式替换，如 [Recipe 31.6](#ch31.xhtml#scriptblock_as_delegate) 所述：

```
"hello world" -replace '\b(\w)',{ $_.Value.ToUpper() }
```

在将字符串转换为大写或小写时，要记住你这样做的动机。最常见的原因之一是用于比较字符串，如 [示例 5-6](#ch05.xhtml#using_the_toupper_method_to_normalize_st) 所示。

## 示例 5-6. 使用 ToUpper() 方法规范化字符串

```
## $text comes from the user, and contains the value "quit"
if($text.ToUpper() -eq "QUIT") { ... }
```

不幸的是，当脚本在不同文化中运行时，显式更改字符串的大小写会以微妙的方式失败。许多文化遵循的大小写和比较规则可能与你习惯的不同。例如，土耳其语包含两种字母 *I*：一种带点，一种不带点。小写字母 *i* 的大写版本对应带点的大写 *I*，而不是 `QUIT` 中使用的大写 *I*。这些大小写规则导致 [示例 5-6](#ch05.xhtml#using_the_toupper_method_to_normalize_st) 中的字符串比较代码在土耳其文化中失败。

[Recipe 13.8](#ch13.xhtml#program_invoke_a_script_block_with_alter) 非常清楚地展示了这一点：

```
PS > Use-Culture tr-TR { "quit".ToUpper() -eq "QUIT" }
False
PS > Use-Culture tr-TR { "quIt".ToUpper() -eq "QUIT" }
True
PS > Use-Culture tr-TR { "quit".ToUpper() }
QUİT
```

对于以不区分大小写的方式将某些输入与硬编码字符串进行比较，更好的解决方案是使用 PowerShell 的 `-eq` 运算符，而不必自行更改任何大小写。`-eq` 运算符默认不区分大小写且与文化无关：

```
PS > $text1 = "Hello"
PS > $text2 = "HELLO"
PS > $text1 -eq $text2
True

PS > Use-Culture tr-TR { "quit" -eq "QUIT" }
True
```

有关编写文化感知脚本的更多信息，请参阅 [Recipe 13.6](#ch13.xhtml#write_culture-aware_scripts)。

## 另请参阅

- [Recipe 3.12，“了解类型和对象”](#ch03.xhtml#learn_about_types_and_objects)
- [Recipe 13.6，“编写文化感知脚本”](#ch13.xhtml#write_culture-aware_scripts)
- [Recipe 31.6，“将脚本块用作 .NET 委托或事件处理程序”](#ch31.xhtml#scriptblock_as_delegate)

# 5.12 修剪字符串

## 问题

你想要从字符串或用户输入中移除前导或尾随空格。

## 解决方案

使用字符串的 `Trim()` 方法从该字符串中移除所有前导和尾随空白字符。

```
PS > $text = " `t Test String`t `t"
PS > "|" + $text.Trim() + "|"
|Test String|
```

## 讨论

`Trim()` 方法会清除字符串开头*和*结尾的所有空白字符。如果你只想清除其中之一，可以分别调用 `TrimStart()` 或 `TrimEnd()` 方法来移除字符串开头或结尾的空白字符。如果你想从字符串开头或结尾移除特定字符，`Trim()`、`TrimStart()` 和 `TrimEnd()` 方法提供了相应的选项。要从字符串末尾修剪一组特定字符，请将该列表提供给该方法，如 [示例 5-7](#ch05.xhtml#trimming_a_list_of_characters_from_the_e) 所示。

### 示例 5-7. 从字符串末尾修剪一组字符

```
PS > "Hello World".TrimEnd('d','l','r','o','W',' ')
He
```

如果你想替换字符串中的任意位置的文本（而不仅仅是开头或结尾），请参阅 [Recipe 5.8](#ch05.xhtml#replace_text_in_a_string)。

> **注意**
>
> 乍一看，以下试图从字符串末尾修剪文本 `\"World\"` 的命令似乎工作不正确：
>
> ```
> PS > "Hello World".TrimEnd(" World")
> He
> ```
>
> 之所以发生这种情况，是因为 `TrimEnd()` 方法接受一个要从字符串末尾移除的字符列表。如果需要，PowerShell 会自动将字符串转换为字符列表，在本例中将你的字符串转换为字符 `W`、`o`、`r`、`l`、`d` 和一个空格。这些实际上与 [示例 5-7](#ch05.xhtml#trimming_a_list_of_characters_from_the_e) 中使用的字符相同，因此效果也相同。

## 另请参阅

- [Recipe 5.8，“替换字符串中的文本”](#ch05.xhtml#replace_text_in_a_string)

# 5.13 格式化日期以供输出

## 问题

你想要控制 PowerShell 显示或格式化日期的方式。

## 解决方案

要控制日期的格式，请使用以下选项之一：

- `Get-Date` cmdlet 的 `-Format` 参数：

  ```
  PS > Get-Date -Date "05/09/1998 1:23 PM" -Format FileDateTime
  19980509T1323000000
  
  PS > Get-Date -Date "05/09/1998 1:23 PM" -Format "dd-MM-yyyy @ hh:mm:ss"
  09-05-1998 @ 01:23:00
  ```

- PowerShell 的字符串格式化（`-f`）运算符：

  ```
  PS > $date = [DateTime] "05/09/1998 1:23 PM"
  PS > "{0:dd-MM-yyyy @ hh:mm:ss}" -f $date
  09-05-1998 @ 01:23:00
  ```

- 对象的 `ToString()` 方法：

  ```
  PS > $date = [DateTime] "05/09/1998 1:23 PM"
  PS > $date.ToString("dd-MM-yyyy @ hh:mm:ss")
  09-05-1998 @ 01:23:00
  ```

- `Get-Date` cmdlet 的 `-UFormat` 参数，支持 Unix 日期格式字符串：

  ```
  PS > Get-Date -Date "05/09/1998 1:23 PM" -UFormat "%d-%m-%Y @ %I:%M:%S"
  09-05-1998 @ 01:23:00
  ```

## 讨论

将日期转换为字符串的常见需求之一是用于文件名、目录名和类似场景。对于这些非常常见的场景，`Get-Date` cmdlet 为其 `-Format` 参数提供了四个简单的选项：`FileDate`、`FileDateUniversal`、`FileDateTime` 和 `FileDateTimeUniversal`。这些选项以本地或通用时区返回日期（“`19980509`”）或日期和时间（“`19980509T1323000000`”）的表示形式。

除了这些标准格式字符串外，`-Format` 参数还支持标准 .NET `DateTime` 格式字符串。这些格式字符串允许你以多种标准格式（如系统的短日期或长日期模式）或完全自定义的方式显示日期。有关如何指定标准 .NET `DateTime` 格式字符串的更多信息，请参阅 [附录 E](#app05.xhtml#net_datetime_formatting)。

如果你已经习惯了 Unix 风格的日期格式字符串（或正在转换使用复杂格式字符串的现有脚本），`Get-Date` cmdlet 的 `-UFormat` 参数可能会有所帮助。它接受 Unix `date` 命令接受的格式字符串，但不提供标准 .NET 日期格式字符串无法提供的任何功能。

在处理日期和时间的字符串版本时，请注意它们是*国际化*问题最常见的来源——这些问题源于在与编写脚本时不同的文化环境的机器上运行脚本。在北美，“05/09/1998”表示“1998 年 5 月 9 日”。但在许多其他文化中，它表示“1998 年 9 月 5 日”。只要可能，请使用并比较 `DateTime` 对象（而不是字符串）与其他 `DateTime` 对象，因为这可以避免这些文化差异。[示例 5-8](#ch05.xhtml#comparing_datetime_objects_with_the_-gt_) 演示了这种方法。

### 示例 5-8. 使用 -gt 运算符比较 DateTime 对象
