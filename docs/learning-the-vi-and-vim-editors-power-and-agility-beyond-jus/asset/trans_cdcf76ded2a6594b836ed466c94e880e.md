
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

You can separate the different elements of an `ex` command with spaces, if you find the command easier to read that way. For example, you can separate line addresses, patterns, and commands in this way. You cannot, however, use a space as a separator inside a pattern or at the end of a substitute command. (We show some examples later, in the section [“Combining ex Commands”](#ch05.html#vi8-ch-5-sect-2.7).)

你可以根据需要用空格分隔 `ex` 命令的不同元素，使命令更易读。例如，你可以以这种方式分隔行地址、模式和命令。然而，你不能在模式内部或替换命令末尾使用空格作为分隔符。（我们在后面的章节 [“合并 ex 命令”](#ch05.html#vi8-ch-5-sect-2.7) 中展示了一些示例。）

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

As described in the section [“Movement by Line Number”](#ch03.html#vi8-ch-3-sect-4), you can also use the `CTRL-G` command to display the current line number. You can thus identify the line numbers corresponding to the start and end of a block of text by moving to the start of the block, typing `CTRL-G`, and then moving to the end of the block and typing `CTRL-G` again.

如章节 [“按行号移动”](#ch03.html#vi8-ch-3-sect-4) 中所述，你还可以使用 `CTRL-G` 命令显示当前行号。因此，你可以通过移动到文本块的开头，输入 `CTRL-G`，然后移动到文本块的末尾再次输入 `CTRL-G`，从而确定块的起始和结束行号。

Yet another way to identify line numbers is with the `=` command in `ex`:

在 `ex` 中使用 `=` 命令来识别行号是另一种方法：

`:=`

Print the total number of lines.

打印总行数。

`:.=`

Print the line number of the current line. (The period is a shorthand that means “the current line”; we discuss this next.)

打印当前行的行号。（句点是“当前行”的简写；我们接下来会讨论。）

`:/pattern/=` 

Print the line number of the next line that matches `pattern`. (The search starts from the current line. The use of search patterns is described shortly, in the section [“Search Patterns”](#ch05.html#vi8-ch-5-sect-2.4).)

打印匹配 `pattern` 的下一行的行号。（搜索从当前行开始。搜索模式的使用将在稍后章节 [“搜索模式”](#ch05.html#vi8-ch-5-sect-2.4) 中描述。）

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

Without a number following them, `+` and `-` are equivalent to +1 and –1, respectively.^([2](#ch05.html#idm46601106525168)) Similarly, `++` and `--` each extend the range by an additional line, and so on. Thus `:+++` moves you forward by three lines. The `+` and `-` can also be used with search patterns, as shown in the next section.

如果没有跟随数字，`+` 和 `-` 分别等同于 +1 和 –1。^([2](#ch05.html#idm46601106525168)) 同样，`++` 和 `--` 各自将范围扩展一行，依此类推。因此 `:+++` 会向前移动三行。`+` 和 `-` 也可与搜索模式一起使用，如下一节所示。

The number `0` stands for the top of the file (imaginary line zero). `0` is equivalent to `1-`, and both allow you to move or copy lines to the very start of a file, before the first line of existing text. For example:

数字 `0` 代表文件的顶部（虚拟的第零行）。`0` 等同于 `1-`，两者都允许你将行移动或复制到文件的最开始，即现有文本的第一行之前。例如：

`:-,+t0`

Copy three lines (the line above the cursor through the line below the cursor) and put them at the top of the file.

复制三行（光标上方一行到光标下方一行）并将其放在文件顶部。

#### Search Patterns

#### 搜索模式
