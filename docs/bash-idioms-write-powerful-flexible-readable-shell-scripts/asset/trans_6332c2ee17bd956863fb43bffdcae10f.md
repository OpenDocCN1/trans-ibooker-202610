

## 其他

其他指南：

- 对于“system”脚本，记录到 syslog，让操作系统处理最终目的地、日志轮转等。
- 错误信息应发送到 STDERR，如 echo 'A Bad Thing happened' 1>&2。
- 使用 [ -x /path/to/tool ] || { ...error code block... } 来检查外部工具是否可用。
- 失败时提供有用的信息。
- 设置退出代码，尤其是在失败时。

## 脚本模板

这是一个示例骨架或模板脚本，你可以复制它作为提醒，并在创建新脚本时节省一些（好吧，也许相当多的）输入：

```
#!/bin/bash -
# 或可能是： #!/usr/bin/env bash
# <Name>: <description>
# Original Author & date:
# Current maintainer?
# Copyright/License?
# Where this code belongs? (Hosts, paths, etc.)
# Project/repo?
# Caveats/gotchas?
# Usage? (Better to have `-h` and/or `--help` options!)
# $URL$  # If using SVN
ID=''  # If using SVN
#
PROGRAM=${0##*/}  # bash version of `basename`

# Unofficial bash Strict Mode?
#set -euo pipefail
### CAREFUL: IFS=$'\n\t'

# GLOBAL and constant variables are in UPPER case
LOG_DIR='/path/to/log/dir'

### Consider adding argument handling to YOUR template; see:
# examples/ch08/parseit.sh
# examples/ch08/parselong.sh
# examples/ch08/parselonghelp.sh

# Functions are in Mixed Case
######################################################################
```

```
# Define functions

#-------------------------------------------------------------------------------
# Example function
# Globals: none
# Input:  nothing
# Output: nothing
function Foo {
    local var1="$1"
    ...
} # End of function foo

#-------------------------------------------------------------------------------
# Another example function
# Globals: none
# Input:  nothing
# Output: nothing
function Bar {
    local var1="$1"
    ...
} # End of function bar

###############################################################################
# Main

# Code...
```

## 其他风格指南

我们强烈建议你制定一份风格指南并使用它！如果你不喜欢我们的，并且你也不想自己修改，你可以从别处偷一个：

- The Google Shell Style Guide

虽然我们对其中一些有不同的看法和分歧，但它非常好且相当全面。最好的情况是，你可以直接使用它，最坏的情况是，它是一个起点。有很多值得喜欢的地方，你可以做得更差（比如什么都没有），而且很多其他项目都采用了它，所以……

除其他事项外，我们不同意：

- 百行限制。我们看到了这一点，尤其是在 Google 这样的环境中，但我们有很多超过该限制的脚本，它们“主要是调用其他实用程序并且数据操作相对较少。”
- “缩进两个空格”对我们来说有点浅；我们喜欢对续行使用两个空格，其他情况使用四个空格。
- “${var}”太忙了。我们在可能的情况下更喜欢 "$var"。

