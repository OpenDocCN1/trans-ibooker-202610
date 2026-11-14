
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

如果要支持命名替换（而非基于索引的替换），可以使用 `Format-String` 脚本（见[示例 5.17](#ch05.xhtml#generate_source_code)）。

除了字符串格式化运算符，PowerShell 还提供三个格式化命令（`Format-Table`、`Format-Wide` 和 `Format-List`），可让你轻松生成格式化报表。有关这些 cmdlet 的详细信息，请参阅[“自定义格式化文件”](#app01.xhtml#formatting_output-id001)。

## 另请参阅

- [示例 3.15“创建和初始化自定义对象”](#ch03.xhtml#create_and_initialize_custom_objects)
- [“简单运算符”](#app01.xhtml#simple_operators)
- [“自定义格式化文件”](#app01.xhtml#formatting_output-id001)
- [附录 D *.NET 字符串格式化*](#app04.xhtml#net_string_formatting)

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

由于 PowerShell 字符串是功能完备的 .NET 对象，它们直接支持许多面向字符串的操作。`Contains()` 和 `IndexOf()` 方法只是 `String` 类支持的众多功能中的两个例子。要了解 `String` 类支持的其他功能，请参阅[示例 3.12](#ch03.xhtml#learn_about_types_and_objects)。

**注意**

若要在整个文件中搜索文本或模式，请参阅[示例 9.4](#ch09.xhtml#search_a_file_for_text_or_a_pattern)。

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

有关 `-like` 运算符的详细说明，请参阅[“比较运算符”](#app01.xhtml#comparison_operators)。有关 `-match` 运算符的详细说明，请参阅[“简单运算符”](#app01.xhtml#simple_operators)。有关正则表达式规则和语法的详细列表，请参阅[附录 B](#app02.xhtml#regular_expression_reference)。

有时在尝试将 PowerShell 命令的结果存储在字符串中时会遇到困难，如[示例 5-2](#ch05.xhtml#attempting_to_store_output_of_a_powershe)所示。

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

`-match` 运算符在字符串中搜索指定的模式，但在这种情况下似乎失败了。这是因为所有 PowerShell 命令都生成对象。如果不将输出存储在另一个变量中或传递给另一个命令，PowerShell 会在显示给你之前将输出转换为文本表示形式。在[示例 5-2](#ch05.xhtml#attempting_to_store_output_of_a_powershe)中，`$helpContent` 是一个功能完备的对象，而不仅仅是其字符串表示形式：

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

有关使搜索文本命令输出更容易的脚本，请参阅[示例 1.24](#ch01.xhtml#select-textoutput)。

### 另请参阅

- [示例 1.24“程序：搜索格式化输出中的模式”](#ch01.xhtml#select-textoutput)
- [示例 3.12“了解类型和对象”](#ch03.xhtml#learn_about_types_and_objects)
- [“简单运算符”](#app01.xhtml#simple_operators)
- [“比较运算符”](#app01.xhtml#comparison_operators)
- [附录 B *正则表达式参考*](#app02.xhtml#regular_expression_reference)

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

有关使用输入和输出示例自动学习如何替换字符串中文本的方法，请参阅[示例 5.14](#ch05.xhtml#convert_string_between_formats)。

考虑到其使用的正则表达式的强大功能，`-replace` 运算符也带来了正则表达式的一些陷阱。

首先，与 `-replace` 运算符一起使用的正则表达式通常包含字符（如美元符号，它代表组号），PowerShell 通常将其解释为变量名或转义字符。为防止 PowerShell 解释这些字符，请使用非扩展字符串（单引号），如解决方案所示。
