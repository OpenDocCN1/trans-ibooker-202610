

注意，在清单 7.19 中，创建新组需要超级用户权限。
getent 实用程序和 grep 命令用于显示 /etc/group 文件中的新组记录。/etc/group 文件中的字段由冒号 (:) 分隔，如下所示：

- 组名
- 组密码：x 表示如果存在组密码，则存储在 /etc/gshadow 文件中。
- GID
- 组成员：属于该组的用户帐户，用逗号分隔。

Ubuntu 和 Debian 发行版提倡使用 addgroup 程序而非 groupadd 程序。它们认为 groupadd 命令是一个低级实用程序。

新创建的组没有为其创建组密码。然而，/etc/group 文件中 Project42 组记录中的 x 并不能证明这一点。为了确保没有组密码，清单 7.20 中检查了存储组密码的 /etc/gshadow 文件。

## 清单 7.20：检查组密码

```
$ sudo getent gshadow Project42
Project42::!:
$
```

清单 7.20 中的命令显示了 /etc/gshadow 文件中 Project42 组的记录。第二个字段包含一个感叹号 (!)，表示未为该组设置密码。

如果设置了组密码，则允许用户帐户访问其不属于的组。如果使用组密码，则该密码通常在需要访问该组的各个用户之间共享。这是一种不良的安全实践。密码绝不应共享。每个帐户都需要有自己的密码，并且仅应通过组成员身份（而不是组密码）来允许访问组。

创建新组后，可以通过将用户帐户添加到组来设置组成员身份。清单 7.21 展示了使用 usermod 命令执行此操作的示例。

## 清单 7.21：使用 usermod 将帐户添加到组

```
$ sudo groups DAdams
DAdams : DAdams
$
$ sudo usermod -aG Project42 DAdams
$
$ sudo groups DAdams
DAdams : DAdams Project42
$
$ getent group Project42
Project42:x:1042:DAdams
$
```

请注意，清单 7.21 中的 usermod 命令使用了两个选项，-aG。-G 将 DAdams 帐户添加为 Project42 组的成员，但 -a 开关很重要，因为它会保留 DAdams 帐户的任何先前组成员身份。在将 DAdams 帐户添加为 Project42 组成员后，可以在最后两个命令结果中看到，/etc/group 文件中 Project42 的记录已更新。

如果需要修改特定组，groupmod 命令很有帮助。组的 GID 使用 -g 选项修改，而组名使用 -n 开关修改。在清单 7.22 中，修改了 Project42 组的 GID。

## 清单 7.22：使用 groupmod 修改组

```
$ getent group Project42
Project42:x:1042:DAdams
$
$ sudo groupmod -g 1138 Project42
$
$ getent group Project42
Project42:x:1138:DAdams
$
```

请注意，在清单 7.22 中，Project42 组的 GID 已修改为 1138。getent 命令确认 /etc/group 文件已更新。如果 1138 GID 已由另一个组使用，则 groupmod 命令将显示错误消息且不会更改组的 GID。

要删除组，请使用 groupdel 实用程序。清单 7.23 中显示了示例。

## 清单 7.23：使用 groupdel 删除组

```
$ sudo groupdel Project42
$
$ getent group Project42
$
$ sudo groups DAdams
DAdams : DAdams
$
$ sudo find / -gid 1138 2>/dev/null
$
```

请注意，在清单 7.23 中，一旦删除 Project42 组，getent 命令就会显示 Project42 组记录已从 /etc/group 文件中移除。真正不错的是，该已删除组的任何成员的组信息也已更新，如第三个命令所示。

删除组后，有必要在目录系统中搜索可能具有该组访问设置的任何文件。可以使用 find 命令和已删除组的 GID 执行此审计。此任务的示例显示为第四个命令。如果您需要帮助回忆如何使用 find 实用程序，请返回第 4 章，其中最初介绍了该命令。

添加、修改和删除用户帐户和组是基本但重要的技能。保持这些任务顺利运行可以改善用户的 Linux 系统体验，并使您的系统更加安全。

## 管理电子邮件

电子邮件是互联网上最常用的功能之一。无论是创建小型办公室内电子邮件系统，还是创建支持数千用户的 Linux 电子邮件服务器，了解 Linux 系统上的电子邮件服务已成为必需。

## 了解电子邮件

