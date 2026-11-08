## 第 17 章. 尾声



如果您读到这里，我们感谢您的耐心和兴趣。

自本书开始以来，我们已经走了很远。我们从基础开始——“什么是文本编辑器和文本编辑？”——并逐步深入了解 `vi` 命令模式以及底层的 `ex` 编辑器，包括正则表达式以及 `vi` 和 `Vim` 底层的强大命令语言。

从那里我们深入探讨了 `Vim` 众多功能中的一部分，这些功能使 `Vim` 比原始的 `vi` 强大至少一个数量级。`Vim` 在编辑普通文本方面表现出色（我们就是用它撰写本书的），但它真正大放异彩作为程序员的编辑器。

最后，我们探讨了 `Vim` 的可扩展性如何让您将其构建成功能齐全的 IDE，以及 `vi` 的命令驱动编辑模型如何在其他工具中找到。

只要您通过键盘管理文本信息，我们认为花时间学习 `Vim` 非常值得。就像盲打比两指打字更高效一样，`Vim` 在文本和程序编辑方面比任何鼠标驱动的 GUI 编辑器都要好上一个量子级别。

享受！

# 第四部分. 附录

第四部分提供了对 `vi` 或 `Vim` 用户可能感兴趣的参考资料。本部分包含附录：

* 附录 A，“`vi`、`ex` 和 `Vim` 编辑器”
* 附录 B，“设置选项”
* 附录 C，“vi 的轻松一面”
* 附录 D，“vi 和 Vim：源代码和构建”

## 附录 A. vi、ex 和 Vim 编辑器

本附录以快速参考格式总结了 `vi` 的标准功能。 在冒号提示符下输入的命令（称为 `ex` 命令，因为它们可以追溯到该编辑器的最初创建）包含在内，以及最受欢迎的 `Vim` 功能。

本附录介绍以下主题：

* 命令行语法
* `vi` 操作回顾
* `vi` 命令
* `vi` 配置
* `ex` 基础
* `ex` 命令的字母顺序摘要

### 命令行语法

启动 `Vim` 会话的三种最常见方式是：

```
vim [options] file
vim [options] -c num file
vim [options] -c /pattern file
```

您可以打开 `file` 进行编辑，可选择在第 `num` 行或匹配 `pattern` 的第一行。如果未指定 `file`，编辑器将以空缓冲区打开。

#### 命令行选项

因为 `vi` 和 `ex` 是同一个程序，所以它们共享相同的选项。然而，有些选项仅适用于其中一个版本的程序。方括号表示可选项目。特定于 `Vim` 的命令行选项以此标记：

`+[num]`

在第 `num` 行开始编辑，如果省略 `num`，则在文件的最后一行开始。

`+/pattern`

在匹配 `pattern` 的第一行开始编辑。^(1)

`+?pattern`

在匹配 `pattern` 的最后一行开始编辑。^(1)

`-b`

以二进制模式编辑文件。 {Vim}

`-c` `command`

启动时运行给定的 `ex` 命令。`vi` 仅允许一个 `-c` 选项；`Vim` 接受多达 10 个。该选项的旧形式 `+command` 仍然受支持。

`--cmd` `command`

类似于 `-c`，但在读取任何配置文件之前执行 `command`。 {Vim}

`-C`

Solaris 10 `vi`：与 `-x` 相同，但假定文件已加密。不在 Solaris 11 `vi` 中。

`Vim`：以 `vi` 兼容模式启动编辑器。

`-d`

以差异模式运行。工作方式类似于 `vimdiff`。 {Vim}

`-D`

用于脚本的调试模式。 {Vim}

`-e`

以 `ex` 运行（行编辑而非全屏模式）。

`-h`

打印帮助信息，然后退出。 {Vim}

`-i` `file`

使用指定的 `file` 而不是默认的 (`~/.viminfo`) 来保存或恢复 `Vim` 的状态。 {Vim}

`-l`

进入 Lisp 模式以运行 Lisp 程序（并非所有版本都支持）。

`-L`

列出因编辑器会话中止或系统崩溃而保存的文件（并非所有版本都支持）。对于 `Vim`，此选项与 `-r` 相同。

`-m`

以关闭 `write` 选项启动编辑器，以便用户无法写入文件。`Vim` 仍允许对缓冲区进行更改，但即使使用 `:w!` 覆盖，也不会允许写入文件。 {Vim}

`-M`

不允许文件中文本被修改。这与 `-m` 类似，但此外还阻止对缓冲区的任何更改。 {Vim}

`-n`

不使用交换文件；仅在内存中记录更改。 {Vim}

`--noplugin`

不加载任何插件。 {Vim}

`-N`

以非 `vi` 兼容模式运行 `Vim`。 {Vim}

`-o[num]`

使用 `num` 个打开的窗口启动 `Vim`。默认情况下为每个文件打开一个窗口。 {Vim}

`-O[num]`

使用 `num` 个打开的窗口启动 `Vim`，在屏幕上水平排列（垂直分割）。 {Vim}

`-r [file]`

恢复模式；在编辑器会话中止或系统崩溃后恢复并继续编辑 `file`。如果没有 `file`，则列出可恢复的文件。

`-R`

以只读模式编辑文件。`Vim` 会在您进行更改时警告您并允许更改发生。如果您尝试保存带更改的文件，`Vim` 会提示您覆盖只读上下文。

`-s`

静默；不显示提示。在运行脚本时很有用。此行为也可以通过旧的 `-` 选项设置。对于 `Vim`，仅在使用 `-e` 时适用。

`-s` `scriptfile`

读取并执行指定 `scriptfile` 中给出的命令，就好像它们是从键盘输入的一样。 {Vim}

`-S` `commandfile`

在加载命令行上指定的任何要编辑的文件后，读取并执行 `commandfile` 中给出的命令。`vim -c 'source commandfile'` 的简写。 {Vim}

`-t` `tag`

编辑包含 `tag` 的文件，并将光标定位在其定义处。

`-T` `type`

设置 `term`（终端类型）选项。此值会覆盖 `$TERM` 环境变量。 {Vim}

`-u` `file`

从指定的配置文件读取配置信息，而不是默认的 `*.vimrc` 配置文件。如果 `file` 参数为 `NONE`，`Vim` 不读取任何配置文件，不加载任何插件，并以兼容模式运行。如果参数为 `NORC`，它不读取任何配置文件，但会加载插件。 {Vim}

`-v`

以全屏模式运行（`vi` 的默认值）。

`--version`

打印版本信息，然后退出。 {Vim}

`-V[num]`

详细模式；打印有关正在设置哪些选项以及正在读取或写入哪些文件的消息。您可以设置详细程度级别以增加或减少接收到的消息数量。默认值为 10，表示高详细程度。 {Vim}

`-w` `rows`

设置窗口大小，使每次显示 `rows` 行；在长距离互联网连接上编辑时很有用。`vi` 的旧版本不允许选项与其参数之间有空格。`Vim` 不支持此选项。

`-W` `scriptfile`

将当前会话中键入的所有命令写入指定的 `scriptfile`。由此创建的文件可与 `-s` 选项一起使用。 {Vim}

`-x`

提示输入一个密钥，该密钥将用于尝试使用 `crypt` 加密或解密文件（并非所有版本都支持）。^(2)

`-y`

无模式 `vi`；仅以插入模式运行 `Vim`，没有命令模式。这与将 `Vim` 作为 `evim` 调用相同。 {Vim}

`-Z`



Start `Vim` in restricted mode. Do not allow shell commands or suspension of the editor. {Vim}

尽管大多数人仅通过在 `vi` 中的使用才知道 `ex` 命令，但该编辑器也作为独立程序存在，可以从 shell 中调用（例如，作为脚本的一部分来编辑文件）。在 `ex` 中，你可以输入 `vi` 或 `visual` 命令来启动 `vi`。同样，在 `vi` 中，你可以输入 `Q` 退出 `vi` 编辑器并进入 `ex`。

你可以通过以下几种方式退出 `ex`：

| `:x` || 退出（保存更改并退出）。 |
| --- | --- |
| `:q!` || 不保存更改而退出。 |
| `:vi` || 进入 `vi` 编辑器。 |

