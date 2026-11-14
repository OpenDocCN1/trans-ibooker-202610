# 第 5 章 字符串和非结构化文本

## 5.0 简介

创建和操作文本长期以来一直是脚本语言和传统 Shell 的主要任务之一。事实上，Perl（该语言）最初是作为一个简单（但有用）的文本处理工具而设计的。它已远远超越了这些卑微的起源，但其流行度有力地证明了它满足的需求。

在基于文本的 Shell 中，这种强烈的关注持续存在。当你与系统的大多数交互通过操作程序的基于文本的输出发生时，强大的文本处理实用程序变得至关重要。这些文本解析工具，如 `awk`、`sed` 和 `grep`，构成了基于文本的系统管理的基石。

在 PowerShell 的基于对象的环境中，这种传统工具链的作用不那么关键。你可以通过其他 PowerShell 命令更有效地完成以前需要这些工具的大多数任务。然而，作为基于对象的 Shell 并不意味着 PowerShell 放弃了对文本处理的所有支持。处理字符串和非结构化文本在系统管理员的生活中仍然扮演重要角色。由于 PowerShell 让你能以全保真度（使用 cmdlet 和对象）管理系统的大部分，文本处理工具可以再次专注于实际的文本处理任务。

## 5.1 创建字符串

### 问题

你想创建一个保存文本的变量。

### 解决方案

使用 PowerShell 字符串变量作为存储和处理文本的方式。

要定义支持变量扩展和转义字符的字符串，请用双引号将其括起来：

```
$myString = "Hello World"
```

要定义字面量字符串（即不解释变量扩展或转义字符的字符串），请用单引号将其括起来：

```
$myString = 'Hello World'
```

### 讨论

字符串字面量有两种类型：*字面量*（*非扩展*）和*扩展*字符串。要创建字面量字符串，请在文本周围放置单引号（`$myString = 'Hello World'`）。要创建扩展字符串，请在文本周围放置双引号（`$myString = "Hello World"`）。

在字面量字符串中，单引号之间的所有文本都成为字符串的一部分。在扩展字符串中，PowerShell 会将变量名（如 `$replacementString`）和转义序列（如 `` `n ``）扩展为其值（分别为 `$replacementString` 的内容和换行符）。

有关 PowerShell 字符串中转义序列和替换规则的详细解释，请参阅 “字符串”。

“字面量字符串中的所有文本都是字面量”这一规则的一个例外来自引号字符本身。在任一类型的字符串中，PowerShell 都允许你将该字符串的两个引号字符放在一起以添加引号字符本身：

```
$myString = "This string includes ""double quotes"" because it combined quote characters."
$myString = 'This string includes ''single quotes'' because it combined quote characters.'
```

这有助于防止当你尝试在单引号字符串中包含单引号时出现的转义惨剧。例如：

```
$myString = 'This string includes ' + "'" + 'single quotes' + "'"
```

#### 注意

此示例展示了 PowerShell 通过将其他字符串相加来创建新字符串是多么容易。这是在脚本中构建格式化报表的一种吸引人的方式，但应谨慎使用。由于 .NET Framework（因此 PowerShell）管理字符串的方式，以这种方式向大字符串末尾添加信息会导致明显的性能问题。如果你打算创建大型报表，请参阅 食谱 5.16。

### 另请参阅

食谱 5.16，“生成大型报表和文本流”

“字符串”

## 5.2 创建多行或格式化字符串

### 问题

你想创建一个包含换行符或其他显式格式的文本变量。

### 解决方案

使用 PowerShell *here string* 来存储和处理包含换行符和其他格式信息的文本。

```
$myString = @"
This is the first line
of a very long string. A "here string"
lets you create blocks of text
that span several lines.
"@
```

### 讨论

当 PowerShell 看到字符 `@"` 后跟换行符时，开始一个 here string。当它看到单独一行上的字符 `"@` 时，结束该字符串。这些看似奇怪的限制让你能够创建包含引号字符、换行符和其他符号的字符串，这些符号在创建大块预格式化文本时常用。

#### 注意



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

正如 Recipe 5.1 中所讨论的，PowerShell 字符串有两种类型：字面量（或非展开）和展开字符串。字面量字符串在其文本周围使用单引号，而展开字符串在其文本周围使用双引号。

在字面量字符串中，单引号之间的所有文本都将成为字符串的一部分。在展开字符串中，PowerShell 会将变量名（如 `$ENV:SystemRoot`）和转义序列（如 `` `n ``）展开为其值（如 `SystemRoot` 环境变量和换行符）。

### 注意

与许多使用反斜杠字符 (`\`) 作为转义序列的语言不同，PowerShell 使用反引号 (`` ` ``) 字符。这源于其对系统管理的关注，在路径名中反斜杠无处不在。

有关 PowerShell 字符串内部转义序列和替换规则的详细解释，请参阅“字符串”。

## 另请参阅

Recipe 5.1, “Create a String”

“Strings”

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

