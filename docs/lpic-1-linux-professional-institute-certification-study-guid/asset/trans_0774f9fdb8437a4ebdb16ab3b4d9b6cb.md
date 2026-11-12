


如果需要，一旦您的软件时钟通过 NTP 相当准确，就可以手动将硬件时钟设置为其时间。只需使用本章前面介绍的 `hwclock --systohc` 命令即可。

## 使用 chrony 守护进程

chrony 守护进程（chronyd）相比 ntpd 有许多改进。即使在网络繁忙或系统有一段时间关闭的系统上，甚至在虚拟化系统上，它也能保持准确时间。此外，它同步系统时钟的速度比 ntpd 更快，并且可以轻松配置为本身充当本地时间服务器。除少数情况外，大多数发行版都建议使用 chrony 服务进行软件时钟同步。

在 CentOS 和其他基于 Red Hat 的发行版上，您会发现 chrony 程序默认已安装，但未在启动时启用（默认情况下）。软件包名称为 chrony，可在大多数发行版存储库中找到。因此，如果您需要安装它，可以使用相应的包管理工具（第 2 章将介绍）来完成。

在 Ubuntu 上，您会发现安装 chrony 后，它会自动启动并在启用开机自启。要在 CentOS 上启动它，请使用超级用户权限并在命令行中键入 `systemctl start chronyd`。再次使用相同的命令，但将 start 替换为 enable，以便 chrony 在系统启动时启动。

## 配置 chrony 守护进程

chrony 的主配置文件是 chrony.conf 文件，它可能存储在 /etc/ 或 /etc/chrony/ 目录中。通常，无需修改此配置文件中的任何内容，但您可能想查看几个项目。

配置文件中包含（在其他指令中）要使用的 NTP 时间服务器。用于设置这些的指令名称为 server 或 pool。对于 chrony，server 指令通常用于指定单个时间服务器，而 pool 表示服务器池。Ubuntu 发行版的 /etc/chrony/chrony.conf 文件中的 pool 行如清单 7.49 所示。

### 清单 7.49: 查看 /etc/chrony/chrony.conf 文件中的 pool 指令

```
$ grep ^pool /etc/chrony/chrony.conf
pool ntp.ubuntu.com iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2
$
```

请注意，pool 设置与 /etc/ntp.conf 文件中 ntpd 的 server 设置类似。maxsources 参数是这种相似性的一个例外。它指定来自指定源的时间服务器的最大数量。如果其中一个服务器宕机，chrony 会找到另一个服务器来替代。

> chronyd 使用端口 123，因此在启动 chrony 守护进程之前，请确保该端口可通过防火墙访问。

chrony.conf 文件中另一个应检查的指令是 rtcsync 指令。这个方便的设置指示 chrony 定期更新硬件时间（实时时钟）。如果您在配置文件的单独一行上找到它且没有其他内容，则表示它已为 chrony 设置：

```
rtcsync
```

设置该指令并运行 chrony 后，您将不再需要使用难以记住的 hwclock 命令（hwclock --systohc）来用软件时钟的时间更新实时时钟。这真是一项改进。

如果您修改了配置且 chronyd 尚未启动，请使用超级用户权限和 systemctl 命令来启动它：

```
systemctl start chronyd
```

如果 chronyd 已经启动并且您需要修改配置文件，则需要重新启动它：

```
systemctl restart chronyd
```

（如果您的系统不使用 systemd，请参阅第 2 章了解启动服务的替代方法。）如有必要，请务必启用它，以便 chronyd 服务通过 systemctl enable chronyd 命令在系统启动时启动。

## 管理 chrony 服务

chrony 服务提供 chronyc 命令行实用程序来管理它。有几个可用的命令与 ntpd 命令类似，甚至超越了 ntpd 命令。

如果您熟悉使用 ntpq -p 来查看系统的时间源，则可以改用 chronyc sources -v。此命令的示例在使用 chrony 的 Ubuntu 发行版上的清单 7.50 中显示。

### 清单 7.50: 通过 chronyc sources -v 命令查看源时间服务器

```
$ chronyc sources -v
210 Number of sources = 8

.-- Source mode '^' = server, '=' = peer, '#' = local clock.
/ .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                .- xxxxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.               |     yyyy = measured offset,
||                                \              |     zzzz = estimated error.
||                                 |           |
```

