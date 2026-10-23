

```python
apple_three = int(input())
apple_two = int(input())
apple_one = int(input())

banana_three = int(input())
banana_two = int(input())
banana_one = int(input())
```

其次，我们需要确定苹果队和香蕉队得分。 对于每个队伍，我们将三分、两分和一分得分相加。 我们可以按如下方式进行：

```python
apple_total = apple_three * 3 + apple_two * 2 + apple_one
banana_total = banana_three * 3 + banana_two * 2 + banana_one
```

第三，我们生成输出。 如果苹果队获胜，我们输出 A； 如果香蕉队获胜，我们输出 B； 否则，我们知道比赛平局，所以输出 T。 我们使用 if 语句来做到这一点，如下所示：

```python
if apple_total > banana_total:
    print('A')
elif banana_total > apple_total:
    print('B')
else:
    print('T')
```

这就是我们需要的全部代码。 请参阅第 2-1 节中的完整解决方案。

```python
apple_three = int(input())
apple_two = int(input())
apple_one = int(input())

banana_three = int(input())
banana_two = int(input())
banana_one = int(input())

apple_total = apple_three * 3 + apple_two * 2 + apple_one
banana_total = banana_three * 3 + banana_two * 2 + banana_one

if apple_total > banana_total:
    print('A')
elif banana_total > apple_total:
    print('B')
else:
    print('T')
```

### *Listing 2-1: Solving Winning Team*

如果你将我们的代码提交给评测，你应该会看到所有测试用例都通过了。

### Concept Check

以下版本的代码是否正确解决了问题？

```python
apple_three = int(input())
apple_two = int(input())
apple_one = int(input())

banana_three = int(input())
banana_two = int(input())
banana_one = int(input())

apple_total = apple_three * 3 + apple_two * 2 + apple_one
banana_total = banana_three * 3 + banana_two * 2 + banana_one

if apple_total < banana_total:
    print('B')
elif apple_total > banana_total:
    print('A')
else:
    print('T')
```

- A. Yes
- B. No

> Answer: A. The operators and order of the code are different, but the code is still correct. If the Apples lose, we output B (because the Bananas win); if the Apples win, we output A; otherwise, we know that the game is a tie, so we output T.

继续之前，你可能想尝试解决第 45 页“Chapter Exercises”中的练习 1。

## Problem #4: Telemarketers

有时我们需要编码比迄今为止所见更复杂的布尔表达式。 在本问题中，我们将学习有助于我们做到这一点的布尔运算符。

这是 DMOJ 问题 ccc18j1。

### The Challenge

在本问题中，我们假设电话号码是四位数。 如果电话号码的四位数字满足以下全部三个属性，则属于电话推销员：

- 第一个数字是 8 或 9。
- 第四个数字是 8 或 9。
- 第二个和第三个数字相同。

例如，电话号码为 8119 的四位数字属于电话推销员。

判断电话号码是否属于电话推销员，并指示我们是否应该接听电话或忽略它。

### Input

输入有四行。 这些行分别给出电话号码的第一、第二、第三和第四个数字。 每个数字都是 0 到 9 之间的整数。

### Output

如果电话号码属于电话推销员，输出 ignore； 否则，输出 answer。

### Boolean Operators

属于电话推销员的电话号码必须满足什么条件？ 它的第一个数字必须是 8 或 9。 而且，它的第四个数字必须是 8 或 9。 而且，第二个和第三个数字必须相同。 我们可以使用 Python 的 *Boolean operators* 来编码这种“or”和“and”逻辑。

### or Operator

or 运算符以两个布尔表达式作为其操作数。 如果至少一个操作数为 True，则返回 True，否则返回 False：

```
>>> True or True
True
>>> True or False
True
>>> False or True
True
>>> False or False
False
```

从 or 运算符得到 False 的唯一方法是如果其两个操作数都是 False。

我们可以使用 or 来判断一个数字是 8 还是 9：

