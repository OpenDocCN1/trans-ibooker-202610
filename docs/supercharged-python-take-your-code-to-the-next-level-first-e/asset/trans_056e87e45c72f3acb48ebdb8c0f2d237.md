

Public, John Q.

如果需要，按姓氏字母顺序排列它们则是一件轻而易举的事。但如果有人输入的名字中没有中间名缩写，该怎么办呢？

Jane Austen
Mary Shelley

理想情况下，我们希望也能处理这类情况。我们需要一个模式，能够平滑地处理这两种情况——有中间名缩写或没有。

Austen, Jane
Shelley, Mary

那么，让我们从简单的情况开始：只有名和姓。将两个组分别标记为 `first` 和 `last` 尤其方便，如下面的模式所示。

Click here to view code image

pat = r'(?P<first>\w+) (?P<last>\w+)'

我们可以在程序中成功应用这个模式，让用户输入全名，然后对其进行拆解和分析。

import re
s = 'Jane Austen'
m = re.match(pat, s)

运行这段代码后，我们就可以打印姓名的两个部分了。请注意，在将组名传递给 **print** 函数之前，组名必须是字符串格式——因此必须用单引号括起来。

Click here to view code image

```
print('first name = ', m.group('first'))
print('last name = ', m.group('last'))
```

这将打印

```
first name = Jane
last name = Austen
```

有了这种划分，就可以轻松地以“姓，名”的顺序打印或存储姓名：

Click here to view code image

```
print(m.group('last') + ', ' + m.group('first'))
```

这将打印

```
Austen, Jane
```

识别中间名缩写并将其放在正确位置的模式要复杂一些。让我们将这个中间名缩写设为可选。

Click here to view code image

```
pat = r'(?P<first>\w+) (?P<mid>\w\. )?(?P<last>\w+)'
```

请注意，名字后面的空格是强制性的，但中间名缩写后面只有在匹配到中间名时才会跟随一个空格。如果将此模式与姓名进行匹配，它会可选地识别中间名缩写，但不会强制要求。因此，以下所有姓名都能成功匹配：

```
Brian R. Overland
John R. Bennett
John Q. Public
Jane Austen
Mary Shelley
```

在每种情况下，都可以访问 group(name)，其中 name 为 'first'、'mid' 或 'last'。但是，在某些情况下（即未匹配到该命名组时），group('mid') 会返回特殊值 None。不过，我们可以对其进行测试。因此，我们可以编写以下函数来拆解姓名并重新格式化。

Click here to view code image

```
pat = r'(?P<first>\w+) (?P<mid>\w\. )?(?P<last>\w+)'

def reorg_name(in_s):
    m = re.match(pat, in_s)
    s = m.group('last') + ', ' + m.group('first')
    if m.group('mid'):
        s += ' ' + m.group('mid')
    return s
```

通过将此函数应用于输入的每个姓名，将结果放入列表中，然后对列表进行排序，我们就可以将所有姓名以字母顺序（姓在前，名在后）的格式存储：

```
Austen, Jane
Bennett, John R.
Overland, Brian R.
Public, John Q.
Shelley, Mary
```

在这种情况下，使用命名组很有帮助，因为它给了我们一种方式来引用一个组——即中间名缩写和点号——而这个组可能根本不会被匹配。无论如何，能够将组称为“first”、“mid”和“last”，使代码更清晰，也更易于维护。

作为本节的最后一个示例，你可以使用命名组来要求重复先前标记的字符序列。第 6 章展示了如何使用数字来引用命名组的重复。

```
pat = r'(\w+) \1'
```

这个模式的命名组版本是

Click here to view code image

```
pat = r'(?P<word>\w+) (?P=word)'
```

这个模式在以下函数调用中能得到正匹配：

Click here to view code image

```
m = re.search(pat, 'The the dog.', flags=re.I)
```

## 7.8 “RE.SPLIT” 函数

考虑前面章节中介绍的逆波兰表示法（RPN）解释器。使用正则表达式帮助将文本分析为标记的另一种方法是使用 `re.split` 函数。

![](img/6e45221a8147f2442c653a3d4a866570_449_0.png)

Click here to view code image

```
list = re.split(pattern, string, maxsplit=0, flags=0)
```

在这个语法中，_pattern_ 是一个正则表达式模式，支持到目前为止展示的所有语法；然而，它指定的不是要查找的模式，而是要跳过的模式。中间的所有文本都被视为一个标记。因此，_pattern_ 实际上代表的是标记分隔符，而不是标记本身。

