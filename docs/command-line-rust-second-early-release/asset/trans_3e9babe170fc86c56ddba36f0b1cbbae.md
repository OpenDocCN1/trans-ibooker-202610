
-   你了解到 `Iterator::all` 函数会在所有元素对给定谓词求值为 `true` 时返回 `true`，该谓词是一个接受元素的闭包。许多类似的 `Iterator` 方法接受闭包作为参数，用于测试、选择和转换元素。
-   你使用了 `str::split_whitespace` 和 `str::chars` 方法将文本拆分为单词和字符。
-   你使用了 `Iterator::count` 方法来计数项目的数量。
-   你编写了一个函数，根据标志参数有条件地格式化值或空字符串，以支持信息的打印或省略。
-   你将单元测试组织到 `tests` 模块中，并从名为 `super` 的父模块导入函数。
-   你看到了如何使用 `std::io::Cursor` 创建一个假文件句柄，用于测试期望实现 `BufRead` 的函数。
-   在大约 200 行 Rust 代码中，你编写了一个相当不错的替代品，替代了最广泛使用的 Unix 程序之一。

1 There are many who know how of old did men, In counsel gather; little good did they get; In secret they plotted, it was sore for them later, And for Gjuki’s sons, whose trust they deceived.
许多人知道古时人们如何，在商议中聚集；收获甚少；他们暗中密谋，后来为此痛苦，并让格尤基之子蒙受欺骗，辜负了他们的信任。

2 When my youngest first started brushing his own teeth before bed, I would ask if he’d brushed and flossed. The problem was that he was prone to fibbing, so it was hard to trust him. In an actual exchange one night, I asked “Did you brush and floss your teeth?” Yes, he replied. “Did you brush your teeth?” Yes, he replied. “Did you floss your teeth?” No, he replied. So clearly he failed to properly combine Boolean values because a `true` statement and a `false` statement should result in a `false` outcome.
当我最小的孩子第一次开始在睡前自己刷牙时，我会问他是否刷牙并使用了牙线。问题是他容易撒谎，所以很难信任他。某天晚上的一次实际交流中，我问“你刷牙并用牙线了吗？”他回答“是”。“你刷牙了吗？”他回答“是”。“你用牙线了吗？”他回答“没有”。所以很明显他未能正确组合布尔值，因为一个 `true` 语句和一个 `false` 语句的结果应为 `false`。

3 A more literal translation might be “Corner spider, rest easy, my soot-broom is idle.”
更直译可能是“角落的蜘蛛，放心吧，我的煤灰扫帚闲着呢。”

## 第 6 章。唯一性的巢穴

## 致抢先发布版读者的说明

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.
抢先发布电子书让你获得书籍的最早形态——作者在写作时的原始未编辑内容——这样你就可以在这些标题正式发布前很久就利用这些技术。

This will be the 6th Chapter of the final book. Please note that the GitHub repo will be made active later on.
这将是最终书籍的第 6 章。请注意 GitHub 仓库将在稍后激活。

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at kyle.clark@gmail.com.
如果你对如何改进本书的内容和/或示例有任何建议，或者在本章中发现缺失的材料，请联系作者 kyle.clark@gmail.com。

> *There’s only one everything*
> 
> —The Might Be Giants
> *只有一样东西*
> 
> —The Might Be Giants

The `uniq` program (pronounced *unique*) will find the distinct strings in lines of text either from a file or `STDIN`. Among its many uses, it is often employed to count how many times each unique string is found. I think you will find it challenging to create a program that mimics the output from the original tool. Here are some of the skills you will learn:
`uniq` 程序（发音为 *唯一*）会从文件或 `STDIN` 中查找文本行中的不同字符串。在其众多用途中，它经常被用来统计每个唯一字符串出现的次数。我认为你会发现创建模仿原始工具输出的程序具有挑战性。你将学习以下一些技能：

