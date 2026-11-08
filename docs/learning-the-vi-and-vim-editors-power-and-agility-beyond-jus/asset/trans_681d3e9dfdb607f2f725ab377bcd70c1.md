
在我们的示例中，作者希望将 `a` 改为 `b` 或其他字符。为此，他使用自己喜欢的 `vi` 命令移动到 `a*` 并进行修改。命令行编辑结合庞大的存储历史，使他能够随时通过在 `Bash` 历史记录中定位该“脚本”并再次编辑它来返回。

## Bash 中的多行命令

`Bash` 确实有一个选项可以让编辑多行命令更愉快：`shopt -s lithist`。这会导致 `Bash` 将多行命令以多行形式存储在历史文件中，而不是压缩到一行并用分号分隔。当启用时，召回的命令看起来像这样：

```
$ for man in a*
do
  printf "\n\n\n$man, look at man page? "
  read yesno
  if [ ${yesno:-yes} = "yes" ]
  then
    man $man
  fi
  printf "\n\nhit enter to continue "
  read dummy
done
```

你仍然必须使用水平移动命令在召回的文本中移动；`j` 和 `k` 在历史列表中上下移动，而不是在召回的多行命令内部。然而，你可以添加额外的键绑定在物理屏幕行之间移动。执行此操作的 `readline` 命令是 `next-screen-line` 和 `previous-screen-line`.^([2](#ch16.html#idm46601070980288)) 你在你的 `.inputrc` 文件中这样做；请参阅章节 [“.inputrc 文件”](#ch16.html#vi8-ch-16-sect-4.3)，以及 `readline`(3) 手册页。

## 使用 Vim 编辑 Bash 命令

如果内置编辑器对你来说不够用，你只需输入 `v` 就可以在你想要更改的命令上调用你喜欢的编辑器。这会将命令行内容放入由你的 `EDITOR` 环境变量定义的编辑器中。我们当然期望这将是 `vi` 或 `Vim`；然而，你可以选择任何你喜欢的编辑器。

### 警告

这里有个问题。`Bash` 在编辑器退出时会立即执行编辑器缓冲区中的任何内容。假设你输入类似这样的内容：

```
$ rm -fr /
```

然后输入 ESC 并输入 `v` 进入编辑器。如果你随后决定什么都不做并退出编辑器（`:q` 或 `:q!`），原始文本就会执行，然后就炸了！

安全退出 `Vim` 并避免此副作用的方法是使用 `:cq` 命令退出。这会告诉 `Vim` 以非零返回码退出。反过来，这会告诉 `Bash` 发生了错误且不应执行任何命令。

由于被此功能坑过，Elbert 认为这足以成为考虑 `Z shell`（见下文）的理由。

## 其他程序

`Bash` 并不是唯一使用 `readline` 的程序。如果在程序构建时可用，`GDB`（GNU 调试器）会使用它，GNU Awk（`gawk`）也为其内置的 AWK 调试器使用它。在大多数 GNU/Linux 系统上，用于交互式互联网文件传输的 `ftp` 程序也使用它。

在 `GDB` 中集成 `readline` 特别有帮助，因为调试通常涉及输入重复命令，能够轻松搜索和编辑之前的命令会使调试变得不那么繁琐。例如，考虑在链表中跟踪一条“next”指针链。

## .inputrc 文件

> 等等！还有更多！

> 几乎每个深夜电视广告都是这样做的

你可以通过在初始化文件中放置命令来自定义 `readline` 的行为。`INPUTRC` 环境变量指向此文件。如果未设置 `INPUTRC`，则 `readline` 会在你的主目录中查找名为 `.inputrc` 的文件。如果该文件不可用，`readline` 会回退到 `/etc/inputrc`。

`readline`(3) 手册页描述了此文件的格式和可能的内容。该库会随着时间的推移而增长和发展，因此我们在这里不会提供所有内容的描述。相反，我们展示一位作者个人的 `.inputrc` 文件：

```
set editing-mode vi
set horizontal-scroll-mode On
control-h: backward-delete-char
set comment-begin #
set expand-tilde On
"\C-r": redraw-current-line
```

以下是这些行各自作用的简要说明：

`set editing-mode vi`

这会打开 `vi` 编辑模式。默认是 `Emacs` 模式。因此，即使在 `GDB`、`ftp` 或任何其他使用 `readline` 的程序中，也会使用 `vi` 命令集。

`set horizontal-scroll-mode On`

这会导致 `readline` 仅在屏幕上显示单行。不是换行，长行的右边界会用 `>` 字符标记。向右移动超过 `>` 会滚动该行。当行的左侧移出屏幕时，左侧会用 `<` 字符标记。

`control-h: backward-delete-char`

这会导致 `^H`（通常由 `BACKSPACE` 键发送）删除字符。

`set comment-begin #`

这会导致在发出 `#` 命令（“插入注释”）时，`readline` 插入 `#` 字符。对于 shell 来说，这会注释掉当前行，但将其插入历史记录以便以后召回和编辑。无论如何，这对 `Bash` 来说是默认设置，但我们的作者的文件自 2002 年以来就没有更改过！

`set expand-tilde On`

这会导致 `readline` 在进行词展开时执行波浪线展开。如果 `readline` 与不执行波浪线展开的 shell（如 `rc` shell）一起使用，这很有用。

`"\C-r": redraw-current-line`

这会导致 `CTRL-R` 重绘当前行。如果系统输出与你的输入混合在一起，这很有用。^([3](#ch16.html#idm46601070923536))

查看 `readline` 手册页；还有许多其他选项，包括一些可在支持颜色的终端模拟器上使输出着色的选项。

# 其他 Unix Shell

原始具有命令行编辑的 shell 是 Korn shell（`ksh`），最初由 David Korn 在贝尔实验室开发。要启用 `vi` 模式，请使用 `set -o vi`（`Bash` 就是从这里得到的）。`ksh` 的编辑器不基于 `readline` 库.^([4](#ch16.html#idm46601070913840))

同样，`Z shell`（`zsh`）也有自己的 `vi` 命令行模式；它与 `ksh` 和 `Bash` 的有所不同，如果你习惯了其中一个 shell，可能需要添加额外的键绑定。我们将在下一节更详细地讨论 `Z shell` 的具体内容。好的一面是，`zsh` 可以轻松编辑多行命令。

最后，`tcsh`（“Tenex `csh`”）也提供 `vi` 模式，可通过 `bindkey -v` 启用。由于我们都不是 `tcsh` 用户，我们对此没有太多可说的。

## Z Shell (zsh)

如前所述，[Z Shell](https://www.zsh.org) 具有自己的命令行模式，并配备强大的多行历史编辑器。考虑之前的示例。以下说明了 `zsh` 中的差异，这些差异通过专门的提示提供了更清晰的视觉上下文：

```
{elhannah,/usr/bin} for man in a*
for> do
for>    printf "\n\n\n$man, look at man page? "
for>    if [ ${yesno:-yes} = "yes" ]
for if> then
for then>       man $man
for then> fi
for>    printf "\n\nhit enter to continue "
for>    read dummy
for> done 
```

现在我们来到了命令行编辑的真正区别！这里我们展示在输入 `ESC K` 后，`zsh` 如何在编辑模式下呈现多行命令：

```
{elhannah,/usr/bin} for man in a*
do
             printf "\n\n\n$man, look at man page? "
             if [ ${yesno:-yes} = "yes" ]
then
             man $man
fi
             printf "\n\nhit enter to continue "
             read dummy
done
```

你现在拥有一个迷你的 `vi` 会话，可以进行更精确的编辑。

### 警告
