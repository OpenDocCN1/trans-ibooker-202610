## 第 5 章. 介绍 ex 编辑器

如果这是一本关于 `vi` 和 `Vim` 的书，为什么我们要包含另一编辑器的章节呢？好吧，`ex` 并不是另一编辑器。`vi` 是更通用、底层的行编辑器 `ex` 的可视模式。一些 `ex` 命令在你在 `vi` 中工作时可能对你有用，因为它们可以为你节省大量编辑时间。大多数这些命令可以在不离开 `vi` 的情况下使用：你可以将 `ex` 命令行视为第三种模式，与常规命令模式和插入模式并列。

我们在前几章中看到的各种 `vi` 移动和文本修改命令很不错，但如果只有这些，你还不如使用记事本之类的东西。`vi` 爱好者喜欢 `vi` 的原因在于 `ex`：`ex` *就是力量所在！*

### 注意
`Vim` 提供了底层的 `ex` 编辑器，相对于原始版本有许多增强。在 `vi` 为 `Vim` 的系统上，`ex` 通常也会以 `ex` 模式调用 `Vim`。

在本章和以下章节中 第一部分，我们对 `vi` 和 `Vim` 区别不大，因为这些章节中的所有内容都适用于两者。阅读时，请随意将“`vi`”理解为代表“`vi` 和 `Vim`。”

你已经知道如何将文件视为一系列编号的行。`ex` 为你提供了更具移动性和范围的编辑命令。有了 `ex`，你可以在文件之间轻松移动，并以多种方式从一个文件向另一个文件传输文本。你可以快速编辑大于单个屏幕的文本块。而且通过全局替换，你可以在文件中对给定模式进行替换。

本章介绍 `ex` 及其命令。你将学习如何：
* 使用行号在文件中移动
* 使用 `ex` 命令复制、移动和删除文本块
* 保存文件和文件的一部分
* 处理多个文件（读取文本或命令，在文件间切换）

# ex 命令

在 `vi` 或任何其他屏幕编辑器发明之前，人们通过打印终端而不是今天的位图屏幕、指向设备和终端仿真程序与计算机通信。行号是快速识别要处理的文件部分的一种方式，行编辑器由此发展而来编辑这些文件。程序员或其他计算机用户通常会在打印终端上打印出一行（或多行），给出仅更改该行的编辑命令，然后重新打印以检查编辑后的行。^1

我们现在不再在打印终端上编辑文件，但一些 `ex` 行编辑器命令仍然对建立在 `ex` 之上的更复杂的视觉编辑器的用户有用。虽然使用 `vi` 进行许多编辑更简单，但 `ex` 的行方向在你想要对文件多个部分进行大规模更改时具有优势。

### 注意
我们将在本章中看到的许多命令都有文件名参数。虽然可以这样做，但通常在文件名中包含空格是一个非常糟糕的主意。`ex` 会感到极度困惑，你会花上不值得的时间试图让文件名被接受。使用下划线、破折号或句点分隔文件名的各个部分，你会更开心。

在你开始简单地死记硬背 `ex` 命令（更糟的是忽略它们）之前，让我们先消除行编辑器的一些神秘感。看到 `ex` 在直接调用时如何工作，将有助于理解有时晦涩的命令语法。

打开一个你熟悉的文件并尝试几个 `ex` 命令。就像你可以对文件调用 `vi` 编辑器一样，你可以通过在 shell 提示符下运行 `ex` 命令来对文件调用 `ex` 行编辑器。如果你调用 `ex`，你会看到关于文件中总行数的消息，以及一个冒号命令提示。例如：

```
$ ex practice
"practice" 8L, 261B
Entering Ex mode.  Type "visual" to go to Normal mode.
:
```

除非你给出导致显示一行或多行的 `ex` 命令，否则你不会看到文件中的任何行。

`ex` 命令由行地址（可以简单地是行号）加上命令组成；它们以按 ENTER 键结束。例如，最基本的命令之一是 `p` 表示打印（到屏幕）。因此，例如，如果你在提示符下输入 `1p`，你会看到文件的第一行：

```
:1p
With a screen editor you can
:
```

事实上，你可以省略 `p`，因为单独的行号等同于对该行的打印命令。要打印多行，你可以指定行号范围（例如 `1,3`——用逗号分隔的两个数字，中间可以有或没有空格）。例如：

```
:1,3
With a screen editor you can
scroll the page, move the cursor,
delete lines, insert characters, and more,
```

没有行号的命令被假定会影响当前行。因此，例如，替换命令（`s`），它允许你用另一个词替换一个词，可以这样输入：

