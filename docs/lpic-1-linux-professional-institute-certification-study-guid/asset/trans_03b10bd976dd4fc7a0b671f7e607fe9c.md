

记录所有严重性为 critical、alert 或 emergency 的内核事件消息。要仅记录具有特定严重性的消息，请在优先级关键字之前使用等号：

```
kern.=crit
```

您还可以对功能或优先级使用通配符。以下条目记录所有具有 emergency 严重性级别的事件：

```
*.emerg
```

操作条目定义 rsyslogd 应如何处理接收到的 syslog 消息。共有六种操作选项：

- 转发到常规文件
- 将消息通过管道传输给应用程序
- 在终端或系统控制台上显示消息
- 将消息发送到远程主机
- 将消息发送到用户列表
- 将消息发送给所有登录用户

### 列表 7.29：Ubuntu 18.04 的 rsyslogd.conf 配置条目

```
auth,authpriv.*         /var/log/auth.log
*.*;auth,authpriv.none  -/var/log/syslog
kern.*                  -/var/log/kern.log
mail.*                  -/var/log/mail.log
mail.err                /var/log/mail.err
*.emerg                 :omusrmsg:*
```

列表 7.29 中显示的第一个条目定义了一条规则，用于处理所有 auth 和 authpriv 设施类型的消息。这表明您可以通过逗号分隔来指定多个设施类型。该规则还为优先级使用了通配符，因此会记录所有严重性级别。该规则指示所有安全事件消息将被记录到 /var/log/auth.log 文件。

第二个条目定义了一条规则，用于处理所有事件 (*.*)，但安全事件（.none 优先级）除外。事件消息会发送到 /var/log/syslog 文件。文件名前的减号告诉 rsyslogd 每次写入后不要同步文件，从而提高性能。这种做法的缺点是，如果在下一次正常系统同步之前系统崩溃，您可能会丢失事件消息。

最后一个条目定义了一条规则，用于处理所有 emergency 事件。omusrmsg 命令表示您希望将事件消息发送到系统上的某个用户帐户。当您指定通配符时，该规则会将所有 emergency 事件消息发送到系统上的所有用户。

为了进行比较，列表 7.30 显示了 CentOS 7 系统 rsyslogd 配置文件中的条目。

### 列表 7.30：CentOS 7 的 rsyslog.conf 配置文件

```
*.info;mail.none;authpriv.none;cron.none /var/log/messages
authpriv.* /var/log/secure
mail.* -/var/log/maillog
cron.* /var/log/cron
*.emerg :omusrmsg:*
uucp,news.crit /var/log/spooler
local7.* /var/log/boot.log
```

请注意，基于 Red Hat 的系统使用 /var/log/messages 文件记录信息性消息，使用 /var/log/secure 文件记录安全消息。

## 将日志消息发送到日志服务器

当今许多数据中心中常见的一种服务器是中央日志主机，它接收并存储所有日志客户端系统的日志。使用 rsyslog 应用程序的配置文件，将系统配置为充当日志客户端相当容易。

要将所有日志消息发送到中央日志主机服务器，请编辑 /etc/rsyslogd.conf 配置文件并转到文件末尾。您需要向文件中添加一行，其语法遵循 syslog 协议的标准 facility.priority action。通常，大多数管理员将所有内容发送到远程日志服务器，因此使用 *.*. 来指定 facility.priority。但是，将日志消息发送到远程服务器的操作具有以下特殊语法：

```
TCP/UDP[(z#)]HOST:[PORT#]
```

这种操作语法相当令人困惑，因此让我们逐步讲解：