在研究 Linux 中的电子邮件服务器之前，让我们先了解 Linux 如何处理电子邮件。Linux 遵循 Unix 处理电子邮件的方法。Unix 操作系统的主要创新之一是使电子邮件处理软件模块化。

与使用单个庞大程序来处理发送和接收邮件所需的所有部分不同，Linux 使用多个小程序协同工作来处理消息。电子邮件功能被分解为独立的部分，然后分配给系统上运行的独立程序。图 7.2 显示了大多数开源电子邮件软件如何在 Linux 环境中将电子邮件功能模块化。

## 图 7.2 Linux 模块化电子邮件环境

![](img/ca7617340ddbe2360d36ce3428729fa9_427_0.png)

如您所见，Linux 电子邮件服务器通常分为三个独立功能：

- 邮件传输代理 (MTA) 将传入电子邮件（以及正在本地投递的传出电子邮件）发送到邮件投递代理 (MDA) 或本地用户的收件箱。对于正在传输到远程系统的外出消息，该代理会与远程主机上的另一个 MTA 程序建立通信链接以传输电子邮件。

- 邮件投递代理 (MDA) 是一个将消息投递到本地用户收件箱的程序。

- 邮件用户代理 (MUA) 是用户用于读取存储在其邮箱中的消息的界面。MUA 不接收消息；它们仅显示用户邮箱中已有的消息。

这三个功能之间的界线通常比较模糊。某些 Linux 电子邮件软件包将 MTA 和 MDA 功能合并，而其他软件包则合并 MDA 和 MUA 功能。

## 选择电子邮件软件

Linux 世界中广泛使用的三个流行 MTA 软件包是：

**Sendmail** Sendmail MTA 程序最初是 Linux 中最流行的 MTA 程序之一，主要归功于其极高的多功能性。Sendmail 中的几个标准功能已成为电子邮件系统的代名词——消息转发、用户别名和邮件列表。

然而，多功能性带来了复杂性。Sendmail 程序的大型配置文件对于初级邮件管理员来说往往难以应对。大约在 2005 年，几个安全漏洞困扰着 Sendmail，这也导致其受欢迎程度下降。

**Postfix** Wietse Venema（IBM 的安全专家和程序员）编写了 *Postfix* 程序，旨在作为 MTA 软件包的完整替代品。Postfix 是作为模块化程序编写的；它使用几个不同的程序来实现 MTA 功能。

Postfix 的最佳功能之一是其简单性。此外，它比 Sendmail 等 MTA 产品更安全。虽然不如 Exim 灵活，但 Postfix 仍然非常受欢迎。您可以在 www.postfix.org 上了解更多信息。

**Exim** Philip Hazel 于 1995 年为剑桥大学开发了 *Exim* MTA 程序。虽然本质上它是 Sendmail 的即插即用替代品，但配置有很大不同。

Exim 的最佳功能之一是其灵活性。它在大多数 Linux 发行版存储库中可用，并带有合理的默认配置。有关 Exim 的详细信息，请访问 www.exim.org。

## 使用电子邮件

除了了解几个流行的 MTA 程序的名称之外，了解如何使用 MDA 应用程序也很重要。对于管理 Linux 系统的人来说，其他电子邮件管理任务（如查看电子邮件队列和转发电子邮件消息）也是必需的。

## 发送和接收电子邮件

历史上，binmail 程序一直是 Linux 系统上最流行的 MDA 程序。您可能不认识其正式名称，但您可能使用过其系统名称：*mail*。binmail 这个名字来源于它在系统上的典型位置，/bin/mail（或 /usr/bin/mail）。

binmail 程序因其简单性而变得流行。默认情况下，它可以读取存储在 /var/spool/mail/ 目录中的电子邮件消息，或者您可以提供命令行选项来指向用户的 $HOME/mail 文件。binmail 执行其工作不需要配置。

然而，其简单性意味着 binmail 的功能有限。正因为如此，一些邮件管理员一直在寻找替代 MDA 程序，并且它不再在所有 Linux 发行版中默认安装。

![](img/ca7617340ddbe2360d36ce3428729fa9_428_0.png)

如果您想跟随使用 binmail 程序，可能需要在您的 Linux 发行版上安装它。对于 Ubuntu，包名为 bsd-mailx，而对于 CentOS，虽然通常默认安装，但包名为 mailx。软件包安装在第 2 章中介绍。

