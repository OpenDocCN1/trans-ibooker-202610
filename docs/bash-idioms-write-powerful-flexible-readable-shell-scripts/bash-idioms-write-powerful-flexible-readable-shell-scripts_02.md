## Acknowledgments

## bash

感谢 GNU 软件基金会和 Brian Fox 编写了 bash。非常感谢 Chet Ramey，他从 20 世纪 90 年代初到中期大约从 1.14 版开始一直维护和改进 bash。你（复数）给了我们一个伟大的工具，在很多方面都帮助了我们。

## Reviewers

非常感谢我们的审阅者 Doug McIlroy、Ian Miell、Curtis Old 和 Paul Troncone！他们都提供了宝贵的反馈、建议，有时还提供了替代方案，指出我们忽略的问题，总的来说极大地改进了本书。本书中的任何错误或遗漏都是我们自己的，不是他们的。

## O’Reilly

感谢 O’Reilly 的整个团队，没有他们这本书就不复存在（出于很多原因）——如果有的话，内容也不会或看起来不会像现在这样好！

感谢 Mike Loukides 提出最初的想法，并请我们信任我们去实现它。感谢 Suzanne “Zan” McQuade 帮助完善它。非常感谢 Nicole Taché 在漫长的写作过程中进行编辑、 sanity 检查，并总体上容忍我们，以及 Kristen Brown 在制作过程中做同样的事情。特别感谢 Tools 部门的 Nick Adams 修复我们众多的（极其严重的）AsciiDoc 错误，并提供超出职责范围的工具链帮助。还要感谢 Kim Sandoval（校对）、Cheryl Lenser（索引）、Liz Wheeler（校对）、David Futato（内页设计）、Karen Montgomery（封面设计）以及 O’Reilly 的其他优秀团队。

## From Carl

感谢 JP 在此书上的所有工作，他的彻底性、细节关注以及愿意容忍我。感谢所有 O’Reilly 的人帮助将其实现。

我在这本书上的工作献给我妻子 Cynthia，她在我写作时忍受我奇怪的工作时间，并令人信服地假装对我写的内容感兴趣。我在这本书上的工作如我们在 Bethel University 所说，是为了上帝的荣耀和邻舍的益处。

## From JP

感谢 Carl 的所有工作；我们似乎又一次安排了互补的日程。感谢 Mike 再次推动事情启动，以及 Nicole 保持其运转并对我们的工作、生活和时间管理问题保持耐心。

本书献给我妻子 Karen，她是负责所有事情的执行副总裁，没有她我的生活就无法运转。感谢你的难以置信的支持、耐心和理解。最后，感谢 Kate 和 Sam，感谢你们把“除非你流血或着火，否则我有书要做”当作答案。

## A Big “if” Idiom

为了让你开始理解 bash 习惯用法，我们将介绍一个 bash 构造，它允许你用更简洁的语法做你可能通常用 if/then/else 构造做的事情。我们将在本章中向你展示的习惯用法有一些真正的优势——主要是简洁性——也有一些需要避免的陷阱。但如果你不了解 bash 习惯用法，你可能无法识别或意识到正在发生的事情。

看看这段代码：

```
[[ -n "$DIR" ]] && cd "$DIR"
```

你觉得这看起来像一个 if 语句吗？如果你熟悉 bash，你会认识到它在功能上是相同的；你会把它理解为一个 if 语句，即使代码中没有出现 if 关键字。

发生了什么？

## The Big “if”

为了解释这个习惯用法，让我们先看一个类似但更简单的例子：

```
cd tmp && rm scratchfile
```

这也是，实际上，是一个 if 语句。如果 cd 命令成功，则（并且仅在成功时）执行 rm 命令。这里的“习惯用法”是使用双与号（&&），通常读作“和”，来分隔两个命令。

逻辑或哲学课程教授规则：“A AND B” 的表达式当且仅当 A 和 B 都为真时为真。因此如果 A 为假，就没有必要考虑 B 的值。例如，考虑“我有一只狗 AND 我有一只猫”。如果我没有狗，那么对我来说这个复合表达式为假，无论我的猫情况如何。

让我们将其应用于 bash。记住 bash 的基本功能是执行程序。语句的第一部分执行 cd 命令。类似于 AND 的逻辑，如果第一个命令失败，bash 将不会费心执行第二部分，即 rm 命令。

使用 && 的目的是提醒你 AND 行为。Bash 并不是真正对这两个结果执行“AND”操作。（如果这是 C/C++，那就是另一回事了，尽管条件执行是相同的。）然而，这个 bash 习惯用法确实提供了第二个命令的条件执行——如果第一个命令失败，它就不会运行。

让我们回到并查看我们给出的原始示例，即这个表达式：

```
[[ -n "$DIR" ]] && cd "$DIR"
```

你现在觉得你“明白”了吗？第一个表达式测试名为 DIR 的变量的值的长度是否非零。如果它有值，也就是说如果其长度非零——并且仅在非零时——cd 命令将尝试更改到由 DIR 值命名的目录。

我们本可以将其写成显式的 if 语句：

```
if [[ -n "$DIR" ]]; then
  cd "$DIR"
fi
```

对于不太熟悉 bash 的人来说，这种后一种格式肯定更易读、更容易理解。但 then 子句中几乎没有发生什么，只是 cd 命令，所以在语法上显得有点“臃肿”。你必须根据你的读者可能是谁以及 then 子句中是否可能添加其他命令来决定使用哪一种。我们将在后面的章节中就此话题提供更多意见。

![](img/553ffa1bc15093729b691f706befc0ac_17_0.png)

## bash Help

bash help 命令对于任何内置命令都很好，但 help test 提供了关于 test 表达式的特别有帮助的线索，比如 -n。你也可以查看 man bash，但如果你这样做，你需要搜索“conditional expressions”。bash 手册页非常长；help thing 有更短、更集中的主题。如果你不确定所讨论的命令是否是 bash 内置命令，只需用 help 命令尝试一下，或使用 type -a thing 来查找。

