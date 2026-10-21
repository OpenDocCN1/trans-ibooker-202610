

or:

```
# Only if *interactive* bash in a terminal!
if [ -t 1 -a -n "$PS1" -a -n "SBASH_VERSION" ]; then
  echo 'Interactive bash in a terminal'
else
  echo 'NOT interactive bash in a terminal'
fi
```

注意我们故意使用了 []（test）而不是 [[]]（bash 条件），因为其要点之一是此代码可能在非 bash 环境中运行，而该环境不支持 [[]]（例如 dash）。

## Summary

本章介绍的是那些并不漂亮但你终究需要知道的惯用法。有很多种方法可以在 bash 中处理文件，通常涉及重定向和/或管道，因此我们展示了一些典型的 bash 模式。Bash 脚本本身也是文件，因此我们谈到了将文件变成可执行脚本或库的魔法咒语，如何退出并清理，以及其他有用的惯用法。虽然“here-documents”并不完全是文件，但它们经常取代单独的文本文件，或文件中的文件，我们在别处使用过它们，所以这里也有。

## Beyond Idioms: Working with bash

代码往往会增长。复杂性往往会增长。跟我们一起说：“‘临时’解决方案比任何东西都更永久！” 把这句话刻在石头上，并在你编写代码、思考编写代码以及当你想“嘿，我想我可以把这个自动化”时牢记在心。提前规划。保持灵活。保持简单。对你自己和他人都使其易于使用。

好吧，但……什么？

本章收集了一些我们想谈论的额外有用材料，但它们无处可放。我们或许可以说这里已经超出了纯“惯用法”的范畴，但这一切都是关于每天在 bash 提示符下完成工作的。

当你编写那个“一次性”脚本时，花几分钟额外时间使用良好的名称并添加注释。最后，再花几分钟思考环境和代码可能如何演变，以及可能需要哪些更改或例外。在所有细节都清晰地记在脑海中时添加功能和灵活性，比六个月后要容易得多。这个代码真的“一次适合所有”（哈哈！），还是可能存在例外？case 语句能否根据 $HOSTNAME 处理一些调整，或者你是否必须询问用户问题？你的脚本需要参数还是临时文件？也许它需要做一些 $RANDOM 选择？继续阅读。

如今生活相当忙碌，工作更是如此。你能做些什么让它更简单？你能将源代码仓库结构与部署目标匹配起来，使其更容易映射事物去向吗？你能编写新的命令（脚本）来记住选项和细节吗？你能自动化原本繁琐的手动任务吗？很可能。我们希望我们在前几章中已经给你提供了很多工具来做所有这些，但在我们结束之前，我们还有一些想法可以增强灵活性和生活质量要谈论。

## Prompts

bash 提示符其实并没有真正的惯用法。你默认得到的提示符取决于你的 bash 来自何处以及配置文件如何被调整，每个用户都喜欢不同的提示符设置。但有一些事情需要注意。

我们不会详细介绍如何控制提示符——这在 bash 文档、*bash Cookbook*（配方 16.2 及其他地方）以及网络上都有涵盖。请参阅以下内容：

- https://www.gnu.org/software/bash/manual/html_node/Controlling-the-Prompt.html
- http://www.bashcookbook.com/bashinfo/source/bash-4.2/examples/scripts.noah/prompt.bash
- https://www.tldp.org/HOWTO/Bash-Prompt-HOWTO/index.html
- https://sourceforge.net/projects/bashish

我们将简要介绍不同的提示符，因为有一些新颖且鲜为人知的功能，如表 10-1 所示。

Table 10-1. bash prompts

| Prompt | Use | Default |
|---|---|---|
| PS0 | bash v4.4+ pre-execution prompt | None |
| PS1 | Bourne/bash primary prompt | '\s-\v\$ ' |
| PS2 | Bourne/bash secondary prompt | '> ' |
| PS3 | bash select prompt | '#? ' |
| PS4 | bash debug parameter | '+ ' |
| PROMPT_COMMAND | command before $PS1 | None |

### PS0

