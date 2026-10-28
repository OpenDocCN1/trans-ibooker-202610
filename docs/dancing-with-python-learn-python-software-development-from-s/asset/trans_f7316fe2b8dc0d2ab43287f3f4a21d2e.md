
如果你只想知道一个**字符串**是否包含在另一个字符串中，请使用 **in**。

```
"dream" in quote
```

True

```
"nightmare" in quote
```

False

要测试一个字符串是否不包含在另一个字符串中，请使用 **not in**。

```
"Hamlet" not in quote
```

True

## 2.7.3 切片和提取子字符串

我们刚刚看到了如何访问单个字符以及如何在**字符串**中查找子字符串的位置。我们使用 *切片表示法* 来提取字符串的一部分，并将其作为新字符串返回。

以我们的 quote 为例：

```
quote
```

'perchance to dream'

我们执行以下操作来提取从索引 3 开始、长度为 6 的子字符串：

```
quote[3:9]
```

'chance'

冒号前面的数字是起始索引，冒号后面的数字是起始索引加上子字符串长度。表达式 **quote[3:9]** 的意思是：

> 从索引 3 开始，收集 quote 中
> 该索引以及所有小于 9 的有效索引处的字符。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_79_0.png)

与加法类似，我们可以使用乘法来重复字符串多次。

```
3 * text
```

'la la la '

```
10 * text
```

'la la la la la la la la la la '

```
0 * text
```

''

Python 提供了多种将数据和文本格式化为字符串的方式。最新的一种称为格式化字符串字面量，或者更简单地说，称为 f-string。

要创建 f-string，请在起始引号定界符前加上 f 或 F。在字符串内部，花括号“{”和“}”中的任何表达式都会被求值，结果会被粘贴回字符串的相应位置。

```
a = 2
b = 5
f"The sum of a and b is {a + b}"
```

'The sum of a and b is 7'

如果你不在 f-string 中求值表达式，就不要使用它们，因为 Python 会浪费时间扫描花括号。我经常将 f-string 与 print 函数一起使用，来格式化显示给用户的信息。

我将在第 4 章“字符串伴你同行”中更详细地介绍字符串及其操作。

## 2.8 列表操作

你可以在列表上执行许多与字符串相同的操作，包括切片。

```
years = [2021, 1983, 1976, 1997, 1990]
years
```

[2021, 1983, 1976, 1997, 1990]

```
len(years)
```

5

```
years[0]
```

2021

```
years[-2]
```

1997

```
years[1:3]
```

[1983, 1976]

字符串和列表之间的一个主要区别是，你可以更改列表中的元素：列表是可变的。

```
years[0] = 2000
years
```

[2000, 1983, 1976, 1997, 1990]

对于字符串，任何修改都必须构造一个新的、不同的字符串。字符串是不可变的，这意味着你不能更改它们的结构或内容。我可以让一个变量指向新字符串，但我不能更改字符串对象本身。

```
letters = "abcd"
letters
```

'abcd'

```
letters[0] = "A"
```

TypeError: 'str' object does not support item assignment

```
letters = "A" + letters[1:]
letters
```

'Abcd'

既然我们知道可以更改列表，我们就可以看看一些原地修改列表的操作和方法。

```
years = [2021, 1983, 1976, 1997, 1990]
years
```

[2021, 1983, 1976, 1997, 1990]

“+” 运算符将两个列表拼接成一个新列表。

```
years + [1961, 1980]
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980]

```
years
```

[2021, 1983, 1976, 1997, 1990]

*扩展* 方法将第二个列表中的元素追加到第一个列表元素的后面。

```
years.extend([1961, 1980])
years
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980]

使用 *追加* 方法将新元素放置在**列表**的末尾。

