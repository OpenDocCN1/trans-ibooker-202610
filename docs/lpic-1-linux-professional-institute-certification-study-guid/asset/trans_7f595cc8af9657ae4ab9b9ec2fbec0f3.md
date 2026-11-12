

## 清单 9.21: 演示作业控制（续）

```
This is a test program 29090
Loop #1

[2]+ Stopped       ./test19.sh
$ ./test19.sh

This is a test program 29105
Loop #1

[3]+ Stopped       ./test19.sh
$ jobs -l
[1]  29075 Stopped       ./test19.sh
[2]- 29090 Stopped       ./test19.sh
[3]+ 29105 Stopped       ./test19.sh
$ kill -9 29105
$ jobs -l
[1]- 29075 Stopped       ./test19.sh
[2]+ 29090 Stopped       ./test19.sh
$
```

在清单 9.21 中，我们启动了三个独立进程，然后将其停止。jobs 命令列表显示了这三个进程及其状态。请注意，根据 PID 编号，默认进程（即带加号的进程）是 29105，也就是最后启动的进程。
我们随后使用 kill 命令向默认进程发送了 SIGHUP 信号。在下一个 jobs 列表中，之前带减号的作业 29090 现在是默认作业。

## 重启停止的作业

在 Bash 作业控制下，您可以将任何停止的作业重新启动为后台进程或前台进程。前台进程会接管您正在使用的终端的控制权，因此使用该功能时要小心。

要在后台模式下重启作业，请使用 bg 命令，并指定作业编号：

```
$ bg 2
[2]+ ./test20.sh &
Loop #2
$ Loop #3
Loop #4
```

```
$ jobs
[1]+ Stopped                 ./test20.sh
[2]- Running                 ./test20.sh &
```

```
$ Loop #6
Loop #7
Loop #8
Loop #9
Loop #10
This is the end of the test program

[2]- Done ./test20.sh
$
```

由于我们以后台模式重启了作业，命令行界面提示符会出现，允许我们继续执行其他命令。jobs 命令的输出现在显示该作业确实正在运行（您可以从现在显示在监视器上的输出中看出这一点）。

要在前台模式下重启作业，请使用 fg 命令，并指定作业编号：

```
$ jobs
[1]+ Stopped ./test20.sh
$ fg 1
./test20
Loop #2
Loop #3
```

由于作业正在前台模式下运行，我们在作业完成之前不会收到新的命令行界面提示符。

## 如钟表般准时运行

我相信，当您开始使用脚本时，会遇到希望在预设时间运行脚本的情况，通常是在您不在场的时候。在预选时间运行脚本有两种常见方法：

- at 命令
- cron 表

每种方法都使用不同的技术来安排脚本运行的时间和频率。以下部分将介绍这些方法中的每一种。

## 使用 at 命令调度作业

at 命令允许您指定 Linux 系统运行脚本的时间。它会将作业提交到队列中，并指示 shell 在何时运行该作业。另一个命令 atd 在后台运行并检查作业队列中是否有要运行的作业。大多数 Linux 发行版在启动时会自动启动此服务。

atd 命令会检查系统上的一个特殊目录（通常为 /var/spool/at），查找使用 at 命令提交的作业。默认情况下，atd 命令每隔 60 秒检查一次此目录。如果存在作业，atd 命令会检查作业设置的运行时间。如果时间与当前时间匹配，atd 命令会运行该作业。以下部分将介绍如何使用 at 命令提交要运行的作业以及如何管理作业。

### at 命令格式

基本的 at 命令格式非常简单：

```
at [-f filename] time
```

默认情况下，at 命令将来自 STDIN 的输入提交到队列。您可以使用 -f 参数指定用于读取命令（您的脚本文件）的文件名。

time 参数指定您希望 Linux 系统运行作业的时间。您可以非常灵活地指定时间。at 命令支持多种不同的时间格式：

- 标准小时和分钟，例如 10:15
- AM/PM 指示符，例如 10:15PM
- 特定命名时间，例如 now、noon、midnight 或 teatime（4PM）

如果您指定的时间已过，at 命令将在第二天的该时间运行作业。

