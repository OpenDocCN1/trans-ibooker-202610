

- 整行都会受到影响。例如，如果你的光标在某行的中间，并且你发出命令前往下一句的末尾，则包含该句开头和结尾的整行都会被更改，而不仅仅是句子本身。^([5](#ch07.html#idm46601101826640))

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

如果设置了 Lisp 模式，`=` 会被 `vi` 使用，而 Vim 则用于文本格式化。在许多现代版本的 `vi` 中，`_` 等同于 `^` 命令，而 Vim 有一个使用 `v`、`V` 和 `^V` 键的“视觉模式”。（参见章节 [“视觉模式移动”](#ch08.html#vi8-ch-8-sect-6.1)。）寓意是仔细测试你的版本。

根据你的终端，你可能还可以将映射序列与特殊功能键关联。你也可以映射命令模式已使用的键，但那样你将失去该键的默认功能；我们在后面的章节 [“映射键的更多示例”](#ch07.html#vi8-ch-7-sect-3.6) 中展示了一个示例。章节 [“几个便捷映射”](#ch14.html#vi8-ch-14-sect-1) 提供了一些额外的、更强大的映射示例。

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

