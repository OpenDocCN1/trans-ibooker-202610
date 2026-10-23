

真实的程序不会去计算那些我们已经知道取值的表达式。我们不需要 Python 来告诉我们 15 不等于 10。更常见的是，我们会在这类表达式中使用变量。例如，`number != 10` 就是一个取值取决于 `number` 所引用对象的表达式。

关系运算符同样适用于字符串。检查相等性时，大小写很重要：

```
>>> 'hello' == 'hello'
True
>>> 'Hello' == 'hello'
False
```

如果一个字符串在字母顺序中排在另一个之前，那么它就小于另一个字符串：

```
>>> 'brave' < 'cave'
True
>>> 'cave' < 'cavern'
True
>>> 'orange' < 'apple'
False
```

但当同时涉及小写和大写字符时，情况可能会出人意料：

```
>>> 'apple' < 'Banana'
False
```

很奇怪，对吧？这与字符在计算机内部的存储方式有关。一般来说，大写字母在字母顺序上位于小写字母之前。再看看这个：

```
>>> '10' < '4'
True
```

如果这些是数字，那么结果会是 False。但字符串是从左到右逐字符进行比较的。Python 会比较 `'1'` 和 `'4'`，由于 `'1'` 更小，`<` 运算符返回 True。请确保你的值的类型和你认为的一致！

有一个关系运算符 `in` 适用于字符串而不适用于数字。如果第一个字符串至少出现在第二个字符串中一次，它就返回 True，否则返回 False：

```
>>> 'ppl' in 'apple'
True
>>> 'ale' in 'apple'
False
```

### 概念检查

以下代码的输出是什么？

```
a = 3
b = (a != 3)
print(b)
```

- A. True
- B. False
- C. 3
- D. 此代码会产生语法错误

**答案：B。表达式 a != 3 的计算结果为 False；随后 b 被赋值为这个 False 值。**

## if 语句

现在我们来探索 Python 的 if 语句的几种变体。

### *单独的 if*

假设我们有两个变量 *apple_total* 和 *banana_total* 存储最终得分，并且如果 *apple_total* 大于 *banana_total*，我们想输出 A。我们可以这样做：

```
>>> apple_total = 20
>>> banana_total = 6
>>> if apple_total > banana_total:
...     print('A')
...
A
```

正如我们所料，Python 输出了 A。

if 语句以关键字 `if` 开头。*关键字* 是对 Python 有特殊含义且不能用作变量名的单词。关键字 `if` 后面跟着一个布尔表达式、一个冒号，以及一个或多个缩进的语句。这些缩进的语句通常被称为 if 语句的*代码块*。如果布尔表达式为 True，代码块就会执行；如果布尔表达式为 False，代码块就会被跳过。

注意提示符从 `>>>` 变成了 `...`。这提醒我们已经处于 if 语句的代码块内部，必须缩进代码。我选择缩进四个空格，所以要缩进代码的话，请按四次空格键。有些 Python 程序员会按 TAB 键来缩进，但本书中我们只使用空格。

当你输入 `print('A')` 并按下回车键后，你应该会看到另一个 `...` 提示符。由于这个 if 语句中我们没有其他内容要写，再次按下回车键来取消这个提示符并返回 `>>>` 提示符。这次额外的回车键操作是 Python shell 的一个奇特之处；当我们在文件中编写 Python 程序时，并不需要这样的空行。

让我们看一个在 if 语句代码块中放入两条语句的例子：

```
>>> apple_total = 20
>>> banana_total = 6
>>> if apple_total > banana_total:
...     print('A')
...     print('Apples win!')
...
A
Apples win!
```

两条 print 调用都会执行，产生两行输出。

让我们试试另一个 if 语句，这次它的布尔表达式为 False：

```
>>> apple_total = 6
>>> banana_total = 20
>>> if apple_total > banana_total:
...     print('A')
...
```

这次 print 函数没有被调用：`apple_total > banana_total` 为 False，所以 if 语句的代码块被跳过了。

### 带 elif 的 if

让我们使用三个连续的 if 语句：如果苹果队获胜就输出 A，如果香蕉队获胜就输出 B，如果平局就输出 T：

```
>>> apple_total = 6
>>> banana_total = 6
>>> if apple_total > banana_total:
...     print('A')
...
>>> if banana_total > apple_total:
...     print('B')
...
>>> if apple_total == banana_total:
...     print('T')
...
T
```

