

我们建议使用 #!/bin/bash - 或 #!/usr/bin/env bash。我们强烈不建议使用 #!/bin/sh，除非你是有意将代码限制在 Bourne shell 兼容性上，换句话说不使用“bashism”。过去在 Linux 上这样做是可以的，但许多 Linux 发行版已改用 Debian Almquist shell 或 dash 作为 /bin/sh，因为它比 bash 更小更快，运行脚本时速度更快，就像系统启动时使用的所有脚本一样。但当你真正需要 bash 时使用 sh 是一个坏主意，因为正如我们刚才所说，它很脆弱并且掩盖了程序员的意图。

#!/usr/bin/env bash 可移植性更高，因为只要 bash 在 $PATH 中，它就会去找到它。当 bash 不在 /bin/ 中时，这非常有用，这在 BSD、Solaris 和其他非 Linux 系统上就是这种情况。唯一的问题是，生成 env，然后搜索 $PATH，然后用 bash 替换自身的开销更慢。有了现代计算机的资源，这几乎从不重要，但如果你在受限系统上开发或在短时间内运行大量脚本或实例，这种开销会累积起来（示例 9-11）。

### 示例 9-11. Shebang 速度测试

```
$ head examples/ch09/shebang*
==> examples/ch09/shebang-bash.sh <==
#!/bin/bash -
:

==> examples/ch09/shebang-env.sh <==
#!/usr/bin/env bash
:

$ time for ((i=0; i<1000; i++)); do examples/ch09/shebang-bash.sh; done
real    0m3.279s
user    0m1.273s
sys     0m0.915s

$ time for ((i=0; i<1000; i++)); do examples/ch09/shebang-env.sh; done
real    0m4.291s
user    0m1.313s
sys     0m1.425s
```

需要提到的另一个细节是我们展示的尾随 -。这是为了防止非常古老的解释器欺骗攻击；有关详细信息，请参阅 Unix FAQ 的第 4.7 节。Linux 内核（但其他内核不这样做！）将第一个“单词”之后的所有内容（在本例中为 /bin/bash）视为单个参数，因此我们用“结束参数”的连字符“使用”该参数。请注意，这在 #!/usr/bin/env bash 中不需要，因为 bash 部分本身就是参数。

## 非官方 bash 严格模式

Perl 有一个 use strict 指令，用于“限制不安全构造”，如果未在之前使用变量声明和初始化变量等，它会触发语法检查或运行时错误。非官方 bash 严格模式文章认为以下内容在 bash 中做了同样的事情（在精神上）：

```
set -euo pipefail
IFS=$'\n\t'
```

我们并不完全同意那样更改 $IFS，因为这会破坏很多 bash 习惯用法（参见第 89 页的“为乐趣和利润而摆弄 $IFS，以读取文件”）。作者提出了很好的论点，但这可能需要大量工作来迁移遗留代码。set 命令非常好：

- -e 将在第一次出错时退出整个脚本。
- -u 将在你使用未设置变量时退出整个脚本。
- -o pipefail 将在管道中任何命令，而不仅仅是最后一个命令，以非零退出时使管道失败。

添加 -u 会破坏很多现有脚本，但修复起来会很容易，而且你可能会发现一些你不知道的变量名拼写错误，这正是重点所在。-eo pipefail 最初可能破坏得更少，而当它确实“破坏”脚本时，该脚本可能早已损坏，但错误被掩盖了，这些选项只是揭露了问题。

这绝对值得考虑，也许可以添加到你的模板和风格指南中。

另请参阅：

- http://redsymbol.net/articles/unofficial-bash-strict-mode
- help set
- https://perldoc.perl.org/strict

## 退出代码

如果没有特别说明，你的 bash 脚本将以最后运行的命令的退出（或返回）代码退出。有时这没问题，有时你可能需要在执行健全性检查失败后或出于其他条件提前退出。

在 bash 中，零表示 *成功*，非零表示失败。这与许多其他语言相反，但原因是有一种成功方式但有许多失败方式，你可能需要表示失败的方式，例如：

```
if grep --quiet "$HOSTNAME" /etc/hosts; then ...
```

bash 退出代码只有八位，因此最大值是 255，但你希望返回代码值保持在 125 及以下，因为 126 及以上已被占用，如表 9-1 所示。

### 表 9-1. bash 退出/返回代码

| Exit/return code | Use/description |
|---|---|
| 0 | Success |
| 1–125 | User-defined failure |
| 2 | bash builtins incorrect usage |
| 126 | Command found but not executable |
| 127 | Command not found |
| 128–255 | Command exited with signal N-128 |

另请参阅 GNU 文档关于退出状态。