# vi 操作回顾

本节提供以下内容的回顾：

* `vi` 模式
* `vi` 命令语法
* 状态行命令

## 命令模式

打开文件后，你处于命令模式。从命令模式，你可以：

* 调用插入模式
* 发出编辑命令
* 将光标移动到文件中的不同位置
* 调用 `ex` 命令
* 调用 shell
* 保存文件的当前版本
* 退出编辑器

## 插入模式

在插入模式下，你向文件中输入新文本。通常使用 `i` 命令进入插入模式。按下 `ESC` 键可退出插入模式并返回命令模式。进入插入模式的命令完整列表将在后面，在小节 “插入命令” 中提供。

## vi 命令语法

在 `vi` 中，编辑命令的一般形式如下：

```
[n] operator [m] motion
```

基本编辑 `operators` 为：

| `c` || 开始更改。 |
| --- | --- |
| `d` || 开始删除。 |
| `y` || 开始 yank（或复制）。 |

如果当前行是操作的对象，则 `motion` 与操作符相同：`cc`、`dd`、`yy`。否则，编辑操作符作用于由光标移动命令或模式匹配命令指定的对象。例如，`cf.` 更改到下一个句点。`n` 和 `m` 是操作执行的次数，或操作执行的对象数。如果同时指定 `n` 和 `m`，效果为 `n` × `m`。

操作对象可以是以下任何文本块：

`word`

包括字符直到空白字符（空格或制表符）或标点符号。首字母大写的对象是仅识别空白的变体形式。

`sentence`

直到 `.`、`!` 或 `?`，后跟两个空格。Vim 只查找单个后续空格。

`paragraph`

直到下一空白行或由 `para=` 选项定义的 `nroff`/`troff` 段落宏。

`section`

直到由 `sect=` 选项定义的下一个 `nroff`/`troff` 节标题。

`motion`

直到由 motion 指定符指定的字符或其他文本对象，包括模式搜索。

### 示例

| `2cw` || 更改接下来的两个词。 |
| --- | --- |
| `d}` || 删除到下一个段落。 |
| `d^` || 删除回行首。 |
| `5yy` || 复制接下来的五行。 |
| `y]]` || 复制到下一个节。 |
| `cG` || 更改到编辑缓冲区末尾。 |

更多命令和示例可在本附录后面，在小节 “更改和删除文本” 中找到。

### 视觉模式（仅 Vim）

Vim 提供了一个额外的功能，`visual mode`。这允许你突出显示文本块，然后这些文本块成为删除或保存（yanking）等编辑命令的对象。Vim 的图形版本允许你使用鼠标以类似方式突出显示文本。有关更多信息，请参阅前面小节 “视觉模式移动”。

| `v` || 在视觉模式下逐字符选择文本。 |
| --- | --- |
| `V` || 在视觉模式下逐行选择文本。 |
| `CTRL-V` || 在视觉模式下以块形式选择文本。 |

## 状态行命令

大多数命令在输入时不会在屏幕上回显。但是，屏幕底部的状态行用于编辑这些命令：

| `/` || 向前搜索模式。 |
| --- | --- |
| `?` || 向后搜索模式。 |
| `:` || 调用 `ex` 命令。 |
| `!` || 调用一个 Unix 命令，该命令以缓冲区中的对象作为输入并用命令的输出替换它。你在 `!` 之后输入一个 motion 命令来描述应传递给 Unix 命令的内容。命令本身显示在状态行上。 |

输入到状态行的命令必须通过按下 `ENTER` 键来完成。^(3) 此外，错误消息和 `CTRL-G` 命令的输出会显示在状态行上。

# vi 命令

`vi` 在命令模式下提供大量单键命令。Vim 提供额外的多键命令。

## 移动命令

某些版本的 `vi` 不识别扩展键盘键（例如，箭头键、向上翻页、向下翻页、home、insert 和 delete）；有些版本识别。所有版本都识别本节中的键。许多 `vi` 用户更喜欢使用这些键，因为它有助于将手指保持在键盘的基准行上。命令前面的数字重复移动。移动命令也用于操作符之后。操作符作用于移动的文本。

### 字符

| `h`, `j`, `k`, `l` || 左、下、上、右（ ← , ↓ , ↑ , → ） |
| --- | --- |
| `Space bar` || 右 |
| `BACKSPACE` || 左 |
| `CTRL-H` || 左 |
| `CTRL-N` || 下 |
| `CTRL-P` || 上 |

### 文本

| `w`, `b` || 按“word”向前、向后移动（字母、数字和下划线构成单词）。 |
| --- | --- |
| `W`, `B` || 按“Word”向前、向后移动（仅空白分隔项目）。 |
| `e` || 单词末尾。 |
| `E` || Word 末尾。 |
| `ge` || 前一个单词末尾。 {Vim} |
| `gE` || 前一个 Word 末尾。 {Vim} |
| `)`, `(` || 下一句、当前句的开头。 |
| `}`, `{` || 下一段、当前段的开头。 |
| `]]`, `[[` || 下一节、当前节的开头。 |
| `][`, `[]` || 下一节、当前节的末尾。 {Vim} |

### 行

文件中长行可能在屏幕上显示为多行。它们从一个屏幕行换行到下一个屏幕行。虽然大多数命令按文件中定义的行工作，但少数命令按它们在屏幕上显示的行工作。Vim 选项 `wrap` 允许你控制长行的显示方式。

| `0`, `$` || 当前行的第一个、最后一个位置。 |
| --- | --- |
| `^`, `_` || 当前行第一个非空白字符。 |
| `+`, `-` || 下一行、上一行的第一个非空白字符。 |
| `ENTER` || 下一行的第一个非空白字符。 |
| `num` `|` || 当前行的第 `num` 列。 |
| `g0`, `g$` || 屏幕行第一个、最后一个位置。 {Vim} |
| `g^` || 屏幕行第一个非空白字符。 {Vim} |
| `gm` || 屏幕行中间。 {Vim} |
| `gk`, `gj` || 向上、向下移动一行屏幕。^(a) {Vim} |
| `H` || 屏幕顶行（Home 位置）。 |
| `M` || 屏幕中间行。 |
| `L` || 屏幕最后一行。 |
| `num` `H` || 顶行之后第 `num` 行。 |
| `num` `L` || 最后一行之前第 `num` 行。 |
| ^(a) 这些是冗余的，因为去掉 `g` 后结果相同。 |
| --- |

### 屏幕

| `CTRL-F`, `CTRL-B` || 向前、向后滚动一屏。 |
| --- | --- |
| `CTRL-D`, `CTRL-U` || 向下、向上滚动半屏。 |




# Vim 命令

### 滚动和重绘
| `CTRL-E`, `CTRL-Y` || 在屏幕底部、顶部显示多一行。如果可能，Vim 会保持光标在同一行——例如，如果光标在第 50 行并且你使用此移动，光标会向上或向下移动以保持在第 50 行。 |
| `z ENTER` || 将光标所在行重新定位到屏幕顶部。 |
| `z.` || 将光标所在行重新定位到屏幕中间。 |
| `z-` || 将光标所在行重新定位到屏幕底部。 |
| `CTRL-L` || 重绘屏幕（不滚动）。 |
| `CTRL-R` || `vi`: 重绘屏幕（不滚动）。 |
|  || Vim: 重做上一次撤销的更改。 |

### 屏幕内
| `H` || 移动到首行——屏幕上顶行第一个字符。 |
| --- | --- |
| `M` || 移动到屏幕中间行第一个字符。 |
| `L` || 移动到屏幕最后一行第一个字符。 |
| `n` `H` || 移动到距离顶行 `n` 行下方行的第一个字符。 |
| `n` `L` || 移动到距离最后一行 `n` 行上方行的第一个字符。 |

