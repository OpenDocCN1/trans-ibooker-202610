

1 打印一个 20 × 20 的星号或星号（*）矩阵。在 IDLE 中，演示完成此任务的最慢方式和最快方式。（提示：最快的方式是否使用字符串拼接或 join 方法？）进行比较和对比。然后使用装饰器来分析打印星号的两种方式的速度。

2 编写一个生成器，打印直到指定限制的所有整数的完全平方数。然后编写一个函数，判断一个整数参数是否是完全平方数，如果它落在这个序列中——也就是说，如果 n 是一个整数参数，短语 n in square_iter(n) 应该返回 True 或 False。

## 5. 精确格式化文本

在为商业和专业用途编程时，你希望格式化文本以创建外观精美的表格和演示文稿。在这方面，Python 拥有丰富的资源。它有几种方式来修改和增强以文本字符形式打印的信息。

本章详细介绍了所有三种方法，从字符串格式化运算符 %s 开始，它通常提供最快速、最简单的解决方案。为了获得最完整的控制，你可能希望使用 format 函数或 format 方法，它们支持许多选项，甚至允许你打印带有千位分隔符（,）的大数字。

### 5.1 使用百分号运算符（%）进行格式化

这是一个格式化输出的简单问题。假设你想以以下形式打印一个句子，其中 a、b 和 c 当前分别等于 25、75 和 100，但它们可以有任何值。你希望通过引用变量来得到以下结果。

```
25 plus 75 equals 100.
```

这应该很容易。但是，如果你使用 **print** 函数，它会在数字 100 和点（.）之间放置一个空格，因此你会得到以下结果：

```
25 plus 75 equals 100.
```

你如何处理那个不需要的空格？**print** 函数允许你通过将 **sep** 参数设置为空字符串来关闭打印字段之间默认的空格放置。但是在这种情况下，你必须自己放置所有空格。

Click here to view code image

```
print(a, ' plus ', b, ' equals ', c, '.', sep='')
```

这有效，但很丑陋。

更好的方法是使用 **str** 类格式化运算符（%）来格式化输出，使用类似于 C 语言“printf”函数的格式说明符。以下是修改示例的方法：

Click here to view code image

```
print('%d plus %d equals %d.' % (a, b, c))
```

这不是更好吗？

表达式 `(a, b, c)` 实际上是一个包含三个参数的元组，每个参数分别对应格式字符串中 `%d` 的每个出现。`(a, b, c)` 中的括号是严格要求的——除非只有一个参数，否则不需要。

Click here to view code image

```
>>> 'Here is a number: %d.' % 100
'Here is a number: 100.'
```

当然，这些元素可以通过编程方式拆分。以下是一个示例：

```
n = 25 + 75
fmt_str = 'The sum is %d.'
print(fmt_str % n)
```

此示例打印以下内容：

```
The sum is 100.
```

字符串格式化运算符，%，可以出现在以下两个版本之一中。

![](img/6e45221a8147f2442c653a3d4a866570_290_0.png)

Click here to view code image

```
format_str % value    # Single value
format_str % (values)   # One or more values
```

如果有多个 _value_ 参数，则与打印字段（由类型字符和百分号 % 标记）对应的参数必须放在元组内。以下两个语句都是有效的：

```
print('n is %d' % n)
print('n is %d and m is %d' % (n, m))
```

下一个示例也有效，因为它将三个数字组织成一个元组。

```
tup = 10, 20, 30
print('Answers are %d, %d, and %d.' % tup)
```

这些语句打印以下内容：

```
Answers are 10, 20, and 30.
```

### 5.2 百分号（%）格式说明符

格式说明符 %d 代表十进制整数。它是一种常见格式，但格式化运算符（%）也适用于其他格式，如表 5.1 所示。

# Table 5.1. Percent-Sign Specifiers