有关 PowerShell 格式化运算符的解释，请参阅 Recipe 5.6。有关 PowerShell 转义字符的更多信息，请键入 `Get-Help` `about_Special_Characters`。

## 另请参阅

Recipe 5.6, “Place Formatted Information in a String”

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

如果你也想包含换行符，请使用非展开*此处字符串*，如示例 5-1 所示。

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

非展开字符串是管理包含特殊字符的文件和文件夹的有用方法，这些特殊字符否则可能会被解释为转义序列。有关管理名称中包含特殊字符的文件的更多信息，请参阅 Recipe 20.7。

正如 Recipe 5.1 中所讨论的，“字面量字符串中的所有文本都是字面量”这一规则的一个例外来自引号字符本身。在任一类型的字符串中，PowerShell 都允许你将该字符串的两个引号字符放在一起，以包含引号字符本身：

```
$myString = "This string includes ""double quotes"" because it combined quote characters."
$myString = 'This string includes ''single quotes'' because it combined quote characters.'
```

## 另请参阅

Recipe 5.1, “Create a String”

Recipe 20.7, “Manage Files That Include Special Characters”

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

如果你发现自己正在手工制作基于文本的报告，请**停下来**！让 PowerShell 的内置命令为你完成所有工作。相反，发出自定义对象，以便你的用户能像使用常规 PowerShell 命令一样轻松地使用你的脚本。有关更多信息，请参阅 Recipe 3.15。

有关 PowerShell 格式化运算符的详细解释，请参阅“简单运算符”。有关格式化规则的详细列表，请参阅附录 D。




虽然主要用于控制信息布局，但字符串格式化运算符也是字符串连接的一种可读替代方案：

```
PS > $number1 = 10
PS > $number2 = 32
PS > "$number2 divided by $number1 is " + $number2 / $number1
32 divided by 10 is 3.2
```

字符串格式化运算符使其更易读：

```
PS > "{0} divided by {1} is {2}" -f $number2, $number1, ($number2 / $number1)
32 divided by 10 is 3.2
```

如果要支持命名替换（而非基于索引的替换），可以使用 `Format-String` 脚本（见示例 5.17）。

除了字符串格式化运算符，PowerShell 还提供三个格式化命令（`Format-Table`、`Format-Wide` 和 `Format-List`），可让你轻松生成格式化报表。有关这些 cmdlet 的详细信息，请参阅“自定义格式化文件”。

## 另请参阅

- 示例 3.15“创建和初始化自定义对象”
- “简单运算符”
- “自定义格式化文件”
- 附录 D *.NET 字符串格式化*

## 5.7 在字符串中搜索文本或模式

### 问题

你想确定字符串是否包含另一个字符串，或查找一个字符串在另一个字符串中的位置。

### 解决方案

PowerShell 提供多种选项帮助你在字符串中搜索文本。

使用 `-like` 运算符确定字符串是否匹配给定的 DOS 样式通配符：

```
PS > "Hello World" -like "*llo W*"
True
```

使用 `-match` 运算符确定字符串是否匹配给定的正则表达式：

```
PS > "Hello World" -match '.*l[l-z]o W.*$'
True
```

使用 `Contains()` 方法确定字符串是否包含特定字符串：

```
PS > "Hello World".Contains("World")
True
```

使用 `IndexOf()` 方法确定一个字符串在另一个字符串中的位置：

```
PS > "Hello World".IndexOf("World")
6
```

### 讨论

由于 PowerShell 字符串是功能完备的 .NET 对象，它们直接支持许多面向字符串的操作。`Contains()` 和 `IndexOf()` 方法只是 `String` 类支持的众多功能中的两个例子。要了解 `String` 类支持的其他功能，请参阅示例 3.12。

**注意**

若要在整个文件中搜索文本或模式，请参阅示例 9.4。

尽管它们使用相似的字符，但简单通配符和正则表达式的用途截然不同。通配符比正则表达式简单得多，因此受到的限制也更多。虽然通配符的规则只需四点即可概括，但讲授和阐释正则表达式用法的书籍却汗牛充栋。

**注意**

正则表达式的一个常见用途是搜索跨越多行的字符串。默认情况下，正则表达式不跨行搜索，但可以使用 `singleline` (`?s`) 选项指示它们这样做：

```
PS > "Hello `n World" -match "Hello.*World"
False
PS > "Hello `n World" -match "(?s)Hello.*World"
True
```

通配符适合简单的文本搜索，而正则表达式适合更复杂的文本搜索。

有关 `-like` 运算符的详细说明，请参阅“比较运算符”。有关 `-match` 运算符的详细说明，请参阅“简单运算符”。有关正则表达式规则和语法的详细列表，请参阅附录 B。

有时在尝试将 PowerShell 命令的结果存储在字符串中时会遇到困难，如示例 5-2 所示。

#### 示例 5-2. 尝试将 PowerShell 命令的输出存储在字符串中

```
PS > Get-Help Get-ChildItem

NAME
    Get-ChildItem

SYNOPSIS
    Gets the items and child items in one or more specified locations.

