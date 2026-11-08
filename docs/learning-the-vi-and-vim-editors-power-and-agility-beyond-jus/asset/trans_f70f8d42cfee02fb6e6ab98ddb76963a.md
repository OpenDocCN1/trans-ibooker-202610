
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

使用每个 `parameter` 为选项设置一个值，或者如果未提供 `parameter`，则打印所有与默认值不同的选项。对于布尔选项，每个 `parameter` 可以表述为 `option` 或 `no` `option`；其他选项可以使用语法 `option` `=` `value` 进行赋值。指定 `all` 以列出当前设置。形式为 `set` `option?` 显示 `option` 的值。请参阅 [附录 B](#app02.html#vi8-app-b) 中列出 `set` 选项的表格。

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
