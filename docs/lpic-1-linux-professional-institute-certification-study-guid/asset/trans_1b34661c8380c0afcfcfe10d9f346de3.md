
要查找可执行程序，等等。表 1.1 列出了一些更常用的环境变量。

### 表 1.1 常用环境变量

| 名称 | 描述 |
| :--- | :--- |
| BASH_VERSION | 当前Bash shell实例的版本号（第1章） |
| EDITOR | 部分shell命令使用的默认编辑器（第1章） |
| GROUPS | 用户帐户的组成员身份（第7章） |
| HISTFILE | 用户shell命令历史文件的名称（第1章） |
| HISTSIZE | 历史文件中存储的命令最大数量（第1章） |
| HOME | 当前用户的主目录名称（第1章） |
| HOSTNAME | 当前系统的主机名（第8章） |
| LANG | shell的语言环境类别（第6章） |
| LC_* | 覆盖LANG的各种语言环境设置（第6章） |
| LC_ALL | 覆盖LANG的shell语言环境类别（第6章） |
| LD_LIBRARY_PATH | 在搜索标准库目录之前要搜索的库目录的冒号分隔列表（第2章） |
| PATH | 用于搜索命令的目录的冒号分隔列表（第1章） |
| PS1 | 主shell命令行界面提示符字符串（第1章） |
| PS2 | 次shell命令行界面提示符字符串 |
| PWD | 用户帐户的当前工作目录（第1章） |
| SHLVL | 当前shell级别（第1章） |
| TZ | 用户的时区，如果与系统时区不同（第6章） |
| UID | 用户帐户的用户识别号（第7章） |
| VISUAL | 部分shell命令使用的默认屏幕编辑器（第1章） |

你可以使用set命令显示shell中可用的活动环境变量的完整列表，如清单1.13所示。

### 清单 1.13：使用set显示活动环境变量

```
$ set
[...]
BASH=/bin/bash
[...]
HISTFILE=/home/Christine/.bash_history
[...]
HISTSIZE=1000
HOME=/home/Christine
HOSTNAME=localhost.localdomain
[...]
PS1='$ '
PS2='> '
[...]
SHELL=/bin/bash
[...]
$
```

![](img/ca7617340ddbe2360d36ce3428729fa9_64_0.png)

> 除了set实用程序外，还可以使用env和printenv命令显示变量。env和printenv实用程序允许您查看本地定义的变量，例如在shell脚本中创建的变量（将在第9章介绍）以及环境变量。

当你在shell提示符处输入程序名（命令）时，shell会在PATH环境变量中列出的所有目录中搜索该程序。如果shell找不到该程序，您将收到命令未找到错误消息。清单1.14显示了一个示例。

### 清单 1.14：查看PATH如何影响命令执行

```
$ echo $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:
/home/Christine/.local/bin:/home/Christine/bin
$
$ ls /home/Christine/Hello.sh
/home/Christine/Hello.sh
$
$ Hello.sh
bash: Hello.sh: command not found...
$
```

请注意，在清单1.14中，程序Hello.sh位于不在PATH环境变量目录中的目录中。因此，当在shell提示符处输入Hello.sh程序名时，会显示命令未找到错误消息。

要运行不位于PATH目录位置中的程序，必须在命令行处输入程序名时提供命令的绝对目录引用，如清单1.15所示。

### 清单 1.15：在PATH目录外执行程序

```
$ /home/Christine/Hello.sh
Hello World
$
```

在这些情况下，which实用程序非常有用。它会搜索PATH目录以查找程序。如果找到程序，它会显示其绝对目录引用。这样可以节省您自己查看PATH变量输出的麻烦，如清单1.16所示。

### 清单 1.16：使用which实用程序

```
$ which Hello.sh
/usr/bin/which: no Hello.sh in (/usr/local/bin:/usr/bin:
/usr/local/sbin:/usr/sbin:/home/Christine/.local/bin:/home/Christine/bin)
$
$ which echo
/usr/bin/echo
$
```

请注意，which实用程序未找到Hello.sh程序并显示它搜索的所有PATH目录。但是，它确实找到了echo命令。

如果程序位于PATH目录中，可以通过简单地输入命令名来运行它。如果需要，还可以包含其绝对目录引用来执行它，如清单1.17所示。

### 清单 1.17：使用不同引用来运行命令

```
$ echo Hello World
Hello World
$
$ /usr/bin/echo Hello World
Hello World
$
```

