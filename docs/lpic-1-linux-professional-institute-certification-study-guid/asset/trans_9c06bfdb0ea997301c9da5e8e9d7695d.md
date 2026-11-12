

虽然 FHS 有助于标准化 Linux 上文件的位置，但并非所有 Linux 发行版都完全遵循它。最好查阅特定 Linux 发行版的文档，了解其如何管理虚拟目录结构中的文件。

FHS 定义了每个 Linux 系统都应具备的核心文件夹名称和位置，以及它们应包含的数据类型。表 4.16 列出了 FHS 定义的几个更常见的文件夹。

**表 4.16 常见的 Linux FHS 文件夹**

| 文件夹 | 描述 |
|---|---|
| / | 根文件系统 |
| /boot | 包含用于引导系统的引导加载程序文件 |
| /dev | 存放设备文件 |
| /home | 包含用户数据文件 |
| /lib | 存放共享库和内核模块 |
| /media | 传统上用作可移动设备的挂载点 |
| /mnt | 用作可移动设备的当前挂载点 |
| /opt | 包含可选第三方程序的数据 |
| /tmp | 包含系统用户创建的临时文件 |
| /usr | 包含标准 Linux 程序的数据 |
| /usr/bin | 包含本地用户程序和数据 |
| /usr/lib | 存放用于编程和软件包的库 |
| /usr/local | 包含本地安装特有程序的数据 |
| /usr/sbin | 包含系统程序和数据 |

您可以在 Linux 基金会的参考规范中阅读完整的 FHS 标准：refspecs.linuxfoundation.org。

## 使用工具定位文件

除了将 FHS 作为指南之外，在 Linux 系统中查找文件还有很多方法。本节将探讨几种用于定位文件的工具。

### 使用 which 命令

which 命令会显示作为参数传递的 shell 命令的完整路径名。清单 4.53 展示了使用此实用程序的示例。

### 清单 4.53：使用 which 命令

```
$ which passwd
/usr/bin/passwd
$
$ which shutdown
/usr/sbin/shutdown
$
$ which line
/usr/bin/which: no line in (/usr/local/bin:/usr/bin:/usr/local/sbin:
/usr/sbin:/home/Christine/.local/bin:/home/Christine/bin)
$
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:
/home/Christine/.local/bin:/home/Christine/bin
$
```

在清单 4.53 的第一个示例中，which 命令用于查找 passwd 命令的程序位置。它显示 /usr/bin/passwd 的完整路径名。shutdown 实用程序位于 sbin 目录中。但是，line 程序未在此系统上安装，which 实用程序会显示它为查找程序而搜索的所有目录。它使用 PATH 环境变量（其内容也在清单 4.53 中显示）来确定要搜索的目录。

> 环境变量是修改进程环境的配置设置。当您键入命令（程序）名称时，PATH 变量会设置 Linux 将搜索程序二进制文件的目录。其他命令（例如 which 实用程序）也会使用它。请注意，PATH 列表中的目录名称由冒号 (:) 分隔。

which 命令还可用于快速确定命令是否使用了别名。清单 4.54 展示了一个示例。

### 清单 4.54：使用 which 命令查看命令别名

```
$ which ls
alias ls='ls --color=auto'
    /usr/bin/ls

$

$ unalias ls

$

$ which ls
/usr/bin/ls

$
```

在清单 4.54 中，对 ls 命令使用 which 实用程序时，它会显示 ls 命令当前有一个别名。因此，当您键入 ls 时，就相当于键入了 ls --color=auto 命令。在 ls 上使用 unalias 命令后，which 实用程序仅显示 ls 程序的位置。

### 使用 whereis 命令

查找文件的另一个命令是 whereis 实用程序。此实用程序允许您定位任何命令的程序二进制文件，以及源代码文件和手册页。清单 4.55 展示了使用 whereis 实用程序的示例。

### 清单 4.55：使用 whereis 命令

```
$ whereis diff
diff: /usr/bin/diff /usr/share/man/man1/diff.1.gz
/usr/share/man/man1p/diff.1p.gz

$

$ whereis line
line:

$
```

