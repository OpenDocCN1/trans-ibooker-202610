## 第 7 章 高级编辑

本章向你介绍 `vi` 和 `Vim` 以及其底层 `ex` 编辑器的一些更高级功能。在开始学习本章介绍的概念之前，你应合理熟悉本书前面章节所介绍的内容。

正如我们在前面章节中所做的那样，本章介绍适用于所有 `vi` 版本的通用功能，但以 `Vim` 为背景。当你在此处单独看到“`vi`”时，通常可以将其理解为“`vi` 和 `Vim`”。

我们将本章分为五个部分。第一部分讨论了多种设置选项的方法，以允许你自定义编辑环境。你将学习如何使用 `set` 命令以及如何使用 `*.exrc` 文件创建多种不同的编辑环境。



第二部分讨论了如何在编辑器内部执行 Unix 命令，以及如何使用编辑器通过 Unix 命令过滤文本。

第三部分讨论了通过将长命令序列简化为缩写，甚至简化为仅使用一个击键的命令（这称为映射键）来保存长命令序列的各种方法。它还包括关于 @-函数的部分，该函数允许您将命令序列存储在寄存器中。

第四部分讨论了从 Unix 命令行或 shell 脚本内部使用 `ex` 脚本。脚本提供了一种强大的方式来进行重复编辑。

第五部分讨论了一些对程序员特别有用的功能。有控制行缩进的选项，以及显示不可见字符（具体为制表符和换行符）的选项。还有在程序代码块或 C 和 C++ 函数中很有用的搜索命令。

# 自定义 vi 和 Vim

`vi` 和 `Vim` 在各种终端上的运行方式不同。

在现代 Unix 系统中，编辑器从 `terminfo` 终端数据库获取有关终端类型的操作说明。^(1)

您还可以从编辑器内部设置一些影响其运行方式的选项。例如，您可以设置一个右边界，使 `vi` 自动换行，这样您就不需要按 ENTER。

您可以通过使用 `ex` 命令 `:set` 从编辑器内部更改选项。此外，每当 `vi` 和 `Vim` 启动时，它们都会读取您主目录中名为 `.exrc` 的文件以获取进一步的操作说明。通过在此文件中放置 `:set` 命令，您可以修改每次使用编辑器时的行为方式。

您还可以在本地目录中设置 `.exrc` 文件，以初始化您希望在不同环境中使用的各种选项。例如，您可以为编辑英文文本定义一组选项，为编辑源程序定义另一组选项。主目录中的 `.exrc` 文件先执行，然后是当前目录中的文件。

最后，存储在环境变量 `EXINIT` 中的任何命令都会在启动时执行。`EXINIT` 中的设置优先于主目录 `.exrc` 文件中的设置。

## `:set` 命令

可以使用 `:set` 命令更改的选项有两种类型：切换选项，其为开启或关闭状态，以及接受数值或字符串值的选项（例如边界的位置或文件名称）。

切换选项默认可能为开启或关闭。要打开切换选项，命令为：

```
:set option
```

要关闭切换选项，命令为：

```
:set nooption
```

例如，要指定模式搜索应忽略大小写，输入：

```
:set ic
```

如果您希望 `vi` 在搜索中恢复区分大小写，请使用命令：

```
:set noic
```

许多选项都有完整名称和缩写。在前面的示例中，`ic` 是 `ignorecase` 的缩写；您也可以输入 `set ignorecase` 来忽略大小写，并输入 `set noignorecase` 来恢复默认行为。

`Vim` 允许您使用以下命令切换选项的值：

```
:set option!
```

一些选项被分配了值。例如，`window` 选项设置屏幕“窗口”中显示的行数。您可以使用等号（=）为这些选项设置值：

```
:set window=20
```

在编辑会话期间，您可以检查正在使用的选项。命令：

```
:set all
```

显示完整的选项列表，包括您已设置的选项和编辑器“选择的”默认值。

显示应如下所示：^(2)

```
autoindent              nomodelines                     noshowmode
autoprint               nonumber                        noslowopen
noautowrite             open                            nosourceany
nobeautify              nooptimize                      tabstop=8
directory=/var/tmp      paragraphs=IPLPPPQPP LIpplpipbp taglength=0
noedcompatible          prompt                          tags=tags /usr/lib/tags
noerrorbells            noreadonly                      term=xterm
noexrc                  redraw                          noterse
flash                   remap                           timeout
hardtabs=8              report=5                        ttytype=xterm
noignorecase            scroll=11                       warn
nolisp                  sections=NHSHH HUnhsh           window=23
nolist                  shell=/bin/bash                 wrapscan
magic                   shiftwidth=8                    wrapmargin=0
mesg                    showmatch                       nowriteany
```

您可以使用以下命令按名称查找任何单个选项的当前值：

```
:set option?
```

命令：

```
:set
```

显示您在 `.exrc` 文件中或在当前会话期间专门更改或设置的选项。例如，显示可能如下所示：

```
number sect=AhBhChDh window=20 wrapmargin=10
```

## `.exrc` 文件

控制您个人编辑环境的 `.exrc` 文件位于您的主目录中。您可以使用 `Vim` 修改 `.exrc` 文件，就像修改任何其他文本文件一样。（当然，任何新设置在您重新启动 `Vim` 或使用 `:source` 命令显式重新读取文件之前都不会生效。）

如果您还没有 `.exrc` 文件，只需像创建任何其他文件一样创建它。将您希望在每次编辑时生效的 `set`、`ab` 和 `map` 命令输入到此文件中。（`ab` 和 `map` 将在本章后面讨论。）示例 `.exrc` 文件可能如下所示：

```
set nowrapscan wrapmargin=7
set sections=SeAhBhChDh nomesg
map q :w^M:n^M
ab ORA O'Reilly Media, Inc.
```

由于该文件在进入视觉模式（`vi`）之前实际上由 `ex` 读取，因此 `.exrc` 中的命令不需要前置冒号。

## 替代环境

除了读取主目录中的 `.exrc` 文件外，编辑器还会读取当前目录中名为 `.exrc` 的文件。这使您可以设置适合特定项目的选项。

在所有现代版本的 `vi` 中，包括 `Vim`，您必须先在主目录的 `.exrc` 文件中设置 `exrc` 选项，然后编辑器才会读取当前目录中的 `.exrc` 文件：

```
set exrc
```

这种机制可防止其他人将可能危及系统安全的命令的 `.exrc` 文件放置到您的工作目录中。^(3)

例如，您可能希望在主要用于编程的目录中有一组选项：

```
set number autoindent sw=4 terse
set tags=/usr/lib/tags
```

以及在用于文本编辑的目录中另一组选项：

```
set wrapmargin=15 ignorecase
```

请注意，您可以在主目录的 `.exrc` 文件中设置某些选项，并在本地目录中取消设置它们。

您还可以通过将选项设置保存在除 `.exrc` 以外的文件中，并使用 `:so` 命令读取该文件来定义替代编辑环境。（`so` 是 `source` 的缩写。）

例如：

```
:so .progoptions
```

编辑器不会使用搜索路径来查找 `:so` 的文件。因此，不以 `/` 开头的文件名被视为相对于当前目录。

本地 `.exrc` 文件对于定义缩写和键映射（本章后面描述）也很有用。使用标记语言编写书籍或其他文档的作者可以轻松地将该书中使用的所有缩写保存在书籍创建的目录中的 `.exrc` 文件中。



请注意，这假设书中的所有文件都在同一目录中。如果它们分布在子目录中，你就必须将 `*.exrc*` 文件复制到每个子目录，或采用不同的方法，例如使用 Vim 的 `autocmd` 功能，该功能允许你根据正在编辑的文件的文件名扩展名设置选项或执行操作。这样可以轻松地为例如 DocBook XML 的编辑自定义一种方式，为例如 AsciiDoc 或 LaTeX 自定义另一种方式。参见章节 “自动命令”。

# 一些有用的选项
当你输入 `:set all` 时可以看到，有很多选项可以设置。其中许多选项由编辑器内部使用，通常不会更改。其他选项在某些情况下很重要，而在其他情况下则不重要（例如，`noredraw` 和 `window` 在跨大陆的 `ssh` 会话中可能很有用）。章节 “Heirloom 和 Solaris vi 选项” 中的 表 B-1 包含每个选项的简要描述。我们建议你花一些时间尝试设置选项。如果某个选项看起来有趣，请尝试设置它（或取消设置它），并观察在编辑时发生了什么。你可能会发现一些出乎意料的有用工具。

如章节 “行内移动” 中前面讨论的那样，一个选项 `wrapmargin` 对于编辑非程序文本至关重要。`wrapmargin` 指定了在输入时自动换行文本所使用的右边界的大小。（这可以节省手动输入回车符。）^(4) 典型值是 7 到 15：
```
:set wrapmargin=10
```
另外三个选项控制编辑器在执行搜索时的行为。通常，搜索会区分大小写（*foo* 与 *Foo* 不匹配），会环绕到文件开头（意味着你可以在文件的任何位置开始搜索仍能找到所有出现的位置），并且在模式匹配时识别通配符。控制这些选项的默认设置分别是 `ignorecase`、`wrapscan` 和 `magic`。要更改这些默认值，你需要设置相反的切换选项：`noignorecase`、`nowrapscan` 和 `nomagic`。

程序员可能特别感兴趣的选项包括 `autoindent`、`expandtab`、`list`、`number`、`shiftwidth`、`showmatch` 和 `tabstop`，以及它们的相反切换选项。

最后，请考虑使用 `autowrite` 选项。设置后，当你发出 `:n`（下一个）命令转到下一个要编辑的文件时，以及在用 `:!` 运行 shell 命令之前，编辑器会自动写入已更改缓冲区的內容。

# 执行 Unix 命令
你可以在编辑时显示或读入任何 Unix 命令的结果。感叹号（`!`）告诉 `ex` 创建一个 shell 并将其后的内容视为 Unix 命令：
```
:!command
```
所以如果你正在编辑并且想在不退出 `vi` 的情况下 double-check 当前目录，你可以输入：
```
:!pwd
```
当前目录的完整路径会出现在你的屏幕上；按 ENTER 继续在文件同一位置编辑。