前两个 if 语句的代码块被跳过了，因为它们的布尔表达式为 False。但第三个 if 语句的代码块会执行，输出 T。

当你把一个 if 语句放在另一个后面时，它们是相互独立的。每个布尔表达式都会被求值，无论前面的布尔表达式是 True 还是 False。

对于任意给定的 apple_total 和 banana_total 值，我们的 if 语句中只有一个能够执行。例如，如果 `apple_total > banana_total` 为 True，那么第一个 if 语句会执行，但另外两个不会。我们可以编写代码来强调只允许一个代码块执行。下面是我们如何实现这一点：

```
① >>> if apple_total > banana_total:
...     print('A')
② ... elif banana_total > apple_total:
...     print('B')
③ ... elif apple_total == banana_total:
...     print('T')
...
T
```

这现在是一个单独的 if 语句，而不是三个独立的 if 语句。因此，不要在 `...` 提示符处按回车键；而是输入 `elif` 行。

为了执行这个 if 语句，Python 首先对第一个布尔表达式①进行求值。如果它为 True，那么就输出 A，并跳过其余的 elif 部分。如果它为 False，Python 继续对第二个布尔表达式②进行求值。如果它为 True，那么就输出 B，并跳过剩下的 elif 部分。如果它为 False，Python 继续对第三个布尔表达式③进行求值。如果它为 True，那么就输出 T。

关键字 `elif` 代表“else-if”。把它当作一个提醒：只有当 if 语句中它之前的所有“其他”部分都没有执行时，才会检查 `elif` 表达式。

这个版本的代码等同于之前使用三个独立 if 语句的代码。如果我们想允许执行超过一个代码块，就必须使用三个独立的 if 语句，而不是一个带有 `elif` 代码块的 if 语句。

### 带 else 的 if

如果 if 语句中的所有布尔表达式都为 False，我们可以使用 `else` 关键字来运行代码。下面是一个例子：

```
>>> if apple_total > banana_total:
...     print('A')
... elif banana_total > apple_total:
...     print('B')
... else:
...     print('T')
...
T
```

Python 会从上到下对布尔表达式进行求值。如果其中任何一个为 True，Python 就会运行对应的代码块，并跳过 if 语句的其余部分。如果所有布尔表达式都为 False，Python 就会执行 `else` 代码块。

注意不再需要测试 `apple_total == banana_total`。到达 if 语句的 `else` 部分的唯一条件是 `apple_total > banana_total` 为 False 且 `banana_total > apple_total` 为 False，也就是说，当两个值相等时。

你应该使用独立的 if 语句吗？还是带 elif 的 if 语句？还是带 else 的 if 语句？这通常取决于个人偏好。如果你希望最多只有一个代码块执行，就使用一串 elif。`else` 有助于让代码更清晰，并且省去了编写兜底布尔表达式的需要。比 if 语句的具体风格重要得多的是编写正确的逻辑！

### 概念检查

以下代码运行后，x 的值是多少？

```
x = 5
if x > 2:
    x = -3
if x > 1:
    x = 1
else:
    x = 3
```

- A. -3
- B. 1
- C. 2
- D. 3
- E. 5

**答案：D。因为 x > 2 为 True，第一个 if 语句的代码块会执行。赋值语句 x = -3 使 x 指向 -3。现在看第二个 if 语句。这里 x > 1 为 False，所以 else 代码块执行，x = 3 使 x 指向 3。我建议把 if x > 1 改成 elif x > 1，然后观察程序行为的变化！**

### 概念检查

下面的两段代码做的事情完全一样吗？假设 temperature 已经指向一个数字。

**代码片段 1：**

```
if temperature > 0:
    print('warm')
elif temperature == 0:
    print('zero')
else:
    print('cold')
```

**代码片段 2：**

```
if temperature > 0:
    print('warm')
elif temperature == 0:
    print('zero')
print('cold')
```

- A. 是
- B. 否

**答案：B。代码片段 2 总是会输出 cold 作为最后一行输出，因为 print('cold') 没有缩进！它不与任何 if 语句关联。**

### 解决问题

现在是时候来解决“获胜队伍”（Winning Team）问题了。在本书中，我通常会先展示完整代码，然后进行讨论。但这次的解决方案比第 1 章中的那些要长，所以我决定在这种情况下先分三部分展示代码，然后再整体展示。

首先，我们需要读取输入。这需要六次 `input` 调用，因为我们有两个队伍，每个队伍有三条信息。我们还需要将每条输入转换为整数。代码如下：

