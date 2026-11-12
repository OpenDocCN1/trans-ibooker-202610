

### TABLE 7.13 用于过滤的常用 journalctl MATCHES 参数（续）

| 匹配项 | 说明 |
| --- | --- |
| `_TRANSPORT=transport` | 仅匹配通过指定 `transport` 方式接收的条目。 |
| `_UDEV_SYSNAME=dev` | 仅匹配来自指定设备的条目。 |
| `_UID=userid` | 仅匹配由指定用户 ID 生成的条目。 |

请记住，表 7.13 仅列出了几个常用的 `MATCHES`。要查看所有可用的各种 `MATCHES`，请在命令行中输入 `man systemd-journal-fields`。

> 要使用 journalctl 查看所有各种日志条目，您需要以 root 用户身份登录、使用超级用户权限，或者通常属于 systemd-journal 组。请注意，您的发行版可能为此任务使用不同的组。

当您在日志中查找特定事件条目时，请使用所需的筛选器和选项来定位特定项目。在截取的清单 7.36 中，仅显示 ssh.service systemd 服务单元的当日条目。

### 清单 7.36：在 journalctl 命令中使用筛选器

```
$ sudo journalctl --since=today _SYSTEMD_UNIT=ssh.service
-- Logs begin at Wed 2018-07-25 12:02:39 EDT, end at Wed 2019-05-29 [...]
May 29 14:10:50 Ubuntu1804 sshd[772]: Server listening on 0.0.0.0 port 22.
May 29 14:10:50 Ubuntu1804 sshd[772]: Server listening on :: port 22.
May 29 14:10:55 Ubuntu1804 sshd[772]: Received SIGHUP; restarting.
[...]
May 29 16:09:38 Ubuntu1804 sshd[2047]: Connection closed by 127.0.0.1 [...]
May 29 16:22:58 Ubuntu1804 sshd[2112]: Accepted password for Christine [...]
May 29 16:22:58 Ubuntu1804 sshd[2112]: pam_unix(sshd:session): session [...]
$
```

使用各种 journalctl 筛选器使浏览日志文件变得容易得多。

> journalctl 实用程序具有类似于在日志文件上使用 `tail -f` 的功能。只需使用 journalctl `-f` 或 `--follow` 开关，您就会看到最后几条条目以及添加到日志中的其他条目。观看完毕后，按 Ctrl+C 组合键退出。

## 维护日志

除了配置持久性日志并控制日志磁盘使用情况外，您还可以采用一些手动管理活动来维护您的日志文件。

您可以使用 journalctl --disk-usage 命令检查日志文件的当前磁盘使用情况。在 Ubuntu 发行版上的示例显示在清单 7.37 中。

### 清单 7.37：检查日志文件磁盘使用情况

```
$ journalctl --disk-usage
Archived and active journals take up 344.0M in the file system.
$
```

清单 7.37 中命令的输出显示了所有日志文件（包括活动和归档文件）在分区上占用了多少当前磁盘空间。

虽然 systemd-journald 可以通过 journald.conf 文件中的设置自动清理磁盘空间，但您也可以手动执行此操作。在这种情况下，您可以使用 journalctl 命令中提供的 vacuum... 好吧，实际上是 vacuum 选项：

- --vacuum-size
- --vacuum-time

如您所料，--vacuum-size 会删除日志文件，直到日志文件占用的磁盘空间达到指定大小。您在选项后跟一个数字并附加一个单位（K、M、G 或 T）来设置大小。请注意，这只会删除归档的日志文件，对任何活动的日志文件都没有影响。

对于 --vacuum-time 选项，您可以指定允许的最旧日志条目，其余条目将被删除。时间用数字和时间单位（s、min、h、days、months、weeks 或 years）表示，例如 10months。与大小选项一样，时间选项仅影响归档的日志文件。活动文件保持不变。

如果需要，您可以组合这两个不同的开关，但清单 7.38 中的截取示例仅使用大小选项来减少磁盘使用量。

### 清单 7.38：清理日志文件磁盘使用情况

```
$ journalctl --disk-usage
Archived and active journals take up 344.0M in the file system.
$
$ sudo journalctl --vacuum-size=300M
Deleted archived journal
[...]
Vacuuming done, freed 24.0M of archived journals from /var/log/journal/
e9af6ca5a8fb4a70b2ddec4b1894014d.
$
$ journalctl --disk-usage
Archived and active journals take up 320.0M in the file system.
$
```