承认一下，知道 help test 会告诉你 -n 的含义是棘手的，但然后，你足够聪明买了这本书，所以现在你知道了。这里还有一个微妙的小技巧；去试试这个：help [。
你受宠若惊。

## Or ELSE...

使用 || 字符分隔 bash 列表中的两个项目也有一个类似的习惯用法。读作“或”，第二部分仅在第一部分失败时执行。这是为了提醒你“OR”的逻辑规则，即：A OR B。整个表达式为真，如果 A 为真或 B 为真。换句话说，如果 A 为真，B 为真或假并不重要。例如，考虑短语“我有一只狗 OR 我有一只猫”。如果我确实有一只狗，那么对我来说这个表达式为真，无论我的猫情况如何。

将其应用于 bash：

```
[[ -z "$DIR" ]] || cd "$DIR"
```

你觉得你能解释这个吗？如果变量长度为零，则第一部分为“真”，因此没有必要执行后半部分；不会运行 cd 命令。但如果 $DIR 的长度非零，测试将返回“假”，只有在这种情况下我们才会运行 cd 命令。

你可以把这行 bash 读作“要么 $DIR 为零长度，要么我们尝试 cd 到该目录。”

将其写成显式的 if 语句有点奇怪，因为没有 then 操作要执行。|| 后的代码就像 else 子句：

```
if [[ -z "$DIR" ]]; then
  :
else
  cd "$DIR"
fi
```

“：” 是 shell 中的空语句——在这种情况下什么也不做。

总之：用 && 分隔的两个命令就像 if 及其 then 子句；用 || 分隔的两个命令就像 if 及其 else 子句。

## More than One

你可能想在 || 之后的这个类似 else 的子句中做不止一件事（或在 && 之后的类似 then 的子句中），而这就存在危险。可能会诱惑你写这样的东西：

```
### Caution: not what you might think!
cd /tmp || echo "cd to /tmp failed." ;  exit
```

“或”连接告诉我们，如果 cd 失败，我们将执行 echo 命令，告诉用户 cd 失败。但这里有个陷阱：exit 将无论如何都会发生。不是你期望的，对吧？





把分号看作等同于换行符，整个逻辑就会变得清晰得多（并且更明显地不是你想要的）：

```
cd /tmp || echo "cd to /tmp failed."
exit
```

我们如何得到想要的行为？我们可以将 echo 和 exit 组合在一起，使它们作为一个整体成为“或”右侧的子句，像这样：

```
### 成功执行 cd 或输出消息后退出
cd /tmp || { echo "cd to /tmp failed." ; exit ; }
```

大括号是 bash 中复合命令的语法，即将语句分组在一起。你可能见过使用圆括号的类似写法，但使用圆括号会在子 shell 中执行语句，也称为子进程。这会带来我们不需要的开销，而且在子 shell 内部执行的 exit 也不会起到多大作用。

![](img/553ffa1bc15093729b691f706befc0ac_19_0.png)

#### 关闭复合命令

bash 的一个怪癖要求复合命令的关闭语法必须特定。它必须在闭合大括号前以分号或换行符结束。如果你使用分号，则需要在大括号前留有空白，以便大括号能被识别为保留字（否则它会被误认为 shell 变量语法中的闭合大括号，例如 ${VAR}）。这就是前面的示例以看似多余的分号结尾的原因：{ echo "..." ; exit ; }。使用换行符时，最后的分号就不需要了：

```
### 成功执行 cd 或输出消息后退出
cd /tmp || { echo "cd to /tmp failed." ; exit
}
```

但这样可能读起来不够清晰。在左侧边缘，它显得位置很奇怪；加上缩进后逻辑分组更合理，但看起来却很单薄。

我们建议你坚持使用额外的分号，并且不要忘记它与闭合大括号之间的空格。

#### 再来一次，多于一个

如果需要更复杂的逻辑怎么办？如果有多重 AND 和 OR 构造又如何？它们是如何处理的？你认为下面这行代码会做什么？

```
[ -n "$DIR" ] && [ -d "$DIR" ] && cd "$DIR" || exit 4
```

如果 DIR 变量不为空并且 DIR 变量指定的文件是一个目录，那么它会 cd 到该目录；否则脚本会退出并返回 4。

这做出了你可能期望的结果——但也许不是你想的那种原因。你可能会从这个例子中推断出 && 的优先级高于 || 运算符，但事实并非如此。它们只是从左到右进行分组。bash 的语法规定 && 和 || 运算符具有相同的优先级并且是左结合的。需要说服力吗？看这些例子：

```
$ echo 1 && echo 2 || echo 3
1
2
$
```

但也有：

```
$ echo 1 || echo 2 && echo 3
1
3
$
```

注意它总是评估最左边的运算符，无论是 AND 还是 OR；决定求值顺序的不是运算符优先级，而是简单的左结合性。

#### 不要这样做

既然我们谈到了 if 语句（或如何不使用它们），这里有一个你可能会在旧脚本中经常看到的显式 if 示例。我们在这里展示它是为了解释这种惯用法，但也强烈建议你永远不要模仿这种风格。这是代码：

```
##### 不要这样写 if 语句来检查
if [ $VAR"X" = X ]; then
    echo empty
fi
```

```
##### 或者这样
if [ "x$VAR" == x ]; then
    echo empty
fi
```

```
##### 或者这种主题的其他变体
```

不要这样做。这段代码在做什么？它们在检查变量 VAR 是否为空。它们通过给 VAR 的值追加一个字符（这里是 X）来实现。如果 resulting 字符串仅匹配该字母本身，则变量为空。不要这样做。

有更好的方法来进行此检查。这里有一个简单的替代方案：

```
### 变量的长度是否为零？即为空或空值
if [[ -z "$VAR" ]]; then
    echo empty
fi
```

![](img/553ffa1bc15093729b691f706befc0ac_21_0.png)

#### 单括号与双括号

这个不该做的例子使用了单括号 [ 和 ] 来包围它们正在测试的条件。这不是我们要你避免的。我们要你避免的是字符串追加和比较；相反，使用 -z 或 -n 来进行这些测试。那么为什么我们的示例都使用双括号 [[ 和 ]] 来编写我们的 if（和非 if）语句呢？它们是 bash 的新增功能（不在原始 sh 命令中），并且避免了单括号所表现出的一些令人困惑的边缘情况行为（变量是否在引号内）。我们用单括号展示这个例子是因为这种类型的比较在旧脚本中经常出现。你可能需要坚持使用单括号，如果你的目标是在各种平台和/或非 bash 平台（例如 dash）之间保持可移植性。作为旁注，双括号是关键字，而左单括号是内置命令，这种差异可能解释了一些细微的行为差异。我们的建议是除非不可避免，否则使用双括号语法。

你可以通过使用 -n 选项或直接引用变量来检查相反的情况，即检查字符串长度是否非零：

```
### 这检查长度是否非零，即不为空，不为空值
if [[ -n "$VAR" ]]; then
    echo "VAR has a value:" $VAR
fi
```

```
### 同样在这里
if [[ "$VAR" ]]; then
    echo even easier this way
fi
```

所以你可以看到没有必要使用那种在旧版 test 命令（"["）中必要的方法，而这种命令现在很少使用。我们认为你应该看到它，这样你就能在旧脚本中识别它。现在你也知道更好的写法了。

#### 风格和可读性：回顾

在本章中，我们研究了一种特定的 bash 惯用法——“无 if” if 语句。它看起来不像传统的 if/then/else，但行为完全可以一样。除非你能识别它，否则你阅读的一些脚本可能会仍然晦涩。这种惯用法也值得用来检查在执行命令之前是否满足必要的先决条件，或在不打断脚本主要逻辑流程的情况下进行简短的错误检查。

通过使用 && 和 || 运算符，你可以编写 if/then/else 逻辑，而无需使用那些熟悉的关键字。但 bash 确实有 if、then 和 else 作为关键字。那么什么时候使用它们，什么时候使用简写？答案取决于可读性。

对于复杂逻辑，使用熟悉的关键字更有意义。但对于带单个操作的简单测试和检查情况，使用 && 和 || 运算符非常方便，并且不会分散主要逻辑流程的注意力。使用 help test 来提醒你可以使用哪些测试，例如 -n -r，并考虑将帮助文本复制到注释中以供将来参考。

无论是熟悉的 if 语句还是惯用的“无 if”语句，我们都鼓励使用双括号语法。

现在你已经深入了解了一种 bash 惯用法，让我们看看其他惯用法，并真正提升你的 bash 水平。

#### 循环术语

不只是 C 风格的 for 循环——bash 包含其他语法和风格；有些对 Python 程序员更熟悉，但每种都有其用途。有一种 for 循环没有明显的参数，可用于脚本和函数内部。还有一种类似迭代器的 for 循环，具有显式值和可以来自其他命令的值。

#### 循环结构

循环结构在编程语言中很常见。自 C 语言发明以来，许多编程语言都采用了 C 风格的 for 循环。因为它将初始化代码、终止条件和迭代代码都集中在一个地方，所以它是一个功能强大且可读的构造。例如，在 C（或 Java，或...）中：

```
/* NOT bash */
for (i=0; i<10; i++) {
    printf("%d\n", i);
}
```

bash 在只有少量语法差异的情况下遵循了几乎相同的方法：

```
for ((i=0; i<10; i++)); do
    printf '%d\n' "$i"
done
```

请特别注意双圆括号的使用。与大括号不同，bash 使用 do 和 done 来包含循环的语句。与 C/C++ 一样，for 循环的惯用用法是空的 for 循环，产生有意的无限循环（你也会看到 while true; do）：



```
for ((;;)); do
    printf 'forever'
done
```

但这并不是 bash 中唯一的 for 循环形式。下面是 shell 脚本中常见的一种习惯用法：

```
for value; do
    echo "$value"
    # Do more stuff with $value...
done
```

这看起来好像少了点什么，对吧？value 的值从哪里来？在命令行中这样使用不会对你产生任何作用，但如果在 shell 脚本中使用，那么 for 循环就会遍历脚本的参数。也就是说，它会依次使用 $1，然后是 $2，然后是 $3，等等，作为 value 的值。

把这个 for 循环放在一个名为 myloop.sh 的文件中，然后你可以这样运行它，看看会打印出三个参数（“-c”、“17”和“core”）：

```
$ bash myloop.sh -c 17 core
-c
17
core
$
```

这种简写的 for 循环在函数定义中也很常见：

```
function Listen {
    for arg; do
        echo "arg to func: '$arg'"
    done
    echo "Inside func: \$0 is still: '$0'"
}
```

在函数定义内部，参数 $1、$2 等是传递给该函数的参数，而不是传递给外部 shell 脚本的参数。因此，在函数定义内部，for 循环会遍历传递给该函数的参数。

这种极简的 for 循环遍历的是一个隐含的值列表——传递给脚本或函数的参数。当在脚本主体中使用时，它遍历传递给脚本的参数；当在 shell 函数内部使用时，它遍历传递给该函数 的参数。

这绝对是 bash 中晦涩的习惯用法之一。你需要知道如何阅读它，但我们会在后面的章节中讨论如何编写它（剧透：正如 Python 所说，显式优于隐式）。

我们可能想要一个同样简单的循环，但使用我们自己选择的显式值，而不是局限于参数——bash 正好有这个功能。

#### Explicit Values

在 bash 中，for 循环可以被赋予一个要遍历的值列表，像这样：

```
for num in 1 2 3 4 5; do
    echo "$num"
done
```

由于 bash 处理的是字符串，我们并不局限于数字：

```
for person in Sue Neil Pat Harry; do
    echo $person
done
```

当然，值列表可以同时包含变量和字面量：

```
for person in $ME $3 Pat ${RA[2]} Sue; do
    echo $person
done
```

for 循环的另一个值来源可以来自其他命令，要么是单个命令，要么是命令管道：

```
for arg in $(some cmd or other | sort -u)
```

这种类型的例子有：

```
for arg in $(cat /some/file)
for arg in $(</some/file)    # Faster than shelling out to cat
for pic in $(find . -name '*.jpg')
for val in $(find . -type d | LC_ALL=C sort)
```

一个常见的用法，尤其是在旧脚本中，是这样的：

```
for i in $(seq 1 10)
```

因为 seq 命令会生成一系列数字。这种情况可以被认为等价于：

```
for ((i = 1; i <= 10; i++))
```

后者的 for 更高效，也更易读。（请注意，循环结束后，i 的值在这两种形式之间会不同 [10 与 11]，不过通常我们不会在循环外部使用该值。）

还有这种变体，但它有 bash 版本的可移植性问题，因为大括号展开是在 v3.0 中引入的，而展开的数值值的零填充则是在 v4.0 中引入的：

```
for i in {01..10}; do echo "$i"; done
```

![](img/553ffa1bc15093729b691f706befc0ac_27_0.png)

#### Leading Zeros

当 {start..end..inc} 大括号展开中的前两个项之一以零开头时，它会强制每个输出值具有相同的宽度——在 bash v4.0 或更新版本中使用零在左侧填充它们。因此 {098..100} 会得到：098 099 100，而 {98..0100} 会填充到四个字符，结果为：0098 0099 0100。

这种大括号展开构造在你希望生成的数字成为更大字符串的一部分时尤其有用。你只需将大括号构造作为字符串的一部分。例如，如果你想生成五个类似 log01.txt 到 log05.txt 的文件名，你可以写：

```
for filename in log{01..5}.txt ; do
  # Do something with the filenames here
  echo $filename
done
```

![](img/553ffa1bc15093729b691f706befc0ac_27_1.png)

#### Braces Versus printf -v

你也可以用一个数值 for 循环然后用 printf -v 来从数字构造文件名，但大括号展开似乎更简单。当你除了文件名之外还需要数值时，使用数值 for 循环和 printf。

seq 命令对于生成浮点样式的数字序列仍然非常有用。你在起始值和结束值之间指定一个增量：

```
for value in $(seq 2.1 0.3 3.2); do
  echo $value
done
```

会产生：

+   2.1
+   2.4
+   2.7
+   3.0

只要记住 bash 不做浮点运算。你可能希望生成这些值以便在脚本内部传递给其他程序。

#### Similar to Python

下面是在 bash 的 for 循环中看到的另一个常见短语：

```
for person in ${name_list[@]}; do
  echo $person
done
```

它可能会产生如下输出：

```
Arthur
Ann
Henry
John
```

看到这个例子，你可能会想，这个 bash for 循环就像 Python，可以遍历迭代器对象返回的值。好吧，在这个例子中 bash 确实在遍历一系列值，但这些值并不是来自迭代器对象。相反，名称在循环开始前就全部列出。

构造 ${name_list[@]} 是 bash 用于列出 bash 数组所有值的语法，此后称为列表。 （参见第 7 章引言中的术语讨论。在此示例中，列表称为 name_list。）bash 在准备运行命令时进行替换。因此 for 循环看不到列表语法；替换先发生。for 循环得到的内容就好像我们显式输入了值：

```
for person in Arthur Ann Henry John
```

字典呢？Python 所称的“字典”，bash 称之为“关联数组”，其他人称之为“键/值对”或“哈希”（再次参见第 7 章引言）。构造 ${hash[@]} 对于键/值对的值来说是可以的。要遍历哈希的键（即索引），请添加一个感叹号。构造 ${!hash[@]} 可以使用，如以下代码片段所示：

```
### We want a hash (i.e., key/value pairs)
declare -A hash
### Read in our data
while read key value; do
  hash[$key]="$value"
done
### Show us what we've got, though they won't
### likely be in the same order as read
for key in "${!hash[@]}"; do
  echo "key $key === value ${hash[$key]}"
done
```

这里有一个替代示例：

```
### We want a hash (i.e., key/value pairs)
declare -A hash
### Read in our data: word and # of occurrences
while read word count; do
  let hash[$word]+="$count"
done
### Show us what we’ve got, though the order
### is based on the hash, i.e., we don’t control it
for key in "${!hash[@]}";do
  echo "word $key count = ${hash[$key]}"
done
```

本章更多是关于 for 等循环结构，但如果你想要关于列表和哈希的更多细节和示例，请参见第 7 章。

#### Quotes and Spaces

关于这个 for 循环，还有一个重要的方面需要考虑。你注意到我们在前面示例中引号使用不一致吗？如果列表中的值包含空格（例如，如果每个条目都有名字和姓氏），那么我们的示例 for 循环：

```
for person in ${namelist[@]}; do
  echo $person
done
```

可能会产生如下输出：

```
Art
Smith
Ann
Arundel
Hank
Till
John
Jakes
```

for 循环为我们列表中的四个名字打印出八个不同的值。为什么？怎么会这样？答案在于 bash 对 ${namelist[@]} 的替换。它只是把那些名字放在变量表达式的位置。那就留下八个词的列表，像这样：

```
for person in Art Smith Ann Arundel Hank Till John Jakes
```

for 循环只是得到一个词列表。它不知道它们来自哪里。

bash 有语法可以解决这个难题：给列表表达式加上引号，每个值都会被引用。

```
for person in "${namelist[@]}"
```



will be translated to:

```
for person in "Art Smith" "Ann Arundel" "Hank Till" "John Jakes"
```

and that will yield the desired result:

Art Smith
Ann Arundel
Hank Till
John Jakes

If your for loop is going to iterate over a list of filenames, then you should be sure to use the quotation marks, since filenames might have a space in them.

There is one last twist to all this. The list syntax can use either * or @ to list all the elements of the list: ${namelist[*]} works just as well...except when put inside quotes. The expression:

```
"${namelist[*]}"
```

will be evaluated with all of the values inside of a single string. In this example:

```
for person in "${namelist[*]}"; do
  echo $person
done
```

would result in a single line of output, like this:

Art Smith Ann Arundel Hank Till John Jakes

Though a single string might be useful in some contexts, it is especially pointless in a for loop—there would be only one iteration. We recommend using @ unless you are positive that you need *.

See also “Quoting” on page 129.

#### Developing and Testing for Loops

It turns out that “for list do something” loops are extremely useful in all kinds of ways. Let’s take two simple examples: running SSH commands on a list of servers and renaming files, like for file in *.JPEG; do mv -v $file ${file/JPEG/jpg}; done. But how do you develop and test a script or even a simple for command? The same way you develop anything else: start simple and go one step at a time. But in particular, you use echo (see Example 2-1). Note that the bash builtin echo has a number of interesting options, but is not POSIX (see “POSIX Output” on page 56). The most interesting and often used are -e (to enable interpretation of backslash escapes) and -n (to suppress the automatic trailing newline).

#### Example 2-1. File rename—test version

```
##### Building and testing a rename command, note the echo
for file in *.JPEG; do echo mv -v $file ${file/JPEG/jpg}; done
```

```
##### Simple multi-node SSH, note the 1st echo (can do on 1 line, broken for book)
for node in web-server{00..09}; do
echo ssh $node 'echo -e "$HOSTNAME\t$(date "%F") $(uptime)"';
done
```

Once it’s working as expected, remove that leading echo and go. Of course, if you are using redirection in the block, you have to be careful about that, perhaps changing | to .p., > to .gt., and so on until you get each stage working.

![](img/553ffa1bc15093729b691f706befc0ac_31_0.png)

#### Execute the Same Command Across Multiple Hosts

This is way out of scope for this book, but if you need to run the same command on many hosts, you should probably be using Ansible, Chef, Puppet, or a similar tool. Sometimes you have a really quick and dirty need, and one of these tools might be useful:

- **clusterssh**
  Written in Perl, it opens a bunch of unmanaged terminals in windows.
- **mssh (MultiSSH)**
  GTK+-based multi-SSH client in a single GUI window.
- **mussh**
  MUltihost SSH Wrapper shell script.
- **pconsole**
  Intended for tiling window managers, spawns a terminal per host.
- **multixterm**
  Written in Expect & Tk, drives multiple xterms.
- **PAC Manager**
  A Perl SecureCRT-like GUI on Linux.

#### while and until

We mentioned while in passing previously, and it works as you’d expect—“execute block while criteria exit status is zero”:

```
while <CRITERIA>; do <BLOCK>; done
```

It’s often used in reading files; see several examples in Chapter 9. For argument parsing, see “Parsing Options” on page 75.

Unlike other languages, in bash until is just ! while, or “execute block while criteria exit status is not zero”:

```
until <CRITERIA>; do <BLOCK>; done
##### Same:
! while <CRITERIA>; do <BLOCK>; done
```

This is really handy for something like waiting for a node to be created or rebooted (Example 2-2).

##### Example 2-2. Wait for reboot

```
until ssh user@10.10.10.10; do sleep 3; done
```

#### Style and Readability: Recap

In this chapter, we first took a quick look at the C/C++ style numerical for loop. Then we went further. Bash is very string oriented and has some other styles of for loops worth knowing. Its minimalist loop for variable provides implicit (and arguably obscure) iteration over the arguments to a script or a function. An explicit list of values, string or otherwise, provided to the for loop also gives us the perfect mechanism for iterating over all the elements of a list or over all keys in a hash.

We now know that both ${namelist[@]} and ${namelist[*]} show all the values of the list, but if they are enclosed in double quotes, the result is different: separate strings versus one large string. The same is true for the special shell variables $@ and $*. They both represent the list of all the arguments to the script (i.e., $1, $2, etc.). When enclosed in double quotes, though, they also result in either multiple strings or a single string. Why bring that up now? Only to circle back to our simplest for loop:

```
for param
```

and say that this is equivalent to:

```
for param in "$@"
```

We argue that the second form is better because it shows more explicitly which values are being iterated over. However, there is a counterargument that the $@ variable name itself and the necessity of the quotes are both specialized knowledge that is no more obvious to the naive reader than just the first, simple form. If you really prefer the first form, simply add a comment:

```
for param    # Iterate over all the script arguments
```

When looping over a sequence of integer values, the C-style for loop with double parentheses is probably the most readable as well as the most efficient. (If efficiency is a big concern, be sure to use declare -i i early in your script to make your variable "i" an explicit integer, avoiding conversion to/from a string.)

Knowing that you have all these values readily available, what might you do with them? What goes on inside the loop, making use of these values? Decisions must be made about the values encountered, and decision-making brings us to another important feature in bash: its supercharged and superflexible case statement, the topic of the next chapter.

for commands are extremely useful but can be tricky to develop. Start simple and use echo until you’re sure your command is working as intended. And remember the “syntactic sugar” while and until commands for readability where useful.

#### Make Your Case

Many programming languages provide a “switch” or “match” statement, an n-way branch, used as an alternative to a string of if/then/else clauses. There is a similar construct in bash: the case statement. It comes with powerful pattern matching and is very useful in scripting.

The keywords case and in delineate the value you want to compare against various patterns. Here’s a simple example:

```
case "$var" in
  yes ) echo "glad you agreed" ;;
  no )
    echo "sorry; good bye"
    exit
  ;;
  * ) echo "invalid answer. try again" ;;
esac
```

...which you can probably figure out. It checks to see if the value in $var is “yes” or “no” and executes the corresponding statement(s). It even has a default action. The end of the case statement is marked by esac, which is case spelled backward. This example is pretty readable, but it just scratches the surface. You’ll also note that you used two different block styles, a “one-liner” for yes and a more typical block (closed by ;;...more on that later) for no. Which you use depends on what you are doing and how the code lines up for readability.

![](img/553ffa1bc15093729b691f706befc0ac_35_0.png)

#### ( in case

The syntax for the case statement includes an optional “(” to match the “)” in the example. For example, we could have written ("yes") instead of just "yes") and similarly for the other items. We’ve rarely seen this used, though. After all, who wants to type an extra character?

