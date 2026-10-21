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
