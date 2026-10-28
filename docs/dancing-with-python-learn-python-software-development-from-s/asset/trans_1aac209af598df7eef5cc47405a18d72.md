


```
print(content)
```

```
["Shall I compare thee to a summer's day?\n", 'Thou art more lovely and more temperate.\n', 'Rough winds do shake the darling buds of May,\n', "And summer's lease hath all too short a date.\n", 'Sometime too hot the eye of heaven shines,\n', 'And often is his gold complexion dimmed;\n', 'And every fair from fair sometime declines,\n', "By chance, or nature's changing course, untrimmed;\n", 'But thy eternal summer shall not fade,\n', "Nor lose possession of that fair thou ow'st,\n", "Nor shall death brag thou wand'rest in his shade,\n", "When in eternal lines to Time thou grow'st.\n", 'So long as men can breathe, or eyes can see,\n', 'So long lives this, and this gives life to thee.']
```

readline 函数一次一行地读取文本文件。成功从文件中读取的每一行的长度至少为一个，因为该行有尾随换行符，或者是文件中的最后一行非空行。如果该行为空，我们就到达了文件末尾。

下面的代码逐行读取文件，然后打印该行的全大写形式，并在前面加上其行号。

```
input_file = open("src/examples/sonnet-18.txt", "r")

line_number = 0
line = input_file.readline()

while line:
    line_number += 1
    print(f"{line_number:>2} {line.upper()}", end="")
    line = input_file.readline()  # Don't forget this!

input_file.close()
```

```
1  SHALL I COMPARE THEE TO A SUMMER'S DAY?
2  THOU ART MORE LOVELY AND MORE TEMPERATE.
3  ROUGH WINDS DO SHAKE THE DARLING BUDS OF MAY,
4  AND SUMMER'S LEASE HATH ALL TOO SHORT A DATE.
5  SOMETIME TOO HOT THE EYE OF HEAVEN SHINES,
6  AND OFTEN IS HIS GOLD COMPLEXION DIMMED;
7  AND EVERY FAIR FROM FAIR SOMETIME DECLINES,
8  BY CHANCE, OR NATURE'S CHANGING COURSE, UNTRIMMED;
9  BUT THY ETERNAL SUMMER SHALL NOT FADE,
10 NOR LOSE POSSESSION OF THAT FAIR THOU OW'ST,
11 NOR SHALL DEATH BRAG THOU WAND'REST IN HIS SHADE,
12 WHEN IN ETERNAL LINES TO TIME THOU GROW'ST.
13 SO LONG AS MEN CAN BREATHE, OR EYES CAN SEE,
14 SO LONG LIVES THIS, AND THIS GIVES LIFE TO THEE.
```

### Exercise 8.12

解释在 print 的调用中 {line_number:>2} 的作用。如果我省略 end=""，打印行为会如何改变？

### Exercise 8.13

你知道可以使用 import 读取 Python 模块并使其内容可供使用。你也可以读取代码文本文件并调用 exec 运行其字符串结果。研究 exec 函数并将其与 import 进行比较。有任何安全问题吗？

write 对字符串和 writelines 对字符串列表的行为与其读取对应函数互补。两个函数都不会自动向字符串追加换行符 '\n'。记住，如果你想写入文件的末尾，请使用 "a" 打开文件。

### Exercise 8.14

为什么我们不需要 writeline 函数？

### Exercise 8.15

编写一个函数 my_file_copy，带有两个文件路径参数：source_file 和 destination_file。在检查 source_file 存在后，使用 read 和 write 将其内容复制到 destination_file。务必检查异常。

除了我们在 6.5 节看到的 sep 和 end 可选参数外，print 函数还有另一个参数叫做 file。你可以用它代替 write 用于文本文件。它会自动追加换行符，除非你用 end 更改它。

```
output_file = open("src/examples/print-test.txt", "w")

print("First line", file=output_file)
print("Second line", file=output_file)

output_file.close()
```

我们也使用 read 读取二进制文件。如果我们为 read 包含一个正整数参数，它就是从二进制文件读取的字节数。对于文本文件，它就是要读取的字符数。记住，一个 Unicode 字符可能使用多于一个字节！

```
input_file = open("src/examples/sonnet-18.txt", "rb")
content = input_file.read(39)
input_file.close()

content

b"Shall I compare thee to a summer's day?"
```

```
type(content)

bytes
```

```
content[0]

83
```

```
bin(content[0])  # binary

'0b1010011'
```

