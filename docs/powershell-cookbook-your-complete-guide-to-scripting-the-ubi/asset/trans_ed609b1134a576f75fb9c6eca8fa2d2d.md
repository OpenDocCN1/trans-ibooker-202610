
这些限制虽然有用，但有时会在你从互联网复制并粘贴 PowerShell 示例时导致问题。网页通常会在行尾添加空格，这可能会干扰此处字符串对开头的严格要求。如果 PowerShell 在脚本定义此处字符串时产生错误，请检查该此处字符串在其第一个引号字符后是否包含多余的空格。

与字符串字面量一样，此处字符串可以是字面量（使用单引号）或展开字符串（使用双引号）。

# 5.3 在字符串中放置特殊字符

## 问题

你想要在字符串变量中放置特殊字符（如制表符和换行符）。

## 解决方案

在展开字符串中，使用 PowerShell 的转义序列来包含制表符和换行符等特殊字符。

```
PS > $myString = "Report for Today`n----------------"
PS > $myString
Report for Today
----------------
```

## 讨论

正如 [Recipe 5.1](#ch05.xhtml#create_a_string) 中所讨论的，PowerShell 字符串有两种类型：字面量（或非展开）和展开字符串。字面量字符串在其文本周围使用单引号，而展开字符串在其文本周围使用双引号。

在字面量字符串中，单引号之间的所有文本都将成为字符串的一部分。在展开字符串中，PowerShell 会将变量名（如 `$ENV:SystemRoot`）和转义序列（如 `` `n ``）展开为其值（如 `SystemRoot` 环境变量和换行符）。

### 注意

与许多使用反斜杠字符 (`\`) 作为转义序列的语言不同，PowerShell 使用反引号 (`` ` ``) 字符。这源于其对系统管理的关注，在路径名中反斜杠无处不在。