除了指定运行作业的时间外，您还可以使用几种不同的日期格式包含特定日期：

- 标准日期格式，例如 MMDDYY、MM/DD/YY 或 DD.MM.YY
- 文本日期，例如 Jul 4 或 Dec 25，可包含年份，也可不包含
- 您还可以指定时间增量：
- 现在 + 25 分钟
- 明天 10:15PM
- 10:15 + 7 天

使用 at 命令时，作业会被提交到作业队列。作业队列保存由 at 命令提交的作业以供处理。共有 26 个不同的作业队列可用于不同优先级别。作业队列使用小写字母 a 到 z 引用。默认情况下，所有 at 作业都提交到作业队列 a，即最高优先级队列。如果您希望以降低优先级运行作业，可以使用 -q 参数指定字母。

### 检索作业输出

作业在 Linux 系统上运行时，没有与之关联的显示器。相反，Linux 系统使用提交作业的用户的电子邮件地址。任何发往 STDOUT 或 STDERR 的输出都会通过邮件系统发送给用户。

## 清单 9.22: 使用 at 命令启动作业

```
$ date
Thu Feb 28 18:48:20 EST 2019
$ at -f test3.sh 18:49
job 2 at Thu Feb 28 18:49:00 2019
$ mail
Heirloom Mail version 12.5 7/5/10. Type ? for help.
"/var/spool/mail/rich": 1 message 1 new
>N  1 Rich                 Thu Feb 28 18:49  15/568   "Output from your job "
&
Message 1:
From rich@localhost.localdomain  Thu Feb 28 18:49:00 2019
Return-Path: <rich@localhost.localdomain>

X-Original-To: rich
Delivered-To: rich@localhost.localdomain
Subject: Output from your job     2
To: rich@localhost.localdomain
Date: Thu, 28 Feb 2019 18:49:00 -0500 (EST)
From: rich@localhost.localdomain (Rich)
Status: R

"This script ran at 18:49:00"
"This is the end of the script"
&
```

如清单 9.22 所示，运行 at 命令时，它会生成一条警告消息，指示系统用于运行脚本的 shell（分配给 /bin/sh 的默认 shell，在 Linux 中为 Bash shell），以及分配给作业的作业编号和作业计划运行的时间。

作业完成后，显示器上不会显示任何内容，但系统会生成一封电子邮件。电子邮件显示脚本生成的输出。如果脚本没有产生任何输出，默认情况下不会生成电子邮件。您可以通过在 at 命令中使用 -m 选项来更改此行为。即使脚本没有生成任何输出，这也会生成一封指示作业已完成的电子邮件。

### 列出待处理作业

atq 命令允许您查看系统上待处理的作业：

```
$ at -f test21.sh 19:15
warning: commands will be executed using /bin/sh
job 7 at 2007-11-04 10:15
$ at -f test21.sh 4PM
warning: commands will be executed using /bin/sh
job 8 at 2007-11-03 16:00
$ at -f test21.sh 1PM tomorrow
warning: commands will be executed using /bin/sh
job 9 at 2007-11-04 13:00
$ atq
7 2007-11-04 10:15 a
8 2007-11-03 16:00 a
9 2007-11-04 13:00 a
$
```

作业列表显示作业编号、系统将运行作业的日期和时间，以及作业所在的作业队列。

### 删除作业

了解作业队列中待处理作业的信息后，可以使用 atrm 命令删除待处理作业：

```
$ atrm 8
$
$ atq
7 2007-11-04 10:15 a
9 2007-11-04 13:00 a
$
```

只需指定要删除的作业编号即可。您只能删除自己提交执行的作业，不能删除其他人提交的作业。

![](img/ca7617340ddbe2360d36ce3428729fa9_563_0.png)

> 如果您使用的是采用 systemd 启动方法的 Linux 发行版，也可以使用 systemd-run 命令安排作业在特定时间运行。systemd-run 命令使用的格式有些复杂，但它允许您精确调整作业运行的具体时间（精确到毫秒，为您提供了大量选项。要列出使用 systemd-run 命令调度的作业，需要使用 systemctl 命令。如果您想了解有关 systemd-run 语句的更多信息，请查阅 Linux 系统文档。