### 搜索
| `/pattern` || 向前搜索 `pattern`。以 `ENTER` 结束。 |
| --- | --- |
| `/pattern/+ num` || 在 `pattern` 之后前往第 `num` 行。向前搜索 `pattern`。 |
| `/pattern/- num` || 在 `pattern` 之前前往第 `num` 行。向前搜索 `pattern`。 |
| `?pattern` || 向后搜索 `pattern`。以 `ENTER` 结束。 |
| `?pattern?+ num` || 在 `pattern` 之后前往第 `num` 行。向后搜索 `pattern`。 |
| `?pattern?- num` || 在 `pattern` 之前前往第 `num` 行。向后搜索 `pattern`。 |
| `:noh` || 暂停搜索高亮直到下一次搜索。{Vim} |
| `n` || 重复上一次搜索。 |
| `N` || 反向重复上一次搜索。 |
| `/` || 向前重复上一次搜索。以 `ENTER` 结束。 |
| `?` || 向后重复上一次搜索。以 `ENTER` 结束。 |
| `*` || 向前搜索光标下的单词。仅匹配完全单词。{Vim} |
| `#` || 向后搜索光标下的单词。仅匹配完全单词。{Vim} |
| `g*` || 向前搜索光标下的单词。匹配该单词的字符即使嵌入在更长的单词中。{Vim} |
| `g#` || 向后搜索光标下的单词。匹配该单词的字符即使嵌入在更长的单词中。{Vim} |
| `%` || 查找当前括号、 brace 或方括号的匹配项。 |
| `f x` || 将光标向前移动到当前行上的 `x`。 |
| `F x` || 将光标向后移动到当前行上的 `x`。 |
| `t x` || 将光标向前移动到当前行上 `x` 之前的字符。 |
| `T x` || 将光标向后移动到当前行上 `x` 之后的字符。 |
| `,` || 反转最近一次 `f`、`F`、`t` 或 `T` 的搜索方向。 |
| `;` || 重复最近一次 `f`、`F`、`t` 或 `T`。 |

### 行号
| `CTRL-G` || 显示当前行号。 |
| --- | --- |
| `gg` || 移动到文件中的第一行。{Vim} |
| `num G` || 移动到行号 `num`。 |
| `G` || 移动到文件中的最后一行。 |
| `: num` || 移动到行号 `num`。 |

### 标记
| `m x` || 在当前位置放置标记 `x`。 |
| `` `x `` || （反引号。）将光标移动到标记 `x`。 |
| `'x` || （撇号。）移动到包含 `x` 的行的开头。 |





| ``` `` ``` || (反引号。) 返回最近一次跳转之前的位置。 |
| `''` || (撇号。) 与前者类似，但返回到行首。 |
| `'"` || (撇号引号。) 移动到上次编辑文件时的位置。 {Vim} |
| `` `[ ``, `` `] `` || (反引号括号。) 移动到上一次文本操作的开头/结尾。 {Vim} |
| `'[`, `']` || (撇号括号。) 与前者类似，但返回操作发生所在行的行首。 {Vim} |
| `` `. `` || (反引号句点。) 移动到文件中的最后一次更改。 {Vim} |
| `'.` || (撇号句点。) 与前者类似，但返回到行首。 {Vim} |
| `'0` || (撇号零。) 移动到上次退出 Vim 时所在的位置。 {Vim} |
| `:marks` || 列出活动标记。 {Vim} |

## Insert Commands

| `a` || 在光标后追加。 |
| --- | --- |
| `A` || 追加到行尾。 |
| `c` || 开始更改操作。 |
| `C` || 更改至行尾。 |
| `gi` || 在上次编辑文件的位置插入。 {Vim} |
| `gI` || 在行首插入。 {Vim} |
| `i` || 在光标前插入。 |
| `I` || 在行首插入。 |
| `o` || 在光标下方打开一行。 |
| `O` || 在光标上方打开一行。 |
| `R` || 开始覆盖文本。 |
| `s` || 替换一个字符。 |
| `S` || 替换整行。 |
| `ESC` || 终止插入模式。 |

以下命令在插入模式下工作：

| `BACKSPACE` || 删除前一个字符。 |
| --- | --- |
| `DELETE` || 删除当前字符。 |
| `TAB` || 插入制表符。 |
| `CTRL-A` || 重复上一次插入。 {Vim} |
| `CTRL-D` || 将行左移至上一个 shiftwidth。 |
| `^ CTRL-D` || 将光标移至行首，但仅限一行。 |
| `0 CTRL-D` || 将光标移至行首，并将自动缩进级别重置为零。 |
| `CTRL-E` || 插入光标正下方找到的字符。 {Vim} |
| `CTRL-H` || 删除前一个字符（与退格相同）。 |
| `CTRL-I` || 插入制表符。 |
| `CTRL-K` || 开始插入多键字符。 {Vim} |
| `CTRL-N` || 插入光标左侧模式的下一个补全。 {Vim} |
| `CTRL-P` || 插入光标左侧模式的上一个补全。 {Vim} |
| `CTRL-T` || 将行右移至下一个 shiftwidth。 {Vim} |
| `CTRL-U` || 删除当前行。 |
| `CTRL-V` || 原样插入下一个字符。 |
| `CTRL-W` || 删除前一个词。 |
| `CTRL-Y` || 插入光标正上方找到的字符。 {Vim} |
| `CTRL-\[` || (ESC) 终止插入模式。 |

上表中列出的某些控制字符由 `stty` 设置。 你的终端模拟器的设置可能不同。

## Edit Commands

请注意 `c`、`d` 和 `y` 是基本编辑操作符。

### Changing and deleting text

以下列表并非详尽，但说明了最常见的操作：

| `cw` || 更改一个词。 |
| --- | --- |
| `cc` || 更改行。 |
| `c$` || 从当前位置更改文本至行尾。 |
| `C` || 与 `c$` 相同。 |
| `dd` || 删除当前行。 |
| `num` `dd` || 删除 `num` 行。 |
| `d$` || 从当前位置删除文本至行尾。 |
| `D` || 与 `d$` 相同。 |
| `dw` || 删除一个词。 |
| `d}` || 删除至下一个段落。 |
| `d^` || 删除回行首。 |
| `d/` `pattern` || 删除至 `pattern` 的第一次出现处。 |
| `dn` || 删除至 `pattern` 的下一次出现处。 |
| `df` `x` || 删除当前行上至包含 `x` 的位置。 |
| `dt` `x` || 删除当前行上至（不包括）`x` 的位置。 |
| `dL` || 删除至屏幕上的最后一行。 |
| `dG` || 删除至文件末尾。 |
| `gqap` || 将当前段落重新格式化为 `textwidth`。 {Vim} |
| `g~w` || 切换词的大小写。 {Vim} |
| `guw` || 将词改为小写。 {Vim} |
| `gUw` || 将词改为大写。 {Vim} |
| `p` || 在光标后插入最后一次删除或 yank 的文本。 |
| `P` || 在光标前插入最后一次删除或 yank 的文本。 |
| `gp` || 与 `p` 相同，但将光标留在插入文本的末尾。 {Vim} |
| `gP` || 与 `P` 相同，但将光标留在插入文本的末尾。 {Vim} |
| `]p` || 与 `p` 相同，但匹配当前缩进。 {Vim} |
| `[p` || 与 `P` 相同，但匹配当前缩进。 {Vim} |
| `r` `x` || 用 `x` 替换字符。 |
| `R` `text` || 用新的 `text` 替换（覆盖），从光标开始。 `ESC` 结束替换模式。 |
| `s` || 替换一个字符。 |
| `4s` || 替换四个字符。 |
| `S` || 替换整行。 |
| `u` || 撤销上一次更改。 |
| `CTRL-R` || 重做上一次更改。 {Vim} |
| `U` || 恢复当前行。 |
| `x` || 删除当前光标位置的字符。 |
| `X` || 向后删除一个字符。 |
| `5X` || 删除前五个字符。 |
| `.` || 重复上一次更改。 |
| `~` || 反转大小写并将光标右移。 {`vi`, and Vim with option `notildeop`} |
| `~w` || 反转词的大小写。 {Vim with option `tildeop`} |
| `~~` || 反转行的大小写。 {Vim with option `tildeop`} |
| `CTRL-A` || 增加光标下的数字。 {Vim} |
| `CTRL-X` || 减少光标下的数字。 {Vim} |

### Copying and moving

