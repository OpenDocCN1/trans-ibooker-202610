

遍历一个字符串的字符并不能帮助我们找到另一个字符串中对应的字符。但是如果我们能跟踪自己在字符串中的位置——我们在第一个停车位，我们在第二个停车位，等等——我们就可以查找每个字符串中对应的字符。我们到目前为止学到的 for 循环并不是这样做的方法。这样做的方法是使用索引和一种新的 for 循环。

## 索引

字符串中的每个字符都有一个*索引*，它表示其位置。第一个字符的索引是 0，第二个字符的索引是 1，以此类推。在自然语言中，我们通常从 1 开始计数。在英语中，没有人会说“*hello* 中位置 0 的字符是 *h*。”但大多数编程语言，包括 Python，都从 0 开始。

要使用索引，我们在字符串后面加上方括号中的索引。以下是一些索引的示例：

```
>>> word = 'splore'
>>> word[0]
's'
>>> word[3]
'o'
>>> word[5]
'e'
```

如果我们愿意，可以在索引中使用变量：

```
>>> where = 2
>>> word[where]
'l'
>>> word[where + 2]
'r'
```

对于一个非空字符串，我们可以使用到的最高索引是其长度减 1。（空字符串没有有效索引。）例如，'splore' 的长度为 6，因此索引 5 是其最高索引。更大就会报错：

```
>>> word[len(word)]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

```
>>> word[len(word) - 1]
'e'
```

我们如何访问字符串中从右数第二个字符？这样做就行：

```
>>> word[len(word) - 2]
'r'
```

但有更简单的方法。Python 支持负索引作为访问字符的另一种选择。索引 -1 是最右边的字符，索引 -2 是从右数第二个字符，以此类推：

```
>>> word[-2]
'r'
>>> word[-1]
'e'
>>> word[-5]
'p'
>>> word[-6]
's'
>>> word[-7]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

计划是使用索引来访问昨天和今天停车信息的对应位置。我们可以使用每个字符串的索引 0 来访问第一个停车位的信息，使用索引 1 来访问第二个停车位的信息，以此类推。但在执行该计划之前，我们需要学习一种新的 for 循环。

## CONCEPT CHECK

以下代码的输出是什么？

```
s = 'abcde'
t = s[0] + s[-5] + s[len(s) - 5]

print(t)
```

- A. aaa
- B. aae
- C. aee
- D. This code produces an error

Answer: A. 三个索引都指向 'abcde' 中的第一个字符。首先，s[0] 指向 'a'，因为 'a' 在字符串中的索引是 0。其次，s[-5] 指向 'a'，因为 'a' 是从右数第五个字符。第三，s[len(s) - 5] 指向 'a'，因为索引求值为 0：5（字符串的长度）减去 5。

## Range for loops

Python 的 range 函数生成整数范围，我们可以用这些范围来控制 for 循环。与遍历字符串的字符不同，range for 循环遍历整数。如果我们只给 range 提供一个参数，我们会得到从 0 到该参数减 1 的范围：

```
>>> for num in range(5):
...     print(num)
...
0
1
2
3
4
```

注意 5 没有输出。

如果我们给 range 提供两个参数，我们会得到从第一个参数到但不包括第二个参数的序列：

```
>>> for num in range(3, 7):
...     print(num)
...
3
4
5
6
```

我们可以通过包含第三个参数来使用不同的*步长*计数。默认步长为 1，即每次加一。让我们试试其他几个步长：

```
>>> for num in range(0, 10, 2):
...     print(num)
...
0
2
4
6
8
>>> for num in range(0, 10, 3):
...     print(num)
...
0
3
6
9
```

我们也可以倒着计数，但*不能*这样做：

```
>>> for num in range(6, 2):
...     print(num)
...
```

这不起作用，因为默认情况下 range 是向上计数。步长为 -1 让我们可以一次倒退一个：

```
>>> for num in range(6, 2, -1):
...     print(num)
...
6
5
4
3
```

要从 6 倒数到 0 包括 0，我们需要第二个参数的值为 -1：

```
>>> for num in range(6, -1, -1):
...     print(num)
...
6
5
4
3
2
1
0
```

有时快速查看 range 中的数字而不编写循环会很有帮助。不幸的是，range 函数并不会直接显示这些数字：

```
>>> range(3, 7)
range(3, 7)
```

我们可以将该结果传递给 list 函数来得到我们想要的：

```
>>> list(range(3, 7))
[3, 4, 5, 6]
```

当使用 range 调用时，list 函数会生成 range 的整数列表。我们稍后会学习所有关于列表的知识；现在，请记住 list 作为诊断 range 错误的辅助工具。

## CONCEPT CHECK

以下循环执行了多少次迭代？

```
for i in range(10, 20):
    # Some code here
```

- A. 9
- B. 10
- C. 11
- D. 20

Answer: B. range 会遍历数字 10、11、12、13、14、15、16、17、18 和 19。有 10 个数字，因此有 10 次迭代。

## Range for Loops Through Indices

假设我们有字符串给出昨天和今天的停车位信息：

```
>>> yesterday = 'CC.'
>>> today = '.C.'
```

给定一个索引，我们可以查看昨天和今天该索引的信息：

```
>>> yesterday[0]
'C'
>>> today[0]
'.'
```

我们可以使用 range for 循环遍历索引来处理每对对应的字符。我们知道昨天和今天的长度相同。但该长度可能是 1 到 100 之间的任何值，因此我们不能写类似 range(3) 的东西。我们希望使用索引 0、1、2，等等，一直迭代到字符串长度减 1。我们可以通过使用其中一个字符串的长度作为 range 的参数来实现：

```
>>> for index in range(len(yesterday)):
...     print(yesterday[index], today[index])
... 
C .
C C
..
```

我把循环变量叫做 index。其他流行名称包括 i（index 的第一个字母）和 ind。从现在开始我会使用 i。

不要把这个循环变量叫做 status 或 information。这些名称意味着它会取 'C' 和 '.' 值，而实际上它取的是整数。

## Solving the Problem

有了我们的 range for 循环，我们就可以解决 Occupied Spaces 问题了。我们的策略是从字符串开头到结尾遍历每个索引。我们可以检查昨天信息和今天信息在每个索引处的内容。使用嵌套的 if 语句，我们将确定停车位是否在两天都被占用。

Listing 3-3 给出了我们的解决方案。

```
n = int(input())
yesterday = input()
today = input()

① occupied = 0

② for i in range(len(yesterday)):
    ③ if yesterday[i] == 'C' and today[i] == 'C':
        ④ occupied = occupied + 1

print(occupied)
```

Listing 3-3: Solving Occupied Spaces

程序首先读取三行输入：n 指停车位的数量；yesterday 和 today 分别指昨天和今天的停车位信息。

注意我们不再引用停车位的数量（n）。我们本可以用它来告诉我们字符串的长度，但我选择忽略它，因为在现实场景中它通常没有提供。

我们使用 occupied 变量来计算昨天和今天都被占用的停车位数量。我们将该变量初始设为 0 ①。

现在我们进入 range for 循环，它遍历昨天和今天的有效索引 ②。对于每个这样的索引，我们检查停车位是否昨天被占用且今天被占用 ③。如果是，则我们通过将 occupied 增加 1 来将其计入总数 ④。

当 range for 循环终止时，我们将遍历完所有停车位。通过 occupied 变量可以访问到昨天和今天都被占用的停车位总数。剩下的就是输出该总数。

这就解决了这个问题。是时候把你的代码提交给评测了。

## Problem #7: Data Plan

我们了解到 for 循环对于处理从输入读取的数据很有用。它们在读取数据时也经常很有用

