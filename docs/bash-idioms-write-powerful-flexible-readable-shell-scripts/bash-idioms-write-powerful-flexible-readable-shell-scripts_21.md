## 函数

别名无法将参数传递到别名中间，但函数可以做到这一点，正如我们在第 6 章中介绍的那样。我们开始有点脱离习惯用法而更偏向配方，但我们无法抗拒示例 10-12 中展示的方便的小函数。

## 示例 10-12. 函数 mcd

```
function mcd {
  \mkdir -p "$1"
  \cd "$1"
}
```

当然，这非常 trivial，并且不做健全性或错误检查，也不允许设置权限（-m mode），但即使这个版本也是一个很好的节省时间工具，如果你经常创建目录。注意开头的 \ 会抑制别名扩展，因此我们确保获得纯粹的 bash 内置。我们把扩展它作为读者的练习。





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
