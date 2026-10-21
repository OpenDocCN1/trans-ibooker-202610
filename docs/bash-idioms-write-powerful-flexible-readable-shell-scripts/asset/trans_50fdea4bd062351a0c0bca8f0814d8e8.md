

# 创建测试文件（不拼写出“word”以保持输出 < 80 列）
IFS_TEST_FILE='/tmp/ifs-test.txt'
cat <<'EoF' > $IFS_TEST_FILE
line1 wd1 wd2 wd3
line2 wd1 wd2 wd3
line3 wd1 wd2 wd3
EoF

#-------------------------------------------------------------------------------
echo 'Normal $IFS and `read` operation; split into words:'
printf 'IFS before: %q\n' "$IFS"
while read line w1 w2 w3; do
    printf 'IFS during: %q\nline = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

#-------------------------------------------------------------------------------
echo ''
echo 'Temporary $IFS change for `read` inline:'
echo 'Words are NOT split, yet $IFS appears unchanged, because only the read'
echo 'line has the changed $IFS. We also shortened "line" to "ln" to make'
echo 'it fit a book page.'
printf 'IFS before: %q\n' "$IFS"
while IFS='' read line w1 w2 w3; do
    printf 'IFS during: %q\nln = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

#-------------------------------------------------------------------------------
function Read_A_File {
    local file="$1"
    local IFS=''

    while read line w1 w2 w3; do
    printf 'IFS during: %q\nline = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
    done < $IFS_TEST_FILE
}

echo ''
echo 'Temporary $IFS change for `read` in a function; NOT split, $IFS changed:'
printf 'IFS before: %q\n' "$IFS"
Read_A_File
printf 'IFS after: %q\n' "$IFS"

#-----------------------------
echo ''
echo 'But you may not need to change $IFS at all... See `help read` and'
echo 'note the parts about:'
echo '    ...leftover words assigned to the last NAME'
echo '    ...[read line until] DELIM is read, rather than newline'
echo 'Normal $IFS and `read` operation using only 1 variable:'
printf 'IFS before: %q\n' "$IFS"
while read line; do
    printf 'IFS during: %q\tline = %q\n' "$IFS" "$line"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

The output is shown in Example 9-7.

# Example 9-7. Changing IFS for read example: output

```
Normal $IFS and `read` operation; split into words:
$IFS before: $' \t\n'
IFS during: $' \t\n'      line = line1, w1 = wd1, w2 = wd2, w3 = wd3
IFS during: $' \t\n'      line = line2, w1 = wd1, w2 = wd2, w3 = wd3
IFS during: $' \t\n'      line = line3, w1 = wd1, w2 = wd2, w3 = wd3
IFS after:  $' \t\n'
```

Temporary $IFS change for `read` inline:
Words are NOT split, yet $IFS appears unchanged, because only the read line has the changed $IFS. We also shortened "line" to "ln" to make it fit a book page.

```
IFS before: $' \t\n'
IFS during: $' \t\n'      ln = line1\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: $' \t\n'      ln = line2\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: $' \t\n'      ln = line3\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS after:  $' \t\n'
```

Temporary $IFS change for `read` in a function; NOT split, $IFS changed:

```
IFS before: $' \t\n'
IFS during: '' line = line1\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: '' line = line2\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: '' line = line3\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS after:  $' \t\n'
```

But you may not need to change $IFS at all... See `help read` and note the parts about:
    ...leftover words assigned to the last NAME
    ...[read line until] DELIM is read, rather than newline
Normal $IFS and `read` operation using only 1 variable:

```
IFS before: $' \t\n'
IFS during: $' \t\n'      line = line1\ wd1\ wd2\ wd3
IFS during: $' \t\n'      line = line2\ wd1\ wd2\ wd3
IFS during: $' \t\n'      line = line3\ wd1\ wd2\ wd3
IFS after:  $' \t\n'
```

See also:

+   - “Local Variables” on page 110
+   - “Reading Files” on page 85
+   - “ANSI-C Quoting” on the bash man page

## Pretend Files

我们都知道 UNIX、Linux 以及当然的 bash 认为一切都是文件，对吧？一切都是文件。那就是 The Way。但如果您只需要处理文件的一部分会怎样？也许您有一组节点定期报告统计信息，您想通过 diff 查看是否有节点出现或消失。让我们假设文件是制表符分隔的，并且每个节点有多条记录，节点名称作为第一个字段，但您按小时、天或任意时间聚合它们。

您可以使用临时文件，尽管这很繁琐：

```
cut -f1 /path/to/previous-report.log | sort -u > /tmp/previous-report.log
cut -f1 /path/to/current-report.log | sort -u > /tmp/current-report.log
diff /tmp/previous-report.log /tmp/current-report.log
rm /tmp/previous-report.log /tmp/current-report.log
```