```
:1
With a screen editor you can
:s/screen/line/
With a line editor you can
```

注意，更改后的行在命令发出后会被重新打印。你也可以这样做同样的更改：

```
:1s/screen/line/
With a line editor you can
```



即使你将从 `vi` 中调用 `ex` 命令并且不会直接使用它们，花几分钟在 `ex` 本身中也是值得的。你会体会到需要如何告诉编辑器要操作哪一行（或多行），以及要执行哪个命令。

在你的 `practice` 文件中输入几个 `ex` 命令后，你应该在同一文件上调用 `vi`，以便在更熟悉的视觉模式下查看它。在 `ex` 中工作时，命令 `:vi` 可将你从 `ex` 切换到 `vi`。

要从 `vi` 中调用 `ex` 命令，你必须输入特殊的底行字符 `:`（冒号）。然后输入 `ex` 命令并按 ENTER 执行。例如，在 `ex` 编辑器中，你只需在冒号提示符下输入行号即可移动到该行。要从 `vi` 中使用此命令移动到文件的第 6 行，请输入：

```
:6
```

然后按 ENTER。

在以下练习之后，我们仅讨论从 `vi` 执行的 `ex` 命令。

## Exercise: The ex Editor

练习：ex 编辑器

This exercise should be run from inside a terminal emulator window:

此练习应在终端仿真器窗口内运行：

| At the shell prompt, invoke the `ex` editor on a file called `practice`: || `ex practice` |
| --- | --- |
| A message appears: || `"practice" 8L, 261B Entering Ex mode. Type "visual" to go to Normal mode.` |
| Go to and print (display) the first line: || `:1` |
| Print (display) lines 1 through 3: || `:1,3` |
| Substitute `line` for `screen` on line 1: || `:1s/screen/line/` |
| Invoke the `vi` editor on file: || `:vi` |
| Go to the first line: || `:1` |

## Problem Getting to Visual Mode

问题：进入视觉模式

While editing in `vi`, you accidentally end up in the `ex` editor.

在 `vi` 中编辑时，你不小心进入了 `ex` 编辑器。

A `Q` in the command mode of `vi` invokes `ex`. Any time you are in `ex`, the command `vi` returns you to the `vi` editor.

在 `vi` 的命令模式下输入 `Q` 会调用 `ex`。只要你在 `ex` 中，命令 `vi` 就会将你返回到 `vi` 编辑器。

## Editing with ex

使用 ex 编辑

Many `ex` commands that perform normal editing operations have an equivalent in `vi` that does the job more simply. Obviously, you will use `dw` or `dd` to delete a single word or line rather than using the `delete` command in `ex`. However, when you want to make changes that affect numerous lines, you will find the `ex` commands more useful. They allow you to modify large blocks of text with a single command.

许多执行常规编辑操作的 `ex` 命令在 `vi` 中都有等价物，可以更简单地完成工作。显然，你会使用 `dw` 或 `dd` 来删除单个单词或行，而不是在 `ex` 中使用 `delete` 命令。然而，当你想要进行影响多行的更改时，你会发现 `ex` 命令更有用。它们允许你用单个命令修改大块文本。

These `ex` commands are listed here, along with abbreviations for those commands. Remember that in `vi`, each `ex` command must be preceded with a colon. You can use the full command name or the abbreviation, whichever is easier to remember:

这里列出了这些 `ex` 命令以及它们的缩写。请记住，在 `vi` 中，每个 `ex` 命令前面必须加上冒号。你可以使用完整命令名或缩写，以你更容易记住的方式为准：

| Full name || Abbreviation || Meaning |
| --- | --- | --- |
| `delete` || `d` || Delete lines |
| --- | --- | --- |
| `move` || `m` || Move lines |
| `copy` || `co` || Copy lines |
|  || `t` || Copy lines (a synonym for `co`; short for “to”) |

You can separate the different elements of an `ex` command with spaces, if you find the command easier to read that way. For example, you can separate line addresses, patterns, and commands in this way. You cannot, however, use a space as a separator inside a pattern or at the end of a substitute command. (We show some examples later, in the section “Combining ex Commands”.)

你可以根据需要用空格分隔 `ex` 命令的不同元素，使命令更易读。例如，你可以以这种方式分隔行地址、模式和命令。然而，你不能在模式内部或替换命令末尾使用空格作为分隔符。（我们在后面的章节 “合并 ex 命令” 中展示了一些示例。）

### Line Addresses

### 行地址

For each `ex` editing command, you have to tell `ex` which line number(s) to edit. And for the `ex` `move` and `copy` commands, you also need to tell `ex` where to move or copy the text to.