如果你想连续给出多个 Unix 命令而不在此期间返回编辑会话，可以使用以下 `ex` 命令创建一个 shell：
```
:sh
```
当你想退出 shell 并返回到 `vi` 时，按 CTRL-D。（这甚至在 `gvim`，Vim 的 GUI 版本中也有效。）

你可以将 `:read` 与 shell 调用结合起来，将 Unix 命令的结果读入你的文件。最简单的例子是：
```
:read !date
```
或更简单地：
```
:r !date
```
将系统的日期信息读入你的文件文本中。通过在 `:r` 命令前加上行地址，你可以在文件的任意所需位置读入命令的结果。默认情况下，它会在当前行之后插入。

假设你正在编辑一个文件，想从名为 `phone` 的文件按字母顺序读入四个电话号码。`phone` 的内容为：
```
Willing, Sue  333-4444
Walsh, Linda  555-6666
Quercia, Valerie  777-8888
Dougherty, Nancy  999-0000
```
命令：
```
:r !sort phone
```
在 `phone` 的内容通过 `sort` 过滤器处理后将其读入：
```
Dougherty, Nancy  999-0000
Quercia, Valerie  777-8888
Walsh, Linda  555-6666
Willing, Sue  333-4444
```
假设你正在编辑一个文件，想从目录中插入另一个文件中的文本，但记不起新文件的名称。你*可以*按老方法执行此任务：退出文件，给出 `ls` 命令，记下正确的文件名，重新进入文件并搜索你的位置。

或者你可以更少步骤地完成任务：

| 按键序列 | 结果 |
| --- | --- |
| `:!ls` | ```
file1        file2         letter
newfile      practice
``` 显示当前目录中的文件列表。注意正确的文件名。按 ENTER 继续编辑。 |
| `:r newfile` | ```
"newfile" 35L, 1569C          2,1           Top
``` 读入新文件。 |

### 提示
一位作者经常将 `r` 命令与 `%` 作为文件名结合起来，以便轻松更正文档中的拼写错误：
```
:w
:$r !spell %
```
这会保存文件，然后将 `spell` 命令对该文件输出读入缓冲区末尾。（在某些系统上，你可能想使用 `:r !spell % | sort -u` 来获取已排序的拼写错误词列表。）

有了缓冲区中的拼写错误词列表，我们的作者会逐一检查它们，在文件中搜索并更正错误，并在完成时从列表中删除每个词。

## 通过命令过滤文本
你还可以将文本块作为标准输入发送给 Unix 命令。该命令的输出会替换缓冲区中的文本块。你可以从 `ex` 或 `vi` 过滤文本。两种方法的主要区别在于，你在 `ex` 中用行地址指示文本块，而在 `vi` 中用文本对象（移动命令）指示。

### 使用 ex 过滤文本
第一个示例演示如何使用 `ex` 过滤文本。假设前例中的姓名列表不是包含在名为 `phone` 的单独文件中，而是已经包含在当前文件的第 96 行到第 99 行中。你只需输入要过滤的行地址，后跟感叹号和要执行的 shell 命令。例如，命令：
```
:96,99!sort
```
将第 96 行到第 99 行通过 `sort` 过滤器传递并用 `sort` 的输出替换这些行。

### 使用 vi 移动命令过滤文本
在 `vi` 模式下，通过输入感叹号后跟任何指示文本块的 `vi` 移动按键，然后是 shell 命令行来通过 Unix 命令过滤文本。例如：
```
!)command
```
将下一句话通过 `command` 传递。

使用此功能时 `vi` 的行为有几个不寻常之处：
* 感叹号不会立即出现在你的屏幕上。当你输入要过滤的文本对象的按键时，感叹号会出现在屏幕底部，*但你用来引用对象的字符不会出现。*
* 文本块必须超过一行，因此你只能使用移动超过一行的按键（`G`、`{ }`、`( )`、`[[ ]]`、`+`、`-`）。为重复效果，数字可在感叹号或文本对象之前。 （例如，`!10+` 和 `10!+` 都表示接下来的 10 行。）除非指定足够多的对象以超过单行，否则诸如 `w` 之类的对象不起作用。你也可以使用斜杠（`/`）后跟模式和回车来指定对象。这会将文本一直到该模式作为命令的输入。





- 整行都会受到影响。例如，如果你的光标在某行的中间，并且你发出命令前往下一句的末尾，则包含该句开头和结尾的整行都会被更改，而不仅仅是句子本身。^(5)

- 有一个特殊的文本对象只能与此命令语法一起使用；你可以通过输入第二个感叹号来指定当前行：

```
!!command
```

记住，整个序列或文本对象都可以前面加上一个数字以重复效果。例如，要像前面的示例那样更改第 96 行到第 99 行，你可以将光标定位在第 96 行并输入以下任一内容：

```
4!!sort
```

或：

```
!4!sort
```

再举一个例子，假设你在文件中有一段文本，想要将其从小写字母更改为大写字母。你可以使用 `tr` 命令处理该部分以更改大小写。在此示例中，第二句话是要通过命令过滤的文本块：

```
One sentence before.
With a screen editor you can scroll the page
move the cursor, delete lines, insert characters,
and more, while seeing the results of your edits
as you make them.
One sentence after.
```

| Keystrokes | Results |
|---|---|
| `!)` | 
```
One sentence after.
~
~
~
.,.+4! 
```
行号和感叹号出现在最后一行，以提示你输入 shell 命令。`)` 表示句子是待过滤的文本单位。
|
| `tr '[:lower:]' '[:upper:]'` | 
```
One sentence before.
WITH A SCREEN EDITOR YOU CAN SCROLL THE PAGE
MOVE THE CURSOR, DELETE LINES, INSERT CHARACTERS,
AND MORE, WHILE SEEING THE RESULTS OF YOUR EDITS
AS YOU MAKE THEM.
One sentence after.
```
输入 shell 命令并按 `ENTER`。输入内容将被输出替换。
|

要重复之前的命令，语法是：

```
! object !
```

在编辑电子邮件时，有时将文本通过 `fmt` 程序过滤以在发送消息前“美化”它会很有用。记住，“原始”输入将被输出替换。幸运的是，如果出现错误——例如发送错误消息而不是预期的输出——你可以撤消该命令并恢复行。

# 保存命令

经常在文件中反复输入相同的长短语。有多种不同的方法可以保存长命令序列，既可在命令模式下，也可在插入模式下。当你调用其中一个已保存的序列执行时，你只需输入几个字符（甚至只有一个），整个序列就会像你逐一输入整个命令序列一样执行。

## 单词缩写

你可以在插入模式下输入缩写时，让编辑器自动将其扩展为完整文本来定义缩写。要定义缩写，请使用此 `ex` 命令：

```
:ab abbr phrase
```

`abbr` 是指定 `phrase` 的缩写。组成缩写的字符序列仅在插入模式下作为完整单词输入时才会被扩展；`abbr` 不会在单词内部扩展。

假设在文件 `practice` 中，你想输入包含经常重复的短语的文本，例如难以处理的产品或公司名称。命令：

```
:ab imrc International Materials Research Center
```

将 International Materials Research Center 缩写为首字母 `imrc`。现在，每当你在插入模式下输入 `imrc` 时，`imrc` 就会扩展为完整文本：

| Keystrokes | Results |
|---|---|
| `ithe imrc` | 
```
the International Materials Research Center
```
|

缩写在你按下非字母数字字符（例如标点符号）、空格、回车或 `ESC`（返回命令模式）时立即扩展。在选择缩写时，选择在输入文本时通常不会出现的字符组合。如果创建的缩写在不希望扩展的地方扩展，可以通过输入以下内容禁用缩写：

```
:unab abbr
```

（在执行 `:unab` 命令时按 `ENTER` 时会扩展缩写，但不用担心——它仍然会被正确禁用。）要列出当前已定义的缩写，请输入：

```
:ab
```

组成缩写的字符也不能出现在短语的末尾。例如，如果你发出命令：

```
:ab PG This movie is rated PG
```

`vi` 会给出消息“No tail recursion,”并且缩写不会被设置。该消息意味着你试图定义一个会反复扩展自身、创建无限循环的东西。如果发出命令：

```
:ab PG the PG rating system
```

你不会收到警告消息。

经测试，我们在这些 `vi` 版本上获得了以下结果：

Solaris `/usr/xpg7/bin/vi` 和 “Heirloom” `vi`

尾递归版本不允许，而在扩展中间包含名称的版本只扩展一次。

Vim

两种形式都被检测到并且只扩展一次。

如果你使用的是 Unix `vi`，我们建议你在将缩写作为定义短语的一部分重复之前先测试你的版本。

## 使用 map 命令

在编辑时，你可能会发现自己经常使用某个命令序列，或者偶尔使用非常复杂的命令序列。为了节省按键次数，或节省记住序列所需的时间，你可以使用 `map` 命令将序列分配给未使用的键。

`map` 命令的工作方式与 `ab` 非常相似，只是你为命令模式定义宏，而不是为插入模式定义：

`:map` `x sequence`

将字符 `x` 定义为编辑命令的 `sequence`。

`:unmap` `x`

禁用为 `x` 定义的 `sequence`。

`:map`

列出当前映射的字符。

在开始创建自己的映射之前，你需要知道 `vi` 在命令模式下不使用且可供用户定义命令使用的键：

字母

`g`, `K`, `q`, `V`, and `v`

控制键

`^A`, `^K`, `^O`, `^W`, and `^X`

符号