| MS | 名称/IP 地址 | 层级 | 轮询 | 可达性 | 上次接收 | 最后样本 |
|----|-----------------|---------|------|-------|--------|-------------|
| ^+ | alphyn.canonical.com | 2 | 10 | 377 | 413 | -461us[ -418us] +/-  101ms |
| ^+ | golem.canonical.com | 2 | 10 | 337 | 142 | +30us[  +30us] +/-   95ms |
| ^+ | chilipepper.canonical.com | 2 | 10 | 377 | 918 | -797us[ -760us] +/-   81ms |
| ^+ | pugot.canonical.com | 2 | 10 | 377 | 21 | -2184us[-2184us] +/-   87ms |
| ^* | 4.53.160.75 | 2 | 10 | 377 | 229 | -327us[ -281us] +/-   50ms |
| ^+ | vps3.cobryce.com | 2 | 10 | 377 | 416 | +4806us[+4850us] +/-   70ms |
| ^+ | B1-66ER.matrix.gs | 2 | 10 | 377 | 21m | -315us[ -363us] +/-   60ms |
| ^+ | 2.time.dbsinet.com | 2 | 9 | 175 | 601 | -3138us[-3097us] +/-   93ms |

$

该显示中包含大量有用信息。请注意，顶部显示 chronyd 当前使用的时间服务器源数量（8）。

与此类似但提供更多时间服务器源统计信息的是 chronyc sourcestats。示例在清单 7.51 中显示。

### 清单 7.51: 通过 chronyc sourcestats 命令查看时间服务器统计信息

```
$ chronyc sourcestats
210 Number of sources = 8
```

| 名称/IP 地址 | NP | NR | 跨度 | 频率 | 频率偏移 | 偏移 | 标准偏差 |
|-----------------|----|----|------|-----------|-----------|--------|---------|
| alphyn.canonical.com | 31 | 15 | 87m | +0.322 | 0.287 | -180us | 677us |
| golem.canonical.com | 31 | 18 | 91m | -0.006 | 0.137 | -470us | 275us |
| chilipepper.canonical.com | 31 | 14 | 96m | -0.044 | 0.163 | -661us | 383us |
| pugot.canonical.com | 28 | 16 | 87m | -0.029 | 0.350 | -637us | 575us |
| 4.53.160.75 | 31 | 20 | 90m | +0.003 | 0.166 | -553us | 370us |
| vps3.cobryce.com | 30 | 14 | 87m | -0.195 | 0.531 | +4453us | 936us |
| B1-66ER.matrix.gs | 30 | 13 | 72m | -0.095 | 0.351 | +523us | 635us |
| 2.time.dbsinet.com | 27 | 16 | 71m | +0.073 | 0.327 | -2801us | 431us |

$

如果您想查看软件时钟是否正在同步（类似于 netstat 命令显示的内容），并了解有关软件时钟性能的更多信息，请尝试 chronyc tracking 命令。示例在清单 7.52 中显示。

### 清单 7.52: 通过 chronyc tracking 命令查看软件时钟信息

```
$ chronyc tracking
Reference ID    : 0435A04B (4.53.160.75)
Stratum         : 3
Ref time (UTC)  : Sat Jun 01 19:22:01 2019
System time : 0.000197749 seconds slow of NTP time
Last offset : -0.000001978 seconds
RMS offset : 0.001266906 seconds
Frequency : 31.578 ppm fast
Residual freq : +0.000 ppm
Skew : 0.077 ppm
Root delay : 0.034032539 seconds
Root dispersion : 0.02259230 seconds
Update interval : 1027.0 seconds
Leap status : Normal
$
```

还有几个其他有用的 chronyc 命令可用于管理和监控 chrony。要查看所有命令，请在命令行中输入 man chronyc。

## 摘要

在本章中，我们深入探讨了创建、修改和删除用户帐户和组。我们探讨了在故障排除和使用系统电子邮件方面有帮助的电子邮件实用程序和概念。此外，我们研究了对故障排除和安全监控至关重要的日志文件。最后，我们了解了如何正确维护系统时间。

这些技能将帮助您确保所管理的所有服务器都能高效运行。此外，您的系统问题将更少，遇到的故障可以更快解决，而有了这些技能，您可以花更多时间远离工作。

## 考试要点

描述管理用户帐户的相关角色。/etc/login.defs 和 /etc/default/useradd 文件配置 useradd 命令默认行为的各种设置。由于这些文件中的指令设置因发行版而异，在使用 useradd 实用程序创建帐户之前，最好先浏览它们。创建帐户时，/etc/passwd、/etc/shadow 和 /etc/group 文件都会被修改。具体取决于用户帐户创建配置，可能会创建用户主目录，并从 /etc/skel/ 目录复制文件到其中。要修改用户帐户，请使用 usermod 命令；要删除帐户，请使用 userdel 实用程序。可以使用 getent 实用程序查看帐户信息。使用 chage 命令查看帐户的 /etc/shadow 记录信息并对其进行修改。