对于每个 `ex` 编辑命令，你必须告诉 `ex` 要编辑哪一个或哪些行号。对于 `ex` 的 `move` 和 `copy` 命令，你还需要告诉 `ex` 将文本移动或复制到哪里。

You can specify line addresses in several ways:

你可以以多种方式指定行地址：

* With explicit line numbers
* 使用明确的行号
* With symbols that help you specify line numbers relative to your current position in the file
使用有助于相对于文件当前位置指定行号的符号
* With search patterns as addresses that identify the lines to be affected
使用搜索模式作为地址来标识受影响的行

Let’s look at some examples.

让我们看一些示例。

#### Defining a Range of Lines

#### 定义行范围

You can use line numbers to explicitly define a line or range of lines. Addresses that use explicit numbers are called absolute line addresses. For example:

你可以使用行号显式定义一行或一行范围。使用显式数字的地址称为绝对行地址。例如：

`:3,18d`

Delete lines 3 through 18, inclusive.

删除第 3 行到第 18 行（含）。

`:160,224m23`

Move lines 160 through 224 to follow line 23. (Like `delete` and `put` in `vi`.)

将第 160 行到第 224 行移动到第 23 行之后。（类似于 `vi` 中的 `delete` 和 `put`。）

`:23,29co100`

Copy lines 23 through 29 and put them after line 100. (Like `yank` and `put` in `vi`.)

复制第 23 行到第 29 行并将其放在第 100 行之后。（类似于 `vi` 中的 `yank` 和 `put`。）

To make editing with line numbers easier, you can also display all line numbers on the left of the screen. The command:

为了使使用行号编辑更方便，你还可以将所有行号显示在屏幕左侧。命令：

```
:set number
```

or its abbreviation

或其缩写

```
:set nu
```

displays line numbers. The file `practice` then appears:

显示行号。此时 `practice` 文件显示如下：

```
1  With a line editor                  "screen" changed to "line" earlier
2  you can scroll the page,
3  move the cursor, delete lines,
4  insert characters, and more
```

The displayed line numbers are not saved when you write a file, and they do not print if you print the file. Remember that long lines wrap on the screen but still count as a single line with respect to their numbers. Line numbers are displayed either until you quit the editing session or until you disable the `set` option:

显示的行号在写入文件时不会保存，并且在打印文件时也不会打印。请记住，长行在屏幕上会换行，但就其编号而言仍算作单行。行号会一直显示，直到你退出编辑会话或禁用 `set` 选项：

```
:set nonumber
```

or:

或：

```
:set nonu
```

Vim lets you toggle the setting with:

Vim 允许你使用以下命令切换设置：

```
:set nu!
```

To temporarily display the line numbers for a set of lines, you can use the `#` sign as a command. For example:

要临时显示一组行的行号，你可以使用 `#` 符号作为命令。例如：

```
:1,10#
```

displays the line numbers from line 1 to line 10.

显示第 1 行到第 10 行的行号。

As described in the section “Movement by Line Number”, you can also use the `CTRL-G` command to display the current line number. You can thus identify the line numbers corresponding to the start and end of a block of text by moving to the start of the block, typing `CTRL-G`, and then moving to the end of the block and typing `CTRL-G` again.

如章节 “按行号移动” 中所述，你还可以使用 `CTRL-G` 命令显示当前行号。因此，你可以通过移动到文本块的开头，输入 `CTRL-G`，然后移动到文本块的末尾再次输入 `CTRL-G`，从而确定块的起始和结束行号。

Yet another way to identify line numbers is with the `=` command in `ex`:

在 `ex` 中使用 `=` 命令来识别行号是另一种方法：

`:=`

Print the total number of lines.

打印总行数。

`:.=`

Print the line number of the current line. (The period is a shorthand that means “the current line”; we discuss this next.)

打印当前行的行号。（句点是“当前行”的简写；我们接下来会讨论。）

`:/pattern/=` 

Print the line number of the next line that matches `pattern`. (The search starts from the current line. The use of search patterns is described shortly, in the section “Search Patterns”.)

打印匹配 `pattern` 的下一行的行号。（搜索从当前行开始。搜索模式的使用将在稍后章节 “搜索模式” 中描述。）

#### Line-Addressing Symbols

#### 行地址符号

Besides line numbers, you can also use symbols for line addresses. A dot (`.`) stands for the current line, and `$` stands for the last line of the file. `%` stands for every line in the file; it’s the same as the combination `1,$`. These symbols can also be combined with absolute line addresses. For example:

