
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