-   How to write to a file or `STDOUT` (pronounced *standard out*)
如何写入文件或 `STDOUT`（发音为 *标准输出*）
-   How and why to write a closure to capture a variable
如何以及为何编写闭包来捕获变量
-   About the don’t repeat yourself (DRY) concept
关于不要重复自己（DRY）概念
-   About the `Write` trait and the `write!` and `writeln!` macros
关于 `Write` trait 以及 `write!` 和 `writeln!` 宏
-   How and why to use temporary files
如何以及为何使用临时文件
-   How to indicate the lifetime of a variable
如何指示变量的生命周期
-   How to write tests that pass arguments on the command line or through STDIN
如何编写通过命令行参数或通过 STDIN 传递参数的测试

## uniq 的工作原理

Following is part of the manual page for the BSD version of uniq. The challenge program in this chapter will only implement the reading of a file or STDIN, writing to a file or STDOUT, and counting the lines for the -c flag:
以下是 BSD 版本 uniq 手册页的一部分。本章中的挑战程序将仅实现从文件或 STDIN 读取、写入文件或 STDOUT，以及为 -c 标志计数行：

```
UNIQ(1)                                      BSD General Commands Manual
UNIQ(1)

NAME
    uniq -- report or filter out repeated lines in a file

SYNOPSIS
    uniq [-c | -d | -u] [-i] [-f num] [-s chars]
    [input_file [output_file]]

DESCRIPTION
    The uniq utility reads the specified input_file
    comparing adjacent lines,
        and writes a copy of each unique input line to the
    output_file.  If
        input_file is a single dash ('-') or absent, the
    standard input is read.
        If output_file is absent, standard output is used for
    output.  The second
        and succeeding copies of identical adjacent input
    lines are not written.
        Repeated lines in the input will not be detected if
    they are not adja-
        cent, so it may be necessary to sort the files first.

    The following options are available:
```

-c      Precede each output line with the count of the number of times
the line occurred in the input, followed by a single space.
-c      在每一行输出前加上该行在输入中出现的次数，后跟一个空格。

-d      Only output lines that are repeated in the input.
-d      仅输出在输入中重复的行。

-f num  Ignore the first num fields in each input line when doing compar-
isons.  A field is a string of non-blank characters separated
from adjacent fields by blanks.  Field numbers are one based,
i.e., the first field is field one.
-f num  在比较时忽略每行输入中的前 num 个字段。字段是由非空白字符组成的字符串，由空白与相邻字段分隔。字段编号从 1 开始，即第一个字段是字段一。

-s chars
        Ignore the first chars characters in each input line when doing
comparisons.  If specified in conjunction with the -f option, the
first chars characters after the first num fields will be
ignored.  Character numbers are one based, i.e., the first char-
acter is character one.
-s chars
        在比较时忽略每行输入中的前 chars 个字符。如果与 -f 选项一起指定，则在前 num 个字段之后的前 chars 个字符将被忽略。字符编号从 1 开始，即第一个字符是字符一。

-u      Only output lines that are not repeated in the input.
-u      仅输出在输入中未重复的行。

-i      Case insensitive comparison of lines.
-i      不区分大小写地比较行。

In the *06_uniq* directory of the Git repository, you will find various input files I'll use for testing. To start, note that `uniq` will print nothing when given an empty file:
在 Git 仓库的 *06_uniq* 目录中，你会找到我用于测试的各种输入文件。首先请注意，给 `uniq` 一个空文件时它不会打印任何内容：

```
$ uniq tests/inputs/empty.txt
```

Given a file with just one line, the one line will be printed:
给定仅包含一行的文件，该行将被打印：

```
$ uniq tests/inputs/one.txt
a
```

It will also print the number of times a line occurs before the line when run with the -c option. The count is right-justified in a field 4 characters wide and is followed by a single space and then the line:
使用 -c 选项运行时，它还会在行前打印该行出现的次数。计数在宽 4 个字符的字段中右对齐，其后跟一个空格然后是该行：