除了行号，你还可以使用符号作为行地址。点号（`.`）代表当前行，`$` 代表文件的最后一行。`%` 代表文件中的每一行；它等同于组合 `1,$`。这些符号也可以与绝对行地址组合。例如：

`:.,$d`

Delete from the current line to the end of the file.

从当前行删除到文件末尾。

`:20,.m$`

Move from line 20 through the current line (inclusive) to the end of the file.

将第 20 行到当前行（含）移动到文件末尾。

`:%d`

Delete all the lines in the file.

删除文件中的所有行。

`:%t$`

Copy all the lines and place them at the end of the file (making a consecutive duplicate).

复制所有行并将其放在文件末尾（形成连续的重复）。

In addition to an absolute line address, you can specify an address relative to the current line. The symbols `+` and `-` work like arithmetic operators. When placed before a number, these symbols add or subtract the value that follows. For example:

除了绝对行地址外，你还可以指定相对于当前行的地址。符号 `+` 和 `-` 像算术运算符一样工作。当放在数字前面时，这些符号会加上或减去其后的值。例如：

`:.,.+20d`

Delete from the current line through the next 20 lines.

从当前行删除到接下来的 20 行。

`:226,$m.-2`

Move lines 226 through the end of the file to two lines above the current line.

将第 226 行到文件末尾移动到当前行上方两行处。

`:.,+20#`

Display line numbers from the current line to 20 lines further on in the file.

显示从当前行到文件后 20 行的行号。

In fact, you don’t need to type the dot (`.`) when you use `+` or `-` because the current line is the assumed starting position.

事实上，使用 `+` 或 `-` 时不需要输入点号（`.`），因为当前行是假定的起始位置。

Without a number following them, `+` and `-` are equivalent to +1 and –1, respectively.^(2) Similarly, `++` and `--` each extend the range by an additional line, and so on. Thus `:+++` moves you forward by three lines. The `+` and `-` can also be used with search patterns, as shown in the next section.

如果没有跟随数字，`+` 和 `-` 分别等同于 +1 和 –1。^(2) 同样，`++` 和 `--` 各自将范围扩展一行，依此类推。因此 `:+++` 会向前移动三行。`+` 和 `-` 也可与搜索模式一起使用，如下一节所示。

The number `0` stands for the top of the file (imaginary line zero). `0` is equivalent to `1-`, and both allow you to move or copy lines to the very start of a file, before the first line of existing text. For example:

数字 `0` 代表文件的顶部（虚拟的第零行）。`0` 等同于 `1-`，两者都允许你将行移动或复制到文件的最开始，即现有文本的第一行之前。例如：

`:-,+t0`

Copy three lines (the line above the cursor through the line below the cursor) and put them at the top of the file.

复制三行（光标上方一行到光标下方一行）并将其放在文件顶部。

#### Search Patterns

#### 搜索模式



## 使用搜索模式
另一种 `ex` 指定行的方式是使用搜索模式。例如：

`:/pattern/d`

:
删除包含 `pattern` 的下一行。

`:/pattern/+d`

:
删除包含 `pattern` 的下一行之后的下一行。（你也可以使用 `+1` 代替单独的 `+`。）

`:/pattern1/,/pattern2/d`

:
从包含 `pattern1` 的第一行删除到包含 `pattern2` 的第一行。

`:.,/pattern/m23`

:
将从当前行（`.`）到包含 `pattern` 的第一行的文本移至 23 行之后。

请注意，模式由斜杠在 *之前* 和 *之后* 限定。斜杠之间的任何空格或制表符都被解释为要搜索的模式的一部分。

使用 `?` 作为分隔符而不是 `/` 如果你想在文件中向后搜索。

如果使用 `vi` 和 `ex` 按模式进行删除，这两个编辑器的操作方式有所不同。假设你的文件 `practice` 包含以下行：

```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing results
of your edits as you make them.
```

要删除到单词 `while`，请执行以下操作：

| 按键 | 结果 |
|---|---|
| `d/while` | `With a screen editor you can scroll the page, move the cursor, while seeing results of your edits as you make them.` `vi` 删除到 `pattern` 命令从光标删除到单词 `while`，但保留两行的其余部分。 |
| `:.,/while/d` | `With a screen editor you can scroll the of your edits as you make them.` `ex` 命令删除整个指定的行范围——在本例中，同时删除当前行和包含 `pattern` 的行。所有行都将被完整删除。 |

## 重新定义当前行位置
有时，在命令中使用相对行地址可能会导致意外的结果。例如，假设光标在第 1 行，你想打印第 100 行及其下面的五行。如果输入：

```
:100,+5 p
```

