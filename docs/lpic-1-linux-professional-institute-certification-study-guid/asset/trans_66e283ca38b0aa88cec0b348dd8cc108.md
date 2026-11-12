

## 清单 7.33: 查看 CentOS 发行版中的 /etc/logrotate.d/ 文件

```
$ ls /etc/logrotate.d/
bootlog glusters libvirtd.qemu psacct syslog
chrony iscsiuiolog numad samba wpa_supplicant
cups libvirtd ppp sshd yum
$
$ cat /etc/logrotate.d/bootlog
/var/log/boot.log
{
missingok
daily
copytruncate
rotate 7
notifempty
}
$
```

请注意，/etc/logrotate.d/boot.log 的内容与 /etc/logrotate.conf 文件中 /var/log/wtmp 和 /var/log/btmp 日志文件的设置类似。各种日志文件的几个更常见的特定 logrotate 指令在表 7.10 中显示。

## 表 7.10 针对特定日志文件的更常见 logrotate 指令

| Directive | Description |
| :--- | :--- |
| hourly | 日志文件按小时轮转。如果启用此设置，logrotate cron 作业的计划通常需要修改。 |
| daily | 日志文件按天轮转。 |
| weekly n | 日志文件按周轮转，在每周的第 n 天轮转，其中 0 等于星期日，1 等于星期一，2 等于星期二，以此类推，6 等于星期六。7 是一个特殊数字，表示日志文件每 7 天轮转一次，无论当前是星期几。 |
| monthly | 日志文件在当前月内首次运行 logrotate 时轮转。 |
| size n | 基于大小而非时间轮转日志文件，其中 n 表示触发轮转的文件大小（n 后面不加任何内容或 k 假设为千字节，M 表示兆字节，G 表示千兆字节）。 |
| rotate n | 被轮转超过 n 次的日志文件将根据其他指令被删除或发送邮件。如果 n 等于 0，轮转的文件将被删除，而不是被轮转。 |
| dateformat format-string | 使用 _格式字符串_ 规范修改 dateext 设置的日期字符串。 |
| missingok | 如果日志文件丢失，不发出错误消息，继续处理下一个日志文件。 |
| notifyempty | 如果日志文件为空，不轮转此日志文件，继续处理下一个日志文件。 |

您可以设置更多指令来精细调整日志文件的轮转。要查看所有各种设置，请在命令行键入 **man logrotate**。

许多发行版在 /var/lib/logrotate/ 目录中维护一个 logrotate 状态文件，这对于故障排除或查看最新的日志轮转很有帮助。在 CentOS 上，文件名为 logrotate.status，在 Ubuntu 上，它是状态文件。

## 制作日志条目

如果您在 Linux 系统上创建并运行脚本（参见第 9 章），您可能希望记录自己的应用程序事件。您可以使用 _logger_ 命令行工具完成此操作：

```
logger [-isd] [-f _file_] [-p _priority_] [-t _tag_] [-u _socket_] [_message_]
```

-i 选项指定创建日志条目的程序的进程 ID（PID），作为事件消息的一部分。-p 选项允许您指定事件优先级。-t 选项允许您指定一个标签，添加到事件消息中，以帮助更轻松地在日志文件中找到该消息。您可以将消息作为命令行中的文本指定，或使用 -f 选项将其指定为文件。-d 和 -u 切换开关是用于将错误消息发送到网络的高级选项。-s 选项将事件消息发送到标准错误输出。

在脚本中使用 _logger_ 的示例如下：

```
$ logger This is a test message from rich
```

在 Ubuntu 系统上，您可以查看 /var/log/syslog 文件的末尾以查看日志条目：

```
$ tail /var/log/syslog
```

```
...
Feb 8 20:21:02 myhost rich: This is a test message from rich
```

请注意，rsyslogd 为该消息添加了时间戳、主机和用户帐户。这是一个非常好的故障排除工具！

## 查找事件消息

通常，大多数 Linux 发行版在 /var/log 目录中创建日志文件。取决于 Linux 系统的安全性，许多日志文件所有人都可读，但有些可能不可读。

