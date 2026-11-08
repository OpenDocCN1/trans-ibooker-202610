
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

初始的 `:` 表示一个 `ex` 命令。随着你输入命令，它会回显在状态行上。通过按下 ENTER 键执行命令。`Address` 是 `command` 的对象行号或行范围。`Options` 和 `addresses` 稍后会描述。`ex` 命令在章节 [“ex 命令字母顺序摘要”](#app01.html#vi8-app-a-sect-6) 中描述。

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

有关使用模式的更多信息，请参阅 [第 6 章，“全局替换”](#ch06.html#vi8-ch-6)。

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