你会收到 Vim 的错误信息：“E16: Invalid range。” `vi` 会告诉你，“First address exceeds second。” 命令失败的原因是第二个地址是相对于当前光标位置（第 1 行）计算的，所以你的命令实际上是这样说的：

```
:100,6 p
```

你需要一种方法告诉命令将第 100 行视为“当前行”，即使光标在第 1 行。

`ex` 提供了这样一种方法。当你使用分号代替逗号时，第一行地址会被重新计算为当前行。例如，命令：

```
:100;+5 p
```

打印所需行。`+5` 现在是相对于第 100 行计算的。分号在搜索模式以及绝对地址中都很有用。例如，要打印包含 `pattern` 的下一行及其后面的 10 行，输入命令：

```
:/pattern/;+10 p
```

## 全局搜索
你已经知道如何使用 `/`（斜杠）在文件中搜索字符模式。`ex` 有一个全局命令 `g`，它可以搜索模式并在找到时显示包含该模式的所有行。命令 `:g!` 与 `:g` 相反。使用 `:g!`（或其同义词 `:v`）来搜索所有 *不* 包含 `pattern` 的行。

你可以在文件的全部行上使用全局命令，也可以使用行地址将全局搜索限制为指定的行或行范围：

`:g/pattern`

:
查找（移动到）文件中 `pattern` 的最后一次出现。

`:g/pattern/p`

:
查找并显示文件中包含 `pattern` 的所有行。Vim 显示该行然后提示你，“Press ENTER or type command to continue.”

`:g!/pattern/nu`

:
查找并显示文件中不包含 `pattern` 的所有行；同时显示找到的每一行的行号。

`:60,124g/pattern/p`

:
查找并显示 60 到 124 行之间包含 `pattern` 的任何行。

正如你所预期的那样，`g` 也可以用于全局替换。我们将在 第 6 章，“全局替换” 中讨论这个问题。

## 组合 ex 命令
你并不总是需要输入冒号来开始一个新的 `ex` 命令。在 `ex` 中，竖线 (`|`) 是命令分隔符，允许你从同一个 `ex` 提示符组合多个命令（与分号在 shell 提示符下分隔多个命令的方式非常相似）。当你使用 `|` 时，请跟踪你指定的行地址。如果一个命令影响了文件中行的顺序，下一个命令将使用新的行位置执行其工作。例如：

`:1,3d | s/thier/their/`

:
删除 1 到 3 行（现在你位于文件的顶行），然后对当前行进行替换（在你调用 `ex` 提示符之前它是第 4 行）。

`:1,5 m 10 | g/pattern/nu`

:
将 1 到 5 行移至 10 行之后，然后显示所有包含 `pattern` 的行（带行号）。

注意使用空格使命令更易读。

# 保存和退出文件
![](img/lvv8_0501.png)

与 IAmDevloper 不同（参见 图 5-1），你已经学习了 `vi` 命令 `ZZ` 来写入（保存）文件并退出。但你经常会想使用 `ex` 命令退出文件，因为这些命令提供了更大的控制。我们已经在顺带提及过其中一些命令。现在让我们更正式地看一下：

`:w`

:
写入（保存）缓冲区到文件但不退出。你可以在（并且应该）在整个编辑会话中使用 `:w` 来保护你的编辑免受系统故障或重大编辑错误的影响。

`:q`

:
退出编辑器（并返回到 shell 提示符）。

`:wq`

:
写入文件然后退出编辑器。写入无条件发生，即使文件未被更改。这会更新文件的修改时间。

`:x`

:
写入文件然后退出（退出）编辑器。文件仅在被修改时才会被写入。^(3)

编辑器保护现有文件和缓冲区中的编辑。例如，如果你想将缓冲区写入现有文件，你会收到警告。同样，如果你已在文件上调用 `vi`、进行了编辑并想在不保存编辑的情况下退出，`vi` 会给出错误信息，例如：

```
No write since last change.
```

这些警告可以防止代价高昂的错误，但有时你仍想继续执行命令。在命令后加上感叹号 (`!`) 可覆盖警告：

```
:w!
:q!
```

当你尝试在不保存文件的情况下退出时，Vim 会友好地告诉你这一点：

```
E37: No write since last change (add ! to override)
```

`:w!` 也可以用于保存使用 `vi -R` 或 `view` 以只读模式打开的文件中的编辑（假设你对文件有写权限）。

`:q!` 是一个基本编辑命令，允许你在不影响原始文件的情况下退出，无论你在会话中做了任何更改。^(4) 缓冲区的内容将被丢弃。