DESCRIPTION
    The `Get-ChildItem` cmdlet gets the items in one or more specified locations. If
    the item is a container, it gets the items inside the container, known as child
    items. You can use the Recurse parameter to get items in all child containers
    and use the Depth parameter to limit the number of levels to recurse.

    `Get-ChildItem` doesn't display empty directories. When a `Get-ChildItem`
    command includes the Depth or Recurse parameters, empty directories aren't
    included in the output.

(...)

PS > $helpContent = Get-Help Get-ChildItem
PS > $helpContent -match "empty directories"
False
```

`-match` 运算符在字符串中搜索指定的模式，但在这种情况下似乎失败了。这是因为所有 PowerShell 命令都生成对象。如果不将输出存储在另一个变量中或传递给另一个命令，PowerShell 会在显示给你之前将输出转换为文本表示形式。在示例 5-2 中，`$helpContent` 是一个功能完备的对象，而不仅仅是其字符串表示形式：

```
PS > $helpContent.Name
Get-ChildItem
```

要使用 PowerShell 命令的基于文本的表示形式，可以显式地将其通过 `Out-String` cmdlet 传递。`Out-String` cmdlet 将其输入转换为你在屏幕上习惯看到的基于文本的形式：

```
PS > $helpContent = Get-Help Get-ChildItem | Out-String -Stream
PS > [bool] ($helpContent -match "empty directories")
True
```

有关使搜索文本命令输出更容易的脚本，请参阅示例 1.24。

### 另请参阅

- 示例 1.24“程序：搜索格式化输出中的模式”
- 示例 3.12“了解类型和对象”
- “简单运算符”
- “比较运算符”
- 附录 B *正则表达式参考*

## 5.8 替换字符串中的文本

### 问题

你想用另一个字符串替换字符串的一部分。

### 解决方案

PowerShell 提供多种选项帮助你用其他文本替换字符串中的文本。

在字符串本身上使用 `Replace()` 方法执行简单替换：

```
PS > "Hello World".Replace("World", "PowerShell")
Hello PowerShell
```

使用 PowerShell 的正则表达式 `-replace` 运算符执行更高级的正则表达式替换：

```
PS > "Hello World" -replace '(.*) (.*)','$2 $1'
World Hello
```

### 讨论

`Replace()` 方法和 `-replace` 运算符都提供了在字符串中替换文本的有用方法。`Replace()` 方法最快但也受限最多。它将指定的确切字符串的每次出现替换为你提供的确切替换字符串。`-replace` 运算符提供更大的灵活性，因为其参数是可以匹配和替换复杂模式的正则表达式。

**注意**

有关使用输入和输出示例自动学习如何替换字符串中文本的方法，请参阅示例 5.14。

考虑到其使用的正则表达式的强大功能，`-replace` 运算符也带来了正则表达式的一些陷阱。

首先，与 `-replace` 运算符一起使用的正则表达式通常包含字符（如美元符号，它代表组号），PowerShell 通常将其解释为变量名或转义字符。为防止 PowerShell 解释这些字符，请使用非扩展字符串（单引号），如解决方案所示。



## 另一个不太常见的陷阱：在替换文本中使用正则表达式特殊字符

例如：

```
PS > "Power[Shell]" -replace "[Shell]","ful"
Powfulr[fulfulfulfulful]
```

这显然不是我们想要的结果。在正则表达式中，方括号包围的一组字符意味着“匹配方括号内的任意字符”。在我们的例子中，这转换为“将字符 S、h、e、l 替换为 ‘ful’”。

为了避免这种情况，我们可以使用正则表达式转义字符来转义方括号：

```
PS > "Power[Shell]" -replace "\[Shell\]","ful"
Powerful
```

然而，这意味着需要知道所有正则表达式特殊字符并修改输入字符串。有时我们无法控制输入字符串，此时 `[Regex]::Escape()` 方法非常有用：

```
PS > "Power[Shell]" -replace ([Regex]::Escape("[Shell]")),"ful"
Powerful
```

对于极其高级的正则表达式替换需求，可以使用脚本块来完成替换任务，如 Recipe 31.6 所述。例如，将单词边界（`\b`）后的第一个字符（`\w`）大写：

```
PS > "hello world" -replace '\b(\w)',{ $_.Value.ToUpper() }
Hello World
```

有关 `-replace` 运算符的更多信息，请参阅“简单运算符”和附录 B。

## 参见

- Recipe 5.14，“在不同格式之间转换字符串”
- “简单运算符”
- 附录 B，*正则表达式参考*

# 5.9 基于文本或模式拆分字符串

## 问题

你想要基于某些字面文本或正则表达式模式拆分字符串。

## 解决方案

使用 PowerShell 的 `-split` 运算符按字符序列或特定字符串拆分：

```
PS > "a-b-c-d-e-f" -split "-c-"
a-b
d-e-f
```

要按模式拆分，请将正则表达式作为第一个参数提供：

```
PS > "a-b-c-d-e-f" -split "b|[d-e]"
a-
-c-
-
-f
```

## 讨论

要拆分字符串，许多已熟悉 C# 的初级脚本编写者会使用 .NET Framework 的 `String.Split()` 和 `[Regex]::Split()` 方法。虽然这些方法在 PowerShell 中仍可用，但 PowerShell 的 `-split` 运算符提供了一种更自然的方式将字符串拆分为更小的字符串。当不带参数使用时（一元拆分运算符），它按空白字符拆分字符串，如示例 5-3 所示。

### 示例 5-3. PowerShell 的一元拆分运算符

```
PS > -split "Hello World `t How `n are you?"
Hello
World
How
are
you?
```

当带参数使用时，它将参数视为正则表达式，然后基于该模式拆分。

```
PS > "a-b-c-d-e-f" -split 'b|[d-e]'
a-
-c-
-
-f
```

如果替换模式避免了在正则表达式中有特殊含义的字符，可以用它基于另一个字符串拆分字符串。

```
PS > "a-b-c-d-e-f" -split '-c-'
a-b
d-e-f
```

如果替换模式包含在正则表达式中有特殊含义的字符（如 `.` 字符，表示“任意字符”），请使用 `-split` 运算符的 `SimpleMatch` 选项，如示例 5-4 所示。

### 示例 5-4. PowerShell 的 SimpleMatch 拆分选项

```
PS > "a.b.c" -split '.'
(A bunch of newlines. Something went wrong!)

