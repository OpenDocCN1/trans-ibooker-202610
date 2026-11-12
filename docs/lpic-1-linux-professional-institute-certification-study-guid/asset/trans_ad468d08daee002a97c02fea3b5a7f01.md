
### 105.2 自定义或编写简单脚本 (第 9 章)

- 使用标准 sh 语法（循环、测试）。
- 使用命令替换。
- 测试命令返回的值，判断成功或失败，或命令提供的其他信息。
- 执行链式命令。
- 条件性地向超级用户发送邮件。
- 通过 shebang (#!) 行正确选择脚本解释器。
- 管理脚本的位置、所有权、执行权限和 suid 权限。
- 以下是使用的文件、术语和实用程序的部分列表：for, while, test, if, read, seq, exec, ||, &&.

## 106 用户界面和桌面

### 106.1 安装和配置 X11 (第 6 章)

- 了解 X11 架构。
- 基本了解和掌握 X Window 配置文件。
- 覆盖 Xorg 配置的特定方面，例如键盘布局。
- 了解桌面环境的组件，例如显示管理器和窗口管理器。
- 管理对 X 服务器的访问并在远程 X 服务器上显示应用程序。
- 了解 Wayland。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/X11/xorg.conf, /etc/X11/xorg.conf.d, ~/.xsession-errors, xhost, xauth, DISPLAY, X.

### 106.2 图形桌面 (第 6 章)

- 了解主要桌面环境。
- 了解访问远程桌面会话的协议。
- 以下是使用的文件、术语和实用程序的部分列表：KDE, Gnome, Xfce, X11, XDMCP, VNC, Spice, RDP.

### 106.3 辅助功能 (第 6 章)

- 具备视觉设置和主题的基本知识。
- 具备辅助技术 (ATs) 的基本知识。
- 以下是使用的文件、术语和实用程序的部分列表：高对比度/大字体桌面主题，屏幕阅读器，盲文显示器，屏幕放大镜，屏幕键盘，粘滞/重复键，慢速/反弹/切换键，鼠标键，手势，语音识别.

## 107 管理任务

### 107.1 管理用户和组账户及相关系统文件 (第 7 章)

- 添加、修改和删除用户及组。
- 管理密码/组数据库中的用户/组信息。
- 创建和管理特殊用途及受限账户。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/passwd, /etc/shadow, /etc/group, /etc/skel, chage, getent, groupadd, groupdel, groupmod, passwd, useradd, userdel, usermod.

### 107.2 通过计划作业自动执行系统管理任务 (第 9 章)

- 管理 cron 和 at 作业。
- 配置用户对 cron 和 at 服务的访问。
- 了解 systemd 计时器单元。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/cron.{d,daily,hourly,monthly,weekly}, /etc/at.deny, /etc/at.allow, /etc/crontab, /etc/cron.allow, /etc/cron.deny, /var/spool/cron/, crontab, at, atq, atrm, systemctl, systemd-run.

### 107.3 本地化和国际化 (第 6 章)

- 配置区域设置和环境变量。
- 配置时区设置和环境变量。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/timezone, /etc/localtime, /usr/share/zoneinfo, 环境变量 (LC_*, LC_ALL, LANG, TZ), /usr/bin/locale, tzselect, timedatectl, date, iconv, UTF-8, ISO-8859, ASCII, Unicode.

## 108 必需的系统服务

### 108.1 维护系统时间 (第 7 章)

- 设置系统日期和时间。
- 将硬件时钟设置为正确的 UTC 时间。
- 配置正确的时区。
- 使用 ntp 和 chrony 进行基本 NTP 配置。
- 了解使用 pool.ntp.org 服务。
- 了解 ntpq 命令。
- 以下是使用的文件、术语和实用程序的部分列表：/usr/share/zoneinfo, /etc/timezone, /etc/localtime, /etc/ntp.conf, /etc/chrony.conf, date, hwclock, timedatectl, ntpd, ntpdate, chronyc, pool.ntp.org.

### 108.2 系统日志 (第 7 章)

- rsyslogd 的基本配置。
- 了解标准设施、优先级和操作。
- 查询 systemd 日志。
- 按日期、服务或优先级等条件过滤 systemd 日志数据。
- 删除旧的 systemd 日志数据。
- 从救援系统或文件系统副本中检索 systemd 日志数据。
- 了解 rsyslogd 与 systemd-journald 的交互。
- logrotate 的配置。
- 了解 syslog 和 syslog-ng。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/rsyslog.conf, /var/log, logger, logrotate, /etc/logrotate.conf, /etc/logrotate.d/, journalctl, systemd-cat, /etc/systemd/journal.conf, /var/log/journal/.

### 108.3 邮件传输代理 (MTA) 基础 (第 7 章)

- 创建电子邮件别名。
- 配置电子邮件转发。
- 了解常用的 MTA 程序 (postfix, sendmail, qmail, exim)（无需配置）。
- 以下是使用的文件、术语和实用程序的部分列表：~/.forward, sendmail 仿真层命令, newaliases, mail, mailq, postfix, sendmail, exim.

### 108.4 管理打印机和打印 (第 6 章)

- CUPS 的基本配置（适用于本地和远程打印机）。
- 管理用户打印队列。
- 排除常规打印问题。
- 从配置的打印机队列中添加和删除任务。
- 以下是使用的文件、术语和实用程序的部分列表：CUPS 配置文件、工具和实用程序；/etc/cups；lpd 旧版接口 (lpr, lprm, lpq).

## 109 网络基础

### 109.1 互联网协议基础 (第 8 章)

- 演示对网络掩码和 CIDR 表示法的理解。
- 了解私有和公有“点分”IP 地址的区别。
- 了解常见 TCP 和 UDP 端口 (20, 21, 22, 23, 25, 53, 80, 110, 123, 139, 143, 161, 162, 389, 443, 465, 514, 636, 993, 995)。
- 了解 UDP、TCP 和 ICMP 的区别及主要特性。
- 了解 IPv4 和 IPv6 之间的主要区别。
- 了解 IPv6 的基本特性。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/services, IPv4, IPv6, 子网划分, TCP, UDP, ICMP.

### 109.2 持久网络配置 (第 8 章)

- 了解基本 TCP/IP 主机配置。
- 使用 NetworkManager 配置以太网和 Wi-Fi。
- 了解 systemd-networkd。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/hostname, /etc/hosts, /etc/nsswitch.conf, /etc/resolv.conf, nmcli, hostnamectl, ifup, ifdown.

### 109.3 基础网络故障排除 (第 8 章)

- 手动配置网络接口，包括使用 iproute2 查看和更改网络接口配置。
- 手动配置路由，包括使用 iproute2 查看和更改路由表以及设置默认路由。
- 调试与网络配置相关的问题。
- 了解旧版 net-tools 命令。
- 以下是使用的文件、术语和实用程序的部分列表：ip, hostname, ss, ping, ping6, traceroute, traceroute6, tracepath, tracepath6, netcat, ifconfig, netstat, route.

### 109.4 配置客户端 DNS (第 8 章)

- 查询远程 DNS 服务器。
- 配置本地名称解析并使用远程 DNS 服务器。
- 修改名称解析的顺序。
- 调试与名称解析相关的错误。
- 了解 systemd-resolved。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/hosts, /etc/resolv.conf, /etc/nsswitch.conf, host, dig, getent.

## 110 安全

### 110.1 执行安全管理任务 (第 10 章)

- 审计系统以查找设置了 suid/sgid 位的文件。
- 设置或更改用户密码及密码过期信息。
- 能够使用 nmap 和 netstat 发现系统上的开放端口。
- 对用户登录、进程和内存使用设置限制。
- 确定哪些用户已登录系统或当前正在登录。
- sudo 的基本配置和用法。
- 以下是使用的文件、术语和实用程序的部分列表：find, passwd, fuser, lsof, nmap, chage, netstat, sudo, /etc/sudoers, su, usermod, ulimit, who, w, last.

### 110.2 设置主机安全 (第 10 章)

- 了解影子密码及其工作原理。
- 关闭未使用的网络服务。
- 了解 TCP 包装器的作用。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/nologin, /etc/passwd, /etc/shadow, /etc/xinetd.d/, /etc/xinetd.conf, /etc/inetd.d/, /etc/inetd.conf, systemd-socket, /etc/inittab, /etc/init.d/, /etc/hosts.allow, /etc/hosts.deny.

### 110.3 使用加密保护数据 (第 10 章)

- 执行 OpenSSH 2 客户端的基本配置和用法。
- 了解 OpenSSH 2 服务器主机密钥的作用。
- 执行 GnuPG 的基本配置、用法和吊销。
- 使用 GPG 加密、解密、签名和验证文件。
- 了解 SSH 端口隧道（包括 X11 隧道）。
- 以下是使用的文件、术语和实用程序的部分列表：ssh, ssh-keygen, ssh-agent, ssh-add, ~/.ssh/id_rsa 和 id_rsa.pub, ~/.ssh/id_dsa 和 id_dsa.pub, ~/.ssh/id_ecdsa 和 id_ecdsa.pub, ~/.ssh/id_ed25519 和 id_ed25519.pub, /etc/ssh/ssh_host_rsa_key 和 ssh_host_rsa_key.pub, /etc/ssh/ssh_host_dsa_key 和 ssh_host_dsa_key.pub, /etc/ssh/ssh_host_ecdsa_key 和 ssh_host_ecdsa_key.pub, /etc/ssh/ssh_host_ed25519_key 和 ssh_host_ed25519_key.pub, ~/.ssh/authorized_keys, /etc/ssh_known_hosts, gpg, gpg-agent, ~/.gnupg/.
