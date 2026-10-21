### Getting User Input

大多数时候，bash 脚本通过命令行参数获取用户输入，但有时你也可能需要询问输入。做到这一点有两种常见方法，即 read 和 select 内置命令。

另见：

- help read
- help select

### read

read 从 STDIN 或文件描述符读取一行并将单词分配给变量列表，或分配到数组（列表）变量中，并且它有许多非常有用的选项（示例 10-5 所示）。





## 示例 10-5. bash 中 read 的帮助

```
$ bash --version
GNU bash, version 4.4.20(1)-release (x86_64-pc-linux-gnu)
...
```

```
$ help read
read: read [-ers] [-a array] [-d delim] [-i text] [-n nchars] [-N nchars] [-p prompt] [-t timeout] [-u fd] [name ...]
Read a line from the standard input and split it into fields.
```

从标准输入读取一行，或如果提供了 -u 选项则从文件描述符 FD 读取。该行会被像词分割一样拆分为字段，第一字段分配给第一个 NAME，第二字段分配给第二个 NAME，依此类推，剩余的词分配给最后一个 NAME。只有 $IFS 中找到的字符才会被识别为词分隔符。

如果没有提供 NAME，则读取的行会存储在 REPLY 变量中。

### 选项：

+   -a array    将读取的词按顺序分配给数组变量 ARRAY 的索引，从零开始
+   -d delim    继续读取直到读取到 DELIM 的第一个字符，而不是换行符
+   -e          在交互式 shell 中使用 Readline 获取该行
+   -i text     使用 TEXT 作为 Readline 的初始文本
+   -n nchars   读取 NCHARS 个字符后返回，而不是等待换行符，但在读取到分隔符之前如果字符少于 NCHARS 则仍会遵守分隔符
+   -N nchars   仅在读取到恰好 NCHARS 个字符后返回，除非遇到 EOF 或 read 超时，忽略任何分隔符
+   -p prompt   在尝试读取之前输出字符串 PROMPT 且不带尾随换行符
+   -r          不允许反斜杠转义任何字符
+   -s          不回显来自终端的输入
+   -t timeout  如果在 TIMEOUT 秒内未读取到完整的输入行，则超时并返回失败。TMOUT 变量的值是默认超时。TIMEOUT 可以是小数。如果 TIMEOUT 为 0，read 会立即返回，不尝试读取任何数据，仅在指定文件描述符上已有输入可用时返回成功。退出状态大于 128 表示超时已超出
+   -u fd       从文件描述符 FD 而不是标准输入读取

### 退出状态：

返回代码为零，除非遇到文件结束、read 超时（此时大于 128）、变量赋值错误，或作为 -u 参数提供的无效文件描述符。

一个非常有用的选项组合是 -eip，它允许你提示输入数据（-p），同时提供预填充的默认值（-i），并允许使用 Readline 功能（如箭头键；参见示例 10-13）来在接受之前编辑默认值（参见示例 10-6）。

## 示例 10-6. 为用户输入提供可编辑的默认值

```
$ read -ei 'default value' -p 'Enter something: '
Enter something: default value
### Then we use readline features, like arrow keys, to change it
Enter something: changed: default value

$ echo $REPLY
changed: default value
```

另一个方便的选项是 -s，用于在输入密码时不显示密码（示例 10-7）。

## 示例 10-7. 提示输入密码

```
$ read -sp 'Enter the secret password: ' ; \
echo -e "\n\nShhhh, the password is: ~$REPLY~"
Enter the secret password:

Shhhh, the password is: ~super secret~
```

对于你的临时脚本，你也可以这样做，但请记住，没有什么比“临时”解决方案更永久的了：

```
read -n1 -p 'CTRL-C to abort or any other key to continue...'
```

超时功能也很棒，用于请求可选输入但不会在无人时完全挂起脚本（参见示例 10-8）。

## 示例 10-8. 带超时的输入提示

```
$ time read -t 4 -p "Are you there?"
Are you there?
real    0m4.000s
user    0m0.000s
sys     0m0.000s
```

read 也用于读取文件和解析输入，正如我们在第 85 页“read”中讨论的那样。

## pause

这主要是为了好玩，但如果你记得旧的 DOS/Windows pause 命令，在 bash 中你这样做：

```
$ read -n1 -p 'Press any key to continue...'
Press any key to continue...
```

## select

顾名思义，select 创建一个简单的菜单并允许你选择一个选项。是否要创建退出或中止选项，由你决定。类似的工具 dialog 拥有更多功能，但那不是 bash 的习惯用法，所以我们不涵盖它。

一个简单的例子：

```
#!/usr/bin/env bash
# select-ssh.sh: Create a menu from ~/.ssh/config to set ssh_target,
# then SSH to it
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch10/select-ssh.sh
##### ----------------------------------------------------------------------------

#ssh_config="$HOME/.ssh/config"  # Real one
# Replace the trailing 'select-ssh.sh' with 'ssh_config'
ssh_config="${0%/*}/ssh_config"  # Idioms test file

PS3='SSH to> '
select ssh_target in Exit \
  $(egrep -i '^Host \w+' "$ssh_config" | cut -d' ' -f2-); do
  case $REPLY in
    1|q|x|quit|exit) exit 0
    ;;
    *) break
    ;;
  esac
done

# This is only an example, so echo what we would have done
echo ssh $ssh_target
```

运行时看起来像这样：

```
$ examples/ch10/select-ssh.sh
1) Exit              3) gitlab.com          5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com          6) kitchen
SSH to> 1

$ examples/ch10/select-ssh.sh
1) Exit              3) gitlab.com          5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com          6) kitchen
SSH to> 6
ssh kitchen
```