The real power of the case statement, and the most idiomatic appearance, comes from using the shell’s pattern matching for the various possible value comparisons:

```
case "$var" in
  [Nn][Oo]* )
    echo "Fine. Leave then."
    exit
  ;;
  [Yy]?? | [Ss]ure | [Oo][Kk]* )
    echo "OK. Glad we agree."
  ;;
  * ) echo 'Try again.'
    continue
  ;;
esac
```

Here is a quick review of bash pattern matching, which you are probably familiar with as command line wildcards (or globs). There are three special characters to watch for: ? matches a single character, * matches any number of characters (including none), and brackets, [], match any of the characters included between the brackets.

In our example, the construct [Yy] matches either uppercase Y or lowercase y. The construct [Nn][Oo]* matches either upper- or lowercase N, followed by either upper- or lowercase O, followed by any number of any other characters. The pattern matches the following words (and others, too): no, No, nO, NO, noway, Not Ever, and nope. It will not be a match if the value of $var is the word never.

Can you guess some possible values for the affirmative case? The vertical bar separates different patterns that would all lead to the same result. (Think “OR” but not the || or.) The words Yes, yes, YES, yEs, yES, yup, Sure, sure, OK, ok, and OKfine and “OK why not” would all work. But these words would not: ya, SURE, oook, and many more.