## 重命名缓冲区
你也可以使用 `:w` 将整个缓冲区（你正在编辑的文件的副本）保存为新文件名。

假设你有一个文件 `practice`，其中包含 600 行。你打开文件并进行大量编辑。你想退出，但也想同时保存 `practice` 的旧版本和你的新编辑以便比较。要将编辑后的缓冲区保存到名为 `practice.new` 的文件中，输入命令：

```
:w practice.new
```

你在文件 `practice` 中的旧版本保持不变（前提是你之前没有使用 `:w`）。现在你可以通过输入 `:q` 来退出对新版本的编辑。

## 保存文件的一部分
在编辑时，你有时会想将文件的一部分保存为单独的新文件。例如，你可能输入了格式代码和文本，想将其用作多个文件的页眉。



# 保存文件的一部分

你可以将 `ex` 行地址与写命令 `w` 结合起来保存文件的一部分。例如，如果你在文件 `practice` 中，想要将 `practice` 的一部分保存为文件 `newfile`，你可以输入：

`:230,$w newfile`

从第 230 行保存到文件末尾到 `newfile` 中。

`:.,600w newfile`

从当前行保存到第 600 行到 `newfile` 中。

## 追加到已保存的文件

你可以使用 Unix 重定向和追加操作符 (`>>`) 与 `w` 结合，将缓冲区全部或部分内容追加到现有文件中。例如，如果你输入：

```
:1,10w newfile
```

然后：

```
:340,$w >> newfile
```

`newfile` 将包含第 1–10 行以及从第 340 行到缓冲区末尾的内容。

# 将文件复制到另一个文件

有时你想将某些其他现有文件中的文本或数据复制到正在编辑的文件中。你可以使用 `ex` 命令读取另一个文件的内容：

```
:read filename
```

或其缩写：

```
:r filename
```

该命令将 `filename` 的内容插入到文件光标位置之后的行开始处。如果你想指定光标所在行之外的行，只需在 `:read` 或 `:r` 命令前输入你想要的行号或其他行地址即可。

假设你正在编辑文件 `practice`，并且想要从名为 `/home/tim` 的另一个目录中读取一个名为 `data` 的文件。将光标定位到想要插入新数据行的上一行，然后输入：

```
:r /home/tim/data
```

`/home/tim/data` 的全部内容被读入 `practice`，从光标所在行的下方开始。

要读取同一文件并将其放在第 185 行之后，你可以输入：

```
:185r /home/tim/data
```

以下是读取文件的其他方法：

`:$r /home/tim/data`

将读取的文件放在当前文件的末尾。

`:0r /home/tim/data`

将读取的文件放在当前文件的最开头。

`:/pattern/r /home/tim/data`

将读取的文件放在当前文件中，`pattern` 所在行的后面。

# 编辑多个文件

`ex` 命令使你能够在多个文件之间切换。编辑多个文件的优点是速度；对于每个想要编辑的文件，退出并重新进入 `vi` 或 `Vim` 需要花费时间。在同一编辑会话中停留并在文件之间切换不仅访问更快，而且你还保留了已定义的缩写和命令序列（参见 第 7 章，“高级编辑”），并且保留 yank 寄存器，以便你可以从一个文件复制文本到另一个文件。

## 在多个文件上调用 Vim

当你首次调用编辑器时，可以命名多个文件进行编辑，然后使用 `ex` 命令在文件之间切换。例如：

```
$ vim file1 file2
```

首先编辑 `file1`。在完成对第一个文件的编辑后，`ex` 命令 `:w` 会写入（保存）`file1`，而 `:n` 会调入下一个文件（`file2`）。

假设你想要编辑两个文件，`practice` 和 `note`：

| Keystrokes | Results |
|---|---|
| `$ vim practice note` | ```With a screen editor you can scroll the page, move the cursor, delete lines, insert characters, and more,``` 打开两个文件 `practice` 和 `note`。第一个命名文件 `practice` 会出现在你的屏幕上。执行任何编辑。 |
| `:w` | ```"practice" 8L, 261C 8,1 All``` 使用 `ex` 命令 `w` 保存已编辑的文件 `practice`。 |
| `:n` | ```Dear Mr. Henshaw: Thank you for the prompt . . .``` 使用 `ex` 命令 `n` 调入下一个文件 `note`。执行任何编辑。 |
| `:x` | ```"note" 19L, 571C written 19,1 All``` 保存第二个文件 `note` 并退出编辑会话。 |

## 使用参数列表

`ex` 实际上让你做的不仅仅是使用 `:n` 移动到参数列表中的下一个文件。`:args` 命令（缩写为 `:ar`）列出命令行上命名的文件，当前文件用方括号括起来：