```
>>> digit = 8
>>> digit == 8 or digit == 9
True
>>> digit = 3
>>> digit == 8 or digit == 9
False
```

请记住，在第 1 章的“Integer and Floating-Point Numbers”中，Python 使用运算符优先级来确定运算符应用的顺序。 or 的优先级低于关系运算符的优先级，这意味着我们通常不需要在操作数周围加上括号。 例如，在 digit == 8 or digit == 9 中，or 的两个操作数是 digit == 8 和 digit == 9。 这与我们写成 (digit == 8) or (digit == 9) 相同。

用英语说“如果数字是 8 或 9”是有意义的。 但在 Python 中这样写是行不通的：

```
>>> digit = 3
>>> if digit == 8 or 9:
...     print('yes!')
...
yes!
```

注意我（错误地！）将第二个操作数写成 9 而不是 digit == 9。 Python 通过输出 yes! 来响应，这显然不是我们想要的，因为 digit 引用的是 3。 原因是 Python 将非零数字视为 True。 由于 9 被视为 True，这使得整个 or 表达式为 True。 在将自然语言翻译成 Python 时，请仔细检查你的布尔表达式，以避免此类错误。

### and Operator

and 运算符如果其两个操作数都为 True，则返回 True，否则返回 False：

```
>>> True and True
True
>>> True and False
False
>>> False and True
False
>>> False and False
False
```

从 And 运算符得到 True 的唯一方法是如果其两个操作数都为 True。

and 的优先级高于 or。 这就是为什么这很重要：

```
>>> True or True and False
True
```

Python 按如下方式解释该表达式，先进行 and：

```
>>> True or (True and False)
True
```

结果为 True，因为 or 的第一个操作数为 True。  
我们可以通过包含括号来强制先进行 or：

```
>>> (True or True) and False
False
```

结果为 False，因为 and 的第二个操作数为 False。

### not Operator

另一个重要的布尔运算符是 not。 与 or 和 and 不同，not 仅接受一个操作数（而不是两个）。 如果其操作数为 True，not 返回 False，反之亦然：

```
>>> not True
False
>>> not False
True
```

not 的优先级高于 or 和 and。

### Concept Check

这里有一个表达式以及带括号的该表达式的版本。 哪一个求值为 True？

- A. not True and False
- B. (not True) and False
- C. not (True and False)
- D. None of the above

Answer: C. The expression (True and False) evaluates to False; the not therefore makes the full expression True.

### Concept Check

考虑表达式 not a or b。

以下哪一项使表达式为 False？

- A. a False, b False
- B. a False, b True
- C. a True, b False
- D. a True, b True
- E. More than one of the above

Answer: C. If a is True, then not a is False. Since b is False, too, both operands to or are False, so the whole expression evaluates to False.

### Solving the Problem

有了布尔运算符，我们就可以解决 Telemarketers 问题。 我们的解决方案在第 2-2 节中。

```
num1 = int(input())
num2 = int(input())
num3 = int(input())
num4 = int(input())

if ((num1 == 8 or num1 == 9) and
    (num4 == 8 or num4 == 9) and
    (num2 == num3)):
    print('ignore')
else:
    print('answer')
```

### Listing 2-2: Solving Telemarketers

与 Winning Team 一样，我们首先读取输入并将其转换为整数。

我们 if 语句的高层结构是三个由 and 运算符连接的表达式； 全部必须为 True，整个表达式才为 True。 我们要求第一个数字为 8 或 9，第四个数字为 8 或 9，并且第二个和第三个数字相等。 如果这三个条件都成立，则我们知道电话号码属于电话推销员，并输出 ignore。 否则，电话号码不属于电话推销员，并输出 answer。

我将布尔表达式拆分为三行。 这需要用额外的一对括号将整个表达式括起来，如我所做的那样。 （如果没有这些括号，你会得到一个语法错误，因为