你可以修改环境变量。容易更改的是控制shell提示符的变量（PS1）。更改此变量的示例显示在清单1.18中。

### 清单 1.18：设置PS1变量

```
$ PS1="My new prompt: "
My new prompt:
```

请注意，要更改环境变量，只需输入变量名，后跟等号（=），然后键入新值。因为PS1变量控制shell提示符，所以更改它的效果会立即显示。

但是，如果使用这种修改环境变量的简单方法，可能会遇到问题。例如，该设置不会在进入子shell后保留。当执行某些任务（例如运行shell脚本（将在第9章介绍）或运行特定命令）时，会创建子shell（有时称为子shell）。

你可以通过查看SHLVL环境变量中存储的数据来确定进程当前是否在子shell中。1表示你不在子shell中，因为子shell有更高的编号。因此，如果SHLVL包含高于1的数字，则表示你在子shell中。

bash命令会自动创建一个子shell，这有助于演示使用简单环境变量修改方法的暂时性，如清单1.19所示。

### 清单 1.19：演示子shell对PS1变量的影响

```
My new prompt: echo $SHLVL
1
My new prompt: bash
$
$ echo $PS1
$
$ echo $SHLVL
2
$ exit
exit
My new prompt:
```

请注意，SHLVL环境变量设置为1，直到通过bash命令进入子shell。还要注意，控制提示符的PS1环境变量不会在进入子shell后保留。

要保留环境变量的设置，需要使用export命令。可以在键入原始变量定义时使用export，如清单1.20所示，也可以在定义变量后使用它，在命令行提示符处键入export variable-name。

### 清单 1.20：使用export保留环境变量的定义

```
My new prompt: export PS1="KeepPrompt: "
KeepPrompt:
KeepPrompt: bash
KeepPrompt:
KeepPrompt: echo $SHLVL
2
KeepPrompt:
KeepPrompt: PS1="$ "
$ export PS1
$
```

请注意，在清单1.20中，第一种方法在同一行上使用了export命令来设置PS1环境变量，并且该定义在子shell中保留。对PS1变量的第二次更改先定义它，然后使用export命令，因此它也会在子shell中保留。

如果变量最初设置为空（空白），例如通常情况下的EDITOR环境变量，可以通过使用unset命令简单地撤销对变量所做的任何修改。此示例显示在清单1.21中。

### 清单 1.21：使用unset命令

```
$ echo $EDITOR
$ export EDITOR=nano
$
$ echo $EDITOR
nano
nano
$
$ unset EDITOR
$
$ echo $EDITOR
$
$
```

![](img/ca7617340ddbe2360d36ce3428729fa9_67_0.png)

使用unset命令时要小心。如果在环境变量（如PS1）上使用它，可能会引起令人困惑的事情。如果变量在修改之前有不同的定义，最好将其改回原始设置，而不是在变量上使用unset。

## 获取帮助

在命令行使用各种实用程序时，有时需要对命令的选项或语法获得额外帮助。虽然搜索引擎很有用，但有时您可能无法访问互联网。幸运的是，Linux系统通常在本地安装了man页面。man页面包含有关命令用途、各种选项、程序语法等内容的文档。这些信息通常由编写实用程序的程序员创建。

要访问特定程序的man页面（例如uname命令），只需在命令行处键入man uname。这个基于文本的帮助系统将接管整个终端显示，允许您阅读所选命令的文档。

默认情况下，man页面使用less分页实用程序（将在本章后面介绍），允许您使用PageUp或PageDown键在显示中前后移动。也可以根据需要使用箭头键或空格键。

> 如果使用man页面阅读内置命令的文档，将到达Bash内置命令的通用命令手册页。
> 
> 使用键查找此页面上的命令可能很乏味。相反，键入/并后跟命令名称。可能需要这样做两到三次才能到达实用程序的文档，但这比不断按箭头键或PageDown键要快得多。

man实用程序的一个便利功能是能够在文档中搜索关键字。只需使用-k选项，如清单1.22所示。

### 清单 1.22：使用man -k命令搜索关键字

```
$ man -k passwd
[...]
passwd (1)             - update user's authentication tokens
[...]
passwd (5)             - password file
[...]
smbpasswd (5)          - The Samba encrypted password file
[...]
$
```

> 而不是使用man -k，可以使用apropos命令。例如，在命令行处输入apropos passwd。但是，man -k更容易键入。
