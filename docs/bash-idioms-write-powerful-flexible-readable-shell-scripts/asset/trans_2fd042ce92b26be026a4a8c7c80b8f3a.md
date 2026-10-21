

## 局部变量

我们在第 54 页的“Local Variables”中谈到过这个问题，但简单回顾一下，local 命令只能在函数内部使用，用来声明对该函数而言是局部的变量。这是一种良好的编程实践，但如果你需要在函数之外拥有“局部”作用域，也有一个办法，而且常用的变量是内部字段分隔符 $IFS。通常你不想更改 $IFS，因为一旦更改，许多预期行为都会失效。但偶尔你确实想更改它，通常只在局部更改，你可以做到（即使在函数之外），方法是像下面我们将要谈到的 read 命令那样，在命令前加上变量赋值：

```
IFS=':' read ...
```

另请参阅第 89 页的“Fiddling with $IFS for Fun and Profit, to Read Files”。

## Readline 技巧

Readline 是 bash 用来读取和编辑命令行行的库。它可能被其他程序使用，但你可以在脚本中很容易地通过 read -e 来访问它。它有很多选项可以调整其行为，似乎许多用户并不了解它们。

你可以探索创建 ~/.inputrc 文件（参见 bind -f 命令），但只是为了好玩，你可以把下面这些添加到 ~/.bashrc 文件中（示例 10-13）。

### 示例 10-13. Readline 调整

```
bind '"\e[A": history-search-backward'   # Like CTRL-R but right on the CLI  ①
bind '"\e[B": history-search-forward'   # Same except forward  ②
bind '"\C-i": menu-complete'            # Cycle through possible completions  ③
bind 'set completion-ignore-case on'      # Ignore case in completions  ④

# These are bash, but feel related to "CLI behavior tweaks"
export HISTCONTROL='erasedups:ignoredups:ignorespace'
export HISTIGNORE='&: [ ]*' # bash >= 3, omit dups & lines starting w/ space+  ⑤ ⑥
```

① 将“上箭头”绑定到 history-search-backward，这意味着如果你开始输入一个命令，然后按“上箭头”，它会在你的历史记录中搜索该字符串。你可以用 CTRL-R 做类似的事情，但“上箭头”更快更直观。然而，CTRL-R 会在命令中间找到字符串，而此功能仅限于行首。

② 将“下箭头”绑定到 history-search-forward，这与第 1 项相同，只是向前查找历史记录。

③ 当你按 TAB 且补全存在歧义时，不再只是蜂鸣并停在那里，而是开始循环遍历可用的补全选项。

④ 在文件名和命令补全时忽略大小写。

⑤ 在保存历史记录时，erasedups 会在保存历史记录前删除与当前行匹配的所有先前行。ignoredups 防止保存先前的重复行，而 ignorespace 防止以前导空格开头的命令被保存到历史记录中。其结果是你的历史记录会更干净，因为没有重复，但你也会失去命令的顺序和序列感，因为重复命令会被删除或从未保存。你也可以选择不将某些命令保存到历史记录中，例如包含敏感数据的命令，但请注意它们仍会在进程列表中可见！

⑥ 与上一个相同，为了与非常旧的 bash 保持向后兼容，以防万一。

## 使用 logger

logger 工具（man logger）非常适合将日志直接发送到 syslog，但我们认为它有一个 bug，即 -t 或 --tag 参数不是强制性的，默认只是用户名。我们推荐示例 10-14 中所示的方法。

### 示例 10-14. logger 标签

```
logger --tag "$0[$$]" <msg>
logger --tag "$PROGRAM[$$]" <msg>
logger --tag "${0##*/}[$$]" <msg>
```

① 最简单的情况是使用 $0 作为脚本名称，$$ 作为其 PID。$0 的问题在于它会根据脚本的调用方式而变化，这既嘈杂又不一致。我们更愿意使用 basename。

② 如果你已经像 $PROGRAM 这样使用 basename（示例 8-3），那么也可以在 logger 中使用它。

③ 如果你没有为脚本的 basename 设置类似 $PROGRAM 的变量，你可以在 logger 标签中直接完成。这开始有点晦涩，但它会起作用。尝试 echo "${0##*/}[$$]" 来查看，但从命令行来看它没那么有趣。

如果你要自行记录日志，请参阅“Getting or Using the Date and Time”第 57 页中的 printf 和 time。

## 使用 jq 处理 JSON

jq 不是 bash，但我们想涵盖它，因为 JSON 非常常见，尤其是在处理云基础设施时，我们见过一些使用 awk、grep 和 sed 等工具来尝试解析 JSON 的糟糕代码。尽可能使用 jq 代替。

![](img/553ffa1bc15093729b691f706befc0ac_127_0.png)

### jq 并非总是可用

