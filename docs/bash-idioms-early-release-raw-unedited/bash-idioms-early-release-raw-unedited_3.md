# 第 2 章。循环用语

## 早期发布读者的注意

使用 Early Release 电子书，你可以在这些标题的正式发布之前很久就利用这些技术——作者在写作时提供的原始未编辑内容。

这将是最终书籍的第 2 章。请注意 GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有评论，或者如果你注意到本章中缺少材料，请联系作者 bash-idioms@jpsdomain.org。

不仅仅是 C 风格的 `for` 循环——bash 包含其他语法和风格；有些对 Python 程序员来说更熟悉，但每种都有其用途。有一种没有明显参数的 `for` 循环，可用于脚本和函数内部。还有一种类似迭代器的 `for` 循环，具有显式值，值可以来自其他命令。

## 循环构造

循环构造在编程语言中很常见。自 C 语言发明以来，许多编程语言都采用了 C 风格的“for”循环。因为它将初始化代码、终止条件和迭代代码全部集中到一个地方，所以非常强大且可读。例如，在 C（或 Java，或 ...）中：

```
/* NOT bash */
for (i=0; i<10; i++) {
    printf("%d\n", i);
}
```

bash 在只有少数语法差异的情况下遵循了几乎相同的方法：

```
for ((i=0; i<10; i++)); do
    printf '%d\n' "$i"
done
```

注意，尤其是双圆括号的使用。与大括号不同，bash 使用“do”和“done”来包围循环的语句。如同 C/C+，使用“for”循环的一个惯用用法是空的“for”循环，给出有意的无限循环（你也会看到 +while true; do）：

```
for ((;;)); do
    printf 'forever'
done
```

但这并不是 bash 中唯一的“for”循环类型。这里是 shell 脚本中的一个常见习惯用法：

```
for value; do
    echo "$value"
    # do more stuff with $value...
done
```

这看起来好像缺少了什么，对吧？value 的值从哪里来？这在命令行上不会为你做任何事情，但如果你在 shell 脚本中使用这个 for 循环，那么 for 循环将遍历脚本的参数。也就是说，它将使用 $1 然后 $2，然后 $3，等等，作为 value 的值。

把这个“for”循环放在名为 myloop.sh 的文件中，然后你可以像这样运行它，看到三个参数（“-c”、“17”和“core”）被打印出来：

```
$ bash myloop.sh -c 17 core
-c
17
core
$
```

这种简化的 for 循环在函数定义中也很常见：

```
function listem {
  for arg; do
    echo "arg to func: '$arg'"
  done
  echo "Inside func: \$0 is still: '$0'"
}
```

在函数定义内部，参数 $1、$2 等是传递给函数而不是传递给包含 shell 脚本的参数。因此，在函数定义内部，for 循环将遍历传递给函数参数。

这个极简的“for”循环遍历一个隐含的值列表——传递给脚本或函数的参数。当在脚本主体中使用时，它遍历传递给脚本的参数；当在 shell 函数内部使用时，它遍历传递给该函数参数。

这绝对是 bash 习惯用法中晦涩的一种。你需要知道如何阅读它，但我们将在下面几节中讨论如何编写它。

我们可能想要一个同样简单的循环，但使用我们自己选择的显式值，而不是局限于参数。bash 正好有这个功能。

## 显式值

在 bash 中，for 循环可以被赋予要循环的值列表，像这样：

```
for num in 1 2 3 4 5; do
    echo "$num"
done
```

由于 bash 处理的是字符串，我们不必只使用数字：

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

## 引用

你可能对所有这些命令中的引号感到好奇。你应该的，bash 中的引用可能相当棘手。继续阅读，我们将在本章结束前介绍到。

for 循环的另一个值来源可以来自其他命令，要么是单个命令，要么是命令管道：

```
for arg in $( some cmd or other | sort -u)
```

这种类型的例子有：

```
for arg in $(cat /some/file)
for pic in $(find . -name '*.jpg')
for val in $(find . -type d | LC_ALL=C sort)
```

一个常见用法，尤其是在旧脚本中，是这样的：

```
for i in $(seq 1 10)
```

因为 seq 命令会生成一系列数字。这种情况可以被认为等同于：

```
for ((i = 1; i <= 10; i++))
```

后者的 for 更高效，也可能更易读。（注意，循环终止后，i 的值在这两种形式之间会不同（10 与 11），不过通常人们不会在循环外部使用该值。）

还有这种变体，但它有 bash 版本可移植性问题，因为大括号展开是在 v3.0 中引入的，而展开数值值的零填充是在 v4.0 中引入的：

```
for {01..10}
```

> 前导零
> 
> 当 {start..end..inc} 大括号展开中的前两个项之一以零开头时，它会强制每个输出值具有相同的宽度——在 bash v4.0 或更新版本中使用零在左侧填充它们。所以 {098..100} 将产生：098 099 100 而 {98..0100} 将填充到四个字符，结果为：0098 0099 0100。

这种大括号展开构造在你希望生成的数字成为更大字符串的一部分时特别有用。你只需将大括号构造作为字符串的一部分。例如，如果你想生成 5 个像 log01.txt 到 log05.txt 这样的文件名，你可以写：

```
for FN in log{01..5}.txt ; do
  # do something with the filenames here
  echo $FN
done
```


Arthur
Ann
Henry
John