有关 PowerShell 字符串内部转义序列和替换规则的详细解释，请参阅[“字符串”](#app01.xhtml#strings)。

## 另请参阅

[Recipe 5.1, “Create a String”](#ch05.xhtml#create_a_string)

[“Strings”](#app01.xhtml#strings)

# 5.4 在字符串中插入动态信息

## 问题

你想要在字符串中放置动态信息（如另一个变量的值）。

## 解决方案

在展开字符串中，包含变量名以插入该变量的值：

```
PS > $header = "Report for Today"
PS > $myString = "$header`n----------------"
PS > $myString
Report for Today
----------------
```

要包含比变量值更复杂的信息，请将其括在子表达式中：

```
PS > $header = "Report for Today"
PS > $myString = "$header`n$('-' * $header.Length)"
PS > $myString
Report for Today
----------------
```

## 讨论

展开字符串中的变量替换是一个足够简单的概念，但子表达式值得稍作澄清。

`子表达式` 是美元符号字符，后跟包含在括号中的 PowerShell 命令（或命令集）：

```
$(subexpression)
```

当 PowerShell 在展开字符串中看到子表达式时，它会计算该子表达式并将结果放入展开字符串中。在解决方案中，表达式 `'-' * $header.Length` 告诉 PowerShell 生成一行长度为 `$header.Length` 的破折号。

在字符串中放置动态信息的另一种方法是使用 PowerShell 的字符串格式化运算符，它使用与 .NET 字符串格式化相同的规则：

```
PS > $header = "Report for Today"
PS > $myString = "{0}`n{1}" -f $header,('-' * $header.Length)
PS > $myString
Report for Today
----------------
```

有关 PowerShell 格式化运算符的解释，请参阅 [Recipe 5.6](#ch05.xhtml#place_formatted_information_in_a_string)。有关 PowerShell 转义字符的更多信息，请键入 `Get-Help` `about_Special_Characters`。

## 另请参阅

[Recipe 5.6, “Place Formatted Information in a String”](#ch05.xhtml#place_formatted_information_in_a_string)

# 5.5 防止字符串包含动态信息

## 问题

你想要阻止 PowerShell 解释字符串内部的特殊字符或变量名。

## 解决方案

使用非展开字符串，让 PowerShell 完全按输入内容解释你的字符串。非展开字符串在其文本周围使用单引号字符。

```
PS > $myString = 'Useful PowerShell characters include: $, `, " and { }'
PS > $myString
Useful PowerShell characters include: $, `, " and { }
```

如果你也想包含换行符，请使用非展开*此处字符串*，如[示例 5-1](#ch05.xhtml#a_nonexpanding_here_string_that_includes) 所示。

##### 示例 5-1. 包含换行符的非展开此处字符串

```
PS > $myString = @'
Tip of the Day
-------------
Useful PowerShell characters include: $, `, ', " and { }
'@

PS > $myString
Tip of the Day
Useful PowerShell characters include: $, `, ', " and { }
```

## 讨论

在字面量字符串中，单引号之间的所有文本都将成为字符串的一部分。这与展开字符串形成对比，在展开字符串中，PowerShell 会将变量名（如 `$myString`）和转义序列（如 `` `n ``）展开为其值（如 `$myString` 的内容和换行符）。

### 注意

非展开字符串是管理包含特殊字符的文件和文件夹的有用方法，这些特殊字符否则可能会被解释为转义序列。有关管理名称中包含特殊字符的文件的更多信息，请参阅 [Recipe 20.7](#ch20.xhtml#manage_files_that_include_special_charac)。

正如 [Recipe 5.1](#ch05.xhtml#create_a_string) 中所讨论的，“字面量字符串中的所有文本都是字面量”这一规则的一个例外来自引号字符本身。在任一类型的字符串中，PowerShell 都允许你将该字符串的两个引号字符放在一起，以包含引号字符本身：

```
$myString = "This string includes ""double quotes"" because it combined quote characters."
$myString = 'This string includes ''single quotes'' because it combined quote characters.'
```

## 另请参阅

[Recipe 5.1, “Create a String”](#ch05.xhtml#create_a_string)

[Recipe 20.7, “Manage Files That Include Special Characters”](#ch20.xhtml#manage_files_that_include_special_charac)

# 5.6 在字符串中放置格式化信息

## 问题

你想要在字符串中放置格式化信息（如右对齐的文本或四舍五入到特定小数位数的数字）。

## 解决方案

使用 PowerShell 的格式化运算符在字符串中放置格式化信息：

```
PS > $formatString = "{0,8:D4} {1:C}`n"
PS > $report = "Quantity Price`n"
PS > $report += "---------------`n"
PS > $report += $formatString -f 50,2.5677
PS > $report += $formatString -f 3,9
PS > $report
Quantity Price
---------------
    0050 $2.57
    0003 $9.00
```

## 讨论

PowerShell 的字符串格式化运算符 (`-f`) 使用与 .NET Framework 中 `String.Format()` 方法相同的字符串格式化规则。它在左侧接受一个格式字符串，在右侧接受要格式化的项目。

在解决方案中，你格式化了两个数字：一个数量和一个价格。第一个数字 (`{0}`) 代表数量，在八个字符的框中右对齐 (`,8`)。它被格式化为四位十进制数字 (:`D4`)。第二个数字 (`{1}`) 代表价格，你将其格式化为货币 (`:C`)。

### 注意

如果你发现自己正在手工制作基于文本的报告，请**停下来**！让 PowerShell 的内置命令为你完成所有工作。相反，发出自定义对象，以便你的用户能像使用常规 PowerShell 命令一样轻松地使用你的脚本。有关更多信息，请参阅 [Recipe 3.15](#ch03.xhtml#create_and_initialize_custom_objects)。

有关 PowerShell 格式化运算符的详细解释，请参阅[“简单运算符”](#app01.xhtml#simple_operators)。有关格式化规则的详细列表，请参阅[附录 D](#app04.xhtml#net_string_formatting)。

