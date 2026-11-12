

该账户可能已过期。账户过期日期通常为临时账户用户（如承包商或实习生）设置。您可以使用 chage 命令查看此信息，如清单 10.21 所示。

### 清单 10.21: 使用 chage 命令检查账户是否过期

```
$ date
Wed Jan  2 16:17:48 EST 2019
$
$ sudo chage -l JArcher
[...]
Account expires		: Jan 01, 2019
[...]
```

请注意，此账户的过期日期已过。因此，JArcher 账户现已过期，用户无法登录。如果这是个错误或您需要修改它，请使用超级用户权限和 chage -E 命令为账户设置新的过期日期。

确认用户使用的是正确密码，并检查账户密码是否已过期。也可使用 chage -l 命令查看此信息。

及时了解关于密码长度和复杂性的当前指南非常重要。美国国家标准与技术研究院（NIST）在 pages.nist.gov/800-63-3/sp800-63b.html 上提供了一份有用的指南，可为您提供帮助。要实施这些建议，您可能需要使用可插拔认证模块（PAM），例如 pam_unix.so、pam_pwhistory.so 和 pam_pwquality.so。有关 PAM 的更多信息，请访问 linux-pam.org。

## 限制 root 访问

适当的账户管理可增强系统安全性。以下是妥善管理账户应做的基本事项列表：

- 不允许登录到 root 用户账户。
- 每个用户账户仅允许一个用户。
- 为临时用户账户设置过期日期。
- 删除未使用的用户账户。

本节重点介绍 root 用户账户。总的来说，即使您是唯一使用该系统的人，登录到 root 用户账户也不是个好主意。有替代方案可以满足各种系统管理员的需求。

## 使用 su 切换用户

使用 su 实用程序，您可以快速登录到其他账户，或仅发出几条命令。要登录到 root 用户账户，请输入 su 后跟一个短划线（-）。您需要输入 root 用户的密码才能获得访问权限：

```
$ su -
Password:
# whoami
root
```

> 在默认情况下阻止 root 登录的任何发行版（例如 Ubuntu）上，您将无法使用 su - 方法登录到 root 账户。虽然您可以只输入 su 然后输入正确密码来登录到 root 用户账户，但这不明智，因为并非所有内容都可能已正确设置，以执行需要超级用户权限的任务。su 命令后的短划线（-）会启动一个新的 shell 环境并执行 root 的配置文件。

要进入另一个用户的账户，您再次使用 su -，但后面跟上账户的用户名：

```
$ su - rich
Password:
$ whoami
rich
```

切换到所需账户后，您可以使用该用户账户的权限发出多条命令。完成任务后，键入 exit 或 logout 以退出账户。

如果您只需使用超级用户权限发出单条命令，可以使用 su -c 命令。例如，更改另一个账户的密码需要超级用户权限。只要您知道 root 账户的密码，就可以完成此操作，如清单 10.22 所示。

### 清单 10.22: 通过 su -c 命令以另一用户身份发出单条命令

```
$ whoami
Christine
$
$ su -c "passwd rich"
[...]
passwd: all authentication tokens updated successfully.
$
$ whoami
Christine
$
```

上一个清单中的第一个密码提示正在询问 root 用户账户密码。之后，passwd rich 命令正在使用 root 用户账户的权限执行。请注意，passwd rich 被引号括起来。每当您向另一个账户传递的命令中包含空格时，就需要对其加引号。

## 以超级用户身份使用 sudo 完成工作

虽然 su 命令很有帮助，但它仍不能完全满足不允许登录到 root 用户账户的安全要求。幸运的是，sudo 实用程序可以对此提供帮助。

以 root 用户身份登录会创建所谓的否认环境。否认环境意味着一个人可以否认自己的行为。因此，如果系统管理员使用 root 账户执行某些非法或捣乱活动，管理员可以合法否认对该活动负责。每个用户都有账户和密码且没有人可以登录到 root 用户账户的系统会建立非否认环境。这意味着行为会被记录，且对这些行为的责任不容易被否认。可以使用 sudo 创建非否认环境。

sudo 实用程序允许用户以超级用户权限发出单条命令。所有 sudo 命令的使用都会记录在日志文件或日志中，其中包含跟踪数据，例如谁在何时做了什么。