| Keystrokes | Results |
|---|---|
| `$ vim practice note` | ```With a screen editor you can scroll the page, move the cursor, delete lines, insert characters, and more,``` 打开两个文件 `practice` 和 `note`。第一个命名文件 `practice` 会出现在你的屏幕上。 |
| `:args` | ```[practice] note 8,1 All``` Vim 在状态行中显示参数列表，当前文件名周围带有方括号。 |

`vi` 的 `:rewind`（`:rew`）命令将当前文件重置为命令行上命名的第一个文件。Vim 提供了相应的 `:last` 命令来移动到命令行上的最后一个文件。如果你只是想返回到上一个文件，Vim 提供了 `:prev` 命令。

## 调入新文件

你不必在编辑会话开始时调入多个文件。你可以在任何时候使用 `ex` 命令 `:e` 切换到另一个文件。如果你想编辑另一个文件，首先需要保存当前文件（`:w`）然后给出命令：

```
:e filename
```

假设你正在编辑文件 `practice`，并且想要编辑文件 `letter` 然后返回到 `practice`：

| Keystrokes | Results |
|---|---|
| `:w` | ```"practice" 8L, 261C 8,1 All``` 使用 `:w` 保存 `practice`。`practice` 已保存并仍保留在屏幕上。现在你可以切换到另一个文件，因为你的编辑已保存。 |
| `:e letter` | ```"letter" 23L, 1344C 1,1 All``` 使用 `:e` 调入文件 `letter`。执行任何编辑。 |

## 文件名快捷方式

编辑器一次“记住”两个文件名作为当前文件名和备用文件名。这些可以通过符号 `%`（当前文件名）和 `#`（备用文件名）引用。`#` 在与 `:e` 结合使用时特别有用，因为它允许你在两个文件之间轻松切换。在刚才给出的示例中，你可以通过输入命令 `:e #` 返回到第一个文件 `practice`。你也可以通过输入 `:r #` 将文件 `practice` 读入当前文件。

如果你尚未先保存当前文件，编辑器不允许你使用 `:e` 或 `:n` 切换文件，除非你在命令后添加感叹号以强制执行。

例如，如果在对 `letter` 进行一些编辑后，你想丢弃编辑并返回到 `practice`，你可以输入 `:e! #`。

以下命令也很有用；它会丢弃你的编辑并返回到当前文件的上次保存版本：

```
:e!
```

与 `#` 符号相比，`%` 主要在将当前缓冲区的内容写入新文件时有用。例如，在前面章节 “重命名缓冲区” 中，我们向你展示了如何使用命令保存文件 `practice` 的第二个版本：

```
:w practice.new
```

由于 `%` 代表当前文件名，该行也可以输入为：

```
:w %.new
```

## 从命令模式切换文件

由于切换回上一个文件是你会经常做的事情，你不必为此移动到 `ex` 命令行。`vi` 命令 `CTRL-^`（CTRL 键加上插入符号键）会为你执行此操作。使用此命令等同于输入 `:e #`。与 `:e` 命令一样，如果当前缓冲区未保存，编辑器不允许你切换回上一个文件。

## 文件间的编辑

当你给 yank 寄存器一个单字母名称时，你就有了一种方便的方式在文件之间移动文本。命名寄存器不会被清除，使用 `:e` 命令将新文件加载到编辑缓冲区时。 因此，通过从一个文件 yank 或删除文本（如果必要则放入多个命名寄存器），使用 `:e` 调入新文件，并将命名寄存器放入新文件，你可以在文件之间传输材料。



以下示例说明如何将文本从一个文件传输到另一个文件：

| Keystrokes | Results |
|---|---|
| `"f4yy` | 将四行提取到寄存器 `f` 中。<br>```With a screen editor you can scroll<br>the page, move the cursor, delete lines,<br>insert characters, and more, while seeing<br>the results of the edits as you make them``` |
| `:w` | 保存文件。<br>```"practice" 8L, 261C               8,1           All``` |
| `:e letter` | 使用 `:e` 进入文件 `letter`。将光标移动到要放置复制文本的位置。<br>```Dear Mr.<br>Henshaw:<br>I thought that you would be interested to know that:<br>Yours truly,``` |
| `"fp` | 将命名寄存器 `f` 中提取的文本放置在光标下方。<br>```Dear Mr.<br>Henshaw:<br>I thought that you would be interested to know that:<br>With a screen editor you can scroll<br>the page, move the cursor, delete lines,<br>insert characters, and more, while seeing<br>the results of the edits as you make them<br>Yours truly,``` |

