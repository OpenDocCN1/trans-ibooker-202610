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