在清单 4.55 中发出的第一个命令会搜索 diff 实用程序的程序二进制文件、源代码文件和手册页。在这种情况下，whereis 命令找到了一个二进制文件以及两个手册页文件。但是，当使用 whereis 定位 line 实用程序的文件时，系统中未找到任何内容。

### 使用 locate 命令

locate 程序是一个非常方便且简单的查找文件的实用程序。该实用程序搜索位于 /var/lib/mlocate/ 目录中的 mlocate.db 数据库，以确定特定文件是否存在于本地系统上。locate 命令的基本语法如下：

```
locate [OPTION]... PATTERN...
```

请注意，在语法中，locate 实用程序使用模式列表来查找文件。因此，您可以使用部分文件名和正则表达式，并结合命令选项来忽略大小写。表 4.17 列出了几个更常用的 locate 命令选项。

| 短选项 | 长选项 | 描述 |
| :--- | :--- | :--- |
| -A | --all | 显示与所有模式匹配的文件名，而不仅显示与模式列表中仅一个模式匹配的文件。 |
| -b | --basename | 仅显示与模式匹配且不包含与模式匹配的任何目录名称的文件名。 |
| -c | --count | 仅显示名称与模式匹配的文件数量，而不显示文件名。 |
| -i | --ignore-case | 在匹配文件名的模式中忽略大小写。 |
| -q | --quiet | 处理时不显示任何错误消息，例如权限被拒绝。 |
| -r | --regexp R | 使用正则表达式 R，而不是模式列表，来匹配文件名。 |
| -w | --wholename | 显示与模式匹配且包含与模式匹配的任何目录名称的文件名。这是默认行为。 |

要使用 locate 命令查找文件，请输入 locate 后跟文件名。如果文件在您的系统上且您有权查看它，locate 实用程序将显示文件的目录路径和名称。清单 4.56 展示了一个示例。

### 清单 4.56：使用 locate 命令查找文件

```
$ locate Project42.txt
/home/Christine/Answers/Project42.txt
$
```

使用 locate 命令的 PATTERN 可能有点棘手，因为默认的文件名通配符展开。如果您在模式中未输入任何通配符，locate 命令默认会向模式添加通配符。因此，如果您输入模式 passwd，它会自动变为 *passwd*。

如果要搜索基准名 passwd 且不进行文件名通配符展开，则必须在模式周围添加引号（单引号或双引号），并在模式前加上 \ 字符。清单 4.57 展示了几个示例。

### 清单 4.57：在不进行文件名通配符展开的情况下使用 locate 命令

```
$ locate -b passwd
/etc/passwd
/etc/passwd-
/etc/pam.d/passwd
/etc/security/opasswd
/usr/bin/gpasswd
[...]
/usr/share/vim/vim74/syntax/passwd.vim
$
$ locate -b '\passwd'
/etc/passwd
/etc/pam.d/passwd
/usr/bin/passwd
/usr/share/bash-completion/completions/passwd
$
```

清单 4.57 的第一个示例显示了如果允许默认文件名通配符展开会发生什么。显示的文件比名为 passwd 的文件多得多。显示的文件太多，以至于清单不得不被精简以适应空间。但是，在第二个示例中，通过使用引号和 \ 字符关闭了文件名通配符展开。将此模式与 locate 实用程序一起使用，可提供显示名为 passwd 的文件的所需结果。

如果您没有权限查看目录的内容，locate 命令无法显示该目录中与您的 PATTERN 匹配的文件。因此，您的显示中可能缺少一些文件。

请记住，locate 命令的 PATTERN 实际上是一个模式列表，因此您可以添加更多模式。只要确保用空格将它们分隔开，如清单 4.58 所示。

### 清单 4.58：将 locate 命令与模式列表一起使用

```
$ locate -b '\passwd' '\group'
/etc/group
/etc/passwd
/etc/iproute2/group
/etc/pam.d/passwd
/usr/bin/passwd
/usr/share/X11/xkb/symbols/group
/usr/share/bash-completion/completions/passwd
$
```