_string_ 与往常一样，是要拆分为标记的目标字符串。_maxsplit_ 参数指定要查找的标记的最大数量。如果将此参数设置为 0（默认值），则没有最大数量限制。

`re.split` 函数的操作是返回一个字符串列表，其中每个字符串都是一个标记，在此情况下，标记是出现在指定搜索模式出现之间的一段文本。

通常，将搜索模式设为空格、一系列空格或逗号。使用正则表达式的一个优点是，你可以将这些组合起来：

```
pat = r', *| +'
```

这个模式实际上是说，“如果一个子字符串由逗号后跟零个或多个空格组成，或者由一个或多个空格组成，则它是一个分隔符。” 如果你仔细想想，这个条件会创造一种情况，即分隔符可以是以下任何一种：一个逗号、一系列至少一个空格，或两者兼有。让我们在目标字符串上尝试这个模式。

Click here to view code image

```
import re
lst = re.split(pat, '3, 5 7 8,10, 11')
```

如果你现在打印列表，你会得到

Click here to view code image

```
['3', '5', '7', '8', '10', '11']
```

这正是我们希望得到的结果。在这种情况下，所有生成的标记都是数字，但它们也可以是任何不包含逗号或内部空格的子字符串。让我们将这个模式应用于 RPN 解释器。你可以使用 `re.split` 函数来拆分如下文本：

```
s = '3 2 * 2 15 * + 4 +'
```

如果你记得 RPN 的工作原理，你会认识到这是以下内容的 RPN 表示：

```
(3 * 2) + (2 * 15) + 4
```

让我们将正则表达式函数应用于目标字符串 s：

```
toks = re.split(pat, s)
```

打印 toks（标记列表）会产生

Click here to view code image

```
['3', '2', '*', '2', '15', '*', '+', '4', '+']
```

这正是我们所期望的。

但是，在对如下字符串进行标记化时会出现问题，该字符串在某些情况下使用数字到运算符的转换来划分标记：

```
s = '3 2* 2 15*+ 4 +'
```

## 7.9 SCANNER 类和 RPN 项目

分析 RPN 应用程序输入的另一种方法是使用 Python 正则表达式包中一个方便的部分，截至撰写本文时，该部分在极少数地方有文档记录。

`re.Scanner` 类使你能够创建自己的 Scanner 对象。你需要通过给它一系列元组来初始化该对象。每个元组包含以下内容：

- 描述要搜索的标记的正则表达式模式。
- 在找到标记时要调用的函数。该函数本身被列为一个对象（它确实是；它是一个可调用对象）。但此函数不是作为调用列出的，并且不包含参数。（但是，调用时会传递两个参数。）该函数可以返回任何类型的对象，并且该对象会连同找到的其他标记一起返回。

然后，通过调用 `scan` 在目标字符串上运行扫描器时，它会按照编程返回一系列对象。这种方法的优点，正如你将看到的，是你不必担心分隔符；你只需查找想要找到的标记即可。

在这里，我们总结了这部分语法。除非你使用 lambda 函数，否则这部分语法应该出现在函数定义之后。

![](img/6e45221a8147f2442c653a3d4a866570_452_0.png)

```
scanner_name = re.Scanner([
    (tok_pattern1, funct1),
    (tok_pattern2, funct2),
    ...
])
```

在这个语法中，每个 `tok_pattern` 实例都是描述某种要识别的标记的正则表达式。每个 _funct_ 是先前定义的可调用对象或 lambda。如果指定 **None** 作为函数，则对于关联的模式不采取任何操作；它会被跳过。

在展示如何编写标记处理函数之前，这里有一个为 RPN 项目编写的示例：

```
scanner = re.Scanner([
    (r'[*/-]', sc_oper),
    (r'\d+\.\d*', sc_float),
    (r'\d+', sc_int),
    (r'\s+', None)
])
```

这个示例表示，“识别三种类型的标记——运算符、整数和浮点数——并通过调用相应的函数来处理每个标记。”

> **注意**
>
> 在这个示例中，重要的是浮点数模式应列在整数模式之前。否则，像 11.502 这样的浮点数会被读作整数 11，后跟一个点（.），再后跟另一个整数。

稍后，在第 8 章中，我们将向 RPN 语言添加变量名（也称为 _标识符_ 或 _符号_）。这些是 RPN 语言中的变量。

Click here to view code image

```
scanner = re.Scanner([
    (r'[a-zA-Z\w*', sc_ident),
    (r'[*/-]', sc_oper),
])
```