`_`, `*`, `\`, and `=`

Vim 使用所有这些字符，除了 `^K`、`^_` 和 `\`。

### 注意

如果设置了 Lisp 模式，`=` 会被 `vi` 使用，而 Vim 则用于文本格式化。在许多现代版本的 `vi` 中，`_` 等同于 `^` 命令，而 Vim 有一个使用 `v`、`V` 和 `^V` 键的“视觉模式”。（参见章节 “视觉模式移动”。）寓意是仔细测试你的版本。

根据你的终端，你可能还可以将映射序列与特殊功能键关联。你也可以映射命令模式已使用的键，但那样你将失去该键的默认功能；我们在后面的章节 “映射键的更多示例” 中展示了一个示例。章节 “几个便捷映射” 提供了一些额外的、更强大的映射示例。

使用映射，你可以创建简单或复杂的命令序列。作为一个简单的例子，你可以定义一个命令来反转单词的顺序。在 `vi` 中，光标如图所示：

```
you can the scroll page
```

将 `the` 放在 `scroll` 之后所需的序列是 `dwelp`：删除单词，`dw`；移动到下一个单词的末尾，`e`；向右移动一个空格，`l`；在那里粘贴已删除的单词，`p`。保存此序列：

```
:map v dwelp
```

使你能够在编辑会话的任何时候使用单个按键 `v` 反转两个单词的顺序。

## 使用 Leader 映射

Vim 为几乎每个键都分配了功能。这使得决定映射哪个键（或键）变得棘手或令人困惑。因此，Vim 提供了一种替代映射方式，让你可以使用特殊变量 `mapleader` 定义映射。默认情况下 `mapleader` 的值为 `\`（反斜杠）。




# Vim 键映射指南

现在你可以定义键映射，而无需选择晦涩、不常用的 Vim 键或牺牲现有的 Vim 键/操作。当使用 `mapleader` 定义时，你只需输入 leader 字符，然后输入映射中定义的键即可。

例如，假设你想为 *quit* Vim 创建一个助记符并选择 `q` 作为易于记忆的键，但你不想放弃使用 `q` 作为某些多字符 Vim 命令的起始字符（例如，`qq` 开始录制宏）。为此，将 `q` 与 leader 映射。操作方法如下：

```
:map <leader>q :q<cr>
```

现在你可以使用键序列 `\q` 执行 `ex` 命令 `:quit`。

如果你偏好除 `\` 之外的其他字符，可以将 `mapleader` 设置为你喜欢的选择。由于 `mapleader` 是 Vim 变量，语法如下：

```
:let mapleader="X"
```

其中 *X* 是你选择的 leader 字符。

## 保护键不被 ex 解释

请注意，在定义映射时，你不能简单地键入某些键，例如 ENTER、ESC、BACKSPACE 和 DELETE，作为要映射命令的一部分，因为这些键在 `ex` 中已有其含义。如果你想将其中一个键作为命令序列的一部分包含在内，必须通过在其前面加上 CTRL-V 来转义该键的正常含义。键入 `^V` 在映射中显示为 `^` 字符。`^V` 之后字符的显示也不如你预期。例如，回车显示为 `^M`，转义显示为 `^`，退格显示为 `^H`，等等。

另一方面，如果你想使用控制字符作为要映射的字符，在大多数情况下，你只需按住 CTRL 键同时按下字母键即可。例如，要映射 `^A`，你只需键入：

```
:map CTRL-A sequence
```

然而，有三个控制字符必须使用 `^V` 转义。它们是 `^T`、`^W` 和 `^X`。例如，如果你想映射 `^T`，必须键入：

```
:map CTRL-VCTRL-T sequence
```

CTRL-V 的使用适用于任何 `ex` 命令，而不仅仅是 `map` 命令。这意味着你可以在缩写或替换命令中键入回车。例如，缩写：

```
:ab 123 one^Mtwo^Mthree
```

展开为：

```
one
two
three
```

这里我们将 CTRL-V ENTER 序列显示为 `^M`，即它在屏幕上显示的方式。（Vim 会以不同颜色高亮 `^M`，以便你能辨认它实际上是一个控制字符。）

你也可以在特定位置全局添加行。命令：

```
:g/^Section/s//As you recall, in^M&/
```

在所有以单词 *Section* 开头的行之前插入一句话，单独成行。`&` 恢复搜索模式。

不幸的是，即使你尝试用 CTRL-V 引号引用它，在 `ex` 命令中也总有一个字符具有特殊含义。请记住，竖线 (`|`) 作为多个 `ex` 命令的分隔符具有特殊含义。你不能在插入模式映射中使用竖线。

现在你已经了解如何使用 CTRL-V 在 `ex` 命令内部保护某些键，可以开始定义一些强大的映射序列了。

## 一个复杂的映射示例

假设你有一个词汇表，其条目如下：

```
map - an ex command which allows you to associate
a complex command sequence with a single key.
```

你希望将此词汇表转换为自定义 XML 格式，使其看起来像这样：

```
<glossaryitem>
<name>map</name>
<para>An ex command...
```

定义复杂映射的最佳方式是手动执行一次编辑，写下你必须键入的每个按键，然后将这些按键重新创建为映射。你希望执行以下序列：

1. 插入 `<glossaryitem>` 标签、一个换行符和 `<name>` 标签。
2. 按 ESC 终止插入模式。
3. 移动到第一个词的末尾（`e`）并添加 `</name>` 标签、一个换行符和 `<para>` 标签。
4. 按 ESC 终止插入模式。
5. 向前移动一个字符，离开闭合的 `>` 字符（`l`）。
6. 删除空格、连字符和后面的空格（`3x`）并将下一个单词大写（`~`）。

如果你必须重复执行不止几次，这将是一项相当繁琐的编辑工作。

使用 `:map` 你可以保存整个序列，以便用单个按键重新执行：

```
:map g I<glossaryitem>^M<name>^[ea</name>^M<para>^[l3x~
```

请注意，你必须使用 CTRL-V 对 ESC 和 ENTER 字符进行“引用”。`^[` 是键入 CTRL-V 后跟 ESC 时出现的序列。`^M` 是键入 CTRL-V ENTER 时显示的序列。

现在，只需键入 `g` 即可执行整个编辑系列。在慢速连接上，你实际上可以看到编辑逐个发生；在快速连接上，它似乎会像魔法一样发生。

如果你第一次尝试键映射失败，不要气馁。定义映射时的小错误可能会导致与预期非常不同的结果。输入 `u` 撤销编辑，然后重试。（这正是我们在开发此映射时所做的。）

有些命令输入映射可以使编辑 XML 更容易，详见后面的章节 [“映射多个输入键”。

## 更多键映射示例

以下示例让你了解在定义键盘映射时可能的巧妙快捷方式：

1. 在移动到词尾时添加文本：

```
:map e ea
```

大多数情况下，你想移动到词尾的唯一原因是添加文本。此映射序列会自动将你置于插入模式。请注意，被映射的键 `e` 在 `vi` 中有含义。你可以映射 `vi` 已使用的键，但在映射生效期间，该键的正常功能不可用。在这种情况下这没什么大不了的，因为 `E` 命令通常与 `e` 相同。

2. 交换两个词：

```
:map K dwElp
```

我们在章节前面讨论过这个序列，但现在你需要使用 `E`（在此及以下示例中假设 `e` 命令映射为 `ea`）。请记住，光标开始于两个词中的第一个。不幸的是，由于 `l` 命令，如果两个词位于行尾，此序列（以及之前的版本）将无法工作：在序列过程中，光标会到达行尾，而 `l` 无法再向右移动。

3. 保存文件并编辑系列中的下一个文件：

```
:map q :w^M:n^M
```

（使用 CTRL-V ENTER 将 `^M` 放入映射中。）请注意，你可以将键映射到 `ex` 命令，但务必用回车结束每个 `ex` 命令。此序列使从一个文件移动到下一个文件变得容易，当你用一个 `vi` 命令打开许多短文件时很有用。将字母 `q` 映射有助于你记住该序列类似于“quit”。^(6)

4. 在单词周围放置 `troff` 加粗代码：

```
:map v i\fB^[e\fP^[
```

此序列假设光标位于单词开头。首先你进入插入模式，然后键入粗体字体的代码。在映射命令中，你不需要键入两个反斜杠来产生一个反斜杠。接下来，通过键入“引用”的 ESC 返回到命令模式。最后，你在单词末尾附加闭合的 `troff` 代码，并返回到命令模式。

请注意，当我们附加到单词末尾时，我们不需要使用 `ea`，因为该序列本身已映射到单个字母 `e`。这表明映射序列可以包含其他映射命令。使用嵌套映射序列的能力由 `remap` 选项控制，该选项通常已启用。

5. 在单词周围放置 HTML 加粗代码，即使光标不在单词开头：

```
:map V lbi<B>^[e</B>^[
```



这个序列与前一个相似；除了使用 HTML 代替 `troff` 外，它还使用 `lb` 来处理将光标定位到单词开头的额外任务。光标可能位于单词中间，因此你希望使用 `b` 命令移动到开头。但如果光标已在单词开头，`b` 命令会将光标移动到前一个单词。为防止这种情况，在使用 `b` 后退之前输入一个 `l`，这样光标就不会从单词的第一字母开始。你可以通过将 `b` 替换为 `B`，将 `e` 替换为 `Ea` 来定义此序列的变体。不过，在所有情况下，`l` 命令都会阻止此序列在光标位于行尾时工作。（你可以附加一个空格来解决这个问题。）

### 6. 反复查找并删除单词或短语周围的括号：^(7)

```
:map = xf)xn
```

此序列假定你首先通过输入 `/(` 然后按 ENTER 找到了一个左括号。

如果你选择删除括号，请使用 `map` 命令：用 `x` 删除左括号，用 `f)` 找到右括号，用 `x` 删除它，然后用 `n` 重复查找左括号。

如果你不想删除括号（例如，它们被正确使用），请不要使用映射命令：改为按 `n` 查找下一个左括号。

你也可以修改此示例中的映射序列来处理成对的引号。

### 7. 在整行周围放置 C/C++ 注释：

```
:map g I/* ^A */^[
```

此序列在行开头插入 `/*` 并在行尾追加 `*/`。你也可以映射一个替换命令来实现同样的效果：

```
:map g :s;.*;/* & */;^M
```

这里，你匹配整行（使用 `.*`），并在重放时（使用 `&`）用注释符号包围该行。注意使用分号作为分隔符，以避免转义注释中的 `/` 字符。

最后，你应该知道有许多键要么执行与其它键相同的任务，要么很少使用。（例如，`^J` 的作用与 `j` 相同。）然而，在使用它们进行映射定义从而大胆禁用其正常用途之前，你应该熟悉 `vi` 命令。

## 插入模式的映射键

通常，映射仅适用于命令模式——毕竟，在插入模式下，键代表它们本身，不应作为命令进行映射。然而，通过在 `map` 命令后添加感叹号 (`!`)，你可以强制它覆盖键的常规含义并在插入模式下产生映射。此功能在你处于插入模式但需要短暂转到命令模式、运行命令然后返回插入模式时很有用。

例如，假设你刚输入了一个词，但忘了对其斜体化（或加上引号等）。你可以定义此映射：

```
:map! + ^[bi<I>^[ea</I>
```

现在，当你在单词末尾输入 `+` 时，你会用 HTML 斜体代码包围该词。`+` 不会出现在文本中。

上面显示的序列转到命令模式 (`^[`)，后退以插入第一个代码 (`bi<I>`)，再次转到命令模式 (`^[`)，并向前移动以追加第二个代码 (`ea</I>`)。由于映射序列以插入模式开始和结束，你可以在标记单词后继续输入文本。

这里是另一个例子：假设你在输入文本时意识到上一行应该以冒号结尾。你可以通过定义此映射序列来纠正这一点：^([8)

```
:map! % ^kA:^[jA
```

现在，如果你在当前行任何位置输入 `%`，你将在上一行末尾追加一个冒号。此命令转到命令模式，向上移动一行并追加冒号 (`^[kA:`)。然后命令再次转到命令模式，移回你所在的这一行，并在行尾停留在插入模式 (`^[jA`)。

请注意，我们对之前的映射命令使用了不常见的字符（`+` 和 `%`）。当一个字符被映射为插入模式时，你就不能再将其作为文本输入（除非在前面加上 `CTRL-V`）。

要恢复字符的正常输入，请使用命令：

```
:unmap! x
```

其中 `x` 是之前为插入模式映射的字符。（尽管编辑器在命令行中输入时展开 `x`，看起来你是在取消映射展开后的文本，但它确实正确地取消了该字符的映射。）

插入模式映射通常更适合将字符字符串绑定到你原本不会使用的特殊键上。它在使用可编程功能键时尤其有用，正如我们即将描述的那样。

## 映射功能键

在过去，串行终端配备了可编程功能键。通过终端的特殊设置模式，你可以设置这些键发送你想要的任何字符或字符。应用程序随后可以利用这些功能键，将其用作常见或重要操作的“快捷方式”。

当今的个人电脑和笔记本电脑键盘也具有功能键，通常顶部一行有 10 或 12 个键，标记为 `F1` 到 `F12`。与其通过特殊硬件模式设置其行为，它们的功能是由终端模拟器和系统上运行的其他程序定义的。

由于终端模拟器在 `terminfo` 数据库中有条目，编辑器可以识别功能键生成的转义序列，从而允许你根据需要将它们映射到特定操作。这是在 `ex` 中完成的，使用语法：

```
:map #1 commands
```

用于功能键编号 1，以此类推。

与其他键一样，映射默认适用于命令模式，但通过同时使用 `map!` 命令，你可以为功能键定义两个单独的值——一个用于命令模式，另一个用于插入模式。例如，如果你是 HTML 用户，你可能希望将字体切换代码放在功能键上，如下所示：^([9)

```
:map #2 i<I>^[
:map! #2 <I>
```

如果你处于命令模式，第一个功能键进入插入模式，输入三个字符 `<I>`，然后返回命令模式。如果你已处于插入模式，该键只需输入三个字符的 HTML 代码。如果序列包含 `^M`，即回车，请按 `CTRL-V` `CTRL-M`。

例如，为了使 F2 可用于映射，你的终端的终端数据库条目必须有 `k2` 的定义，例如：

```
k2=^A@^M
```

依次地，定义中的字符：

```
^A@^M
```

必须是你按下该键时输出的内容。

### 查看功能键生成的内容

要查看功能键输出的内容，请使用带 `-c` 选项（显示每个字符）的 `od`（八进制转储）命令。你需要在功能键后按 ENTER，然后按 `CTRL-D` 使 `od` 打印信息。例如：

```
$ od -c           od reads from standard input
^[[[A             function key pressed
^D                Control-D, EOF
0000000 033   [   [   A  \n
0000005
```

这里，功能键发送了 Escape、两个左方括号和一个 A。

## 映射其他特殊键

许多键盘具有特殊键，例如 `HOME`、`END`、`PAGE UP` 和 `PAGE DOWN`，它们复制了 `vi` 命令。如果你的终端模拟器的 `terminfo` 描述是完整的，编辑器会识别这些键。但如果不是，你可以使用 `map` 命令使它们可用。这些键通常向计算机发送转义序列——一个 Escape 字符后跟一个或多个其他字符的串。要捕获 Escape，你应在映射中按特殊键之前按 `^V`。例如，要将标准键盘上的 `HOME` 键映射为合理的 `vi` 等价物，你可以定义以下映射：

```
:map CTRL-V HOME 1G
```

这在你的屏幕上显示为：

```
:map ^[[H 1G
```



# Vim 映射和脚本

## 类似的映射命令

类似的映射命令显示如下：^(10)

```
:map CTRL-V END G             displays    :map ^[Y G
:map CTRL-V PAGE UP ^F        displays    :map ^[[V ^F
:map CTRL-V PAGE DOWN ^B      displays    :map ^[[U ^B
```

你可能希望将这些映射放在你的 `*.exrc` 文件中。注意，如果特殊键生成了一个长的转义序列（包含多个不可打印字符），`^V` 仅引用初始转义字符，映射将无法工作。你必须找到整个转义序列（使用 `od`，如前所示）并手动输入，在适当的位置进行引用，而不是简单地按下 `^V` 然后按该键。

如果你使用不同种类的终端（例如 Windows 系统上的命令窗口和 `xterm`），你不能期望像刚才展示的那样的映射总是有效。因此，`Vim` 提供了描述此类键映射的可移植方式：

```
:map <Home> 1G           Enter six characters: < H o m e > (Vim)
```

### 注意

`vi` 和 `Vim` 通常按所述提供这些映射，如果没有，你可以按指示映射它们。然而，我们发现此类映射违背了 `vi` 的哲学“永远不要离开键盘”。当我们向用户展示如何使用 `Vim` 时，我们做的（或推荐的）第一件事之一就是将 HOME、END、PAGE UP、PAGE DOWN、INSERT、DELETE 以及所有箭头键映射为“无操作”，以鼓励学习原生的 `vi` 命令。最终结果是更高效的编辑和对真正的 `vi` 命令更好的肌肉记忆，同时有一批键现在可用于映射其他繁重工作。

例如，我们中的一人定期编辑他管理的四家冰淇淋店的数据文件。数据中偶尔会有行包含商店当天的销售信息，以空白分隔。他使用 `Vim` 的 `autocommand` 功能来检测他正在编辑的文件是否匹配商店名称。（有关自动命令的更多信息，请参阅章节 [“自动命令”。）这反过来将 END 键定义为求和四个值并更改行以显示值 *和* 总计。该映射如下所示：^(11)

```
:noremap <end> !!awk 'NF == 4 && $1 + $2 + $3 + $4 > 0 {
  printf "\%s  total: $\%.2f\n", $0, $1 + $2 + $3 + $4;
  exit }; { print $0 }'<cr>
```

这在 `*.vimrc` 文件中是一行很长的代码；我们将其拆分为几行以便在页面上显示。因此，“繁重工作”是在数据文件中看起来像这样的行上使用 END 键：

```
450 235 1002 499
```

它会被转换为：

```
450 235 1002 499  total: $2186.00
```

当他按下 END 键时。请注意，映射中有一个双重健全性检查，使用 `awk` 命令进行计算，但仅当有四个字段（`NF == 4`）时，且仅当将它们相加得到的值大于零时。（有关 `:noremap` 命令的信息，请参阅 `:help :map-modes`。）

## 映射多个输入键

映射多个击键并不仅限于功能键。你也可以映射常规击键的序列。这可以帮助更轻松地输入某些类型的文本，例如 DocBook XML 或 HTML。

以下是一些 `:map` 命令，感谢 Jerry Peek，使输入 DocBook XML 标记变得更容易。以双引号开头的行是注释（这将在后面章节 “ex 脚本中的注释” 中讨论）：

```
:set noremap
" bold:
map! =b </emphasis>^F<i<emphasis role="bold">
map =B i<emphasis role="bold">^[
map =b a</emphasis>^[
" Move to end of next tag:
map! =e ^[f>a
map =e f>
" footnote (tacks opening tag directly after cursor in text-input mode):
map! =f <footnote>^M<para>^M</para>^M</footnote>^[kO
" Italics ("emphasis"):
map! =i </emphasis>^[F<i<emphasis>
map =I i<emphasis>^[
map =i a</emphasis>^[
" paragraphs:
map! =p ^[jo<para>^M</para>^[O
map =P O<para>^[
map =p o</para>^[
" less-than:
map! *l &lt;
...
```

使用这些命令，要输入脚注，你需要进入插入模式并输入 `=f`。编辑器随后会插入开始和结束标签，并在两者之间保持插入模式：

```
All the world's a stage.<footnote>
<para> 
</para>
</footnote>
```

这些宏在本书早期版本的开发过程中证明非常有用；它们可以很容易地适应不同的标记语言，例如 AsciiDoc、LaTeX、Texinfo 或 Sphinx。

## @-函数

命名寄存器提供了创建 *宏* 的另一种方法——复杂的命令序列，你可以用很少的击键重复。

如果你在文本中输入一行命令（`vi` 序列或以冒号 *开头的 `ex` 命令），然后将其删除到命名寄存器中，你可以使用 `@` 命令执行该寄存器的内容。例如，打开新行并输入：

```
cwgadfly CTRL-V ESC
```

这显示为：

```
cwgadfly^[
```

在你的屏幕上。再次按下 ESC 退出插入模式，然后通过输入 `"gdd` 将该行删除到寄存器 `g` 中。现在，每当你将光标放在一个词的开头并输入 `@g` 时，文本中该词就会被更改为 *gadfly*。^([12)

由于 `@` 被解释为 `vi` 命令，句点 (.) 会重复整个序列，即使寄存器包含 `ex` 命令。`@@` 重复上一个 `@`，而 `u` 或 `U` 可用于撤销 `@` 的效果。

`Vim` 使保存文本到命名寄存器变得更容易。在 `vi` 命令模式下，后跟寄存器名称的 `q` 开始将你输入的内容记录到命名寄存器中。单独使用 `q` 结束录制。`Vim` 会在状态行中显示一条正在录制的消息以提醒你。在 `Vim` 中使用寄存器 `a` 进行前面的示例，你会输入 `qacwgadfly^q`，然后可以用 `@a` 执行。

这是一个简单的例子。@-函数很有用，因为它们可以适应非常具体的命令。当你在文件之间编辑时它们尤其有用，因为你可以将命令存储在其命名寄存器中并从编辑的任何文件中访问它们。@-函数也与 [第 6 章，“全局替换” 中讨论的全局替换命令结合使用时很有用。

当然，如果你既使用命名寄存器进行 @-函数，也用于存储 yank 或删除的文本，你需要小心将它们分开，也许使用字母表中较早的字母进行存储，并保留字母表末尾的字母用于 @-函数。

## 从 ex 执行寄存器

你也可以从 `ex` 模式执行保存在寄存器中的文本。在这种情况下，你会输入一个 `ex` 命令，将其删除到命名寄存器中，然后从 `ex` 冒号提示符使用 `@` 命令。例如，输入以下文本：

```
ORA publishes great books.
ORA is my favorite publisher.
1,$s/ORA/O'Reilly Media/g
```

将光标放在最后一行，将命令删除到 `g` 寄存器：`"gdd`。将光标移动到第一行：`kk`。然后从冒号命令行执行寄存器：`:@g` ENTER。你的屏幕现在应该如下所示：

```
O'Reilly Media publishes great books.
O'Reilly Media is my favorite publisher.
```

一些版本的 `vi` 在从 `ex` 命令行使用时将 `*` 与 `@` 视为相同。`Vim` 也这样做，但仅当设置了 `compatible` 选项时。此外，如果在 `@` 或 `*` 命令后提供的寄存器字符是 `*`，则命令取自默认（未命名）寄存器。

## 使用 ex 脚本

某些 `ex` 命令你只在 `vi` 内部使用，例如映射、缩写等。如果你将这些命令存储在你的 `*.exrc` 文件中，命令会在你调用 `vi` 或 `Vim` 时自动执行。任何包含要执行命令的文件称为 *脚本*。

典型的 `*.exrc` 脚本中的命令在 `vi` 之外没有用处。然而，你可以将其他 `ex` 命令保存到脚本中，然后在文件或多个文件上执行该脚本。在这些外部脚本中，你主要会使用替换命令。



对于（技术）作者来说，`ex` 脚本的一个有用应用是确保文档集中术语——甚至拼写的一致性。例如，假设你对两个文件运行了 Unix `spell` 命令，并且该命令打印出了以下拼写错误列表：

```
$ spell sect1 sect2
chmod
ditroff
myfile
thier
writeable
```

通常情况下，`spell` 会标记一些它不认识的技术术语和特殊情况，但它也识别出了两个真正的拼写错误。

由于我们同时检查了两个文件，我们不知道错误出现在哪个文件或文件中的什么位置。虽然有办法查明，而且对于两个文件中的两个错误来说工作量并不大，但你可以很容易想象，对于拼写不好的人或同时校对许多文件的人来说，这项工作会变得多么耗时。

为了让工作更轻松，你可以编写一个包含以下命令的 `ex` 脚本：

```
%s/thier/their/g
%s/writeable/writable/g
wq
```

假设你已将这些行保存到名为 `exscript` 的文件中。该脚本可以在 `vi` 内部使用命令执行：

```
:so exscript
```

或者脚本可以直接从命令行应用于文件。然后你可以按如下方式编辑文件 `sect1` 和 `sect2`：

```
$ ex -s sect1 < exscript
$ ex -s sect2 < exscript
```

`-s`（表示“脚本模式”或“静默模式”）在 `ex` 调用之后，是 POSIX 规定用来告诉编辑器抑制正常终端消息的方式。^(13)

如果脚本比我们这个简单示例长，我们已经节省了相当多的时间。然而，你可能会想是否有什么办法避免对每个要编辑的文件重复此过程。确实，我们可以编写一个 shell 脚本，其中包含——但概括了——`ex` 的调用，使其可用于任意数量的文件。

## Looping in a Shell Script

你可能知道 shell 既是一种编程语言，也是一种命令行解释器。为了对多个文件调用 `ex`，我们使用一种简单的 shell 脚本命令，称为 `for` 循环。`for` 循环允许你对脚本提供的每个参数应用一系列命令。`for` 循环可能是初学者 shell 编程中最有用的部分。即使你不编写其他 shell 程序，也应该记住它。

以下是 `for` 循环的语法：

```
for variable in list
do
    command(s)
done
```

例如：

```
for file in "$@"
do
    ex -s "$file" < exscript
done
```

（`ex` 命令不需要缩进；我们只是为了清晰而缩进了它。）引用 `file` 变量的展开（`$file`）允许脚本即使在文件名中包含空格时也能工作。^(14)

创建此 shell 脚本后，我们将其保存到名为 `correct` 的文件中，并使用命令 `chmod 755 correct` 使其可执行。现在输入以下内容：

```
$ ./correct sect1 sect2
```

`correct` 中的 `for` 循环将每个参数（由 `"$@"` 指定的列表中的每个文件，它代表 *所有参数*）分配给变量 `file`，并对该变量的内容执行 `ex` 脚本。

通过一个输出更可见的例子可能更容易理解 `for` 循环的工作原理。让我们看一个重命名文件的脚本：

```
for file in "$@"
do
    mv "$file" "$file.x"
done
```

假设此脚本位于名为 `move` 的可执行文件中，我们可以这样做：

```
$ ls
ch01 ch02 ch03 move
$ ./move ch??                Just the chapter files
$ ls                         Check the results
ch01.x ch02.x ch03.x move
```

发挥创意，你可以重写脚本以更具体地重命名文件：

```
for nn in "$@"
do
    mv "ch$nn" "sect$nn"
done
```

以这种方式编写脚本时，你将在命令行上指定数字而不是文件名：

```
$ ls
ch01 ch02 ch03 move
$ ./move 01 02 03
$ ls
sect01 sect02 sect03 move
```

`for` 循环不必以 `"$@"`（所有参数）作为要替换的值列表。你也可以指定一个显式列表。例如：

```
for variable in a b c d
```

将 `variable` 依次分配给 *a*、*b*、*c* 和 *d*。或者你可以替换命令的输出。例如：

```
for variable in $(grep -l "Alcuin" *)
```

将 `variable` 依次分配给 `grep` 找到字符串 *Alcuin* 的每个文件的名称。（`grep -l` 打印出内容匹配模式的文件名，而不打印实际匹配的行。）

如果未指定列表：

```
for variable
```

变量将依次分配给每个命令行参数，就像我们在初始示例中所做的那样。四个字符序列 `"$@"` 展开为 `"$1"`、`"$2"`、`"$3"` 等。引号可防止对特殊字符进行进一步解释，并保持包含空格的文件名作为单个项目。

让我们回到主要问题和我们的原始脚本：

```
for file in "$@"
do
    ex -s "$file" < exscript
done
```

似乎同时使用两个脚本——shell 脚本和 `ex` 脚本有点不优雅。事实上，shell 确实提供了一种将编辑脚本包含在 shell 脚本中的方法，我们即将看到。

## Here Documents

在 shell 脚本中，操作符 `<<` 表示将以下行（直到指定字符串）作为命令的输入。（这通常称为 *here document*。）使用此语法，我们可以像这样在 `correct` 中包含我们的编辑命令：

```
for file in "$@"
do

ex -s "$file" << end-of-script
g/thier/s//their/g
g/writeable/s//writable/g
wq
end-of-script

done
```

字符串 `end-of-script` 完全是任意的——它只需要是一个在输入中不会出现的字符串，以便 shell 可以用来识别 here document 何时结束。它也 *必须* 放在行的开头。根据惯例，许多用户使用字符串 `EOF` 或 `E_O_F` 来指定 here document 的结束，以表示文件的结尾。

每种方法都有优点和缺点。如果你想进行一次性的一系列编辑，并且不介意每次重写脚本，here document 提供了有效的方法来完成工作。

然而，将编辑命令写入与 shell 脚本不同的文件更灵活。例如，你可以建立一种约定，总是将编辑命令放在名为 `exscript` 的文件中。然后你只需要编写一次 `correct` 脚本。你可以将其保存在你的个人“工具”目录中（你已将其添加到搜索路径），并在需要时使用它。

## Sorting Text Blocks: A Sample ex Script

假设你想对用自定义版本的 XML 编码的术语表定义文件进行字母排序。每个定义由 `<glossaryitem>` 和 `</glossary​item>` 括起来。每个条目的名称由 `<name>` 和 `</name>` 括起来。术语表文件看起来像这样：

```
<glossaryitem>
<name>TTY_ARGV</name>
<para>The command, specified as an argument vector,
that the TTY subwindow executes.</para>
</glossaryitem>
<glossaryitem>
<name>ICON_IMAGE</name>
<para>Sets or gets the remote image for icon's image.</para>
</glossaryitem>
<glossaryitem>
<name>XV_LABEL</name>
<para>Specifies a frame's header or an icon's label.</para>
</glossaryitem>
<glossaryitem>
<name>SERVER_SYNC</name>
<para>Synchronizes with the server once.
Does not set synchronous mode.</para>
</glossaryitem>
```

你可以通过运行 Unix `sort` 命令对行进行排序来对文件进行字母排序，但你并不真的想逐行排序。你只想对术语表条目进行排序，同时将每个定义——保持不变——与其对应的术语一起移动。事实证明，你可以通过将块连接成一行来将每个文本块视为一个单元。以下是你的 `ex` 脚本的第一个版本：

```
g/^<glossaryitem>/,/^<\/glossaryitem>/j
%!sort
wq
```



每个术语表条目都位于 `<glossaryitem>` 和 `</glossaryitem>` 标签之间。 （注意在结束标签中使用 `\/` 来转义斜杠。） `j` 是 `ex` 命令，用于连接一行（在 `vi` 模式下等价于 `J`）。 因此第一个命令将每个术语表条目连接成一个“行”。 第二个命令随后对文件进行排序，生成的行如下：

```
<glossaryitem> <name>ICON_IMAGE</name> <para>Sets ... </glossaryitem>
<glossaryitem> <name>SERVER_SYNC</name> <para>Synchronizes ... </glossaryitem>
<glossaryitem> <name>TTY_ARGV</name> <para>The command, ... </glossaryitem>
<glossaryitem> <name>XV_LABEL</name> <para>Specifies ... </glossaryitem>
```

这些行现在按术语表条目排序； 不幸的是，每一行中也混杂着 XML 标签和文本（我们使用省略号 \[…\] 来表示省略的文本）。 somehow，你需要插入换行符来“取消连接”这些行。 你可以通过修改你的 `ex` 脚本来做到这一点：在连接它们 *之前* 标记文本块的连接点，然后用换行符替换标记。 以下是扩展的 `ex` 脚本：

```
g/^<glossaryitem>/,/^<\/glossaryitem>/-1s/$/@@/    Append @@ to the end of each line
g/^<glossaryitem>/,/^<\/glossaryitem>/j            Join the entries
%!sort                                             Sort them
%s/@@ /^@/g                                        Break the lines apart
wq                                                 Save the file
```

前三个命令生成的行如下：

```
<glossaryitem>@@ <name>ICON_IMAGE</name>@@ <para>Sets ...</para>@@ </glossaryitem>
<glossaryitem>@@ <name>SERVER_SYNC</name>@@ <para>Synchronizes ...</para>@@ </glossaryitem>
<glossaryitem>@@ <name>TTY_ARGV</name>@@ <para>The command, ...</para>@@ </glossaryitem>
<glossaryitem>@@ <name>XV_LABEL</name>@@ <para>Specifies ...</para>@@ </glossaryitem>
```

请注意每个 `@@` 后面的额外空格。 这些空格来自 `j` 命令，因为它会将每个换行符转换为空格。

第一个命令用 `@@` 标记原始的换行。 你不需要标记块的末尾（在 `</glossaryitem>` 之后），因此第一个命令使用 `-1` 在每个块的末尾向上移动一行。 第四个命令通过将标记（加上额外的空格）替换为换行符来恢复换行。 （你输入换行符的方式是 `CTRL-V CTRL-J`。 这将在稍后讨论。） 现在你的文件已按块排序。

## 微妙的 vi/Vim 差异

在我们刚刚为 `Vim` 的 `ex` 版本编写的脚本中，我们将换行输入为 `CTRL-V CTRL-J`，它显示为 `^@`。 然而，在 `Vim` 中交互编辑时，你必须以不同方式操作； 你将换行输入为 `CTRL-V ENTER`，它显示为 `^M`。

原始的 `vi` 不做区分。 如果你使用的是原始的 `vi`，则在交互式操作和脚本中都将换行输入为 `CTRL-V ENTER`。

## ex 脚本中的注释

你可能希望重用我们刚刚开发的类似脚本，并将其适配到新的情况。 对于这种复杂的脚本，添加注释是明智的，这样别人（甚至是你自己！）更容易重建其工作原理。 在 `ex` 脚本中，执行期间双引号之后的所有内容都会被忽略，因此双引号可以标记注释的开始。 注释可以单独成行。 它们也可以放在任何不将引号解释为命令一部分的命令末尾。 例如，引号对 `map` 命令和 shell 转义有意义，因此你不能用注释结束此类行。

除了使用注释之外，你还可以通过其全名指定命令，这在 `vi` 内部通常会花费太多时间。 最后，如果你添加空格，之前显示的 `ex` 脚本就变成这个更易读的版本：

```
" Mark lines between each <glossaryitem>...</glossaryitem> block
global /^<glossaryitem>/,/^<\/glossaryitem>/-1 substitute /$/@@/
" Now join the blocks into one line
global /^<glossaryitem>/,/^<\/glossaryitem>/ join
" Sort each block--now really one line each
%!sort
" Restore the joined lines to original blocks
%substitute /@@ /^@/g
" Write the file back out and exit
wq
```

### 注

在本书的早期版本中，我们写道：

> 令人惊讶的是，`substitute` 命令在 `ex` 中不起作用，即使其他命令的全名可以工作。

当时这是正确的，至少对于 Solaris 版本的 `vi` 来说是如此。

然而，在测试 “Heirloom” 和 Solaris 11 版本的 `vi` 后，我们发现 `substitute` 作为 `ex` 命令工作得很好。 尽管如此，在脚本中使用完整命令之前，你应该检查本地版本并确保它能工作。

# 超出 ex

如果本讨论激发了你对更强大编辑功能的渴望，你应该知道 Unix 系统提供了比 `ex` 更强大的编辑器： `sed` 流编辑器和 `awk` 数据处理语言。 还有非常流行的 `perl` 编程语言。 有关这些程序的信息，请参阅 O’Reilly 出版的书籍 [*sed 与 awk*](https://www.oreilly.com/library/view/sed-awk/1565922255) 由 Dale Dougherty 和 Arnold Robbins 所著，[*有效的 awk 编程*](https://www.oreilly.com/library/view/effective-awk-programming/9781491904930) 由 Arnold Robbins 所著，[*学习 Perl*](https://www.oreilly.com/library/view/learning-perl-8th/9781492094944) 由 Randal L. Schwarz、brian d foy 和 Tom Phoenix 所著，以及 [*Perl 编程*](https://www.oreilly.com/library/view/programming-perl-4th/9781449321451) 由 Tom Christiansen、brian d foy、Larry Wall 和 Jon Orwant 所著。

# 编辑程序源代码

到目前为止讨论的所有功能，无论是编辑普通文本还是程序源代码都很有用。 然而，还有一些主要对程序员感兴趣的额外功能。 这些包括缩进控制、搜索过程的开始和结束，以及使用 `ctags`。

以下讨论改编自 MKS, Inc.（前身为 Mortice Kern Systems）提供的文档，其为基于 MS-DOS 和 Windows 的系统提供了出色的 `vi` 实现，作为 [MKS 工具包](https://www.ptc.com/en/products/developer-tools/mks-toolkit) 的一部分提供。 经 [MKS 公司](https://www.mkssoftware.com) 许可转载。

## 缩进控制

程序的源代码在许多方面与普通文本不同。 其中最重要的一点是源代码使用缩进的方式。 缩进显示了程序的逻辑结构：语句如何分组到块中。 `vi` 提供了自动缩进控制。 要使用它，请发出以下命令：

```
:set autoindent
```

现在当你用空格或制表符缩进一行时，后续行会自动缩进相同的量。 当你在输入第一行缩进行后按下 `ENTER` 时，光标会移动到下一行并自动缩进与上一行相同的距离。

作为程序员，你会发现这节省了你不少使缩进正确的工作量，尤其是在有多个缩进级别时。

当你启用 `autoindent` 输入代码时，在行首键入 `CTRL-T` 会增加一个缩进级别，而键入 `CTRL-D` 则减少一个级别。

我们应该指出 `CTRL-T` 和 `CTRL-D` 是在 `insert` 模式下键入的，而不像大多数其他命令是在 `command` 模式下键入的。

`CTRL-D` 命令还有两个额外的变体：

`^ ^D`

当你键入 `^ ^D` (`^ CTRL-D`) 时，编辑器会将光标移回行首，但仅限于当前行。 你输入的下一行将从当前的 `autoindent` 级别开始。 这在键入 C/C++ 源代码时输入 C 预处理器命令时特别有用。

`0 ^D`

当你键入 `0 ^D` 时，编辑器会将光标移回行首。 此外，当前的 `autoindent` 级别被重置为零； 你输入的下一行不会自动缩进。



最后，你的终端有 *行擦除* 字符，通常是 `CTRL-U`，它会擦除你迄今为止输入的整行内容。这在 `Vim` 的 GUI 版本中也适用。

在输入源代码时尝试使用 `autoindent` 选项。它简化了获得正确缩进的工作。有时甚至可以帮助你避免错误——例如在 C 源代码中，后退每一级缩进通常都需要一个闭合大括号 (`}`)。

`<<` 和 `>>` 命令在缩进源代码时也很方便。默认情况下，`>>` 将一行向右移动八个空格（即增加八个空格的缩进），而 `<<` 将一行向左移动八个空格。例如，将光标移到一行的开头并按 \> 两次 (`>>`)。你会看到该行向右移动。如果现在按 < 两次 (`<<`)，该行会再次移回。

你可以通过输入数字后跟 `>>` 或 `<<` 来移动多行。例如，将光标移到一个较大段落的第一行并输入 `5>>`。这会移动该段落中的前五行。

默认的移动量是八个空格（向右或向左）。此默认值可以用如下命令更改：

```
:set shiftwidth=4
```

让 `shiftwidth` 与制表位之间的宽度相同是很方便的。默认的制表位也是八个字符位置。

编辑器在进行缩进时会尝试智能处理。通常，当你看到文本每次缩进八个空格时，它实际上是在文件中插入制表符，因为制表符通常扩展为八个空格。这是 Unix 的默认值；在正常输入时按 Tab 以及文件发送到打印机时最为明显——Unix 会用八个空格的制表位将其扩展。

如果你愿意，可以通过更改 `tabstop` 选项来改变制表符在屏幕上的显示方式。例如，如果你有深度缩进的内容，你可能希望使用每四个字符一个制表位的设置，以便行不换行。以下命令可进行此更改：

```
:set tabstop=4
```

你应该同时更改 `shiftwidth`，并将其值设为与制表位相同。

### 警告

请谨慎更改制表位。虽然 `vi` 和 `Vim` 可以使用任意制表位设置显示文件，但许多其他 Unix 程序仍会使用八个字符的制表位来扩展文件中的制表符。

更糟的是：混合使用制表符、空格和不寻常的制表位会使你的文件在编辑器外部查看时完全无法阅读，例如使用 `more` 等分页器查看，或打印时。

在编程并处理制表符和制表位时，你有两个选择：

*   接受八个字符的制表位是 Unix 生活中不可避免的事实，并习惯它。
*   让编辑器在输入时将制表符扩展为空格。你可以通过以下方式实现：

```
:set expandtab
```

当设置了 `expandtab` 时，每次按下 `TAB` 键，编辑器都会输入足够的空格使光标移动到下一个制表位。

如果团队中的每个人都这样做，那么所有的代码格式将保持一致，并且一切会运行良好。这对于 Python 等语言尤其重要，因为代码的缩进表示语句分组，而空格和制表符不匹配会成为灾难的根源。^(15) 顺便提一下，你可以使用 `expand` 实用程序将预先存在的制表符转换为空格。

有时缩进不会按你预期的方式工作，因为你认为的制表符实际上是一个或多个空格。通常，你的屏幕将制表符和空格都显示为空白，使两者无法区分。然而，你可以发出以下命令：

```
:set list
```

这会改变你的显示方式，使制表符显示为控制字符 `^I`，行尾显示为 `$`。这样，你可以发现真正的空格，并可以看到行尾的多余空格。临时等效命令是 `:l` 命令。例如，命令：

```
:5,20 l
```

显示第 5 行到第 20 行，显示制表符和行尾字符。

## 一个特殊的搜索命令

字符 `(`, `[` 和 `{` 都可以称为左括号。当光标停留在这些字符之一上时，按下 `%` 键会将光标从左括号向前移动到对应的右括号——`)`、`]` 或 `}`——同时遵守通常的括号嵌套规则。^(16) 例如，如果你将光标移到以下内容的第一个 `(` 中：

```
if ( cos(a[i]) == sin(b[i]+c[i]) )
{
    printf("cos and sin equal!\n");
}
```

并按下 `%`，你会看到光标跳到该行的末尾的括号处。这就是与左括号匹配的右括号。

同样，如果光标位于其中一个右括号字符上，按下 `%` 会将光标向后移动到对应的左括号字符。例如，将光标移到刚才显示的 `printf` 行之后的闭合大括号上并按下 `%`。

编辑器甚至足够智能，可以为你找到括号字符。如果光标不在括号字符上，按下 `%` 时，它会在当前行向前搜索第一个找到的左括号或右括号字符，然后移动到匹配的括号！例如，将光标放在刚才示例第一行的 `=` 上，`%` 会找到左括号然后移动到右括号。

这个搜索字符不仅可以帮助你在程序中进行长距离的前后移动，还可以让你检查源代码中括号和圆括号的嵌套情况。例如，如果你将光标放在 C 函数开头的第一个 `{` 上，按下 `%` 应该会将你移动到（你认为）结束该函数的 `}`。如果不是正确的那个，某处一定出了问题。如果文件中没有匹配的 `}`，编辑器会发出蜂鸣声提醒你。^(17)

找到匹配括号的另一种技术是打开以下选项：

```
:set showmatch
```

与 `%` 不同，设置 `showmatch`（或其缩写 `sm`）在插入模式下对你有帮助。当你输入 `)` 或 `}` 时，^(18) 光标会短暂地移回匹配的 `(` 或 `{`，然后返回到当前位置。如果不存在匹配项，终端会发出蜂鸣声。如果匹配项只是超出屏幕，编辑器会静默继续。使用默认加载的 `matchparen` 插件，`Vim` 可以高亮显示匹配的圆括号或大括号。

## 使用标签

大型 C 或 C++ 程序的源代码通常分布在多个文件中。有时很难跟踪哪个文件包含哪些函数定义。为简化此事，你可以使用名为 `ctags` 的 Unix 命令以及 `ex` 的 `:tag` 命令。

你在 shell 命令行上发出 `ctags` 命令。其目的是创建信息文件，以便编辑器稍后确定哪些文件定义了哪些函数。默认情况下，此文件称为 `tags`。在编辑会话中，形式如下的命令：

```
:!ctags file.c
```

创建名为 `tags` 的文件在你的当前目录中包含 `file.c` 中定义的函数的信息。如下命令：

```
:!ctags *.c
```

创建描述目录中所有 C 源文件的 `tags` 文件。

### 注释

旧版 Unix 的 `ctags` 处理 C 语言，通常还处理 Pascal 和 Fortran 77。有时甚至处理汇编语言。然而，几乎普遍的是，它们不处理 C++。还有其他版本可用于生成 C++ 以及其他语言和文件类型的 `tags` 文件。有关更多信息，请参阅 “增强标签” 部分。



## 使用标签
现在假设你的 `tags` 文件包含构成 C 程序的所有源文件的信息。另外假设你想要查看或编辑程序中的某个函数，但你不知道该函数位于何处。在 `vi` 模式下，命令为：

```
:tag name
```

它会查看 `tags` 文件以确定包含函数 `name` 定义的文件。然后它读取该文件并将光标定位到名称被定义的那一行。这样，你不需要知道要编辑哪个文件；你只需要决定要编辑哪个函数。

你也可以从 `vi` 的命令模式使用标签功能。将光标放在你想要查找的标识符上，然后输入 `CTRL-\]`。编辑器执行标签查找并移动到定义该标识符的文件。小心光标的放置位置；编辑器使用光标所在位置开始的“word”，而不是包含光标的整个单词。

### 注意
如果你尝试使用 `:tag` 命令读取新文件，而自上次更改以来你尚未保存当前文本，编辑器不会让你进入新文件。你必须先使用 `:w` 命令写出当前文件然后再执行 `:tag`，或者输入：

```
:tag! name
```

来覆盖编辑器不愿丢弃编辑的倾向。

## 增强的标签
Unix `ctags` 可以工作但功能有限。由 Darren Hiebert 编写的“Exuberant `ctags`”程序是 `ctags` 的克隆版，其功能比 Unix `ctags` 强大得多。它产生了一种扩展的 `tags` 文件格式，使标签搜索和匹配过程更加灵活和强大。

不幸的是，Exuberant `ctags` 最后一次更新是在 2009 年。“Universal `ctags`”项目提供了一个维护版本的 `ctags`，其起点是 Exuberant `ctags` 代码库。

本节首先描述 Universal `ctags` 程序，然后描述增强的标签文件格式。

本节还描述了标签栈：使用 `:tag` 或 `^]` 命令保存多个已访问位置的能力。Vim 以及（令人惊讶的是）Solaris 版本的 `vi` 支持标签堆栈。

### Universal ctags
Universal `ctags` 的主页位于 [*https://ctags.io/*](https://ctags.io/)。源代码位于 [*https://github.com/universal-ctags/ctags*](https://github.com/universal-ctags/ctags)。你需要进行网络搜索，看看你的系统包管理器是否允许安装预编译版本，或者是否需要从源代码构建。

以下程序特性的列表改编自 Universal `ctags` 发行版中的 `old-docs/README.exuberant` 文件：
* 它能够为 *all* 类型的 C 和 C++ 语言标签生成标签，包括类名、宏定义、枚举名、枚举器（枚举内的值）、函数（方法）定义、函数（方法）原型/声明、结构成员和类数据成员、结构体名、typedef、联合名和变量。（呼！）
* 它同时支持 C 和 C++ 代码。
* 41 种语言得到支持，包括 C# 和 Java。
* 它在解析代码方面非常健壮，很少会被包含 `#if` 预处理器条件构造的代码所迷惑。
* 它可用于打印出源文件中找到的所选对象的可读列表。
* 它支持生成 GNU Emacs 风格的 `tags` 文件（`etags`）。
* 它可在多种操作系统上运行，包括 Unix、OpenVMS 和 MS-Windows。

Universal `ctags` 生成的 `tags` 文件形式如下所述。

### 新的标签格式
传统上，`tags` 文件有三个以制表符分隔的字段：标签名（通常是标识符）、包含该标签的源文件，以及查找标识符的指示。该指示要么是简单的行号，要么是以斜杠或问号括起来的 `nomagic` 搜索模式。此外，`tags` 文件始终是有序的。

这是 Unix `ctags` 程序生成的格式。事实上，许多版本的 `vi` 允许在搜索模式字段中使用 *any* 命令（这是一个相当大的安全漏洞）。此外，由于一个未记录的实现怪癖，如果行以分号然后是双引号 (`;"`) 结尾，其后所有内容将被忽略。（双引号开始注释，就像在 `.exrc` 文件中一样。）

新格式与传统格式向后兼容。前三个字段相同：标签、文件名和搜索模式。Universal `ctags` 只生成搜索模式，而不是任意命令。特殊属性放在分隔符 `;"` 之后。每个属性之间由制表符分隔，并由两个以冒号分隔的子字段组成。第一个子字段是描述该属性的关键字；第二个是实际值。表 7-1 列出了支持的关键字。

表 7-1. 扩展 ctags 关键字

| 关键字 | 含义 |
|---|---|
| `arity` | 用于函数。定义参数数量。 |
| `class` | 用于 C++ 成员函数和变量。值是类的名称。 |
| `enum` | 用于 `enum` 数据类型中的值。值是 `enum` 类型的名称。 |
| `file` | 用于“静态”标签，即本地于文件。值应为文件名称。如果值为空字符串（仅 `file:`），则理解为与文件名字段相同；这种特殊情况部分是为了紧凑，部分是为了提供一种轻松处理不在当前目录中的 `tags` 文件的方法。文件名字段的值始终相对于 `tags` 文件本身所在的目录。 |
| `function` | 用于本地标签。值是定义它们的函数名称。 |
| `kind` | 值是一个字母，表示标签的词法类型。它可以是 `f` 表示函数，`v` 表示变量，等等。由于默认属性名为 `kind`，单个字母可以表示标签的类型（例如 `f` 表示函数）。 |
| `scope` | 主要用于 C++ 类成员函数。它通常对私有成员为 `private`，对公共成员则省略，这样用户可以将标签搜索限制为仅公共成员。 |
| `struct` | 用于 `struct` 中的字段。值是结构体的名称。 |
| `union` | 用于 `union` 中的字段。值是联合的名称。 |

如果字段不包含冒号，则假定其类型为 `kind`。以下是一些示例：

```
ALREADY_MALLOCED  awk.h   /^#define ALREADY_MALLOCED /;" d
ARRAYMAXED        awk.h   /^  ARRAYMAXED  = 0x4000,;"    e   enum:exp_node::flagvals
array.c   array.c   1;" F
```

`ALREADY_MALLOCED` 是一个 C 宏。`ARRAYMAXED` 是在 `awk.h` 中定义的 C `enum`。第三行有点不同：它是对实际源文件的标签！这是使用 Universal `ctags` 的 `--extras=f` 选项生成的，它允许你给出命令 `:tag array.c`。更有用的是，你可以将光标放在文件名上并使用 `^]` 命令转到该文件（例如，如果你正在编辑 `Makefile` 并希望转到特定的源文件）。

在每个属性的值部分，反斜杠、制表符、回车和换行符分别编码为 `\\`、`\t`、`\r` 和 `\n`。

Universal `tags` 文件可能有一些以 `!_TAG_` 开头的初始标签。这些标签通常排序到文件的前面，有助于识别创建该文件的程序。以下是 Universal `ctags` 生成的内容：



```
!_TAG_FILE_FORMAT       2       /extended format; --format=1 will not append ;" to lines/
!_TAG_FILE_SORTED       1       /0=unsorted, 1=sorted, 2=foldcase/
!_TAG_OUTPUT_EXCMD      mixed   /number, pattern, mixed, or combineV2/
!_TAG_OUTPUT_FILESEP    slash   /slash or backslash/
!_TAG_OUTPUT_MODE       u-ctags /u-ctags or e-ctags/
!_TAG_PATTERN_LENGTH_LIMIT      96      /0 for no limit/
!_TAG_PROC_CWD  /home/arnold/Gnu/gawk/gawk.git/ //
!_TAG_PROGRAM_AUTHOR    Universal Ctags Team    //
!_TAG_PROGRAM_NAME      Universal Ctags /Derived from Exuberant Ctags/
!_TAG_PROGRAM_URL       https://ctags.io/       /official site/
!_TAG_PROGRAM_VERSION   5.9.0   /p5.9.20201206.0/
```

编辑器可以利用这些特殊标签来实现特殊功能。例如，`Vim` 会注意 `!_TAG_FILE_SORTED` 标签，如果文件确实已排序，则使用二分搜索而不是线性搜索来搜索 `tags` 文件。

如果您使用 `tags` 文件，我们建议您获取并安装 Universal `ctags`。

## 标签堆栈

`ex` 命令 `:tag` 和 `vi` 命令模式 `^]` 命令提供了一种基于 `tags` 文件中提供的信息查找标识符的有限方式。`Vim` 和 Solaris `vi` 通过维护标签位置的 `stack` 扩展了此功能。每次发出 `ex` 命令 `:tag` 或使用 `vi` 模式 `^]` 命令时，编辑器会在搜索指定标签之前保存当前位置。然后您可以使用通常的 `vi` 命令 `^T` 或 `ex` 命令返回到已保存的位置。

接下来将介绍 Solaris `vi` 标签堆栈及其示例。Vim 的标签堆栈在 “标签堆栈” 部分进行了描述。

### Solaris vi

令人惊讶的是，Solaris 版本的 `vi` 支持标签堆栈。也许并不令人惊讶，这一功能在 Solaris *ex*(1) 和 *vi*(1) 手册页中完全未记录。为了完整起见，我们在表 7-2、7-3 和 7-4 中总结了 Solaris `vi` 标签堆栈。Solaris `vi` 中的标签堆栈相当简单。^(19)

表 7-2. Solaris `vi` 标签命令：`ex` 命令

| 命令 || 功能 |
| --- | --- |
| `ta`[`g`][`!`] *`tagstring`* || 编辑 `tags` 文件中定义的包含 `tagstring` 的文件。`!` 强制 `vi` 在当前缓冲区已修改但未保存时切换到新文件。 |
| --- | --- |
| `po`[`p`][`!`] || 弹出标签堆栈的一个元素。 |

表 7-3. Solaris `vi` 命令模式标签命令

| 命令 || 功能 |
| --- | --- |
| `^]` || 在 `tags` 文件中查找光标下标识符的位置并移动到该位置。如果启用了标签堆栈，当前位置会自动压入标签堆栈。 |
| --- | --- |
| `^T` || 返回标签堆栈中的上一个位置，即弹出一个元素。 |

表 7-4. Solaris `vi` 标签管理选项

| 选项 || 功能 |
| --- | --- |
| `taglength`, `tl` || 控制要查找的标签的显著字符数。默认值零表示所有字符都显著。 |
| --- | --- |
| `tags` || 值是用于查找标签的文件名列表。默认值是 `"tags /usr/lib/tags"`。 |
| `tagstack` || 设置为 true 时，`vi` 将每个位置堆叠到标签堆栈上。使用 `:set notagstack` 禁用标签堆栈。 |

### Universal ctags and Vim

为了让您感受使用标签堆栈的方法，我们提供了一个使用 Universal `ctags` 和 Vim 的简短示例。

假设您正在处理一个使用 GNU `getopt_long` 函数的程序，并且需要更深入地了解它。

GNU `getopt` 由三个文件组成：`getopt.h`、`getopt.c` 和 `getopt1.c`。

首先，您创建 `tags` 文件，然后开始编辑位于 `main.c` 中的主程序：

```
$ ctags *.[ch]
$ ls
getopt1.c  getopt.c  getopt.h  getopt_int.h  main.c  Makefile  tags
$ vim main.c
```

| 按键 || 结果 |
| --- | --- |
| `/getopt_` || 
```
/* option processing. ready, set, go! */
while ((c = getopt_long(argc, argv, optlist, optab, NULL)) != EOF) {
     switch (c) {
     case 'l':
```
 |
| --- | --- |
|  || 编辑 `main.c` 并移动到 `getopt_long` 的调用处。 |
| `^]` || 
```
int getopt_long (int argc, char *const *argv, const char *options,
             const struct option *long_options, int *opt_index)
{
  return _getopt_internal (argc, argv, options, long_options, opt_index, 0);
}
 
"getopt1.c" 195L, 4750B
```
 |
|  || 对 `getopt_long` 执行标签查找。`Vim` 移动到 `getopt1.c`，将光标放在 `getopt_long` 的定义上。 |

事实证明 `getopt_long` 是 `_getopt_internal` 的“包装”函数。您将光标放在 `_getopt_internal` 上并执行另一次标签搜索：

| 按键 || 结果 |
| --- | --- |
| `3jf_ ^]` || 
```
int _getopt_internal (int argc, char *const *argv, const char *optstring,
                  const struct option *longopts, int *longind, int long_only)
{
  int result;
 
  getopt_data.optind = optind;
  getopt_data.opterr = opterr;
 
  result = _getopt_internal_r (argc, argv, optstring, longopts,
                               longind, long_only, &getopt_data);
 
  optind = getopt_data.optind;
"getopt.c" 1294L, 34298B
```
 |
| --- | --- |
|  || 您现在已移动到 `getopt.c`。要了解有关 `struct` `option` 的更多信息，将光标移动到 `option` 并执行另一次标签搜索。 |
| `jfo; ^]` || 
```
   one).  For long options that have a zero `flag' field, `getopt'
   returns the contents of the `val' field.  */
struct option
{
   const char *name;
   /* has_arg can't be an enum because some compilers complain about
      type mismatches in all the code that assumes it is an int.  */
   int has_arg;
   int *flag;
   int val;
};
 
/* Names for the values of the `has_arg' field of `struct option'.  */
 
"getopt.h" 191L, 6644B
```
 |
|  || 编辑器移动到 `getopt.h` 中 `struct option` 的定义。您现在可以查看解释其用法的注释。 |
| `:tags` || 
```
 # TO tag         FROM line  in file/text
 1  1 getopt_long        29  main.c
 2  1 _getopt_internal   70  getopt1.c
 3  1 option           1185  getopt.c
```
 |
|  || `Vim` 中的 `:tags` 命令显示标签堆栈。 |

输入 `^T` 三次将使您返回到开始时的 `main.c`。标签功能使您在编辑源代码时轻松移动。

^(1) 该数据库的位置因厂商而异。尝试命令 `man terminfo` 以获取有关您特定系统的更多信息。

^(2) `:set all` 的结果在很大程度上取决于您拥有的 `vi` 版本。此特定显示是 Unix `vi` 的典型显示。顺序按列向下排列为字母顺序，忽略任何前导的 `no`。`Vim` 比此处显示的具有 *many* 更多选项。

^(3) 原始版本的 `vi` 会自动读取两个文件（如果存在）。`exrc` 选项关闭了一个潜在的安全漏洞。

^(4) 在计算机上，输入 *carriage return* 意味着按下 ENTER 键。该术语源自打字机，在完成一行后，您使用一个杠杆将纸张向上移动一行并 *return*  carriage（装纸部件）返回到行的开头。这就是 ASCII 字符 LF 和 CR（换行和回车）的由来。

^(5) 当然，总有例外。在此示例中，`Vim` 仅更改当前行。

^(6) `Vim` 提供 `:wn` 来执行此组合操作，但 `:map q :wn^M` 仍然有用。



^(7) 来自 Walter Zintz 所著的“`vi` 高级用户技巧”，刊于 *UnixWorld*，1990 年 4 月。这适用于带括号的文本，而非嵌套的带括号方程。

^(8) 来自“`vi` 高级用户技巧”。

^(9) 功能键 F1 通常是“帮助”键，由终端模拟器保留使用；因此我们的示例使用 F2。

^(10) 你在系统上看到的内容可能与此处显示的转义序列不同。

^(11) 在映射中 `%` 符号必须进行转义，以防止 Vim 将其替换为当前文件名。

^(12) 这有点棘手。`dd` 也会获取行末的换行符，导致 `@g` 在更改当前单词后将光标向下移动一行。要做到完全正确，你必须输入 `"gdf^V^`。呼！

^([13) 传统上，`ex` 为此目的使用单个减号。通常，出于向后兼容性考虑，两者都被接受。

^(14) 文件名中使用空格不是良好做法，但也不罕见。健壮的脚本也应能处理此类文件。

^(15) 空格与制表符之争可能成为宗教性问题。参见 *Silicon Valley* 电视剧中的这段精彩片段：[*https://www.youtube.com/watch?v=SsoOG6ZeyUI*](https://www.youtube.com/watch?v=SsoOG6ZeyUI)。

^(16) 某些版本也使用 `%` 匹配 `<` 和 `>`。

^(17) 请注意，编辑器也会计算引号字符串和注释中的括号，因此 `%` 并非万无一失。

^(18) 在 Vim 中，`showmatch` 也会显示匹配的方括号（`[` 和 `]`）。

^(19) 此信息基于实验得出。YMMV（因人而异）。

# 第二部分。Vim

第二部分描述了最流行的 `vi` 克隆版，名为 Vim（意为“`vi` improved”）。本部分包含以下章节：

* 第 8 章，“Vim（vi Improved）：概述及对 vi 的改进”
* 第 9 章，“图形化 Vim（`gvim`）”
* 第 10 章，“Vim 中的多窗口”
* 第 11 章，“面向程序员的 Vim 增强功能”
* 第 12 章，“Vim 脚本”
* 第 13 章，“Vim 中的其他酷炫功能”
* 第 14 章，“一些 Vim 高级技巧”