PS > "a.b.c" -split '.',0,"SimpleMatch"
a
b
c
```

有关 `-split` 运算符选项的更多信息，请输入 **`Get-Help about_split`**。

虽然正则表达式提供了巨大的灵活性，但 `-split` 运算符通过允许你为拆分操作提供脚本块，给予了你终极灵活性。对于每个字符，它调用脚本块并根据结果拆分字符串。在脚本块中，`$_`（或 `$PSItem`）代表当前字符。例如，示例 5-5 按偶数拆分字符串。

### 示例 5-5. 使用脚本块拆分字符串

```
PS > "1234567890" -split { ($_ % 2) -eq 0 }
1
3
5
7
9
```

当使用脚本块拆分字符串时，`$_` 代表当前字符。对于参数，`$args[0]` 代表整个字符串，`$args[1]` 代表当前正在检查的字符串索引。

要按模式拆分整个文件，请使用 `Get-Content` cmdlet 的 `-Delimiter` 参数：

```
PS > Get-Content test.txt
Hello
World
PS > (Get-Content test.txt)[0]
Hello
PS > Get-Content test.txt -Delimiter l
He

o
Wor
d

PS > (Get-Content test.txt -Delimiter l)[0]
He
PS > (Get-Content test.txt -Delimiter l)[2]
o
Wor
PS > (Get-Content test.txt -Delimiter l)[3]
d
```

有关 `-split` 运算符的更多信息，请参阅“简单运算符”或输入 **`Get-Help about_split`**。

## 参见

- “简单运算符”
- 附录 B，*正则表达式参考*

# 5.10 将字符串合并为更大的字符串

## 问题

你想要将多个单独的字符串合并为单个字符串。

## 解决方案

使用 PowerShell 的一元 `-join` 运算符，使用默认的空分隔符将单独的字符串合并为更大的字符串：

```
PS > -join ("A","B","C")
ABC
```

如果你想定义 PowerShell 用于合并字符串的分隔符，请使用 PowerShell 的二元 `-join` 运算符：

```
PS > ("A","B","C") -join "`r`n"
A
B
C
```

要使用 cmdlet 实现 `-join` 运算符不支持的功能，请使用 `Join-String` cmdlet：

```
PS > 1..5 | Join-String -DoubleQuote -Separator ','
"1","2","3","4","5"
```

## 讨论

`-join` 运算符提供了一种自然的方式来合并字符串。当不带参数使用时（一元 join 运算符），它使用默认的空分隔符连接列表。当在列表和分隔符之间使用时（二元 join 运算符），它使用提供的分隔符连接字符串。

除了性能优势外，`-join` 运算符还解决了尝试手动合并字符串时出现的一个极其常见的困难。

当首次编写使用分隔符（例如逗号和空格）连接列表的代码时，通常会在输出的开头或结尾留下一个孤独的分隔符：

```
PS > $list = "Hello","World"
PS > $output = ""
PS >
PS > foreach($item in $list)
{
    $output += $item + ", "
}

PS > $output
Hello, World,
```

可以通过向 `foreach` 循环添加一些额外逻辑来解决此问题：

```
PS > $list = "Hello","World"
PS > $output = ""
PS >
PS > foreach($item in $list)
{
    if($output -ne "") { $output += ", " }
    $output += $item
}