另一种将文本从一个文件移动到另一个文件的方法是使用 `ex` 命令 `:ya`（yank）和 `:pu`（put）。这些命令的工作方式与等效的 `vi` 命令 `y` 和 `p` 相同，但它们是与 `ex` 的行寻址功能和命名寄存器一起使用的。

例如：

```
:160,224ya  a
```

将第 160 行到第 224 行提取（复制）到寄存器 `a` 中。接下来，你将使用 `:e` 移动到要放置这些行的文件。将光标放在要放置提取行的位置。然后输入：

```
:pu a
```

将寄存器 `a` 的内容放在当前行之后。

# ex 命令摘要

以下是本章中介绍的 `ex` 命令的摘要表（参见表 5-1 至 5-7）。附录 A，“vi、ex 和 Vim 编辑器”⁠，提供了 `vi` 和 Vim 中最有用的 `ex` 命令的更完整的命令参考。

表 5-1. 行打印命令

| Full name | Abbreviation | Meaning |
|---|---|---|
| `Address` |  | 在 `address` 处打印行 |
| `Address range` |  | 在 `address range` 处打印行 |
| `print` | `p` | 打印行 |
|  | `#` | 打印带行号的行 |

表 5-2. 行删除、移动和复制

| Full name | Abbreviation | Meaning |
|---|---|---|
| `delete` | `d` | 删除行 |
| `move` | `m` | 移动行 |
| `copy` | `co` | 复制行 |
|  | `t` | 复制行（`co` 的同义词；“to”的缩写） |
| `yank` | `ya` | 将行提取到命名寄存器 |
| `put` | `pu` | 从命名寄存器放置行 |

表 5-3. 行寻址符号

| Symbol | Meaning |
|---|---|
| `n` | 行号 `n` |
| `.` | 当前行 |
| `$` | 最后一行 |
| `%` | 文件中的所有行 |
| `. +` `n` | 当前行加 `n` |
| `. -` `n` | 当前行减 `n` |
| `/pattern/` | 向前搜索第一个匹配 `pattern` 的行 |
| `?pattern?` | 向后搜索第一个匹配 `pattern` 的行 |

表 5-4. 全局操作

| Full name | Abbreviation | Meaning |
|---|---|---|
| `global` `command` | `g` `command` | 全局（在所有行上）执行 `command` |
| `global!` `pattern` `command` | `g!` `pattern` `command` | 在所有不匹配 `pattern` 的行上执行 `command` |
|  | `v` `pattern` `command` | 在所有不匹配 `pattern` 的行上执行 `command` |

表 5-5. 使用缓冲区和文件

| Full name | Abbreviation | Meaning |
|---|---|---|
| `args` | `ar` | 显示参数列表，当前文件名用方括号括起来 |
| `edit` | `e` | 切换到编辑指定文件 |
| `last` | `la` | 转到参数列表中的最后一个文件 |
| `next` | `n` | 转到命令行上指定的下一个文件 |
| `previous` | `prev` | 返回上一个文件 |
| `read` | `r` | 将指定文件读入编辑缓冲区 |
| `rewind` | `rew` | 返回参数列表中的第一个文件 |
| `write` | `w` | 将编辑缓冲区写入磁盘 |
| `CTRL-^` |  | 返回上一个文件（`vi` 命令） |

表 5-6. 退出编辑器

| Full name | Abbreviation | Meaning |
|---|---|---|
| `quit` | `q` | 退出编辑器 |
|  | `wq` | 无条件写入文件，然后退出 |
| `xit` | `x` | 仅在文件已更改时写入文件，然后退出 |
| `Q` |  | 切换到 `ex`（`vi` 命令） |
| `visual` | `vi` | 从 `ex` 切换到 `vi` |

表 5-7. 文件名简写

| Character | Meaning |
|---|---|
| `%` | 当前文件名 |
| `#` | 上一个文件名 |

^(1) `ex` 源自久负盛名的 Unix 行编辑器 `ed`，后者本身基于一个名为 QED 的更早的行编辑器。这些编辑器在现代系统上的版本仍然可用。

^(2) 在相对地址中，不应将加号或减号符号与后面的数字分开。例如，`+10` 表示“接下来的 10 行”，但 `+` `10` 表示“接下来的 11 行（`1 + 10`）”，这可能不是你想要的（或想要的）。

^(3) `:wq` 与 `:x` 之间的区别在编辑源代码并使用 `make` 时很重要，`make` 根据文件修改时间执行操作。

^(4) 仅丢弃自上次写入以来的更改。