```
hex(content[0])  # hexadecimal

'0x53'
```

给定一个 Unicode 字符串，我们可以将其编码为二进制字符串。然后我们可以使用二进制字符串并将其写入二进制文件。

```
s = "Sîne klâwen æðelen XM ᚺᚱ ᚫᚱ ᚲᚾ ᚹ"
```

```
t = s.encode("utf-8")
t

b'S\xc3\xaene kl\xc3\xa2wen \xc3\xa6\xc3\xb0elen XM \xe1\x9a\xba\xe1\x9a\xb1 \xe1\x9a\xab\xe1\x9a\xb1 \xe1\x9a\xb2\xe1\x9a\xbe \xe1\x9a\xb9'
```

要从字节字符串反向到字符串，请使用 decode。

```
t.decode("utf-8")

'Sîne klâwen æðelen XM ᚺᚱ ᚫᚱ ᚲᚾ ᚹ'
```

Sîne klâwen æðelen
XM ᚺᚱ ᚫᚱ ᚲᚾ ᚹ

Figure 8.5: Example of Unicode text

一个字符串及其编码的字节字符串不是相等对象。

```
s == t

False
```

总结以上内容，从文件中获取信息和向文件中放入信息的两个主要函数是 read 和 write。对于文本文件，我们还有额外的 readline、readlines 和 writelines 函数。

Python 提供了打开和关闭文件的简化语法。

```
with open("src/examples/sonnet-18.txt", "r") as input_file:
    print(input_file.readline())

Shall I compare thee to a summer's day?
```

```
"Ibanez RG": 1987
}
```

现在我们导入 pickle 模块，打开一个二进制文件用于写入，并转储字典。

```
import pickle

with open("work/pickle_example", "wb") as pickle_file:
    pickle.dump(guitars, pickle_file)
```

记住 with/as 会自动关闭 pickle_file。

此时，我们可以继续我们的业务。几个月后，我们想恢复吉他数据。我们打开二进制文件用于读取并加载字典。

```
with open("work/pickle_example", "rb") as pickle_file:
    new_guitars = pickle.load(pickle_file)
```

我们加载的数据看起来正确：

```
print(new_guitars)

{'Fender Stratocaster': 1954, 'Fender Telecaster': 1950, 'Gibson Les Paul': 1952, 'Gibson Flying V': 1958, 'Ibanez RG': 1987}
```

对于此示例，我仍然有 guitars，我们看到旧字典与新字典相同。

```
guitars == new_guitars

True
```

Pickling 对基本类型有效。这里我们对字典、字符串和整数进行了 pickling。

如果有更复杂类型或我们自己构建的类的实例呢？如果实例变量是可 pickling 的，我们不能在有类变量的情况下使用 pickle。Python 可以自动转储和加载类实例。因此类实例是可 pickling 的。

### Exercise 8.17

证明你可以使用 Fraction 制作的分数进行 pickle。

让我们用上一章的多项式来展示这一点。我们不需要编写任何新代码来使 **pickle** 工作。

```
t = UniPoly(1, 't', 1)
p = (t**3 - 7)**5
p

t**15 - 35*t**12 + 490*t**9 - 3430*t**6 + 12005*t**3 - 16807
```

现在我们 *dump* 和 *load* 多项式。

```
with open("work/pickle_example", "wb") as pickle_file:
    pickle.dump(p, pickle_file)

with open("work/pickle_example", "rb") as pickle_file:
    new_p = pickle.load(pickle_file)
```

它们相等：

```
p == new_p

True
```

**dill** 模块扩展了 **pickle** 并处理类变量。[DILL] 与 **pickle** 一样，**dill** 是 Python 专用的。它们创建的文件可能与你自己的 Python 版本显著不同而无法使用。

## 8.8.2 JSON

JSON 是作为 JavaScript 编程语言的一部分创建的标准化的基于文本的交换格式。“JSON” 代表“JavaScript Object Notation”。它与 JavaScript 没有特殊联系，Python 对该格式有良好的支持。[JSON]

JSON 与 Python 数据类型匹配得很好，Python 有四个主要函数用于处理它。

**src/examples/example.json** 文件包含我当前 Visual Studio Code 设置文件的扩展子集。JSON 文件应具有 “.json” 扩展名。让我们使用 *load* 读取示例文件，看看 Python 如何处理它。

```
import json

with open("src/examples/example.json", 'r') as input_file:
    json_ = json.load(input_file)
```