PS > $output
Hello, World
```

或者，直接使用 `-join` 运算符省去麻烦：

```
PS > $list = "Hello","World"
PS > $list -join ", "
Hello, World
```

如果你有 `-join` 运算符未涵盖的高级需求，当然也可以在 PowerShell 中使用 `[String]::Join()` 等 .NET 方法。

有关将字符串合并为更大字符串或报表的更结构化方法，请参阅 Recipe 5.6。

## 参见

- Recipe 5.6，“在字符串中放置格式化信息”

# 5.11 将字符串转换为大写或小写

## 问题

你想要将字符串转换为大写或小写。

## 解决方案

使用字符串的 `ToUpper()` 或 `ToLower()` 方法分别将其转换为大写或小写。

要将字符串转换为大写，请使用 `ToUpper()` 方法：

```
PS > "Hello World".ToUpper()
HELLO WORLD
```

要将字符串转换为小写，请使用 `ToLower()` 方法：

```
PS > "Hello World".ToLower()
hello world
```

## 讨论



# PowerShell 字符串大小写转换与文化敏感性

由于 PowerShell 字符串是功能完备的 .NET 对象，它们直接支持许多面向字符串的操作。`ToUpper()` 和 `ToLower()` 方法是 `String` 类支持的众多功能中的两个示例。要了解 `String` 类支持的其他功能，请参阅 Recipe 3.12。

PowerShell 和 .NET `String` 类的方法都不直接支持仅将单词的首字母大写。如果你只想将单词或句子的首字符大写，请尝试以下命令：

```
PS > $text = "hello"
PS > $newText = $text.Substring(0,1).ToUpper() + $text.Substring(1)
PS > $newText
Hello
```

你也可以使用高级正则表达式替换，如 Recipe 31.6 所述：

```
"hello world" -replace '\b(\w)',{ $_.Value.ToUpper() }
```

在将字符串转换为大写或小写时，要记住你这样做的动机。最常见的原因之一是用于比较字符串，如 示例 5-6 所示。

## 示例 5-6. 使用 ToUpper() 方法规范化字符串

```
## $text comes from the user, and contains the value "quit"
if($text.ToUpper() -eq "QUIT") { ... }
```

不幸的是，当脚本在不同文化中运行时，显式更改字符串的大小写会以微妙的方式失败。许多文化遵循的大小写和比较规则可能与你习惯的不同。例如，土耳其语包含两种字母 *I*：一种带点，一种不带点。小写字母 *i* 的大写版本对应带点的大写 *I*，而不是 `QUIT` 中使用的大写 *I*。这些大小写规则导致 示例 5-6 中的字符串比较代码在土耳其文化中失败。

Recipe 13.8 非常清楚地展示了这一点：

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

有关编写文化感知脚本的更多信息，请参阅 Recipe 13.6。

## 另请参阅

- Recipe 3.12，“了解类型和对象”
- Recipe 13.6，“编写文化感知脚本”
- Recipe 31.6，“将脚本块用作 .NET 委托或事件处理程序”

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

`Trim()` 方法会清除字符串开头*和*结尾的所有空白字符。如果你只想清除其中之一，可以分别调用 `TrimStart()` 或 `TrimEnd()` 方法来移除字符串开头或结尾的空白字符。如果你想从字符串开头或结尾移除特定字符，`Trim()`、`TrimStart()` 和 `TrimEnd()` 方法提供了相应的选项。要从字符串末尾修剪一组特定字符，请将该列表提供给该方法，如 示例 5-7 所示。

### 示例 5-7. 从字符串末尾修剪一组字符

```
PS > "Hello World".TrimEnd('d','l','r','o','W',' ')
He
```

如果你想替换字符串中的任意位置的文本（而不仅仅是开头或结尾），请参阅 Recipe 5.8。

> **注意**
>
> 乍一看，以下试图从字符串末尾修剪文本 `\"World\"` 的命令似乎工作不正确：
>
> ```
> PS > "Hello World".TrimEnd(" World")
> He
> ```
>
> 之所以发生这种情况，是因为 `TrimEnd()` 方法接受一个要从字符串末尾移除的字符列表。如果需要，PowerShell 会自动将字符串转换为字符列表，在本例中将你的字符串转换为字符 `W`、`o`、`r`、`l`、`d` 和一个空格。这些实际上与 示例 5-7 中使用的字符相同，因此效果也相同。

## 另请参阅

- Recipe 5.8，“替换字符串中的文本”

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

除了这些标准格式字符串外，`-Format` 参数还支持标准 .NET `DateTime` 格式字符串。这些格式字符串允许你以多种标准格式（如系统的短日期或长日期模式）或完全自定义的方式显示日期。有关如何指定标准 .NET `DateTime` 格式字符串的更多信息，请参阅 附录 E。

如果你已经习惯了 Unix 风格的日期格式字符串（或正在转换使用复杂格式字符串的现有脚本），`Get-Date` cmdlet 的 `-UFormat` 参数可能会有所帮助。它接受 Unix `date` 命令接受的格式字符串，但不提供标准 .NET 日期格式字符串无法提供的任何功能。

在处理日期和时间的字符串版本时，请注意它们是*国际化*问题最常见的来源——这些问题源于在与编写脚本时不同的文化环境的机器上运行脚本。在北美，“05/09/1998”表示“1998 年 5 月 9 日”。但在许多其他文化中，它表示“1998 年 9 月 5 日”。只要可能，请使用并比较 `DateTime` 对象（而不是字符串）与其他 `DateTime` 对象，因为这可以避免这些文化差异。示例 5-8 演示了这种方法。