The default case isn’t special syntax—it’s just a pattern—but this pattern would match anything. If no other previous pattern has produced a match, then we know that this one will—it matches any number of any characters. Therefore, bash script writers put this one last in the list if they want to catch a default case.





![](img/553ffa1bc15093729b691f706befc0ac_36_0.png)

#### 非正则表达式

case 语句中使用的模式匹配不是正则表达式。在 bash 中，唯一允许使用正则表达式（regex 或 regexp）的地方是在 if 语句中使用 =~ 比较运算符。如果确实需要使用正则表达式，那么你需要使用一系列 if/then/else 语句而不是 case。

#### 一个现实的使用案例

解析命令行选项的代码是常见的使用 case 语句的地方。让我们来看一个简单但相当现实的脚本，它很好地利用了 case 语句。

#### 动机

如果你曾经使用过 Linux 或 Unix，你很可能大量使用了 ls 命令来列出文件名和相关信息。ls 的一些选项在提供更多信息或以特定方式排序时非常方便。你可能会养成使用 ls 或最常使用的选项的某些习惯。因此，你可能会创建一些别名甚至整个脚本来更方便地使用你喜欢的组合。但随后你最终会拥有多个不同的脚本。它们在功能上都相关但各不相同。我们如何将它们全部合并到一个脚本中？