如前面在清单 7.29 和清单 7.30 中所示，大多数 Linux 发行版为不同的事件消息类型创建独立的日志文件，尽管它们不总是对日志文件名达成一致。

单个应用程序在 /var/log 目录下拥有用于其自身应用程序事件消息的单独目录也是常见的，例如 Apache web 服务器的 /var/log/apache2。

由于 rsyslogd 日志文件是文本文件，您可以使用 Linux 中可用的任何标准文本工具（例如 cat、head、tail）以及过滤工具（例如 grep）来查看和搜索这些文件。

管理员的一个常用技巧是将 -f 选项与 tail 命令一起使用来监视日志文件。这将显示日志文件中的最后几行，然后监视文件中的任何新条目并显示它们。

## 使用 systemd-journal 进行日志记录

systemd 系统服务软件包包含用于日志记录的 systemd-journal 日志实用程序。请注意，我们将其称为日志实用程序而不是日志记录实用程序。systemd-journal 程序使用与 syslog 协议完全不同的方法来存储事件消息。但是，它确实存储 syslog 消息以及来自内核、引导事件、服务消息等的注释。

本节讨论如何使用 systemd-journal 程序跟踪 Linux 系统上的事件消息。

## 配置 systemd-journal

systemd-journal 服务从 /etc/systemd/journal.conf 配置文件读取其配置。当您检查此文件时，您会注意到控制应用程序工作方式的设置以及控制诸如日志文件大小等项目的设置。表 7.11 描述了常用修改的指令。

## 表 7.11 journal.conf 文件常用修改的指令

| Directive | Description |
| :--- | :--- |
| Storage= | 设置为 auto、persistent、volatile 或 none。确定 systemd-journal 如何存储事件消息。（默认值为 auto。） |
| Compress= | 设置为 yes 或 no。如果是 yes，日志文件将被压缩。（默认值为 yes。） |
| ForwardToSyslog= | 设置为 yes 或 no。如果是 yes，任何接收到的消息都将转发到系统上运行的单独的 syslog 程序（如 rsyslogd）。（默认值为 yes。） |
| ForwardToWall= | 设置为 yes 或 no。如果是 yes，任何接收到的消息都将作为 wall 消息转发到当前登录到系统的所有用户。（默认值为 yes。） |
| MaxFileSec= | 设置为一个数字，后跟一个时间单位（例如 month、week 或 day），用于设置日志文件轮转（归档）前的时间量。通常，如果使用了大小限制，则不需要此功能。要关闭此功能，请将数字设置为 0 且不带时间单位。（默认值为 1month。） |
| RuntimeKeepFree= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 在使用易失性存储时必须为其他磁盘用途保持空闲的磁盘空间大小。（默认值为当前空间的 15%。） |
| RuntimeMaxFileSize= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 日志文件在易失性存储时可以消耗的磁盘空间大小。 |
| RuntimeMaxUse= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 在使用易失性存储时可以消耗的磁盘空间大小。（默认值为当前空间的 10%。） |
| SystemKeepFree= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 在使用持久性存储时必须为其他磁盘用途保持空闲的磁盘空间大小。（默认值为当前空间的 15%。） |
| SystemMaxFileSize= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 日志文件在持久性存储时可以消耗的磁盘空间大小。 |
| SystemMaxUse= | 设置为一个数字，后跟一个单位（例如 K、M 或 G），用于设置 systemd-journald 在使用持久性存储时可以消耗的磁盘空间大小。（默认值为当前空间的 10%。） |

将日志文件从易失性更改为持久性将在本章后面介绍。因此，表 7.11 中的 Storage 指令设置需要更多解释，因为它们与此活动相关：

- auto: 导致 systemd-journald 查找 /var/log/journal 目录并将事件消息存储在那里。如果该目录不存在，它将事件消息存储在临时 /run/log/journal 目录中，该目录在系统关闭时被删除。

- persistent: 导致 systemd-journald 在 /var/log/journal 目录当前不存在时自动创建它，并将事件消息存储在那里。