### 示例 5-8. 使用 -gt 运算符比较 DateTime 对象



```
PS > $dueDate = [DateTime] "01/01/2006"
PS > if ([DateTime]::Now -gt $dueDate)
{
    "Account is now due"
}

Account is now due
```

## 注意

PowerShell *始终* 假定为北美日期格式，当它解释 `DateTime` 常量（如 `[DateTime] "05/09/1998"`）时。这与所有语言以北美格式解释数字常量（如 `12.34`）的原因相同。如果不是这样，几乎所有处理日期和时间的脚本都会在国际化系统上失败。

有关 `Get-Date` cmdlet 的更多信息，请输入 `Get-Help Get-Date`。有关以区分区域性的方式处理日期和时间的更多信息，请参阅 配方 13.6。

## 另请参阅

配方 13.6，“编写区分区域性的脚本”

附录 E，*.NET DateTime 格式化*

# 5.14 在一种格式与另一种格式之间转换字符串

## 问题

你有一系列文本字符串，想要将它们转换为另一种格式。

## 解决方案

使用 `Convert-String` cmdlet：

```
PS > $phoneNumbers = "5551212","4524587","2112132","8752113"
PS > $replacementExamples = "5551212=(425) 555-1212","4524587=(425) 452-4587"
PS > $phoneNumbers | Convert-String -Example $replacementExamples
(425) 555-1212
(425) 452-4587
(425) 211-2132
(425) 875-2113
```

## 讨论

`Convert-String` cmdlet 接受一种格式的输入文本并将其转换为输出格式。与 PowerShell 中通过正则表达式、捕获组和其他复杂主题实现此功能的特性不同，`Convert-String` cmdlet 只需你提供转换前的数据示例，以及转换完成后应有的样子。

`Convert-String` cmdlet 与 `ConvertFrom-String` cmdlet 基于可在 Excel 中找到的 Flash Fill 技术。它们是你在 Shell 中所能找到的最接近魔法的两样东西。`Convert-String` 不要求你指定转换文本输入所需的确切步骤序列，而是代表你学习这些操作。

除了“原始=替换”格式的示例外，你还可以提供具有 `Before` 和 `After` 属性的对象（如哈希表或 `PSCustomObject`）：

```
$examples =
    @{ Before = "Get-AclMisconfiguration.ps1"
       After = "Gets the AclMisconfiguration from the system" },
    @{ Before = "Get-AliasSuggestion.ps1"
       After = "Gets the AliasSuggestion from the system" }
```

```
PS > dir scripts\Get-* | ForEach-Object Name

Get-AclMisconfiguration.ps1
Get-AliasSuggestion.ps1
Get-Answer.ps1
Get-Arguments.ps1
Get-Characteristics.ps1
Get-Clipboard.ps1
Get-DetailedSystemInformation.ps1
(...)

PS > dir scripts\Get-* | ForEach-Object Name | Convert-String -Example $examples

Gets the AclMisconfiguration from the system
Gets the AliasSuggestion from the system
Gets the Answer from the system
Gets the Arguments from the system
Gets the Characteristics from the system
Gets the Clipboard from the system
Gets the DetailedSystemInformation from the system
(...)
```

与手写的正则表达式或 `String.Replace()` 调用一样，`ConvertFrom-String` 有时可能会误解你的意图。通常可以通过提供更多示例来解决这些问题。一旦你拥有一组能够表达你意图的示例，这些示例将继续适用于未来的类似文本。

有关使用 `String.Replace()` 方法或正则表达式修改字符串的更多信息，请参阅 配方 5.8。

## 另请参阅

配方 5.8，“替换字符串中的文本”

配方 5.15，“将文本流转换为对象”

# 5.15 将文本流转换为对象

## 问题

你有原始的非结构化文本，想要将其解析为 PowerShell 对象。

## 解决方案

使用 `ConvertFrom-String` cmdlet 的 `-Delimiter` 参数来解析简单列格式的数据。如果你不指定属性名称，PowerShell 会自动生成；如果可能，还会自动将字符串转换为更合适的数据类型：

```
$delimiter = "[ ]+(?=\d|Services|Console)"
$output = tasklist.exe | Select -Skip 3 | ConvertFrom-String -Delimiter $delimiter
```

```
PS > $output | Where-Object P2 -lt 1000 | Format-Table

P1                   P2 P3       P4 P5
--                   -- --       -- --
System Idle Process   0 Services  0 8 K
System                4 Services  0 2,072 K
Secure System        72 Services  0 39,256 K
Registry            132 Services  0 99,088 K
smss.exe            524 Services  0 1,076 K
(...)
```

你也可以使用 `-Delimiter` 参数来解析整个字符串。任何与你的捕获组匹配的文本将作为第二个及后续属性出现，你可以按喜好命名它们：

