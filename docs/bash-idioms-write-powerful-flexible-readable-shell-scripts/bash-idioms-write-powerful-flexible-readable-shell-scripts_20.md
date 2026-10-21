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