寄存器名称为字母 `a`–`z`。 大写名称将文本追加到对应的寄存器：

| `Y` || 复制当前行。 |
| --- | --- |
| `yy` || 复制当前行。 |
| `"` `x` `yy` || 复制当前行到寄存器 `x`。 |
| `ye` || 复制文本至词尾。 |
| `yw` || 与 `ye` 类似，但包含词后的空白。 |
| `y$` || 复制行的剩余部分。 |
| `"` `x` `dd` || 删除当前行到寄存器 `x`。 |
| `"` `x` `d` `motion` || 删除到寄存器 `x`。 |
| `"` `x` `p` || 粘贴寄存器 `x` 的内容。 |
| `y]]` || 复制至下一个节标题。 |
| `J` || 将当前行与下一行连接。 |
| `gJ` || 与 `J` 相同，但不插入空格。 {Vim} |
| `:j` || 与 `J` 相同。 |
| `:j!` || 与 `gJ` 相同。 |

## Saving and Exiting

写入文件意味着用编辑缓冲区中的当前文本覆盖文件。

| `ZZ` || 退出 `vi`，仅在有更改时写入文件。 |
| --- | --- |
| `:x` || 与 `ZZ` 相同。 |
| `:wq` || 写入文件并退出。 |
| `:w` || 写入文件。 |
| `:w` `file` || 保存一份副本到 `file`。 |
| `:` `n` `,` `m` `w` `file` || 将第 `n` 到 `m` 行写入新的 `file`。 |
| `:` `n` `,` `m` `w >>` `file` || 将第 `n` 到 `m` 行追加到已有的 `file`。 |
| `:w!` || 写入文件（覆盖保护）。 |
| `:w!` `file` || 用当前文本覆盖 `file`。 |
| `:w %.new` || 将当前缓冲区命名为 `file` 的内容写入为 `file`.`new`。 |
| `:q` || 退出编辑器（如果有更改则失败）。 |
| `:q!` || 退出编辑器（丢弃编辑）。 |
| `Q` || 退出 `vi` 并调用 `ex`。 |
| `:vi` || 在 `Q` 命令后返回 `vi`。 |
| `%` || 在编辑命令中被当前文件名替换。 |
| `#` || 在编辑命令中被备用文件名替换。 |

## Accessing Multiple Files

| `:e` `file` || 编辑另一个 `file`；当前文件变为备用文件，由 `#` 命名。 |
| --- | --- |
| `:e!` || 返回当前文件在上次写入时的版本。 |
| `:e +` `file` || 从 `file` 的末尾开始编辑。 |
| `:e +` `num file` || 在第 `num` 行打开 `file`。 |
| `:e #` || 打开到备用文件中的上一个位置。 |
| `:ta` `tag` || 编辑位于 `tag` 的文件。 |
| `:n` || 编辑文件列表中的下一个文件。 |
| `:n!` || 强制编辑下一个文件。 |
| `:n` `files` || 指定新的 `files` 列表。 |
| `:rewind` || 编辑参数列表中的第一个文件。 |
| `CTRL-G` || 显示当前文件和行号。 |
| `:args` || 显示正在编辑的文件列表。 |
| `:prev` || 编辑文件列表中的上一个文件。 {Vim} |
| `:last` || 编辑文件列表中的最后一个文件。 {Vim} |

## Window Commands (Vim)

下表列出了在 Vim 中控制窗口的常用命令。另请参阅 “ex 命令字母顺序摘要” 部分中的 `split`、`vsplit` 和 `resize` 命令。为简洁起见，控制字符在以下列表中以 `^` 标记。

### Note

所有单字符键均为小写。大写键以 “SHIFT-” 前缀表示。

| `:new` || 打开一个新窗口。 |
| --- | --- |
| `:new` `file` || 在新窗口中打开 `file`。 |
| `:sp`[`lit`] [`file`] || 分割当前窗口。 使用 `file`，在新窗口中编辑该文件。 |
| `:sv`[`split`] [`file`] || 与 `:sp` 相同，但使新窗口只读。 |
| `:sn`[`ext`] [`file`] || 在新窗口中编辑文件列表中的下一个文件。 |
| `:vsp`[`lit`] [`file`] || 与 `:sp` 类似，但垂直分割而非水平分割。 |
| `:clo`[`se`] || 关闭当前窗口。 |
| `:hid`[`e`] || 隐藏当前窗口，除非它是唯一可见的窗口。 |
| `:on`[`ly`] || 使当前窗口成为唯一可见的窗口。 |
| `:res`[`ize`] `num` || 将窗口调整为 `num` 行。 |
| `:wa`[`ll`] || 将所有已更改的缓冲区写入其文件。 |
| `:qa`[`ll`] || 关闭所有缓冲区并退出。 |
| `CTRL-W S` || 与 `:sp` 相同。 |
| `CTRL-W N` || 与 `:new` 相同。 |
| `CTRL-W ^` || 用备用（先前编辑的）文件打开新窗口。 |
| `CTRL-W C` || 与 `:clo` 相同。 |
| `CTRL-W O` || 与 `:only` 相同。 |
| `CTRL-W J` || 将光标移动到下一个窗口。 |
| `CTRL-W K` || 将光标移动到上一个窗口。 |
| `CTRL-W P` || 将光标移动到上一个窗口。 |
| `CTRL-W H` / `CTRL-W L` || 将光标移动到屏幕左侧/右侧的窗口。 |
| `CTRL-W T` / `CTRL-W B` || 将光标移动到屏幕顶部/底部的窗口。 |
| `CTRL-W SHIFT-K` / `CTRL-W SHIFT-B` || 将当前窗口移动到屏幕顶部/底部。 |
| `CTRL-W SHIFT-H` / `CTRL-W SHIFT-L` || 将当前窗口移动到屏幕最左侧/右侧。 |
| `CTRL-W R` / `CTRL-W SHIFT-R` || 旋转窗口向下/向上。 |
| `CTRL-W +` / `CTRL-W -` || 增加/减少当前窗口大小。 |
| `CTRL-W =` || 使所有窗口高度相同。 |

## Interacting with the System

| `:r` `file` || 在光标后读取 `file` 的内容。 |
| --- | --- |
| `:r !` `command` || 在当前行后读取 `command` 的输出。 |
| `:` `num` `r !` `command` || 与前者类似，但放置在第 `num` 行之后（使用零表示文件顶部）。 |
| `:!` `command` || 运行 `command`，然后返回。 |
| `!` `motion command` || 将 `motion` 覆盖的文本发送到 `command`；用输出替换。 |
| `:` `n` `,` `m` `!` `command` || 将第 `n`–`m` 行发送到 `command`；用输出替换。 |
| `num` `!!` `command` || 将 `num` 行发送到 `command`；用输出替换。 |
| `:!!` || 重复上一次系统命令。 |
| `:sh` || 创建子 shell；使用 `EOF` 返回编辑器。 |
| `CTRL-Z` || 挂起编辑器，使用 `fg` 恢复。这会使 `gvim` 图标化。 |
| `:so` `file` || 从 `file` 读取并执行 `ex` 命令。 |

## Macros

| `:ab` `in out` || 在插入模式中使用 `in` 作为 `out` 的缩写。 |
| --- | --- |
| `:unab` `in` || 移除 `in` 的缩写。 |
| `:ab` || 列出缩写。 |
| `:map string sequence` || 将字符 `string` 映射为命令的 `sequence`。 使用 `#1`、`#2` 等表示功能键。 |
| `:unmap` `string` || 移除字符 `string` 的映射。 |
| `:map` || 列出已映射的字符字符串。 |
| `:map! string sequence` || 将字符 `string` 映射到输入模式 `sequence`。 |
| `:unmap!` `string` || 移除字符 `string` 的输入模式映射（你可能需要使用 `CTRL-V` 对字符进行引用）。 |
| `:map!` || 列出为输入模式映射的字符字符串。 |
| `q` `x` || 将键入的字符记录到字母 `x` 指定的寄存器中。如果字母为大写，则追加到寄存器。 {Vim} |
| `q` || 停止录制。 {Vim} |
| `@` `x` || 执行字母 `x` 指定的寄存器。 |
| `@@` || 执行上一次寄存器命令。 |

