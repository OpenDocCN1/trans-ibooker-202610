

guitars[-5]

'a'

使用 find 在字符串的索引范围内定位子字符串。如果 my_string 是一个字符串，则 find 可以有一个到三个参数。

```
my_string.find(substring, start_index=0, end_index=len(my_string))
```

此表示法意味着需要一个参数，即要查找的子字符串。如果没有给出其他参数，find 将从索引 0 开始查找。如果不给出第三个参数，则默认为字符串的长度。

```
guitars.find("o")
```

11

```
guitars.find("o", 12)
```

18

```
guitars.find("o", 2, 9)
```

-1

如果 Python 无法定位子字符串，find 返回 -1。

你也可以使用 index，但如果子字符串不存在则会引发异常。

## 4.4 创建字符串

我们已经看到许多将字面字符串放在单引号、双引号或三引号之间的示例。让我们看看构造字符串的额外方法。你总是创建新的字符串，并且不能修改原始字符串。

```
letters = "abcd"
letters + "efg"
```

'abcdefg'

```
letters
```

'abcd'

```
letters += "efg"
letters
```

'abcdefg'

“+” 运算符连接两个字符串。“Concatenate” 是一个花哨的词，意思是“把它们粘在一起，一个接一个”。你可以将新值赋给变量或使用运算符赋值，就像我在这里做的那样。

字符串连接非常强大，允许你将文本与其他字符串和其他数据类型混合。

```
brands = [
    "Fender",
    "Gibson",
    "Ibanez"
]
```

```
"I own " + str(len(brands)) + " brands of guitars"
```

```
'I own 3 brands of guitars'
```

### 4.4.1 f-string

通常使用 f-string 更简单、更清晰。

```
f"I own {len(brands)} brands of guitars"
```

```
'I own 3 brands of guitars'
```

有时你需要格式化数据，尤其是数字。你可以在大括号内包含格式细节。 [PYF]

```
f"This is 1/7 to 3 decimal places: {(1/7):.3f}"
```

```
'This is 1/7 to 3 decimal places: 0.143'
```

```
f"This is 1/7 to 6 decimal places: {(1/7):.6f}"
```

```
'This is 1/7 to 6 decimal places: 0.142857'
```

对于浮点科学计数法，请在大括号内使用 e 或 E 而不是 f。

```
f"{(1/7000):.5E}"
```

```
'1.42857E-04'
```

如果你要在表格中呈现数字，可能希望指定字段宽度并在左侧用空格或零填充。

```
for j in [1, 141, 1441, 14441]:
    print(f"| {j:5} | {j*j:010} |")
```

|     1 | 0000000001 |
|   141 | 0000019881 |
|  1441 | 0002076481 |
| 14441 | 0208542481 |

j:5 表示“将 j 的值右对齐在宽度为 5 个字符的字段中，并用空格填充。” j*j:010 表示“将 j*j 的值右对齐在宽度为 10 个字符的字段中，并用零填充。”

### 4.4.2 从字符串列表到字符串

如果您有一个字符串列表，可以使用 join 将它们与它们之间的文本连接起来。

```
words = ['If', 'you', 'have', 'a', 'list']

print("".join(words))
```

Ifyouhavealist

```
print(' '.join(words))
```

If you have a list

```
print('\n'.join(words))
```

If
you
have
a
list

我一直很喜欢 join 以及它是一个字符串的方法而不是列表的方法。

另一种创建字符串的方法是替换子字符串中的内容。

```
"My favorite guitar is a Fender.".replace("Fender", "Gibson")
```

'My favorite guitar is a Gibson.'

默认情况下，此方法替换所有出现的位置。您可以通过使用第三个参数来限制更改的数量。

```
"My favorite guitar is a Fender.".replace("e", "E")
```

'My favoritE guitar is a FEndEr.'

```
"My favorite guitar is a Fender.".replace("e", "E", 1)
```

'My favoritE guitar is a Fender.'

replace 的另一种用途是删除子字符串。只需用空字符串 "" 替换子字符串即可。

```
"My favorite guitar is a Fender.".replace(" guitar", "")
```

'My favorite is a Fender.'

![](img/4ffeafa3e3f1373f02971e43b0755ea6_162_0.png)