考虑一个熟悉的例子，名为 Git 的流行源代码控制软件。它有几个相关但不同的功能，所有功能都使用同一个命令名 git 调用，但每个功能都由第二个不同的关键字区分，例如 git clone、git add、git commit、git push 等等。

#### 我们的脚本

我们可以将这种“子命令”方法应用于我们的情况。让我们考虑一些我们想要使用的 ls 相关功能：按文件名长度顺序列出文件（连同长度），仅列出最长的文件名，列出最近修改的最后几个文件，以及使用颜色编码按文件类型列出文件名——这是 ls 的标准功能，但需要一些难以记住的选项。

我们的脚本将命名为 list，但会有第二个词来指定其中一个功能。那个词可以是 color、last、length 或 long。示例 3-1 包含一个执行此操作的脚本。

#### 示例 3-1. 使用 case 的简单包装脚本

```
#!/usr/bin/env bash
##### list.sh: A wrapper script for ls-related tools & simple `case..esac` demo
##### Original Author & date: _bash Idioms_ 2022
##### bash Idioms filename: examples/ch03/list.sh
##### ----------------------------------------------------------------------------
VERSION='v1.2b'

function Usage_Exit {
    echo "$0 [color|last|len|long]"
    exit
}

##### Show each filename preceded by the length of its name, sorted by filename
##### length. Note '-' is valid but uncommon in function names, but it is not
##### valid in variable names. We don't usually use it, but you can.
function Ls-Length {
    ls -1 "$@" | while read fn; do
        printf '%3d %s\n' ${#fn} ${fn}
    done | sort -n
}

(( $# < 1 )) && Usage_Exit
sub=$1
shift

case $sub in
    color)
        ls -N --color=tty -T 0 "$@"    # Colorized ls
    ;;
    last | latest)
        ls -lrt | tail "-n${1:-5}"     # Latest files
    ;;
    len*)
        Ls-Length "$@"                 # Files with name lengths
    ;;
    long)
        Ls-Length "$@" | tail -1       # File with longest name
    ;;
    *)
        echo "unknown command: $sub"    # Default
        Usage_Exit
    ;;
esac
```

