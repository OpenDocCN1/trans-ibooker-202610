

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
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch10/select-ssh.sh
# ----------------------------------------------------------------------------

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

## 别名

既然你正在阅读本书，你可能已经知道 alias 和 unalias 命令，但我们想涵盖几个重要点。首先，最佳安全做法是在需要删除别名时使用 \unalias。开头的 \ 会抑制别名扩展，从而防止恶意的 unalias alias 造成混乱。

其次，一些 Linux 发行版喜欢为 root 或所有用户设置“有帮助”的别名，因此你需要探索你发行版和环境中的默认值。特别是，你会看到 alias rm=rm -i 以及 cp 和 mv 的类似别名，你可能觉得这些别名可取也可能不可取。

示例 10-9 和 10-10 显示了两组你可能考虑使用的别名，或许放在你的 ~/.bashrc 文件中。将其中一些或全部放入你主目录的 .bashrc 文件中。

## 示例 10-9.  trivial DOS/Windows “兼容”

```
alias cls='clear'          # DOS-ish for clear
alias copy='cp'            # DOS-ish for cp
alias del='rm'             # DOS-ish for rm
alias dir='ls'             # DOS-ish for ls
alias ipconfig='ifconfig'  # Windows-ish for ifconfig
alias md='mkdir'           # DOS-ish for mkdir
alias move='mv'            # DOS-ish for mv
alias rd='rmdir'           # DOS-ish for rmdir
alias ren='mv'             # DOS-ish for mv/rename
alias tracert='traceroute' # DOS-ish for traceroute
```

## 示例 10-10. 示例别名

```
# Install `xclip` or `xsel` for Linux copy and paste
alias gc='xsel -b'        # "GetClip" get stuff from right "X" clipboard
alias pc='xsel -bi'       # "PutClip" put stuff to right "X" clipboard
# Or Mac: pbcopy/pbpaste
# Or Windows: gclip.exe/pclip.exe or getclip.exe/putclip.exe

# Cleaner `df`
alias df='df --print-type --exclude-type=tmpfs --exclude-type=devtmpfs'
alias diff='diff -u'      # Make unified diffs the default
alias locate='locate -i'  # Case-insensitive locate
alias ping='ping -c4'     # Only 4 pings by default
alias vzip='unzip -lVm'   # View contents of ZIP file
alias lst='ls -lrt | tail -5' # Show this dir's 5 most recently modified files

# Tweaked from
# https://oreil.ly/1SUg7
```

```
alias randomwords="shuf -n102 /usr/share/dict/words | perl -ne 'print qq(\u$\_);' \
| column"
```

有时让同一命令在不同主机上工作方式不同会很有用。你可以用 case 语句轻松做到这一点（示例 10-11）。

## 示例 10-11. 不同主机的不同别名

```
case "$HOSTNAME" in
  host1* )  # Linux, but only has `xclip`
    alias gc='xclip -out'      # Send X selection to STDOUT
    alias pc='xclip -in'       # Send STDIN to X selection
  ;;

  host2* )  # Mac
    alias gc='pbpaste'         # Send Paste Buffer to STDOUT
    alias pc='pbcopy'          # Send STDIN to Paste Buffer
  ;;

  * )       # Linux default
    alias gc='xsel -b'        # Send X clipboard to STDOUT
    alias pc='xsel -bi'       # Send STDIN to X clipboard
  ;;
esac
```

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