在 `vi` 中，以下字符在命令模式下未使用，可映射为用户自定义命令：

*Letters*
: 
`g`, `K`, `q`, `V`, and `v`

*Control keys*
: 
`CTRL-A`, `CTRL-K`, `CTRL-O`, `CTRL-W`, `CTRL-X`, `CTRL-\_`, and `CTRL-\\`

*Symbols*
: 
`_`, `*`, `\`, `=`, and `#`

### Tip

如果设置了 Lisp 模式，`vi` 会使用 `=`。 不同版本的 `vi` 可能使用这些字符中的一些，因此在使用前请先测试。

Vim 不使用 `CTRL-K`、`CTRL-\_` 或 `CTRL-\\`。 有关更多映射功能，请参阅 Vim 中的 `:help noremap`。

## Miscellaneous Commands

| `<` || 将以下 motion 命令描述的文本左移一个 shiftwidth。 {Vim} |
| --- | --- |
| `>` || 将以下 motion 命令描述的文本右移一个 shiftwidth。 {Vim} |
| `<<` || 将行左移一个 shiftwidth（默认为八个空格）。 |
| `>>` || 将行右移一个 shiftwidth（默认为八个空格）。 |
| `>}` || 右移至段落末尾。 |
| `<%` || 左移至匹配的括号、花括号或方括号。（光标必须位于匹配的符号上。） |
| `count` `==` || 以 C 风格缩进 `count` 行，或使用 `equalprg` 选项指定的程序。 {Vim} |
| `g` || 在 Vim 中启动多个多字符命令。 |
| `K` || 在手册页中查找光标下的词（或通过 `keywordprg` 定义的程序）。 {Vim} |
| `CTRL-O` || 返回上一次跳转。 {Vim} |
| `CTRL-Q` || 与 `CTRL-V` 相同。 {Vim}（在某些终端上，恢复数据流。） |
| `CTRL-T` || 返回标签栈中的上一个位置。 {Solaris `vi`, Vim.} |
| `CTRL-\]` || 对光标下的文本执行标签查找。 |
| `CTRL-\\` || 进入 `ex` 行编辑模式。 |
| `CTRL-^` || 返回先前编辑的文件。 |

# vi Configuration

本节描述以下内容：

*   `:set` 命令
*   示例 `*.exrc` 文件

## The :set Command

`:set` 命令允许你指定更改编辑环境特征的选项。 选项可以放在 `~/.exrc` 或 `~/.vimrc` 文件中，或在编辑会话期间设置。

如果命令放在 `*.exrc` 中，则无需键入冒号：

| `:set` `x` || 启用布尔选项 `x`；显示其他选项的值。 |
| --- | --- |
| `:set no` `x` || 禁用选项 `x`。 `no` 与 `x` 之间不要加空格。 |
| `:set` `x` `=` `value` || 为选项 `x` 指定 `value`。 |
| `:set` || 显示已更改的选项。 |
| `:set all` || 显示所有选项。 |
| `:set` `x` `?` || 显示选项 `x` 的值。 |

附录 B，“设置选项”⁠，提供了“ Heirloom ”和 Solaris 版本的 `vi` 以及 Vim 的 `:set` 选项表。 请参阅该附录了解更多信息。

## Example .exrc File

在 `ex` 脚本文件中，注释以双引号字符开头。 以下代码行是自定义 `*.exrc` 文件的示例：

```
```




```
set nowrapscan                    " Searches don't wrap at end of file
set wrapmargin=7                  " Wrap text at 7 columns from right margin
set sections=SeAhBhChDh nomesg    " Set troff macros, disallow message
map q :w^M:n^M                    " Alias to move to next file
map v dwElp                       " Move a word
ab ORA O'Reilly Media, Inc.       " Input shortcut
```

### 提示

`q` 别名对 Vim 来说不是必需的，因为它有 `:wn` 命令。`v` 别名会隐藏 Vim 命令 `v`，该命令进入逐字符视觉模式操作。

# ex 基础

`ex` 行编辑器是屏幕编辑器 `vi` 的基础。`ex` 中的命令作用于当前行或文件中一行的范围。通常，你在 `vi` 内部使用 `ex`。在 `vi` 中，`ex` 命令前面加上冒号，并在按下 ENTER 时执行。

你也可以单独从命令行调用 `ex`——就像调用 `vi` 一样。（你可以通过这种方式执行 `ex` 脚本。）或者你可以使用 `vi` 命令 `Q` 进入 `ex`。

## ex 命令语法

要从 `vi` 输入 `ex` 命令，输入：

```
:[address] command [options]
```

初始的 `:` 表示一个 `ex` 命令。随着你输入命令，它会回显在状态行上。通过按下 ENTER 键执行命令。`Address` 是 `command` 的对象行号或行范围。`Options` 和 `addresses` 稍后会描述。`ex` 命令在章节 “ex 命令字母顺序摘要” 中描述。

你可以通过以下几种方式退出 `ex`：

| `:x` | 退出（保存更改并退出）。 |
| `:q!` | 不保存更改而退出。 |
| `:vi` | 在当前文件上切换到 `vi` 编辑器。 |

## 地址

如果没有给出地址，则当前行为命令的对象。如果地址指定了一行范围，格式为：

```
x,y
```

其中 `x` 和 `y` 是第一和最后被寻址的行（`x` 必须在缓冲区中位于 `y` 之前）。`x` 和 `y` 各自可以是行号或符号。使用 `;` 代替 `,` 会在解释 `y` 之前将当前行设置为 `x`。记号 `1,$` 地址文件中的所有行，`%` 也是如此。

## 地址符号

| `1,$` | 文件中的所有行。 |
| `x,y` | 行 `x` 到 `y`。 |
| `x;y` | 行 `x` 到 `y`，在计算 `y` 之前将当前行重置为 `x`。 |
| `0` | 文件顶部。 |
| `.` | 当前行。 |
| `num` | 绝对行号 `num`。 |
| `$` | 最后一行。 |
| `%` | 所有行；与 `1,$` 相同。 |
| `x-n` | `x` 之前的 `n` 行。 |
| `x+n` | `x` 之后的 `n` 行。 |
| `-[num]` | 前一个或 `num` 行。 |
| `+[num]` | 后一个或 `num` 行。 |
| `'x` | （撇号。）用 `x` 标记的行。 |
| `''` | （撇号撇号。）上一个标记。 |
| `/pattern/` | 向前到匹配 `pattern` 的行。 |
| `?pattern?` | 向后到匹配 `pattern` 的行。 |

有关使用模式的更多信息，请参阅 第 6 章，“全局替换”。

## 选项

`!`:
表示命令的变体形式，覆盖正常行为。`!` 必须紧跟在命令之后。

`count`:
命令重复的次数。与 `vi` 命令不同，`count` 不能置于命令之前，因为 `ex` 命令前面的数字被视为行地址。例如，`d3` 删除从当前行开始的三个行；`3d` 删除第 3 行。

`file`:
受命令影响的文件名。`%` 代表当前文件；`#` 代表上一个文件。

# ex 命令字母顺序摘要

`ex` 命令可以通过指定任何唯一缩写来输入。在以下参考条目列表中，全名作为参考条目的标题显示，其下方的语法行显示最短可能的缩写。示例假定是从 `vi` 输入的，因此它们包含 `:` 提示符。

### abbreviate

```
ab [string text]
```

当输入时定义 `string` 以被翻译为 `text`。如果未指定 `string` 和 `text`，则列出所有当前缩写。

#### 示例

注意：当你输入 `^V` 后跟 ENTER 时会出现 `^M`。

```
:ab ora O'Reilly Media, Inc.
:ab id Name:^MRank:^MPhone:
```

* * *

### append

```
[address] a[!]
text
.
```

在指定的 `address` 处追加新的 `text`，如果未指定则在当前地址处追加。添加 `!` 以切换输入期间使用的 `autoindent` 设置。也就是说，如果 `autoindent` 已启用，`!` 会禁用它。在输入命令后输入新文本。通过输入仅包含一个句点的行来终止新文本的输入。

#### 示例