```
PS > $expression = 'FirstName=(.*);LastName=(.*)'
PS > $parsed = "FirstName=Lee;LastName=Holmes" |
    ConvertFrom-String -Delimiter $expression -Property Ignored,FName,LName
PS > $parsed.FName
Lee
PS > $parsed.LName
Holmes
```

使用 `-Template` 参数，根据你添加到模板中示例文本的标记，自动解析数据：

```
$template = @"
{FName*:Lee} {LName:Holmes}
{FName*:John} {LName:Smith}
"@

"Lee Holmes","Adam Smith","Some Body","Another Person" |
    ConvertFrom-String -TemplateContent $template
```

```
FName   LName
-----   -----
Lee     Holmes
Adam    Smith
Some    Body
Another Person
```

## 讨论

PowerShell 最强大的功能之一是其基于对象的管道。你无需浪费精力去创建、销毁和重新创建数据的对象表示形式。在其他 Shell 中，当管道将数据转换为纯文本时，你会失去数据的全保真表示。你可以通过过度的文本解析恢复其中一些，但无法恢复全部。

然而，你仍然经常需要与源自 PowerShell 之外的低保真输入进行交互。基于文本的数据文件和遗留程序就是两个例子。

PowerShell 为你可能从其他 Shell 中了解的所有三大文本解析主力工具提供了强大的支持：

**Sed**

: 替换文本。对于此功能，PowerShell 提供 `-replace` 运算符和 `Convert-String` cmdlet。

**Grep**

: 搜索文本。对于此功能，PowerShell 提供 `Select-String` cmdlet 等。

第三个传统的文本解析工具 **Awk**，允许你将一行文本切分为更直观的分组。为此，PowerShell 提供了功能极其强大的 `ConvertFrom-String` cmdlet。

在最简单的形式中，你可以使用 `ConvertFrom-String` cmdlet 根据你提供的分隔符解析面向列的输出。分隔符默认为连续的空白字符，但你也可以提供自定义字符串或更详细的正则表达式。如果可能，PowerShell 还会将文本转换为更合适的数据类型（如整数和日期）。

对于更复杂的需求，`ConvertFrom-String` cmdlet 支持示例驱动解析。与 `Convert-String` cmdlet 一样，这是你在 Shell 中所能体验到的最接近魔法的东西。`ConvertFrom-String` cmdlet 不是强迫你手写复杂的解析器，而是根据你在示例模板中标记数据的方式，自动学习如何提取数据。

让我们考虑尝试解析一个通讯录：

```
Record
------

FName: Lee
LName: Holmes

Record
------

FName: Adam
LName: Smith

Record
------

FName: Some
LName: Body

Last updated: 05/09/2021
```

要让 `ConvertFrom-String` 解析它，我们需要给它一个模板。理解模板的一个好方法是：想象获取一些示例输出，用鼠标高亮示例输出的区域，然后为这些区域命名。




# ConvertFrom-String 模板语法

在模板中，左大括号 `{` 表示选择的开始，右大括号 `}` 表示选择的结束。要命名选择，请在左大括号后提供属性名和冒号。因此，`PowerShell Rocks` 变为 `{FName:PowerShell} {LName:Rocks}`。

让我们开始创建模板。在新文件中，以下面的示例开始，并将其保存为 `addressbook.template.txt`（名称自定）：

```
{Record:Record
------

FName: Some
LName: Body}

Last updated: {LastUpdated:05/09/2021}
```

当你在此输入和模板上运行 `ConvertFrom-String` 时，我们得到：

```
PS > $book = Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt
PS > $book.LastUpdated
05/09/2021

PS > $book.Record

Record
------

FName: Lee
LName: Holmes
```

不过，有多条记录。要告知 `ConvertFrom-String` 输入包含某种模式的多个实例，请在属性名后使用星号：

```
{Record*:Record
------

FName: Some
LName: Body}

Last updated: {LastUpdated:05/09/2021}
```

如果我们运行此命令，会看到 `ConvertFrom-String` 还没有完全弄清记录格式。所以让我们再给它一个示例：

```
{Record*:Record
------

FName: Some
LName: Body}

{Record*:Record
------

FName: Adam
LName: Smith}

Last updated: {LastUpdated:05/09/2021}
```

现在，`ConvertFrom-String` 理解记录和页脚了：

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt)

Record
------
Record...
Record...
Record...

PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).LastUpdated

05/09/2021
```

要告知 `ConvertFrom-String` 记录的内部结构，我们只需标记并命名它。更新模板中的第一条记录：

```
(...)
FName: {FName:Some}
LName: {LName:Body}}
(...)
```

现在 `ConvertFrom-String` 完全理解我们的数据库格式了。

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt)

Record
------
{@{FName=Lee; LName=Holmes}}
{@{FName=Adam; LName=Smith}}
{@{FName=Some; LName=Body}}

PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).Record[0].FName
Lee
```

作为最后的魔术，让我们告诉 PowerShell `LastUpdate` 是 `[DateTime]` 类型。更新模板以包含：