相反，您可以使用这些“假文件”来做同样的事情：

```
diff <(cut -f1 /path/to/previous-report.log | sort -u) \
    <(cut -f1 /path/to/current-report.log | sort -u)
```

好吧，它其实叫做进程替换（在示例 9-8 中说明），但我们觉得假文件听起来更有趣。

Example 9-8. Simple process substitution example

```
$ head *report.log
==> current-report.log <==
always-talking
always-talking
always-talking
always-talking

==> previous-report.log <==
always-talking
going-away
always-talking
going-away
always-talking
going-away
always-talking
going-away
```

```
$ diff -u <(cut -f1 previous-report.log | sort -u) \
<(cut -f1 current-report.log | sort -u)
--- /dev/fd/63 2022-01-09 20:01:37.857658587 -0500
+++ /dev/fd/62 2022-01-09 20:01:37.857658587 -0500
@@ -1,2 +1 @@
always-talking
-going-away
```

## Drop-in Directories

在很多不同的上下文中都能看到的一种习惯用法就是一个目录，您可以向其中添加或删除文件以实现某种效果。这对于使用 RPM（Red Hat Package Manager）或 Debian APT（Advanced Package Tool）等包管理系统 Linux 发行版来说非常方便，因为包可以在安装时添加文件或在卸载时删除文件，而无需以编程方式编辑文件。/etc/cron.d/ 就是一个完美的例子；包可以在安装时轻松添加 cron 作业，并在卸载时删除这些作业，而不影响其他任何东西。

如果支持，我们非常喜欢这种技术用于配置文件。例如，您可以有一个 /etc/syslog-ng/syslog-ng.conf 文件，其中包含 @include "/etc/syslog-ng/syslog-ng.local.d"，然后您可以将文件放入 /etc/syslog-ng/syslog-ng.local.d/ 以根据特定节点的需求调整配置。

您也可以在 bash 代码中非常容易地做到这一点（示例 9-9）。

Example 9-9. Include drop-in files

```
# Source local overrides IF any exist AND are executable
for config in /etc/myscript.cfg.d/*; do
  # Override files MUST be EXECUTABLE or they will be ignored, which makes
  # it easy to disable them.
  [ -x "$config" ] && source "$config"
  # Or change `-x` to `-r` to use readable instead of executable
done
```

这个想法建立在第 23 页的“Wrapper Scripts”之上，因此您可以为自己或用户构建简单、可扩展且功能强大的工具。您编写一个简单的骨架脚本，其中包含一些逻辑来在目录中查找“action”命令，以及一些胶水代码来将这些命令组合在一起并提供使用或帮助。然后您添加一个新模块或操作模板，您或您的用户只需复制模板并填写详细信息即可轻松添加新功能。

## Using a Library

如果您有大量经常使用的代码，也许是一个使用 printf %(datefmt)T 的 Log 函数（参见第 57 页的“Getting or Using the Date and Time”），将其放入库中以减少“WET”¹ 代码和维护是有意义的。当然，一旦这样做，您就会遇到通常的库问题：部署和调用它。部署取决于您自己的通用流程和实践，但我们可以帮助调用它。

具有讽刺意味的是，您将有重复的“样板”代码来调用您的库，并且您无法将其提取到库中，否则导入过程会变得循环。但由于您有用于头注释等的模板（对吧？），您可以只添加示例 9-10 中显示的代码。

# Example 9-10. Sourcing a bash library

```
# Source our library
source /path/to/global/bash-library.lib || {
    echo "FATAL: $0 can't source '/path/to/global/bash-library.lib'"
    exit 1
}
```

我们本可以定义 GLOBAL_LIBRARY=/path/to/global/bash-library.lib，但对于样板代码中的两次使用，似乎不值得。如果您足够聪明，您会将一组全局配置变量构建到该库中，并且您将允许使用节点本地 drop-in 目录（参见第 93 页的“Drop-in Directories”）覆盖变量甚至库函数。

## Shebang!

是什么让一个普通的旧文件成为 shell 脚本？Shebang！

我们认为“shebang”听起来像是一个咒语，事实上它是一个神奇的咒语，告诉内核（而不是守护进程）如何处理您的代码。我们说的是脚本的第一行 #!/bin/bash 或可能是 #!/bin/sh，当然。#! 巧妙地充当注释和魔数，告诉内核查找解释器，在本例中为 bash。

Linux 上最常见的 bash shebang 是 #!/bin/bash，但您经常会看到 #!/bin/sh 和 #!/bin/bash -，您可能会看到 #!/usr/bin/env bash、#!/usr/bin/bash 或其他路径。

1 WET: We Enjoy Typing，与 DRY: Don’t Repeat Yourself 相反。