```
:a                     Begin appending to the current line
Append this line
and this line too.
.                      Terminate input of text to append
```

* * *

### args

```
ar
args file ...
```

打印参数列表的成员（命令行上命名的文件），当前参数（正在编辑的文件）用方括号 (`[ ]`) 打印。
第二个语法是 Vim 的，允许你重置要编辑的文件列表。

* * *

### bdelete

```
[num] bd[!] [num]
```

卸载缓冲区 `num` 并将其从缓冲区列表中移除。添加 `!` 以强制移除未保存的缓冲区。缓冲区也可以通过文件名指定。如果未指定缓冲区，则移除当前缓冲区。{Vim}

* * *

### buffer

```
[num] b[!] [num]
```

开始编辑缓冲区列表中的缓冲区 `num`。添加 `!` 以强制从未保存的缓冲区切换。缓冲区也可以通过文件名指定。如果未指定缓冲区，则继续编辑当前缓冲区。{Vim}

* * *

### buffers

```
buffers[!]
```

打印缓冲区列表的成员。某些缓冲区（例如已删除的缓冲区）未列出。添加 `!` 以显示未列出的缓冲区。`ls` 是此命令的另一个缩写。{Vim}

* * *

### cd

```
cd dir
chdir dir
```

将编辑器的当前目录更改为 `dir`。

* * *

### center

```
[address] ce [width]
```

将行居中于指定的 `width` 内。如果未指定 `width`，则使用 `textwidth`。{Vim}

* * *

### change

```
[address] c[!]
text
.
```

用 `text` 替换（更改）指定的行。添加 `!` 以在输入 `text` 期间切换 `autoindent` 设置。通过输入仅包含一个句点的行来终止输入。

* * *

### close

```
clo[!]
```

关闭当前窗口，除非它是最后一个窗口。如果窗口中的缓冲区未在另一个窗口中打开，则将其从内存中卸载。此命令不会关闭具有未保存更改的缓冲区，但你可以添加 `!` 以隐藏它。{Vim}

* * *

### copy

```
[address] co destination
```



## copy

将 `address` 中包含的行复制到指定的 `destination` 地址。命令 `t`（“to”的缩写）是 `copy` 的同义词。

### 示例

```
:1,10 co 50            Copy first 10 lines to just after line 50
```

## cquit

```
cq[!]
```

使用错误代码退出 Vim。这在 Bash 的“为命令行调用外部编辑器”功能中很有用，当你不希望 Bash 执行编辑缓冲区中的文本时。{Vim}

## delete

```
[address] d [register] [count]
```

删除 `address` 中包含的行。如果指定了 `register`，则将文本保存或追加到指定的寄存器中。寄存器名称为小写字母 `a`–`z`。大写名称将文本追加到对应的寄存器中。如果指定了 `count`，则删除该数量的行。

### 示例

```
:/Part I/,/Part II/-1d       Delete to the line above “Part II”
:/main/+d                    Delete the line below “main”
:.,$d x                      Delete from this line to the last line into register x
```

## edit

```
e[!] [+num] [filename]
```

开始编辑 `filename`。如果未给出 `filename`，则使用当前文件的一个副本重新开始。添加 `!` 即使当前文件自上次更改以来未保存也可以编辑新文件。使用 `+` `num` 参数，从第 `num` 行开始编辑。或者，`num` 可以是 `/` `pattern` 形式的模式。

### 示例

```
:e file                Edit file in the current editing buffer
:e +/^Index #          Edit the alternate file at the first line matching the given pattern
:e!                    Start over again on the current file
```

## file

```
f [filename]
```

将当前缓冲区的名称更改为 `filename`。下次写入缓冲区时，它将被写入文件 `filename`。更改名称时，缓冲区的“未编辑”标志将被设置，以表示你正在编辑一个不存在的文件。如果新文件名与磁盘上已存在的文件相同，你需要使用 `:w!` 来覆盖现有文件。指定文件名时，可使用 `%` 字符表示当前文件名。可以使用 `#` 表示备用文件名。如果未指定 `filename`，则打印当前文件名和缓冲区的状态。

### 示例

```
:f %.new
```

## fold

```
address fo
```

折叠 `address` 指定的行。折叠会将屏幕上的几行折叠成一行，日后可以展开。它不会影响文件的文本。{Vim}

## foldclose

```
[address] foldc[!]
```

关闭指定 `address` 中的折叠，如果未指定则在当前地址处关闭。添加 `!` 可关闭多于一个层级的折叠。{Vim}

## foldopen

```
[address] foldo[!]
```

在指定 `address` 中打开折叠，如果未指定则在当前地址处打开。添加 `!` 可打开多于一个层级的折叠。{Vim}

## global

```
[address] g[!]/pattern/[commands]
```

对包含 `pattern` 的所有行执行 `commands`，如果指定了 `address`，则对该范围内的所有行执行。如果未指定 `commands`，则打印所有此类行。添加 `!` 可对不包含 `pattern` 的所有行执行 `commands`。另请参阅后面的 `v`。

### 示例

```
:g/Unix/p              Print all lines containing “Unix”
:g/Name:/s/tom/Tom/    Change “tom” to “Tom” on all lines containing “Name:”
```

## hide

```
hid
```

关闭当前窗口，除非它是最后一个窗口，但不要从内存中移除缓冲区。此命令在未保存的缓冲区上使用是安全的。{Vim}

## insert

```
[address] i[!]
text
.
```

在指定 `address` 之前的行插入 `text`，如果未指定则在当前地址处插入。添加 `!` 可在输入 `text` 期间切换 `autoindent` 设置。通过输入仅包含一个句点的行来终止新文本的输入。

## join

```
[address] j[!] [count]
```

将指定范围内的文本放在一行中，并调整空白以在句点 (.) 后提供两个空格字符、在 `)` 前不提供空格字符、在其他情况下提供一个空格字符。添加 `!` 可防止空白调整。

### 示例

```
:1,5j!                 Join the first five lines, preserving whitespace
```

## jumps

```
ju
```

打印与 CTRL-I 和 CTRL-O 命令一起使用的跳转列表。跳转列表记录了跳过多行的大多数移动命令。它记录每次跳转前光标的位置。{Vim}

## k

```
[address] k char
```

与 `mark` 相同；请参阅后面的 `mark`。

## last

```
la[!]
```

编辑命令行参数列表中的最后一个文件。{Vim}

## left

```
[address] le [count]
```

左对齐 `address` 指定的行，如果未指定地址则左对齐当前行。用 `count` 个空格缩进这些行。{Vim}

## list

```
[address] l [count]
```

打印指定的行，使制表符显示为 `^I`，行尾显示为 `$`。`l` 类似于 `:set list` 的临时版本。

## map

```
map[!] [string commands]
```

定义一个名为 `string` 的键盘宏，其为指定的 `commands` 序列。`string` 通常是单个字符或序列 `#` `num`，后者代表键盘上的功能键。使用 `!` 为输入模式创建宏。没有参数时，列出当前已定义的宏。

### 示例

```
:map K dwwP            Transpose two words
:map q :w^M:n^M        Write the current file; go to the next file
:map! + ^[bi(^[ea)     Enclose the previous word in parentheses
```

### 提示

Vim 有 `K` 和 `q` 命令，这些示例别名会隐藏它们。

## mark

```
[address] ma char
```

用 `char` 标记指定的行，`char` 为单个小写字母。与 `k` 相同。稍后使用 `'` `x`（撇号加 `x`，其中 `x` 与 `char` 相同）返回该行。Vim 还对大写字母和数字字符使用标记。小写字母的工作方式与 `vi` 相同。大写字母与文件名相关联，可在多个文件之间使用。然而，编号标记保存在特殊的 `.viminfo` 文件中，不能使用此命令设置。

## marks

```
marks [chars]
```

打印由 `chars` 指定的标记列表，如果未指定 `chars` 则打印所有当前标记。{Vim}

### 示例

```
:marks abc             Print marks a, b, and c
```

## mkexrc

```
mk[!] file
```



## exrc