# 练习 4.1

使用 find 定位 " guitar"，然后切片并连接，重新做最后一个示例。

## 4.5 字符串和迭代

访问字符串中字符的另一种方法是使用 for 循环迭代它。

```
characters = []

for c in "Fender":
    characters.append(c)

characters
```

['F', 'e', 'n', 'd', 'e', 'r']

```
guitars
```

'Fender Gibson Taylor'

```
for c in guitars:
    if c.isupper():
        print(c)
```

F
G
T

虽然我们不能在 Python 中进行字符串推导，但我们可以创建一个字符串列表并将它们连接起来。

```
"".join([c for c
in "0X63 5WC - SL46 3AP - BN96 ØVU"
if c.isdigit()])
```

'635463960'

字符串可以通过 zip 与列表和其他支持迭代的集合并行遍历。

```
for a, b in zip("123", [1, 2, 3]):
    print(f"{a = }    {b = }")
```

a = '1'    b = 1
a = '2'    b = 2
a = '3'    b = 3

## 4.6 字符串和切片

我们在第 3.2.6 节中看到的列表切片规则也适用于字符串，只是你不能将字符串切片放在赋值语句的左侧。

```
word = "sesquipedalianism"
len(word)
```

17

```
print(word[3:9])
print(word[:7])
print(word[7:])
```

quiped
sesquip
edalianism

```
print(word[::-3])
print(word[::-1])
```

sqpaas
msinailadepiuqses

```
word[0] = 'S'
```

TypeError: 'str' object does not support item assignment

# 练习 4.2

查找“sesquipedalianism”的英文定义。

## 4.7 字符串测试

这个函数做什么？

```
def validate(name):
    if name:
        if name[0] != '_' and not name[0].isalpha():
            return False
        for character in name[1:]:
            if character != '_' and not character.isalnum():
                return False
        return True
    return False
```

空名称不好。

```
validate("")
```

False

名称中只有一个下划线是可以的。

```
validate("_")
```

True

名称可以是数字和字母。这些是“字母数字”。

```
validate("income2021")
```

True

## Stringing You Along

```
"##hashtaghashtag".startswith("##H")
```

False

![](img/4ffeafa3e3f1373f02971e43b0755ea6_166_0.png)

**练习 4.4**

编写一个使用切片实现 *startswith* 的函数。当然，不要使用 *startswith* 本身！

![](img/4ffeafa3e3f1373f02971e43b0755ea6_166_1.png)

**练习 4.5**

*endswith* 做什么？编写一个模仿其功能的函数。

## 4.8 分割和剥离

假设你有文本

```
text = "  SPECIAL  #  SALE  #  TODAY  "
```

你想将其拆分为包含三个单词的列表。首先，使用 *split* 在哈希标记处拆分字符串。

```
text.split('#')
```

```
['  SPECIAL  ', '  SALE  ', '  TODAY  ']
```

其次，使用 *strip* 从每个单词的左侧和右侧删除空白。在一行中：

```
[word.strip() for word in text.split('#')]
```

```
['SPECIAL', 'SALE', 'TODAY']
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_166_2.png)

**练习 4.6**

尝试使用 *replace* 重写 *strip*。对于嵌入空格的情况，例如 "How are you " 是否有效？

## 5

# 计算与计算

> 我们的伟大力量不在于数字，而在于统一……
> —托马斯·潘恩，《常识》

编程语言在对不同类型数字和数学结构的不同支持方面有所不同。看到浮点数和整数受硬件限制的大小并不罕见。对于科学和工程中的许多计算，我们需要比固定大小所能容纳的更大的数字。

在本章中，我介绍了许多可以在 Python 中进行计算的数字和其他对象形式。我们将研究整数、分数、浮点数、随机数、复数、量子位（量子位）和像多项式这样的符号对象。

# 本章涵盖的主题

+ 5.1 使用 Python 模块 ......................................................... 142
+ 5.2 整数 ......................................................... 145
+ 5.3 浮点数 ......................................................... 156
+ 5.4 有理数 ......................................................... 169
+ 5.5 复数 ......................................................... 172
+ 5.6 符号计算 ......................................................... 177