我们不会在这里解释这个脚本的所有部分，尽管到本书结束时，你将学到所使用的所有功能。我们主要想关注 case 语句：

+   1. 你能认出非 if 的 if 逻辑吗？如果不能，请（重新）阅读第 1 章。
+   2. 这是两个词之间的简单“或”选择。
+   3. 如果 $1 中未给出值，则使用 tail -n5 作为默认值；请参阅第 38 页的“默认值”。
+   4. 此模式将匹配以“len”开头的任何词，因此“len”或“length”都会匹配，但“lenny”和“lens”也会匹配。它随后调用 Ls-Length 函数（我们在脚本中定义），并将传递给此脚本的所有命令行参数（如果有）传递给它。

#### 包装脚本

每个人都有很多事情要做，很多需要记住的事情，所以当你可以自动化或编写脚本为你记住细节时，这就是胜利。我们在示例 3-1 中展示了一种“包装脚本”的方法，但根据你解决的问题的复杂性或你“记住”的细节，你可以使用许多有趣的变化和技巧。在示例 3-1 中，我们调用了一个函数或只是内联放置代码。对于非常短的代码块来说，这效果最好，在我们的经验中，这些在这种类型的包装脚本中非常常见。如果你有更复杂的解决方案，或者你正在使用现有工具，你可以调用那些工具或子脚本，不过你需要调整错误检查和可能的用法选项。你还可以将它与第 93 页的“Drop-in Directories”结合使用，并从目录中 source 所有“模块”，也许可以将代码部分的维护委托给其他人或团队。