不幸的是，尽管 JSON 已变得非常普遍，但 jq 并非像 awk、grep 和 sed 那样默认安装。它也不包含在 Windows 的 Git Bash 中，而 Git Bash 包含 awk、sed、cut、grep、tr 等。

有时你可以避开这个问题。例如，对于 AWS，你可以使用 --output=text，然后使用常规工具。否则，你可以在默认构建中安装 jq，或使用 Python 等工具寻找变通方法，这超出了本书的范围。

本书的源代码包含一个 atlas.json 文件，因此我们可以将其用于一些示例。如果我们想找到本书的标题，可以这样做：

```
$ jq '.title' atlas.json
"bash Idioms"
$ jq -r '.title' atlas.json  # "Raw" mode, without the quotes
bash Idioms
```

#### 但 `grep` 也能工作...有点
```
$ grep 'title' atlas.json
  "titlepage.html",
  "title": "bash Idioms",
```

如果我们想知道各种输出格式是否启用了语法高亮呢？用 grep 做起来比较棘手，但用 jq 很简单：

```
### What do we have?
$ jq '.formats | keys' atlas.json
[
  "epub",
  "html",
  "mobi",
  "pdf"
]

### Bad; this has no context and is useless
$ grep 'syntaxhighlighting' atlas.json
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
  "syntaxhighlighting": true,
```

```
### Good; this tells us that PDF output does have syntax highlighting
$ jq '.formats.pdf.syntaxhighlighting' atlas.json
true
```

```
### Better, but very confusing, show formats and syntax value, see jq docs
$ jq -r '.formats | keys[] as $k | "\($k), \(.[$k] \
| .syntaxhighlighting)"' atlas.json
epub, true
html, true
mobi, true
pdf, true
```

![](img/553ffa1bc15093729b691f706befc0ac_128_0.png)

## 在 Firefox 中读取 JSON

Firefox 拥有出色的内置 JSON 查看器，可以帮助你更好地了解所看到的内容以及如何让 jq 输出它。只需将你的 JSON 输出保存为 myfile.json，然后在 Firefox 中打开 file:///path/to/myfile.json 即可。

## Grepping 进程列表

当你 grep 进程列表时，列表中的一个进程就是你的 grep 命令本身，但那不是你要找的，所以你会在示例 10-15 中看到一种习惯用法来移除 grep 行。

### 示例 10-15. 不要这样做

```
ps auxw | grep 'proggy' | grep -v 'grep'
```

我们认为这种解决方案既丑陋又低效，我们更喜欢示例 10-16。

### 示例 10-16. 这样做

```
ps auxw | grep '[p]roggy'
```

这之所以有效，是因为进程列表中的字符串 “[p]roggy” 与正则表达式 /[p]roggy/ 不匹配，而该正则表达式实际上表示 “proggy”。换句话说，“[p]roggy” != “proggy”。

或者，你可以直接使用 pgrep -fl，或者最坏情况下，使用旧的 pidof。

## 轮转旧文件

日志记录很好，归档也很好，但总有一天你需要“轮转”或删除旧文件。有很多惯用的方法可以做到这一点，我们将讨论其中几种并提出一些建议。

许多轮转旧文件的方法都涉及出色的 find 工具，但其中很大一部分还使用了 -exec 和/或 xargs，而在现代版本的 GNU find 中，这些是不必要的。旧的习惯用法是：

```
find /path/to/files -name 'some-pattern' -a -mtime 5 -exec rm -f \{\} \;
```

```
find /path/to/files -name 'some-pattern' -a -mtime 5 | xargs rm -f
```

```
find /path/to/files -name 'some-pattern' -a -mtime 5 -print0 | xargs -0 rm -f
```

① 这极其低效，因为它为每个文件调用一次新的 rm 实例。

② 这更高效，因为它会批量处理文件（xargs），但如果文件名包含空格就会中断并产生意外的行为。

③ 这也可以处理空格，但它仍然比你需要的更复杂。

如果你使用 GNU find 且不担心可移植性，使用示例 10-17 中的代码会简单得多。

### 示例 10-17. 使用 find 删除旧文件

```
find /path/to/files \( -type f -a -name 'some-pattern' -a -mtime 5 \) -delete
```

请记住以下几点：

- 请参阅 man find 并搜索 atime 和 mtime。-mtime +5 表示文件至少在五天前最后修改过，但“五”并不精确；请参阅手册页了解细节讨论。
- 为了测试，使用 -ls 或 -print 而不是 -delete！但完成后不要忘记更改它。
- 请注意，find 会递归进入路径下任何/所有子目录，除非你使用 -maxdepth 来限制它。
- 你想要使用 ( ) 来分组（find 仅匹配所有条件的东西…然后删除它们），但你必须转义它们以防止 shell 自己解释它们。
- 表达式中选项之间假定有 -a “与” 运算符；如果需要，也可以使用 -o 表示“或”。我们喜欢加上 -a，因为显式优于隐式。