创建一个 `.exrc` 文件，包含用于更改 `ex` 选项和键映射的 `set` 命令。这会保存当前的选项设置，以便以后恢复。`file` 如果未指定，则默认为当前目录中的 `.exrc`。 {Vim}

## move

```
[address] m destination
```

将 `address` 指定的行移动到 `destination` 地址。

### 示例

```
:.,/Note/m /END/       Move a text block to after the line containing “END”
```

## new

```
[count] new
```

创建一个高度为 `count` 行的新窗口，其中包含一个空缓冲区。 {Vim}

## next

```
n[!] [[+num] filelist]
```

从命令行参数列表中编辑下一个文件。使用 `args` 列出这些文件。如果提供了 `filelist`，则用 `filelist` 替换当前参数列表并从第一个文件开始编辑。使用 `+` `num` 参数时，从第 `num` 行开始编辑。或者，`num` 可以是形式为 /`pattern`/ 的模式。

### 示例

```
:n chap*               Start editing all “chapter” files
```

## nohlsearch

```
noh
```

在使用 `hlsearch` 选项时，暂时停止高亮显示搜索的所有匹配项。高亮将在下一次搜索时恢复。 {Vim}

## number

```
[address] nu [count]
```
```
[address] # [count]
```

打印 `address` 指定的每一行，并在前面加上其缓冲区行号。使用 `#` 作为 `number` 的替代缩写。`count` 指定要显示的行数，从 `address` 开始。

## only

```
on [!]
```

使当前窗口成为屏幕上唯一的窗口。打开已修改缓冲区的窗口不会从屏幕上移除（隐藏），除非你同时使用 `!` 字符。 {Vim}

## open

```
[address] o [/pattern/]
```

在 `address` 指定的行或匹配 `pattern` 的行处进入打开模式（`vi`）。使用 `Q` 退出打开模式。打开模式允许你使用常规的 `vi` 命令，但一次只能处理一行。在非常遥远的互联网 `ssh` 连接上可能很有用。

## packadd

```
pa[!] packagename...
```

搜索匹配 `packagename` 的插件目录并加载插件。有关详细信息，请参阅 Vim 帮助。 {Vim}

## preserve

```
pre
```

将当前编辑器缓冲区保存，就好像系统即将崩溃一样。

## previous

```
prev[!]
```

从命令行参数列表中编辑上一个文件。 {Vim}

## print

```
[address] p [count]
```

打印 `address` 指定的行。`count` 指定要打印的行数，从 `address` 开始。`P` 是另一个缩写。

### 示例

```
:100;+5p               Show line 100 and the next five lines
```

## put

```
[address] pu [char]
```

将先前删除或 yank 的行从 `char` 指定的命名寄存器放置到 `address` 指定的行。如果未指定 `char`，则恢复最后一次删除或 yank 的文本。

## qall

```
qa[!]
```

关闭所有窗口并终止当前编辑会话。使用 `!` 丢弃自上次保存以来所做的更改。 {Vim}

## quit

```
q[!]
```

终止当前编辑会话。使用 `!` 丢弃自上次保存以来所做的更改。如果编辑会话包含参数列表中从未访问的额外文件，请通过键入 `q!` 或键入两次 `q` 来退出。Vim 仅在屏幕上仍有其他窗口打开时才关闭编辑窗口。

## read

```
[address] r filename
```

将 `filename` 的文本复制到 `address` 指定的行之后。如果未指定 `filename`，则使用当前文件名。

### 示例

```
:0r $HOME/data         Read the named file in at the top of the current file
```

## read !command

```
[address] r !command
```

将 `command` 的输出读入 `address` 指定的行之后的文本中。

### 示例

```
:$r !spell %           Place the results of spellchecking at the end of the file
```

## recover

```
rec [file]
```

从系统保存区域恢复 `file`。

## redo

```
red
```

恢复最后一次撤销的更改。与 CTRL-R 相同。 {Vim}

## resize

```
res [[±]num]
```

调整当前窗口高度为 `num` 行。如果指定了 `+` 或 `-`，则按 `num` 行增加或减少当前窗口高度。 {Vim}

## rewind

```
rew[!]
```

倒回参数列表并开始编辑列表中的第一个文件。添加 `!` 即使当前文件自上次更改以来未保存也能倒回列表。

## right

```
[address] ri [width]
```

将 `address` 指定的行，或未指定地址时当前行，右对齐到第 `width` 列。如果未指定 `width`，则使用 `textwidth` 选项的值。 {Vim}

## sbnext

```
[count] sbn [count]
```

分割当前窗口并从缓冲区列表中开始编辑第 `count` 个下一个缓冲区。如果未指定计数，则编辑缓冲区列表中的下一个缓冲区。 {Vim}

## sbuffer

```
[num] sb [num]
```

分割当前窗口并在新窗口中从缓冲区列表开始编辑缓冲区 `num`。要编辑的缓冲区也可以通过文件名指定。如果未指定缓冲区，则在新窗口中打开当前缓冲区。 {Vim}

## set

```
se parameter1 parameter2...
```

使用每个 `parameter` 为选项设置一个值，或者如果未提供 `parameter`，则打印所有与默认值不同的选项。对于布尔选项，每个 `parameter` 可以表述为 `option` 或 `no` `option`；其他选项可以使用语法 `option` `=` `value` 进行赋值。指定 `all` 以列出当前设置。形式为 `set` `option?` 显示 `option` 的值。请参阅 附录 B 中列出 `set` 选项的表格。

### 示例

```
:set nows wm=10
:set all
```

## shell

```
sh
```

创建一个新的 shell。shell 终止时恢复编辑。

## snext

```
[count] sn [[+num] filelist]
```

分割当前窗口并从命令行参数列表开始编辑下一个文件。如果提供了 `count`，则编辑第 `count` 个下一个文件。如果提供了 `filelist`，则用 `filelist` 替换当前参数列表并从第一个文件开始编辑。使用 `+n` 参数时，从第 `num` 行开始编辑。或者，`num` 可以是形式为 /`pattern`/ 的模式。 {Vim}

## source

```
so file
```



## `source`
读取（source）并从 `file` 执行 `ex` 命令。

### 示例
```
:so $HOME/.exrc
```

## `split` (`sp`)
```
[count] sp [+num] [filename]
```
分割当前窗口并在新窗口中加载 `filename`，或如果未指定文件则在两个窗口中加载相同的缓冲区。使新窗口高度为 `count` 行，或如果未指定 `count`，则将窗口分割为相等部分。使用 `+n` 参数，从第 `num` 行开始编辑。`num` 也可以是形式为 `/` `pattern` 的模式。{Vim}

## `sprevious` (`spr`)
```
[count] spr [+num]
```
分割当前窗口并在新窗口中开始编辑命令行参数列表中的上一个文件。如果指定了 `count`，则编辑上 `count` 个文件。使用 `+` `num` 参数，从第 `num` 行开始编辑。`num` 也可以是形式为 `/` `pattern` 的模式。{Vim}

## `stop` (`st`)
```
st
```
挂起编辑会话。与 `CTRL-Z` 相同。使用 shell `fg` 命令恢复会话。

## `substitute` (`s`)
```
[address] s [/pattern/replacement/] [options] [count]
```
用 `replacement` 替换指定行中每个 `pattern` 的第一个实例。如果省略 `pattern` 和 `replacement`，则重复上一次替换。`count` 指定从 `address` 开始进行替换的行数。

### 选项
| `c` || 更改前提示确认。 |
| --- | --- |
| `g` || 在每一行中替换 `pattern` 的所有实例（全局）。 |
| `p` || 打印最后一次进行替换的行。 |

### 示例
```
:1,10s/yes/no/g              Substitute on the first 10 lines

:%s/[Hh]ello/Hi/gc           Confirm global substitutions

:s/Fortran/\U&/ 3            Uppercase “Fortran” on the next three lines

:g/^[0-9][0-9]*/s//Line &:/  For every line beginning with one or more digits,
                             add “Line” and a colon
```

## `suspend` (`su`)
```
su
```
挂起编辑会话。与 `CTRL-Z` 相同。使用 shell `fg` 命令恢复会话。

## `sview` (`sv`)
```
[count] sv [+num] [filename]
```
与 `split` 命令相同，但为新缓冲区设置 `readonly` 选项。{Vim}