这个更大的例子实际上是我们编写本书时使用的脚本的简化版和摘录。AsciiDoc 很酷，但我们使用很多标记语言，它们都混在一起了，所以我们可以编写一个工具来为我们记住事情，如示例 3-2 所示。

#### 示例 3-2. 使用 case 的复杂包装脚本

```
#!/usr/bin/env bash
##### wrapper.sh: Simple "wrapper" script demo
##### Original Author & date: _bash Idioms_ 2022
##### bash Idioms filename: examples/ch03/wrapper.sh
#_____________________________________________________________________________
##### Trivial Sanity Checks
```

```
[ -n "$BOOK_ASC" ] || {
    echo "FATAL: export \$BOOK_ASC to the location of the Asciidoc files!"
    exit 1
}
\cd "$BOOK_ASC" || {
    echo "FATAL: can't cd to '$BOOK_ASC'!"
    exit 2
}

SELF="$0"
action="$1"
shift
[ -x /usr/bin/xsel -a $# -lt 1 ] && {
    # Read/write the clipboard on Linux
    text=$(xsel -b)
    function Output {
        echo -en "$*" | xsel -bi
    }
} || {
    # Read/write STDIN/STDOUT
    text=$*
    function Output {
        echo -en "$*"
    }
}

case "$action" in

########################################
##### Content/Markup

###### Headers
 h1 )
        # Inside chapter heading 1 (really AsciiDoc h3)
        Output "[[${$SELF id $text}]]\n=== $text"
    ;;
 h2 )
        # Inside chapter heading 2 (really AsciiDoc h4)
        Output "[[${$SELF id $text}]]\n==== $text"
    ;;
 h3 )
        # Inside chapter heading 3 (really AsciiDoc h5)
        Output "[[${$SELF id $text}]]\n===== $text"
    ;;

###### Lists
 bul|bullet )
        # Bullet list (** = level 2, + = multiline element)
        Output "* $text"
    ;;
 nul|number|order* )
        # Numbered/ordered list (.. = level 2, + = multiline)
        Output ". $text"
    ;;
 term )
        # Terms
        Output "term_here::\n  $text"

```

```
;;

###### Inline
bold )
    # Inline bold (O'Reilly prefers italics to bold)
    Output "*${text}*"
;;
i|italic|itl )      # Inline italics (O'Reilly prefers italics to bold)
    Output "_${text}_"
;;
c|constant|cons )   # Inline constant width (command, code, keywords, more)
    Output "+${text}+"
;;
type|constantbold ) # Inline bold constant width (user types literally)
    Output "*+${text}+*"
;;
var|constantitalic ) # Inline italic constant width (user-supplied values)
    Output "_+${text}+_"
;;
sub|subscript )     # Inline subscript
    Output "~${text}~"
;;
sup|superscript )   # Inline superscript
    Output "^${text}^"
;;
foot )              # Create a footnote
    Output "footnote:[${text}]"
;;
url|link )          # Create a URL with alternate text
    Output "link:\${\$text\$\$[]"   # URL[link shows as]
;;
esc|escape )        # Escape a character (esp. *)
    Output "\$\$${text}\$\$"  # $*$ $
;;

#####################################################
# Tools

id )                ## Convert a hack/recipe name to an ID
    #us_text=${text// /_}  # Space to '_'
    #lc_text=${us_text,,}  # Lowercase; bash 4+ only!
    # Initial `tr -s ' ' ' ' to preserve _ in case we process an ID
    # twice (like from "xref")
    # Also note you can break long lines with a trailing |
    Output $(echo $text | tr -s '_' ' ' | tr '[:upper:]' '[:lower:]' \
    | tr -d '[:punct:]' | tr -s ' ' '_')
;;

index )             ## Creates 'index.txt' in AsciiDoc dir
    # Like:
        # ch02.asciidoc== The Text Utils
        # ch02.asciidoc=== Common Text Utils and similar tools