此提示符由交互式 shell 在你按下回车执行命令后，但在命令运行前显示。它可用于在命令运行前显示“开始时间”或其他标记（参见示例 10-1）。

### Example 10-1. Example bash pre-execution prompt

```
PS0='Start: \D{%Y-%m-%d_%H:%M:%S_%Z}\n'
```

```
### Note the difference between the prompt time and the start time
[user@hostname:T2:L1:C5289:J0:2021-08-27_17:46:04_EDT]
/home/user/bash-idioms$ ls -1 ch*
Start: 2021-08-27_18:47:37_EDT
ch01.asciidoc
...
```

### PS1

这是继承自 Bourne shell 的主要 bash 提示符，你在 bash 中几乎所有交互时间都花在这里，因此值得理解和调整。示例 10-2 中显示的提示符让一些人抓狂，但它很有用，因为它提供了你可能需要的所有信息，非常适合复制粘贴到文档或故障单中。不过有一个问题。日期/时间是在提示符显示时显示的，如果那是几小时或几天前，就会令人困惑。你可以通过意识到这个问题并在开始新命令或会话前按几次回车来解决这个问题。或者，如果你使用 bash v4.4+，你可以使用 PS0，如前例所示。请注意，C5275 命令历史编号在你把密码误输入 bash 历史记录时非常有用。只需 history -d 5275 就搞定了。（参见 help history，并为使用前导空格省略历史命令本身而加分，参见示例 10-13。）

### Example 10-2. Example bash primary prompt

```
export PS1='\n[\u@\h:T\l:$SHLVL:C\! :J\j :D{%Y-%m-%d_%H:%M:%S_%Z}]\n$PWD\$ '
```

```
[user@hostname:T2:L1:C5275:J0:2021-08-27_16:51:20_EDT]
/home/user/bash-idioms$
```

### PS2

当你按下回车但尚未完成命令时会出现此提示，可能因为你尚未关闭引号或 here-document。

### PS3

select 内置命令使用的提示符；参见第 107 页的“select”。

### PS4

当 set -x 激活时，这是调试提示符，或许前缀更合适。注意第一个字符会根据 shell 嵌套级别重复，因此你希望它清晰可见。+ 是一个不错的选择；另见第 120 页的“bash Debugging”。示例 10-3 或许有点繁忙，但信息丰富。

### Example 10-3. Example bash debug prompt

```
export PS4='+xtrace $BASH_SOURCE:$LINENO:$FUNCNAME: '
```

### PROMPT_COMMAND

示例 10-4 显示了一个在 $PS1 被求值并显示之前运行的命令。它用于各种用途，如更新 GUI 终端的窗口标题；显示有关环境的动态细节，例如你所在的 Git 分支；甚至进行一些非常原始且不安全的日志记录。

### Example 10-4. Example bash PROMPT_COMMAND for logging

```
### This should be one line; it is broken to fit the page
export PROMPT_COMMAND='logger -p local1.notice -t "bashlog[$S]" \
"SSH=$SSH_CONNECTION; USER=$USER; PATH=$PWD; COMMAND=$(fc -ln -1)"'
```

### bash Time Zone

这并不是真正的 bash 惯用法，但这是一个不错的提示符技巧。bash 提示符默认以系统时区显示日期和时间，但你可以 export TZ=UTC 或其他时区来更改它。如果你更喜欢将 GUI 保持在本地时区，但希望 bash 显示 UTC 以用于文档目的，或使本地 bash 时间显示与服务器显示匹配，这很有用。请注意，提示符 TZ 不会在你运行外部命令之前更改；仅按回车或运行 bash 内置命令不会导致它更新。

另见第 55 页的“Time for printf”了解更多日期和时间处理。

### Getting User Input

大多数时候，bash 脚本通过命令行参数获取用户输入，但有时你也可能需要询问输入。做到这一点有两种常见方法，即 read 和 select 内置命令。

另见：

- help read
- help select

### read

read 从 STDIN 或文件描述符读取一行并将单词分配给变量列表，或分配到数组（列表）变量中，并且它有许多非常有用的选项（示例 10-5 所示）。