## `t`
```
[address] t destination
```
将 `address` 包含的行复制到指定的 `destination` 地址。`t` 等同于 `copy`，是“to”的缩写。

### 示例
```
:%t$                   Copy the file and add it to the end
```

## `tag` (`ta`)
```
[address] ta tag
```
在 `tags` 文件中查找与 `tag` 匹配的文件和行并在那里开始编辑。

### 示例
运行 `ctags`，然后切换到包含 `main` 的文件：
```
:!ctags *.c

:tag main
```

## `tags`
```
tags
```
打印标签栈中的标签列表。{Vim}

## `unabbreviate` (`una`)
```
una word
```
从缩写列表中移除 `word`。

## `undo` (`u`)
```
u
```
撤销最近一次编辑命令所做的更改。在 `vi` 中，撤销命令会撤销自身，重新执行你刚刚撤销的操作。Vim 支持多级撤销。使用 `redo` 在 Vim 中重做已撤销的更改。

## `unhide` (`unh`)
```
[count] unh
```
分割屏幕以显示缓冲区列表中每个活动缓冲区的窗口。如果指定，则将窗口数量限制为 `count`。{Vim}

## `unmap` (`unm`)
```
unm[!] string
```
从键盘宏列表中移除 `string`。使用 `!` 移除输入模式的宏。

## `v`
```
[address] v/pattern/[command]
```
对所有 *不* 包含 `pattern` 的行执行 `command`。如果未指定 `command`，则打印所有此类行。`v` 等同于 `g!`。参见本列表中稍早的 `global`。

### 示例
```
:v/#include/d          Delete all lines except “#include” lines
```

## `version` (`ve`)
```
ve
```
打印编辑器的版本信息。

## `view` (`vie`)
```
vie [+num] [filename]
```
与 `edit` 相同，但将文件设为 `readonly`。在 `ex` 模式下执行时，返回到普通或可视模式。{Vim}

## `visual` (`vi`)
```
[address] vi [type] [count]
```
在 `address` 指定的行进入可视模式（`vi`）。使用 `Q` 返回到 `ex` 模式。`type` 可以是 `-`、`^` 或 `.` 之一（参见本节稍后的 `z` 命令）。`count` 指定初始窗口大小。

## `visual` (`vi`)
```
vi [+num] file
```
在可视模式（`vi`）下开始编辑 `file`，可选地从第 `num` 行开始。或者，`num` 也可以是形式为 `/` `pattern` 的模式。{Vim}

## `vsplit` (`vs`)
```
[count] vs [+num] [filename]
```
与 `split` 命令相同，但垂直分割屏幕。`count` 参数可用于指定新窗口的宽度。{Vim}

## `wall` (`wa`)
```
wa[!]
```
写入所有带有文件名的已更改缓冲区。添加 `!` 以强制写入标记为 `readonly` 的任何缓冲区。{Vim}

## `wnext` (`wn`)
```
[count] wn[!] [[+num] filename]
```
写入当前缓冲区并打开参数列表中的下一个文件，或如果指定则打开第 `count` 个下一个文件。如果指定了 `filename`，则接下来编辑它。使用 `+` `num` 参数，从第 `num` 行开始编辑。`num` 也可以是形式为 `/` `pattern` 的模式。添加 `!` 以强制写入标记为 `readonly` 的任何缓冲区。{Vim}

## `wq`
```
wq[!]
```
一次性写入并退出文件。文件始终会被写入。`!` 标志强制编辑器覆盖 `file` 的任何当前内容。

## `wqall` (`wqa`)
```
wqa[!]
```
写入所有已更改的缓冲区并退出编辑器。添加 `!` 以强制写入标记为 `readonly` 的任何缓冲区。`xall` 是此命令的别名。{Vim}

## `write` (`w`)
```
[address] w[!] [[>>] file]
```
将 `address` 指定的行写入 `file`，或如果未指定 `address` 则写入缓冲区的全部内容。如果 `file` 也被省略，则将缓冲区内容保存到当前文件名。如果使用 `>>` `file`，则将行追加到指定 `file` 的末尾。添加 `!` 以强制编辑器覆盖 `file` 的任何当前内容。

### 示例
```
:1,10w name_list       Copy the first 10 lines to the file name_list

:50w >> name_list      Now append line 50
```

## `write` (`w`)
```
[address] w !command
```
将 `address` 指定的行写入 `command`。

### 示例
```
:1,66w !pr -h myfile | lpr      Print the first page of the file
```

## `X`
```
X
```



## xit
提示输入加密密钥。这比 `:set key` 更可取，因为输入密钥时不会回显到控制台。要移除加密密钥，只需将 `key` 选项重置为空值。 {Vim}

## x
如果文件自上次写入以来有更改，则写入文件，然后退出。

## yank
`[address] y [char] [count]`
将 `address` 指定的行放入名为 `char` 的寄存器中。寄存器名称为小写字母 `a`–`z`。大写名称将文本追加到对应寄存器。如果未给出 `char`，则将行放入通用寄存器。`count` 指定要 yank 的行数，从 `address` 开始。

### Example
```
:101,200 ya a          Copy lines 101–200 to register a
```

## z
`[address] z [type] [count]`
打印一个文本窗口，将 `address` 指定的行置于顶部。`count` 指定要显示的行数。

### Type
`+`:
将指定行置于窗口顶部（默认）。

`-`:
将指定行置于窗口底部。

`.`:
将指定行置于窗口中央。

`^`:
打印上一个窗口。

`=`:
将指定行置于窗口中央并使当前行停留在此行。

## &
`[address] & [options] [count]`
重复上一次替换（`s`）命令。`count` 指定要进行替换的行数，从 `address` 开始。`options` 与替换命令相同。

### Examples
```
:s/Overdue/Paid/       Substitute once on the current line
:g/Status/&            Redo the substitution on all “Status” lines
:g/Status/&g           Redo the substitution on all “Status” lines globally
```

## @
`[address] @ [char]`
执行由 `char` 指定的寄存器内容。如果给出 `address`，先将光标移动到指定地址。如果 `char` 为 `@`，则重复上一次 `@` 命令。

## =
`[address] =`
打印由 `address` 指示的行号。默认是最后一行的行号。

## !
`[address] !command`
在 shell 中执行 `command`。如果指定了 `address`，则使用 `address` 指定的行作为 `command` 的标准输入，并用输出和错误输出替换这些行。这称为通过 `command` `filtering` 文本。

### Examples
```
:!ls                   List files in the current directory
:11,20!sort -f         Sort lines 11–20 of the current file
```

## < >
`[address] < [count]`
or
`[address] > [count]`
将 `address` 指定的行向左（`<`）或向右（`>`）缩进。缩进行时只会添加或删除前导空格和制表符。`count` 指定要缩进的行数，从 `address` 开始。`shiftwidth` 选项控制缩进的列数。重复 `<` 或 `>` 会增加缩进量。例如，`:>>>` 的缩进量是 `:>` 的三倍。

## ~
`[address] ~ [count]`
用最近一次 `s`（substitute）命令的替换模式替换最后使用的正则表达式（即使来自搜索而不是 `s` 命令）。这相当晦涩；详情请参阅 第 6 章。

## address
`address`
打印 `address` 指定的行。

## ENTER



### 在文件中打印下一行。
打印文件中的下一行。（仅适用于 `ex`，不适用于 `vi` 中的 `:` 提示符。）

^1 如果你使用 Vim 并设置 `*.viminfo` 来恢复文件的最后光标位置，则搜索将从恢复的光标位置按所述方向开始。此外，根据 `*.vimrc` 配置文件中 `wrapscan`（`ws`）选项的设置，搜索将在文件顶部或底部行处停止（`:set nowrapscan`）或继续（`:set wrapscan`）。

^2 该选项已过时；`crypt` 命令的加密强度弱，不应使用它。

^3 如果你使用的是原始 `vi` 或已设置 Vim 的 `compatible` 选项，则 ESC 键将执行命令。这可能会出乎意料。Vim（在 `nocompatible` 模式下）只是取消命令且不执行任何操作。