- TCP/UDP：您可以选择 TCP 或 UDP 协议（在第 8 章中介绍）将日志消息传输到中央日志服务器。UDP 可能会丢失数据，因此如果您的日志消息很重要，应选择 TCP。使用单个 at 符号 (@) 选择 UDP，使用双 at 符号 (@@) 选择 TCP。
- [(z#)]：方括号表示此语法是可选的。z 选择 zlib 在数据遍历网络之前对其进行压缩，# 选择压缩级别，可以是 1（最低压缩）到 9（最高压缩）之间的任意数字。请注意，必须将 z 和数字括在括号内，例如 (z5)。
- HOST：此语法通过完全限定域名（FQDN，例如 example.com）或 IP 地址指定中央日志服务器。如果使用 IPv6 地址，必须将其包含在括号中。
- [PORT#]：方括号表示此语法是可选的。这指定远程中央日志主机上日志服务正在监听传入流量的端口。

举个例子会有所帮助。假设您想将所有日志消息发送到位于 loghost.ivytech.edu 的远程日志主机，并在端口 6514 上监听传入的日志消息 TCP 流量。为了最大限度地减少网络使用，您希望使用最高压缩级别压缩日志消息。在每个客户端系统上，您需要将以下行添加到 /etc/rsyslog.conf 文件的底部：

```
*.* @@(z9)loghost.ivytech.edu:6514
```

做出此修改后，您需要重新加载配置文件或重新启动 rsyslogd 服务，才能开始向活动中央日志服务器传输日志消息。

## 轮转日志文件

不难猜到，对于繁忙的 Linux 系统，生成大型日志文件并不需要太长时间。为了帮助应对这一问题，许多 Linux 发行版安装了 logrotate 实用程序。它会根据时间或文件大小自动将 rsyslogd 日志文件拆分为归档文件。您通常可以通过日志文件名添加的数字扩展名来识别归档日志文件。列表 7.31 给出了 /var/log/btmp 文件的示例（该文件包含登录系统的失败尝试，并使用 lastb 命令显示）。

### 列表 7.31：/var/log/btmp 文件及其轮转版本

```
$ ls /var/log/btmp*
/var/log/btmp   /var/log/btmp-20190501
$
```

logrotate 实用程序不仅仅用于轮转日志文件。它还可以压缩、删除日志文件，并在需要时将日志文件邮件发送到指定帐户。

为确保文件能够及时处理，logrotate 实用程序通常作为 cron 作业每天运行一次（cron 将在第 9 章中介绍）。它使用 /etc/logrotate.conf 配置文件来确定如何管理每个日志文件。列表 7.32 显示了 CentOS 系统上的 logrotate 配置文件。

### 列表 7.32：查看 CentOS 发行版上的 /etc/logrotate.conf 文件

```
$ cat /etc/logrotate.conf
# see "man logrotate" for details
# rotate log files weekly
weekly

# keep 4 weeks worth of backlogs
rotate 4

# create new (empty) log files after rotating old ones
create

# use date as a suffix of the rotated file
dateext

# uncomment this if you want your log files compressed
#compress

# RPM packages drop log rotation information into this directory
include /etc/logrotate.d

# no packages own wtmp and btmp -- we'll rotate them here
/var/log/wtmp {
    monthly
    create 0664 root utmp
        minsize 1M
    rotate 1
}

/var/log/btmp {
    missingok
    monthly
    create 0600 root utmp
    rotate 1
}

# system-specific logs may be also be configured here.
$
```

/etc/logrotate.conf 文件的一个优点是它注释完善（几乎每个 # 后面的项目都是注释），并且相当容易理解。文件的前半部分包含全局指令（设置）。例如，请注意 dateext 选项。该选项指示 logrotate 在归档（轮转）日志文件的名称中使用当前日期，如列表 7.31 所示。如果 *不* 使用 dateext 选项，轮转日志文件将以数字作为文件扩展名，其中最大的数字表示最旧的日志文件：

```
mail.log
mail.log.1
mail.log.2
```

在列表 7.32 的 /etc/logrotate.conf 文件的后半部分中，有针对 /var/log/wtmp 和 /var/log/btmp 日志文件的特定轮转设置。这些选项仅适用于这些特定的日志文件，并会覆盖任何全局设置。

请注意，在列表 7.32 中，非全局设置之前有一行

```
include /etc/logrotate.d
```

此行告诉 logrotate 使用存储在 /etc/logrotate.d/ 目录中的任何配置文件来获取 *额外* 特定日志文件轮转选项。此处的配置文件与它们所管理的日志文件同名，如列表 7.33 所示。