看到这个例子，你可能会想当然地认为这个 bash “for” 循环就像 Python 那样，可以遍历迭代器对象返回的值。没错，在这个例子中 bash 确实在遍历一系列值，但这些值并不是来自迭代器对象。相反，这些名字在循环开始之前就全部写出来了。

构造：${namelist[@]} 是 bash 用于列出 bash 数组所有值的语法，此后称为列表，参见第 4 章中的名称讨论。 （在这个例子中，列表名为 namelist。）bash 在准备运行命令时会对它进行替换。因此，“for” 循环看不到列表语法；替换先发生。 “for” 循环得到的内容就好像我们显式输入了这些值一样：

```
for person in Arthur Ann Henry John
```

字典呢？Python 所称的“字典”，bash 称之为“关联数组”，其他人称之为“键/值对”或“哈希”（同样，参见第 4 章中的名称讨论）。构造：${hash[@]} 对于键/值对的值来说工作正常。要遍历哈希的键（即索引），请加上感叹号。构造 ${!hash[@]} 可以使用，如本代码片段所示：

```
#### we want a hash (i.e., key/value pairs)
declare -A hash
#### read in our data
while read key value; do
    hash[$key]="$value"
done
#### show us what we've got, though they won't
#### likely be in the same order as read
for key in "${!hash[@]}"; do
```

```
echo "key $key ==> value ${hash[$key]}"
done
```

alternate example:

```
#### we want a hash (i.e., key/value pairs)
declare -A hash
#### read in our data: word and #### of occurrences
while read word count; do
    let hash[$word]+="$count"
done
#### show us what we've got, though the order
#### is based on the hash, i.e. we don't control it
for key in "${!hash[@]}";do
    echo "word $key count = ${hash[$key]}"
done
```

本章更多是关于像 for 这样的循环结构，但如果你想了解更多关于列表和哈希的细节和示例，请参见第 4 章。

## Quotes and Blanks

关于这个“for”循环，还有一个重要的方面需要考虑。如果列表中的值包含空白（例如，如果每个条目都有名字和姓氏），那么我们的示例“for”循环：

```
for person in ${namelist[@]}; do
    echo $person
done
```

可能会产生这样的输出：

```
Art
Smith
Ann
Arundel
Hank
Till
```

John
Jakes

for 循环为我们列表中的四个名字打印出八个不同的值。为什么？怎么会这样？答案在于 bash 对 `${namelist[@]}` 的替换。它只是把那些名字放在变量表达式的位置。那就让列表中出现了八个词，像这样：

```
for person in Art Smith Ann Arundel Hank Till John Jakes
```

for 循环只是得到一个词列表。它不知道它们来自哪里。

bash 有语法可以解决这个难题：对列表表达式加上引号，每个值都会被引用。

```
for person in "${namelist[@]}"
```

会被翻译成：

```
for person in "Art Smith" "Ann Arundel" "Hank Till" "John Jakes"
```

这样就会得到期望的结果：

```
Art Smith
Ann Arundel
Hank Till
John Jakes
```

如果你的 for 循环要遍历文件名列表，那么你应该确保使用引号，因为文件名中可能包含空白。

这里还有一个最后的转折。列表语法可以使用“*”或“@”来列出列表的所有元素：`${namelist[*]}` 同样有效... 除了放在引号内时。表达式：

```
"${namelist[*]}"
```

会被评估为所有值都在一个字符串内。在这个例子中：

```
for person in "${namelist[*]}"; do
    echo $person
done
```

那将只产生一行输出，像这样：

```
Art Smith Ann Arundel Hank Till John Jakes
```

虽然在某些上下文中单个字符串可能有用，但在 for 循环中尤其毫无意义——只会有一次迭代。

## Style and Readability

在本章中你看到了 ${namelist[@]} 和 ${namelist[*]} 都列出了列表的所有值，但如果它们被引号包围，结果是不同的：多个字符串 vs. 一个大字符串。特殊 shell 变量 $@ 和 $* 也是如此。它们都代表脚本（即 $1、$2 等） 的所有参数列表。当被引号包围时，它们也会产生多个字符串或单个字符串。为什么现在要提到这个？只是为了回到我们最简单的 for 循环：

```
for param
```

并说这等价于：

```
for param in "$@"
```

有人可能会争辩说第二种形式更好，因为它更明确地显示了正在迭代的值。然而，我们并不信服。变量名本身以及引号的必要性都是专业知识，对普通读者来说并不比第一个简单的形式更明显。如果可读性是驱动因素，只需添加注释：

```
for param    # iterate over all the parameters
```

当遍历一系列整数值时，带双括号的 C 风格 for 循环可能是最可读的，也是最高效的。 （如果效率是主要问题，请确保在脚本早期使用 "declare -i i" 使你的变量 "i" 成为显式整数，避免字符串到/从字符串的转换。）

## Summary

在本章中，我们首先快速浏览了 C/C++ 风格的数值 for 循环。然后我们更深入。Bash 是一个非常以字符串为导向的语言，并且有其他值得了解的 for 循环风格。它的极简循环“for variable”提供了对脚本或函数参数的隐式（而且可能有些晦涩）迭代。提供给 for 循环的显式值列表，无论是字符串还是其他，都提供了遍历列表所有元素或哈希中所有键的完美机制。

既然你已经可以轻松获得所有这些值，你可能会用它们做什么？循环内部会发生什么，利用这些值？必须对遇到的值做出决定——而决策正是 bash 的另一个亮点——它的超强且超灵活的 case 语句，下章的主题。