sudo 实用程序的主要配置文件是 /etc/sudoers 文件。Ubuntu 发行版系统上的 /etc/sudoers 文件片段示例如清单 10.23 所示。

### 清单 10.23: 查看 /etc/sudoers 配置文件

```
$ sudo cat /etc/sudoers
[sudo] password for Christine:
#
# This file MUST be edited with the 'visudo' command as root.
#
[...]
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of the admin group may gain root privileges
%admin ALL=(ALL) ALL

# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
$
```

在清单 10.23 中，以 root 开头的行是典型配置，用于向用户提供完全访问权限。/etc/sudoers 中特定用户的访问权限使用以下格式指定：

```
USERNAME HOSTNAME-OF-SYSTEM=(USER: GROUP) COMMANDS
```

因此，对于 /etc/sudoers 文件中的 root 用户记录，root 账户可以在系统上使用超级用户权限，无论其主机名是什么（ALL=），可以以任何用户身份（ALL:）或以任何组成员身份（:ALL）运行命令，并执行任何命令（ALL）。

切勿使用标准编辑器打开 sudoers 文件。如果多个用户同时打开 sudoers 文件，可能会发生奇怪的事情并损坏文件。visudo 命令会在编辑器中安全地打开该文件，以便您进行更改。它的操作方式与 vi 编辑器相同（第 1 章中介绍过）。

为了简化操作，许多发行版在 /etc/sudoers 文件中添加了提供完全超级用户权限的组，例如 sudo 或 wheel。sudoers 文件中的组记录前面带有百分号（%）。当您需要向新账户提供 sudo 访问权限时，应将用户账户添加到该组（usermod -aG 在第 7 章中介绍过），而不是修改 /etc/sudoers 文件。某些发行版（例如 Ubuntu）在安装过程中会自动将主用户账户添加为 sudo 组成员，如清单 10.24 所示。

### 清单 10.24: 查看 Ubuntu 发行版中 sudo 用户的组

```
$ whoami
Christine
$ groups
Christine [...]sudo [...]
$
```

最好在 /etc/sudoers.d/ 目录中的配置文件里包含其他用户或组，而不是修改 /etc/sudoers 文件。主 sudoers 文件中的 #includedir /etc/sudoers.d 行会导致系统将 /etc/sudoers.d/ 目录中的所有文件作为 sudo 配置的一部分进行读取。

配置完成后，拥有 sudo 权限的用户会在每条需要超级用户权限的命令前加上 sudo 命令，并在系统询问时提供账户密码。几个示例如清单 10.25 所示。

### 清单 10.25: 使用 sudo 获取超级用户权限

```
$ sudo getent shadow BCrusher
[sudo] password for Christine:
BCrusher:$6$[...]Uy.:18060:0:99999:7:::
$
```

```
$ sudo chage -l BCrusher
Last password change	: Jun 13, 2019
[...]
Maximum number of days between password change	: 99999
Number of days of warning before password expires	: 7
$
```

```
$ journalctl -r -n 10 | grep sudo
[...]
Jun 13 15:56:40 Ubuntu1804 sudo[3609]: Christine : TTY=pts/0 ;
PWD=/home/Christine ; USER=root ; COMMAND=/usr/bin/chage -l BCrusher
[...]
Jun 13 15:56:27 Ubuntu1804 sudo[3607]: Christine : TTY=pts/0 ;
PWD=/home/Christine ; USER=root ; COMMAND=/usr/bin/getent shadow BCrusher
$
```

请注意，sudo 命令的使用已通过发送到日志文件的事件得到正确跟踪。因此，所需的非否认环境已存在，增强了系统的安全性。

## 审计用户访问

多个实用程序允许您审计当前正在访问系统的用户以及过去曾访问过该系统的用户。您还可以查看有关用户账户的各种信息。

## 了解 who 和 w 实用程序

使用 who 命令，您可以查看有关您自己的账户的信息，或查看系统上的每个当前用户。示例如清单 10.26 所示。

### 清单 10.26: 使用 who 命令

```
$ who
rich	 tty2		 2019-10-03 13:12
Christine pts/0	 2019-10-03 14:10 (192.168.0.102)
$
$ who am i
Christine pts/0	 2019-10-03 14:10 (192.168.0.102)
$
$ who mom likes
Christine pts/0	 2019-10-03 14:10 (192.168.0.102)
$
```