```
years.append(2012)
years
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980, 2012]

程序员在处理信息和构建数据列表时经常这样做。例如，他们可能以编程方式扫描收入报告，并列出所有收入超过 100 万美元的年份。

*插入* 方法将新元素放置在指定索引处。该索引及更高索引处的所有元素都会向右移动一位。此示例将数字 100 插入到第二个位置。请记住：第二个位置的索引是 1。

```
years.insert(1, 1812)
years
```

[2021, 1812, 1983, 1976, 1997, 1990, 1961, 1980, 2012]

我们可以使用 *弹出* 方法再将其取出。

```
years.pop(1)
```

1812

```
years
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980, 2012]

如果你不带参数使用 *弹出*，它会移除并返回列表中的最后一个元素。从这个意义上说，它与 *追加* 相反。

```
years.pop()
```

2012

```
years
```

[2021, 1983, 1976, 1997, 1990, 1961, 1980]

我将在 3.2 节中详细介绍列表及其操作。

## 2.9 打印

到目前为止，我们只看到了 Python 显示对象或表达式求值后的值。

```
"This is a sample string"
```

'This is a sample string'

如果你是向代码的使用者显示信息，则需要更复杂的工具来从数据生成文本。*打印* 函数是一种将字符串和对象组合成可读形式的通用方法。

在最简单的形式中，*打印* 接受一个**字符串**，或一个可以转换为**字符串**的对象，并将其显示在控制台上。Python 不会显示定界引号。

```
print("This is a sample string")
```

This is a sample string

```
print(17 + 4)
```

21

你可以一次打印多个对象，Python 会用空格分隔它们。

```
print(57, 99, -4.3)
```

57 99 -4.3

### 2.9.1 拼接字符串

你可以在 print 中使用 “+” 拼接字符串。以这种方式连接字符串时，请对任何非字符串对象使用 str。

```
print("Last year's revenue was " + "3" + " million euros")
```

Last year's revenue was 3 million euros

```
print("Last year's revenue was " + 3 + " million euros")
```

TypeError: can only concatenate str (not "int") to str

```
print("Last year's revenue was " + str(3) + " million euros")
```

Last year's revenue was 3 million euros

### 2.9.2 f-字符串

更好的是，使用 f-string。

```
print(f"Last year's revenue was {3} million euros")
```

Last year's revenue was 3 million euros

在这种情况下，我们可以在花括号内编写表达式或函数调用。

```
print(f"Last year's revenue was {revenue(2020)} million euros")
```

Last year's revenue was 3 million euros

我在这里没有定义 revenue，但你可以假设它能访问数据库，或者从列表或其他数据结构中获取结果。

我们在 2.2 节中看到，可以在字符串中使用与定界符相同类型的引号：

```
"Don't use \"odd\" quoted words in your prose."
```

'Don\'t use "odd" quoted words in your prose.'

Python 在输出中插入了一个反斜杠来转义单引号。

### 2.9.3 换行符

这就是通过在字符前插入反斜杠来*转义*字符。另一个例子是在**字符串**中放置换行符：使用 '\n' 插入换行。

```
"See how I broke\nthis line?"
```

```
'See how I broke\nthis line?'
```

当你打印这个字符串时，换行是显而易见的。

```
print("See how I broke\nthis line?")
```

```
See how I broke
this line?
```

空格和换行是字符串中*空白字符*的示例。

默认情况下，*打印* 在显示输出后会插入一个换行符。你可以使用可选的*关键字参数* end 来将其改为其他内容。end 最常见的值是空格 ' ' 和空字符串 ""。

```
print("First string")
print("Second string")
```

```
First string
Second string
```

```
print("First string", end=' ')
print("Second string")
```

```
First string Second string
```

默认行为使用换行符 '\n' 作为 end。我们将在 6.5 节中学习更多关于关键字参数的内容。

## 2.10 条件语句

条件语句是这样一种语句：“如果这是真的，就做那件事，否则就做另一件事。” 我们需要自己决定“这”、“那件事”和“另一件事”的含义。

条件语句会改变执行流程。条件语句不是按顺序逐个求值表达式，而是允许我们根据某个条件是否为真来分支执行不同操作。