| Specif | Meaning | Example |
|--------|---------|---------|
| %d | Decimal integer. | 199 |
| %i | Integer. Same meaning as %d. | 199 |
| %s | Standard string representation of the input. This field says, “Produce a string,” but it can be used to print the standard string representation of any data object. So this can actually be used with integers if you choose. | Thomas |
| %r | Standard %r representation of the input, which is often the same as %s but uses the canonical representation of the object as it appears in Python code. (For more information, see Section 5.7, “‘Repr’ Versus String Conversion.”) | 'Bob' |
| %x | Hexadecimal integer. | ff09a |
| %X | Same as %x, but letter digits A–F are uppercase. | FF09A |
| %o | Octal integer. | 177 |
| %u | Unsigned integer. (But note that this doesn’t reliably change signed integers into their unsigned equivalent, as you’d expect.) | 257 |

| 格式说明符 | 描述 | 示例输出 |
|---|---|---|
| `%f` | Floating-point number to be printed in fixed-point format | `3.1400` |
| `%F` | Same as `%f`. | `33.1400` |
| `%e` | Floating-point number, printing exponent sign (e). | `3.1400e+00` |
| `%E` | Same as `%e` but uses uppercase `E`. | `3.1400E+00` |
| `%g` | Floating point, using shortest canonical representation. | `7e-06` |
| `%G` | Same as `%g` but uses uppercase `E` if printing an exponent. | `7E-06` |
| `%%` | A literal percent sign (`%`). | `%` |

以下是一个示例，使用 `int` 转换和十六进制输出来添加两个十六进制数：e9 和 10。

```
h1 = int('e9', 16)
h2 = int('10', 16)
print('The result is %x.' % (h1 + h2))
```

该示例打印

```
The result is f9.
```

因此，将十六进制 e9 和十六进制 10 相加得到十六进制 f9，这是正确的。
此示例中 h1 和 h2 周围的括号是必要的。否则，示例将使用 h1 作为数据创建格式化字符串，然后尝试将该字符串与数字 h2 连接，从而导致错误。

```
print('The result is %x.' % h1 + h2) # ERROR!
```

当你打印十六进制或八进制数字时，格式化运算符（%）不会在数字前面添加前缀。如果你想打印带有前缀的十六进制数字，你需要自己指定它们。

```
print('The result is 0x%x.' % (h1 + h2))
```

该语句打印以下内容：

```
The result is 0xf9.
```

在较大的字符串中打印子字符串（%s）是格式化运算符的另一种常见用法。以下是一个示例：

Click here to view code image

```
s = 'We is %s, %s, & %s.' % ('Moe', 'Curly',
'Larry')
print(s)
```

这打印

```
We is Moe, Curly, & Larry.
```

这些格式的行为可以通过使用宽度和精度数字来更改。每个打印字段的格式如下所示，其中 c 代表表 5.1 中的一个格式字符。

![](img/6e45221a8147f2442c653a3d4a866570_295_0.png)

```
%[-][width][.precision]c
```

在此语法中，方括号表示可选项，并非字面意思。减号（–）指定打印字段内的左对齐。使用此技术，默认情况下所有数据类型均为右对齐。
但是，以下示例使用左对齐，这不是默认值，方法是将减号（–）作为说明符的一部分。

Click here to view code image

```
>>> 'This is a number: %-6d.' % 255
'This is a number: 255 .'

```

至于语法的其余部分，格式说明符可以采用以下任何一种格式。

```
%c
%widthc
%width.precisionc
%.precisionc

```

对于字符串值，如果指定了 _width_，则文本将放置在大小为 _width_ 的打印字段中。子字符串默认右对齐，并用空格填充。如果打印字段小于子字符串的长度，则忽略 _width_。如果包含 _precision_，则指定字符串的最大大小，如果更长则会被截断。

以下是使用 10 空格打印字段宽度的示例。

Click here to view code image

```
print('My name is %10s.' % 'John')

```

这打印以下内容，包括六个空格的填充。

```
My name is     John.

```