```
(...)
Last updated: {[DateTime] LastUpdated:05/09/2021}
(...)
```

这会产生惊人的结果：

```
PS > (Get-Content addressbook.txt |
    ConvertFrom-String -TemplateFile addressbook.template.txt).LastUpdated

Sunday, May 9, 2021 12:00:00 AM
```

## 另请参阅

- 食谱 1.2，“运行程序、脚本和现有工具”
- 食谱 5.14，“在不同格式之间转换字符串”

# 5.16 生成大型报告和文本流

## 问题

你想要编写一个生成大型报告或大量数据的脚本。

## 解决方案

生成大量数据的最佳方法是尽可能利用 PowerShell 的流式处理行为。选择在命令之间通过管道传输数据的解决方案：

```
Get-ChildItem C:\*.txt -Recurse | Out-File c:\temp\AllTextFiles.txt
```

而不是在每个阶段收集输出：

```
$files = Get-ChildItem C:\*.txt -Recurse
$files | Out-File c:\temp\AllTextFiles.txt
```

如果你的脚本生成大型文本报告（且流式处理不可行），请使用 `StringBuilder` 类：

```
$output = New-Object System.Text.StringBuilder
Get-ChildItem C:\*.txt -Recurse |
    ForEach-Object { [void] $output.AppendLine($_.FullName) }
$output.ToString()
```

而不是简单的文本拼接：

```
$output = ""
Get-ChildItem C:\*.txt -Recurse | ForEach-Object { $output += $_.FullName }
$output
```

## 讨论

在 PowerShell 中，通过管道组合命令是一个基本概念。当脚本和 cmdlet 生成输出时，PowerShell 会尽快将该输出传递给管道中的下一个命令。在解决方案中，检索 *C:* 驱动器上所有文本文件的 `Get-ChildItem` 命令需要很长时间才能完成。然而，由于它们几乎立即*开始*生成数据，PowerShell 可以在 `Get-ChildItem` cmdlet 生成数据时立即将其传递给下一个命令。这适用于任何生成或消费数据的命令，称为*流式处理*。管道几乎在 `Get-ChildItem` cmdlet 完成生成数据时就完成了，并且非常高效地使用内存。

第二个 `Get-ChildItem` 示例（收集其数据）阻止了 PowerShell 利用这种流式处理机会。它首先将所有文件存储在数组中，由于数据量大，这需要很长时间和大量内存。然后，它将所有这些对象发送到输出文件，这也需要很长时间。

然而，大多数命令可以直接消费管道产生的数据，如 `Out-File` cmdlet 所示。对于这些命令，只要你将命令组合成管道，PowerShell 就会提供流式处理行为。对于不直接支持来自管道的数据的命令，`ForEach-Object` cmdlet（别名为 `foreach` 和 `%`）允许你在上一个命令生成数据时处理每一条数据，如 `StringBuilder` 示例所示。

### 创建大型文本报告

生成大型报告时，通常将整个报告存储到字符串中，脚本完成后再将该字符串写入文件。通常可以通过将文本直接流式传输到目标（文件或屏幕）来最高效地完成此操作，但有时这不可行。

由于 PowerShell 让向字符串末尾添加文本变得非常容易（如 `$output += $_.FullName`），许多人最初会选择这种方法。这对于中小型字符串效果很好，但对于大型字符串会导致严重的性能问题。

> **注意**
>
> 作为这种性能差异的示例，请比较以下内容：
>
> ```
> PS > Measure-Command {
>    $output = New-Object Text.StringBuilder
>    1..10000 |
>        ForEach-Object { $output.Append("Hello World") }
> }
>
> (...)
> TotalSeconds : 2.3471592
>
> PS > Measure-Command {
>    $output = ""
>    1..10000 | ForEach-Object { $output += "Hello World" }
> }
>
> (...)
> TotalSeconds      : 4.9884882
> ```
>
> 在 .NET Framework（因此也包括 PowerShell）中，字符串创建后永远不会改变。当你向字符串末尾添加更多文本时，PowerShell 必须通过组合两个较小的字符串来构建一个*新*字符串。对于大型字符串，此操作耗时很长，这就是 .NET Framework 包含 `System.Text.StringBuilder` 类的原因。与普通字符串不同，`StringBuilder` 类假设你将修改其数据——这一假设使其能更高效地适应变化。

# 5.17 生成源代码和其他重复性文本

## 问题

你想要简化大量重复性源代码或其他文本的创建。

## 解决方案

使用 PowerShell 的字符串格式化运算符（`-f`）将动态信息放入预格式化字符串中，然后为每条动态信息重复该替换。

## 讨论

代码生成是几乎所有从基于文本的输入产生输出的技术中都有用的技术。例如，想象一下必须创建一个 HTML 报告来显示当时系统上运行的所有进程。在这种情况下，“代码”是 Web 浏览器理解的 HTML 代码。

HTML 页面以一些标准文本（`<html>`、`<head>`、`<body>`）开始，然后你可能会在 HTML `<table>` 中包含进程。每行将包含你正在处理的进程中每个属性的列。