请注意，在完成清理后，又发出了一次 journalctl --disk-usage 命令，但它显示 320.0M 而不是 vacuum 选项上设置的 300M 大小。这是因为，虽然 disk-usage 开关显示活动和归档日志文件，但 vacuum 选项仅适用于归档日志文件。

> 如果您想备份活动的日志文件，直接先复制它们应该即可。但是，在复制之前，请运行 journalctl --sync 命令，以确保所有条目都已从队列移动到文件中。

## 查看不同的日志文件

如果您需要从已救援的系统中检索日志文件但先查看它，或查看归档或复制的日志文件中的条目，可以使用一些 journalctl 开关来提供帮助。

由于 journalctl 在 /run/log/journal 或 /var/log/journal 目录中查找活动日志文件，您可以使用 -D directory-name 或 --directory=directory-name 选项将其指向包含已复制或其他系统日志文件的不同目录位置。

如果您尝试查看的文件名与 system.journal 或 user-UID.journal 不同，请在 journalctl 命令上使用 --file=pattern 选项。将 pattern 设置为您希望查看的文件的确切名称。但是，如果有多个文件，您可以在模式中使用文件通配符（文件通配符在第 4 章中介绍过）来匹配多个文件。

如果您最近救援了系统，现在有两个或多个带有条目的日志文件需要查看，您可以合并它们。为此，请在 journalctl 实用程序上使用 -m 或 --merge 开关。请记住，这不会物理合并日志文件，而是在输出中合并它们的条目供您查阅。

> systemd-journald 日志实用程序允许您通过 systemd-journal-remote 将系统的日志发送到集中式日志主机系统。要查看中央主机上所有各种日志文件的条目，使用 journalctl 命令时需要使用 -m 或 --merge 选项。

## 创建日志条目

类似于为 syslog 协议程序使用 logger，您可以使用 systemd-cat 工具从命令行或脚本中添加日志条目。为此，必须将命令的 STDOUT 通过管道传输到实用程序中：

```
command | systemd-cat
```

使用 systemd-cat 命令的示例显示在清单 7.39 中。请注意，测试消息已成功添加到活动日志文件。

### 清单 7.39：使用 systemd-cat 实用程序添加日志条目

```
$ echo "Test of systemd-cat" | systemd-cat
$
$ journalctl --no-pager | grep systemd-cat
May 30 17:43:46 Ubuntu1804 cat[2599]: Test of systemd-cat
$
```

如果您的系统允许通过加载 `imuxsock` 模块使已安装的 syslog 协议程序充当日志客户端，您也可以使用 `logger` 实用程序来创建日志条目。此示例在清单 7.40 中有截取。

### 清单 7.40：使用 logger 实用程序添加日志条目

```
$ logger "Test of logger"
$
$ journalctl -r
-- Logs begin at Thu 2018-07-26 18:19:45 EDT, end at Thu 2019-05-30 [...]
[...]
May 30 17:45:29 Ubuntu1804 Christine[2606]: Test of logger
[...]
May 30 17:43:46 Ubuntu1804 cat[2599]: Test of systemd-cat
[...]
$
```

通过在 `journalctl` 命令上使用 `-r` 选项（反向），您可以查看最近的条目，在本例中，这包括 `systemd-cat` 和 `logger` 实用程序的条目。请注意，`logger` 会将用户名添加到条目中。

正确控制您的日志和日志条目有助于排查问题以及追踪潜在的安全问题。接下来，我们将探讨一种与管理日志和日志条目密切相关的概念。

## 维护系统时间

在所有服务器上保持正确的时间至关重要。许多元素依赖于准确的时间，例如设计为在特定时刻运行的程序、期望准确客户端时间的远程服务（如果时间不准确，它们会拒绝客户端），以及保持准确的日志消息时间戳，以便正确调查客户端/服务器问题。

## 了解 Linux 时间概念

本地时间也称为 _墙钟时间_。历史上，人们会看一眼墙上的时钟（或他们的模拟手表）来确定何时参加预定会议、吃午餐以及下班回家。这的官方标准名称是 _本地时间_。