用于以“退出代码”退出整个脚本的命令是，等一下，exit n。用于从函数返回的命令是，再次，等一下，return n，其中“n”是之前定义的可选参数。请记住 return 返回的是退出代码，而不是值。“n”可以是任何 shell 表达式，如 5 或 $val 或更复杂的表达式。请记住如果未给出“n”，状态为最后运行命令的状态。

另请参阅我们在第 79 页“HELP!”中关于 exit 0 或 exit 1 的帮助和用法讨论。

## 这是个陷阱！

trap 内置命令（help trap）允许你在脚本接收到信号时执行代码，其中包括脚本退出时。这是一个很好的地方放置“清理”代码，即使有人 CTRL-C（但不是 kill -9）脚本也能工作。你也可以用它运行调试代码，但对于这种简单情况，只需使用 set -x 或在 bash -x 下运行即可。运行 trap -l 列出系统支持的所有信号，或 trap -p <signal> 显示与该信号关联的 trap 命令。

trap 的典型用途是清理脚本，如前所述。语法是 trap [-lp] [[arg] signal_spec ...]，而 arg 可以是自包含的代码，但最好只是定义一个函数然后调用它会更清晰。这看起来像示例 9-12（输出显示在示例 9-13 中）。

### 示例 9-12. 临时 trap 示例：代码

```
#!/usr/bin/env bash
# trivial_trap.sh: Trivial bash trap example
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch09/trivial_trap.sh
# --------------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!

function Cleanup {
    echo "$FUNCNAME was triggered!  Cleaning up..."
}

echo "Starting script $0..."

echo 'Setting the trap...'
# Will call Cleanup on any of these 6 signals
trap Cleanup ABRT EXIT HUP INT QUIT TERM

echo 'About to exit...'
```

### 示例 9-13. 临时 trap 示例：输出

```
Starting script examples/ch09/trivial_trap.sh...
Setting the trap...
About to exit...
Cleanup was triggered!  Cleaning up...
```

另请参阅 GNU 文档关于 trap 和第 120 页的“bash 调试”。

## Here-Documents 和 Here-Strings

其他语言有类似的构造，比如 C 的 /* ... */，和 Python 的 ''' ... '''，但 bash 有一些有趣的习惯用法用于 here-documents 和 here-strings。我们在示例 8-3 中使用过 here-document，但还有更多需要讨论和解码的内容。

Here-documents 用于在不需要大量分隔符和命令的情况下包含并经常显示文本块。

Here-document 语法是 [fd]<<[-]["'"]word["'"]，其中“fd”是可选的（且很少使用）文件描述符，- 是一个表示“删除前导制表符但不删除空格”的标志，可选引号表示“不插值内容”。word 本身不受参数、变量、算术或文件名扩展或命令替换的影响。举例来说，这意味着它不能是变量。如果 word 的任何部分被引用，则不会对文档进行插值。我们建议只使用单引号，因为它们在 bash 中防止其他地方的插值，因此含义一致，而使用双引号则不然。

实际上，使用 - 意味着你可以在代码中缩进 here-document（但只能使用制表符），同时在文档本身中不保留该缩进，而不加引号意味着你可以在其中使用变量。正如我们在示例 8-3 中看到的那样，这对于显示包含脚本名称和路径的用法消息非常有用，而无需大量 echo 或 printf 行和引号。我们在后面也将其用于第 115 页的“嵌入式文档”。

Here-strings 看起来像 [fd]<<word，这似乎相似，但在这种情况下 word 本身会受到参数、变量和算术扩展的影响；命令替换；和引号删除。它们本质上是一个没有分隔符的更简单的 here-document，结果是一个字符串，并附加换行符。

参见 GNU 文档关于 here-documents 和 here-strings。

## 我们是交互式的吗？

有时你的代码需要知道它是否在交互式会话中运行。它可能需要知道这一点，因为它会改变行为，比如请求输入，或者你可能只想为交互式 shell 设置某些配置选项。

GNU 文档关于交互式 shell 说要使用以下其中一种，因此你会经常看到这些或其变体：

```
case "$ -" in
  *i*) echo This shell is interactive ;;
  *)   echo This shell is not interactive ;;
esac
```

或：

```
if [ -z "$PS1" ]; then
  echo This shell is not interactive
else
  echo This shell is interactive
fi
```

你也可以考虑 -t FD 测试，如果文件描述符打开在终端上则为真。如果未指定文件描述符，则使用 0（STDIN）。你可以将其与其他测试结合，比如 bash 本身，例如：

```
# Only if bash in a terminal!
if [ -t 1 -a -n "$BASH_VERSION" ]; then
  echo 'bash in a terminal'
else
  echo 'NOT bash in a terminal'
fi
```