— function cleanup() {? Ouch, no.

— ;; 关闭 case..esac 块，应与块的开头保持相同的缩进，而不是与块中的代码保持相同的缩进。

- 以下还有其他你可以参考的指南：
  - https://linuxcommand.org/lc3_adv_standards.php
  - https://www.ovirt.org/develop/infra/infra-bash-style-guide.html
  - https://wiki.bash-hackers.org/scripting/style
  - http://mywiki.wooledg.org/BashGuide/Practices
    - Great info: http://mywiki.wooledg.org/BashPitfalls

- 或在网上搜索：
  - "Shell Style Guide"
  - "Bash Style Guide"
  - "Shell script coding standards"

## bash Linter

使用 linter 代替风格指南或作为补充也可能很方便。说实话，我们不用，但我们认为应该涵盖一下。我们知道的是 shellcheck，我们对结果有复杂的感受；它对引号特别挑剔，我们并不总是同意它关于引号或一般方面的建议，但这些东西或多或少是可以调整的。话虽如此，它仍然非常酷，值得一看。

- bash linter: https://www.shellcheck.net
  - https://github.com/koalaman/shellcheck (Haskell)
  - History: https://www.vidarholen.net/contents/blog/?p=859
  - Checks: https://github.com/koalaman/shellcheck/wiki/Checks
  - Tweaking: https://github.com/koalaman/shellcheck/wiki/Directive
  - Ignoring errors: https://github.com/koalaman/shellcheck/wiki/Ignore
  - Note, Eol (2020) CentOS-6 is too old: https://github.com/koalaman/shellcheck/wiki/CentOS6

如果你通过包系统安装，请记住版本可能相当旧。Linux 版本是一个包含单个编译二进制文件的 tarball，所以很容易将当前版本放到你的路径中的某个位置。

## 总结

希望，这本书能让你更好地理解如何阅读和编写“bashy”代码，并且本章为你提供了开始定义适合你的风格（和指南）的地方。

Happy bashing!

## The bash Idioms Style Guide

这是第 11 章要点的副本，但没有评论和示例。示例目录中还有一个 Markdown 文件，因此你可以下载并按需调整，然后使用 pandoc 或其他工具进行渲染或包含。从书的 GitHub 页面获取代码。

![](img/553ffa1bc15093729b691f706befc0ac_152_0.png)

## The bash Idioms Style Guide Is Not Portable

此 bash Idioms 风格指南专门针对 bash，因此不能移植到 POSIX、Bourne、Dash 或其他 shell。如果你需要为这些 shell 编写代码，你将需要测试和调整本指南以考虑这些 shell 支持的语法和功能。

在 Docker 或其他容器中，/bin/sh 不是 bash 且 /bin/bash 可能甚至不存在时尤其要小心！这适用于物联网和其他受限环境，例如工业控制器。参见第 ix 页的“bash in Containers”和第 94 页的“ Shebang!”。

## 可读性

代码的可读性很重要！或者正如 Python 所说，可读性很重要。你只写一次，但你（和其他人）可能会阅读很多次。花额外的几秒钟或几分钟思考明年试图阅读代码的可怜的糊涂的人……很可能就是你。抽象（不要重复自己）和可读性之间存在平衡和张力：

- KISS (Keep It Simple, Stupid!)。
- 可读性：不要“聪明”，要清晰。
- 好的名称至关重要！
- *始终使用头部。*
- 如果可能的话，为 -h、--help 和错误参数发出有用的内容！
  — 优先使用“here”文档（带前导制表符）而不是一堆 echo 行，因为以后需要更新和重新换行时摩擦更少。
- 使用 `source`（而不是 .，它很容易被忽略且更难搜索）来包含配置文件，这些文件应以 .cfg（或 .conf 或你标准的任何名称）结尾。
- 如果可能的话，对所有内容都使用 ISO-8601 日期。
- 如果可能的话，保持事物列表按字母顺序排列；这可以防止重复并使添加或删除项目更容易。示例包括 IP 地址（使用 GNU `sort -V`），主机名，要安装的软件包，`case` 语句，以及变量或数组/列表的内容。
- 如果可能，使用命令的长参数以提高可读性，例如，使用 `diff --quiet` 而不是 `diff -q`，不过要注意对非 GNU/Linux 系统的可移植性。
  — 如果任何选项很短或晦涩，请添加注释。
  — 强烈考虑记录你选择或需要选项的原因，甚至记录你考虑过但出于某种原因未使用的选项。
  — 务必记录任何可能看起来像个好主意但实际上可能导致问题的选项，尤其是如果你在其他地方经常使用它们。

## 注释

- *始终使用头部。*
- 为一年后团队中的新人写注释。
- 对你的函数进行注释。
- 不要对你做了什么进行注释。对你为什么做或不做某事进行注释。
  — 例外：当 bash 本身晦涩时，注释你做了什么。
- 考虑添加关于外部程序选项的注释，尤其是当它们很短或晦涩时。
- 在注释的第一词使用首字母大写，但在注释超过一句话时省略结束标点。

