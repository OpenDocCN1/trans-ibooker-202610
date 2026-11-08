
请记住你可以将数字与移动命令结合使用。例如，`3)` 向前移动三句话。还请记住你可以使用移动命令进行编辑：`d)` 删除到当前句子的末尾；`2y}` 复制（抓取）前两个段落。

还请记住你可以使用诸如 `cw` 和 `ce` 之类的编辑命令配合运动命令。Robert P. J. Day 指出，有趣的是，“虽然 `w` 和 `e` 是略有不同的移动命令，但更改命令 `cw` 和 `ce` 完全做同样的事情。”

# 通过搜索移动

在大型文件中快速移动的最有用方法之一是通过搜索文本，或者更准确地说，搜索 `pattern` 的字符。有时可以执行搜索来查找拼写错误的单词或查找程序中变量的每个出现位置。

搜索命令是特殊字符 `/`（斜杠）。当你输入斜杠时，它会出现在屏幕底行；然后你输入想要查找的 `pattern`：`/` `pattern`。

一个 pattern 可以是一个完整的单词或任何其他字符序列（称为“字符字符串”）。例如，如果你搜索字符 `red`，你将匹配 `red` 作为一个完整单词，但也会匹配 `occurred`。如果在 `pattern` 之前或之后包含空格，则空格会被视为待匹配文本的一部分。与所有底行命令一样，按 `ENTER` 完成。

`vi` 和 `Vim`，像所有其他 Unix 编辑器一样，都有一种特殊的模式匹配语言，允许你查找可变文本模式——例如，以大写字母开头的任何单词，或行首的单词 `The`。我们将在 [第6章，“全局替换”](#ch06.html#vi8-ch-6) 中讨论这种更强大的模式匹配语法。现在，只需将 pattern 视为一个单词或短语即可。

编辑器从光标处开始搜索并向前搜索，如果必要则环绕到文件开头。光标移动到 `pattern` 的第一次出现位置。如果没有匹配，则在状态行显示消息“Pattern not found”。^([5](#ch03.html#idm46601107643376))

使用文件 `practice`，以下是通过搜索移动光标的方法：

Keystrokes: `/edits` `ENTER`

Results:
```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing the
results of your edits as you make them.
```
搜索 pattern `edits`。光标直接移动到匹配文本。注意在按 `ENTER` 之前不要在 `edits` 后输入空格。

Keystrokes: `/scr` `ENTER`

Results:
```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing the
results of your edits as you make them.
```
搜索 pattern `scr`。搜索会环绕到文件前端。

请注意你可以输入任意字符组合；搜索不一定针对完整单词。

要向后搜索，请输入 `?` 而不是 `/`：

```
?pattern
```

在这两种情况下，如果必要，搜索会环绕到文件开头或结尾。

## 重复搜索

你最后搜索的 `pattern` 在整个编辑会话中保持可用。搜索后，而不是重复原始按键，你可以使用 `vi` 命令再次搜索上一个 `pattern`：

`n`:
重复以相同方向搜索。

`N`:
重复以相反方向搜索。

`/` `ENTER`:
向前重复搜索。

`?` `ENTER`:
向后重复搜索。

由于上一个 `pattern` 保持可用，你可以搜索一个 `pattern`，做一些工作，然后使用 `n`、`N`、 `/` 或 `?` 再次搜索同一个 `pattern` 而无需重新输入。你的搜索方向（`/` 为向前，`?` 为向后）显示在屏幕左下角。`Vim` 超越了 `vi`，它将搜索文本放入命令行并允许你使用上下箭头键滚动已保存的搜索命令历史。章节 [“介绍历史窗口”](#ch14.html#vi8-ch-14-sect-1.3.1) 讨论了如何充分利用已保存的搜索文本历史。

为了继续前面的示例，由于 pattern `scr` 仍可用于搜索，你可以执行以下操作：

Keystrokes: `n`

Results:
```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing the
results of your edits as you make them.
```
使用 `n`（next）命令移动到 pattern `scr` 的下一个实例（从 `scr`een 到 `scr`oll）。

Keystrokes: `?you` `ENTER`

Results:
```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing the
results of your edits as you make them.
```
使用 `?` 从光标向后搜索到 `you` 的第一次出现。你需要在输入 pattern 后按 `ENTER`。

Keystrokes: `N`

Results:
```
With a screen editor you can scroll the
page, move the cursor, delete lines, insert
characters, and more, while seeing the
results of your edits as you make them.
```
重复上一次对 `you` 的搜索，但方向相反（向前）。

有时你只想在更前面找到一个词；你不希望搜索环绕到文件 earlier 部分。有个选项 `wrapscan` 控制搜索是否环绕。你可以这样禁用环绕：

```
:set nowrapscan
```

当设置 `nowrapscan` 且向前搜索失败时，`vi` 的状态行显示以下消息：

```
Address search hit BOTTOM without matching pattern
```

`Vim` 显示此消息：

```
E385: search hit BOTTOM without match for: foo
```

当设置 `nowrapscan` 且向后搜索失败时，消息中显示 “TOP” 而不是 “BOTTOM”。

### 通过搜索更改

你可以将 `/` 和 `?` 搜索运算符与更改文本的命令（如 `c` 和 `d`）结合使用。继续前面的示例：

Keystrokes: `d?move` `ENTER`

Results:
```
With a screen editor you can scroll the
page, your edits as you make them.
```
从光标之前删除直到并包括单词 `move`。

注意删除是以字符为基础进行的，整行不会被删除。

本节仅对搜索模式提供了最基本的介绍。[第6章](#ch06.html#vi8-ch-6) 教授了有关模式匹配及其在对文件进行全局更改中使用的更多内容。

## 当前行搜索

还有一些在当前行内操作的搜索命令的迷你版本。命令 `f` `x` 将光标移动到字符 `x`（其中 `x` 代表任意字符）的下一个实例。命令 `t` `x` 将光标移动到下一个 `x` 实例之前的字符。（`f` 是 find 的缩写；`t` 是 to 的缩写，意为“直到”。）然后可以用分号重复使用以“找到”你的路径。

内联搜索命令在此处汇总。以下命令均不会将光标移动到下一行：

`f` `x`:
在行中查找（将光标移动到）`x` 的下一个出现位置，其中 `x` 代表任意字符。

`F` `x`:
在行中查找（将光标移动到）`x` 的上一个出现位置。

`t` `x`:
在行中查找（将光标移动到）`x` 下一个出现位置之前的字符。

`T` `x`:
在行中查找（将光标移动到）`x` 上一个出现位置之后的字符。

`;`:
以相同方向重复上一个查找命令。

`,` (comma):
以相反方向重复上一个查找命令。

使用这些命令中的任何一个时，数字前缀 `n` 可定位第 `n` 次出现。

假设你在 `practice` 中编辑，以下这行：

```
With a screen editor you can scroll the
```

你可以按如下方式查找字母 `o` 的出现：

Keystrokes: `fo`