```





# ch02.asciidoc:=== Finding data
egrep '^===' ch*.asciidoc | egrep -v '^ch00.asciidoc' \
> $BOOK_ASC/index.txt && {
    echo "Updated: $BOOK_ASC/index.txt"
    exit 0
} || {
    echo "FAILED to update: $BOOK_ASC/index.txt"
    exit 1
}
;;

rerun )            ## Run examples to re-create (existing!) output files
    # Only re-run for code that ALREADY HAS a *.out file...not ALL *.sh code
    for output in examples/*/*.out; do
        code=${output/out/sh}
        echo "Re-running code for: $code > $output"
        $code > $output
    done
;;

cleanup )           ## Clean up all the xHTML/XML/PDF cruft
    rm -fv {ch??,app?}.{pdf,xml,html} book.{xml,html} docbook-xsl.css
;;

* )
    \cd -  # UGLY cheat to revert the 'cd' above...
    ( echo "Usage:"
    egrep '\()[[:space:]]+#+ '  $0
    echo ''
    egrep '\()[[:space:]]+## ' $0
    echo ''
    egrep '\()[[:space:]]+### ' $0 ) | grep "$S{1:-.}" | more
;;
esac

我们在这段代码中有很多内容，所以让我们逐一拆解：

+   1. 实际的脚本对本书的 AsciiDoc 源代码执行了大量操作，因此最好确保我们处于正确的位置，并且有一个方便的环境变量设置。

+   2. 我们通常使用 $PROGRAM 来保存 bash 基名，但在这种情况下我们会递归调用此脚本很多次，所以 $SELF 似乎更直观。

+   3. 正如我们在第 11 章中会更详细讨论的那样，使用有意义的变量名而不是位置参数是一个好主意，所以让我们这样做。

4 一旦我们捕获了操作，就不再需要旧的 $1 了，但可能还有更多选项，所以将 $1 移开。

5 如果 /usr/bin/xsel 存在且可执行，并且如果没有更多选项，我们就知道我们正在从 X Window 剪贴板读取和写入，否则我们从参数获取文本并将输出发送到 STDOUT。实际上，我们从编辑器复制，切换到命令行，运行工具，切换回来，然后粘贴。

6 这里是我们开始真正做某事的地方——也就是说，弄清楚我们的“操作”是什么。

7 为了代码组织和可读性，将操作拆分为各个部分；另见 7。

8 让我们从标题的标记开始。

9 这行既是代码也是文档。$action 是我们想要一个顶级（对于本书代码）标题，h1。我们稍后会看到它如何也是文档。

10 执行工作。首先，调用我们自己为文本获取一个 AsciiDoc “ID”，然后输出该 ID 用双方括号括起来，后跟换行符，然后将文本缩进并用 === 表示标题级别，最后调用 Output 函数。希望剩下的代码很容易理解。

11 为了代码组织和可读性，将操作拆分为各个部分；另见 7。

12 你可以用尾随的 \; 来换行；另见“布局”第 130 页。

13 在兜底部分变得有趣起来，它将帮助或用法与未知参数处理以及出色的“帮助中搜索”功能结合在一起。

14 我们将拥有的任何输出包装到一个我们通过管道传输到 more 的子 shell 中，以防它很长。

15 这里是我们谈到的“代码即文档”行，见注释 9。我们 grep 查找 case 语句的闭合括号 )，后跟空格，后跟单个注释标记 #。这给了我们第 1 级“内容/标记”操作。这会提取使 case 语句工作的实际代码行，但由于我们添加了注释，它也向你展示了它做了什么。

16 这对第 2 级“工具”部分做同样的事情。

17 这将对处理 Git 操作的第 3 级做同样的事情，但我们在这里为了简单起见省略了该代码。但它也使用 grep 和 ${1:-.} 来显示我们对 wrapper.sh help heading 之类的帮助，或全部内容（grep "."）。对于这么短的脚本，这可能看起来不算什么大问题，但随着时间的推移它会增长（而且会增长），这就变得非常方便了！

grep 命令和之前提到的“级别”的结果是显示一个帮助消息，该消息已排序但分为“第 1 级”和“第 2 级”部分：

```
$ examples/ch03/wrapper.sh help
Usage:
    h1 )                # Inside chapter heading 1 (really AsciiDoc h3)
    h2 )                # Inside chapter heading 2 (really AsciiDoc h4)
    h3 )                # Inside chapter heading 3 (really AsciiDoc h5)
    bul|bullet )        # Bullet list (** = level 2, + = multiline element)
    nul|number|order* ) # Numbered/ordered list (.. = level 2, + = multiline)
    term )              # Terms
    bold )              # Inline bold (ORA prefers italics to bold)
    i|italic*|itl )     # Inline italics (ORA prefers italics to bold)
    c|constant|cons )   # Inline constant width (command, code, keywords, more)
    type|constantbold ) # Inline bold constant width (user types literally)
    var|constantitalic )# Inline italic constant width (user-supplied values)
    sub|subscript )     # Inline subscript
    sup|superscript )   # Inline superscript
    foot )              # Create a footnote
    url|link )          # Create a URL with alternate text
    esc|escape )        # Escape a character (esp. *)
    id )                ## Convert a hack/recipe name to an ID
    index )             ## Creates 'index.txt' in AsciiDoc dir
    rerun )             ## Run examples to re-create (existing!) output files
    cleanup )           ## Clean up all the xHTML/XML/PDF cruft
```

```
$ examples/ch03/wrapper.sh help heading
    h1 )                # Inside chapter heading 1 (really AsciiDoc h3)
    h2 )                # Inside chapter heading 2 (really AsciiDoc h4)
    h3 )                # Inside chapter heading 3 (really AsciiDoc h5)
```

## 再加一个转折

在与每个模式关联的每段代码的末尾，我们都以双分号结束。在本章开头的第一个示例中，我们写道：

```
"yes") echo "glad you agreed" ;;
```

在 echo 命令之后，我们放置了 ;;，这表示不应再执行任何操作。执行将在 esac 关键字之后继续。

但有时你不希望这种行为。在某些情况下，你可能希望 case 语句中的其他模式被检查，或执行其他操作。bash 的语法允许这样做，使用 ;;& 和 ;& 来表示这些变体。

以下是提供有关 $filename 中路径的详细信息的这种行为的示例：

```
case $filename in
  ./*) echo -n "local "          # Begins with ./
      ;&                           # Fall through!
  [^/]*) echo -n "relative "      # Starts w/ anything but a slash
      ;;&                          # Look for other matches
  /*) echo -n "absolute "         # Begins with a slash
      ;&                           # Fall through
  */*) echo "pathname"            # A slash anywhere
      ;;                           # Done
  *) echo "filename"              # All other
      ;;                           # Done
esac
```

模式将按顺序与 $filename 中的值进行比较。第一个模式是两个字面字符——句点和斜杠——后跟任意字符。如果匹配（例如，如果 $filename 的值为 ./this/file），则脚本将打印“local”但末尾没有换行。下一行是 ;&，它告诉 bash “贯穿”并执行下一个模式相关联的命令（甚至不检查匹配）。因此它也会打印“relative”。与前一个模式不同，此代码段以 ;;& 结束，它告诉 bash 尝试匹配其他模式（按顺序向前）。

所以现在它将检查下一个模式，寻找前导斜杠。如果不匹配，下一个可能匹配。它在字符串中寻找任何位置的斜杠（任意——零个或多个字符，然后是斜杠，然后是任意字符）。如果匹配（在我们的示例中会匹配），它将打印单词 pathname。;; 表示不需要再检查其他模式，操作完成。

#### 风格和可读性：回顾

在本章中，我们描述了 case 语句，即执行流程中的 n 路分支。它的模式匹配功能使其在脚本中非常有用，尽管常见的用法是对特定单词进行简单的字面匹配。

变体 ;;、;;& 和 ;& 提供了一些有用的功能，但可能很棘手。最好使用 if/then/else 而不是 case 语句来构建此类逻辑。

这些符号如此细微地不同，以至于很容易忽略每一步发生的事情。匹配后控制流可能因每个 case 而异：执行更多代码以贯穿、尝试匹配另一个模式，或完成。因此我们强烈建议你在脚本中对你的选择进行注释，以避免混淆或误解。