```
$ uniq -c tests/inputs/one.txt
    1 a
```

Given input that contains two duplicate lines, only one line will be emitted:
如果输入包含两行重复的行，则只会输出一行：

```
$ cat tests/inputs/two.txt
a
a
$ uniq tests/inputs/two.txt
a
```

The line is shown to have a count of 2:
该行显示计数为 2：

```
$ uniq -c tests/inputs/two.txt
    2 a
```

A longer input file shows that uniq only considers the lines in order and not globally as the line one shows several times in this input file:
较长的输入文件显示 uniq 仅按顺序考虑行，而非全局，因为此输入文件中第一行出现了多次：

```
$ cat tests/inputs/three.txt
a
a
b
b
a
c
c
c
a
d
d
d
d
```

When counting, `uniq` starts over at 1 each time it sees a new string:
计数时，每当 `uniq` 看到一个新字符串时计数会从 1 重新开始：

```
$ uniq -c tests/inputs/three.txt
  2 a
  2 b
  1 a
  3 c
  1 a
  4 d
```

If you wanted *actually* unique values, you must first `sort` the input.
Note in the following command that `uniq` will read `STDIN` by default:
如果你想要 *真正* 唯一的值，你必须先 `sort` 输入。
请注意，在以下命令中 `uniq` 默认会读取 `STDIN`：

```
$ sort tests/inputs/three.txt | uniq -c
  4 a
  2 b
  3 c
  4 d
```

The file *tests/inputs/skip.txt* contains a blank line in the input acts just like any other value and so it will reset the counter:
文件 *tests/inputs/skip.txt* 包含的空白行在输入中就像任何其他值一样，因此会重置计数器：

```
$ uniq -c tests/inputs/skip.txt
  1 a
  1
  1 a
  1 b
```

If you study the usage closely, you’ll see a very subtle indication of how to write the output to a file. Notice how `input` and `output` in the following are grouped inside square brackets to indicate that they are optional as a pair. That is, if you provide `input`, then you may also optionally provide `output`:
如果你仔细研究用法，你会看到一个非常微妙的提示，说明如何将输出写入文件。请注意，下面的 `input` 和 `output` 如何被方括号分组，以表示它们作为一对是可选的。也就是说，如果你提供 `input`，那么你也可以选择性地提供 `output`：

```
usage: uniq [-c | -d | -u] [-i] [-f fields] [-s chars]
[input [output]]
```

For example, I can count *tests/inputs/two.txt* and place the output into *out*:
例如，我可以统计 *tests/inputs/two.txt* 并将输出放入 *out*：

```
$ uniq -c tests/inputs/two.txt out
$ cat out
    2 a
```

With no positional arguments, *uniq* will read from STDIN by default:
没有位置参数时，*uniq* 默认会从 STDIN 读取：

```
$ cat tests/inputs/two.txt | uniq -c
    2 a
```

If you want to read from STDIN *and* indicate the output filename, you must use a dash "-" for the input filename:
如果你想从 STDIN 读取 *并且* 指定输出文件名，你必须对输入文件名使用连字符 "-"：

```
$ cat tests/inputs/two.txt | uniq -c - out
$ cat out
    2 a
```

The GNU version works basically the same while also providing many more options:
GNU 版本基本相同，同时还提供更多选项：

```
$ uniq --help
Usage: uniq [OPTION]... [INPUT [OUTPUT]]
Filter adjacent matching lines from INPUT (or standard input),
writing to OUTPUT (or standard output).

With no options, matching lines are merged to the first occurrence.

Mandatory arguments to long options are mandatory for short options too.
    -c, --count            prefix lines by the number of
occurrences
    -d, --repeated         only print duplicate lines, one for
each group
    -D, --all-repeated[=METHOD]  print all duplicate lines
                            groups can be delimited with an
empty line
```
