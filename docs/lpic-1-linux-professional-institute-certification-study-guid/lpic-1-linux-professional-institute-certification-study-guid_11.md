## 第 9 章

## 编写脚本

## 目标

- 105.2 自定义或编写简单脚本
- 107.2 通过调度作业来自动化系统管理任务

![](img/ca7617340ddbe2360d36ce3428729fa9_517_0.png)

Linux 系统管理员常需要反复执行相同任务，例如检查系统可用磁盘空间或创建用户帐户。与其每次都输入多个命令，不如编写在 shell 中运行的脚本，为你自动完成这些任务。本章探讨 Bash shell 脚本的工作原理，并演示如何编写自己的脚本来自动化 Linux 系统上的日常活动。

## Shell 变量

在讨论如何编写脚本之前，了解 Bash shell 如何存储供我们在脚本中使用的数据是个好主意。Bash shell 使用一种称为 *环境变量* 的功能来存储有关 shell 会话和工作环境的信息（因此得名环境变量）。此功能将数据存储在内存中，以便从 shell 运行的任何程序或脚本都能轻松访问它。这是一种存储持久数据的便捷方式，用于标识用户帐户、系统、shell 或任何你需要存储的其他内容的功能。

- Bash shell 中有两种类型的环境变量：
  - 全局变量
  - 局部变量

本节描述每种环境变量类型并展示如何使用它们。

> 尽管 Bash shell 使用一致的特定环境变量，但不同的 Linux 发行版通常会添加自己的环境变量。本章中你看到的环境变量示例可能与你特定 Linux 发行版中可用的略有不同。如果你遇到未在此处介绍的环境变量，请查看你 Linux 发行版的文档。

## 全局环境变量

全局环境变量从 shell 会话和 shell 生成的任何子进程都可见。局部变量仅在创建它们的 shell 中可用。这使得全局环境变量在需要从父进程获取信息的子进程的应用程序中很有用。

当你启动 Bash 会话时，Linux 系统会设置几个全局环境变量（有关此时启动的变量的更多详细信息，请参阅本章稍后的“查找系统环境变量”部分）。系统环境变量始终使用全大写字母，以便与普通用户环境变量区分开来。

要查看全局环境变量，请使用 printenv 命令，如清单 9.1 所示。

## 清单 9.1：printenv 命令的输出

```
$ printenv
HOSTNAME=testbox.localdomain
TERM=xterm
SHELL=/bin/bash
HISTSIZE=1000
SSH_CLIENT=192.168.1.2 1358 22
OLDPWD=/home/rich/test/test1
SSH_TTY=/dev/pts/0
USER=rich
LS_COLORS=no=00:fi=00:di=00;34:ln=00;36:pi=40;33:so=00;35:
bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=00;32:
*.cmd=00;32:*.exe=00;32:*.com=00;32:*.btm=00;32:*.bat=00;32:
*.sh=00;32:*.csh=00;32:*.tar=00;31:*.tgz=00;31:*.arj=00;31:
*.taz=00;31:*.lzh=00;31:*.zip=00;31:*.z=00;31:*.Z=00;31:
*.gz=00;31:*.bz2=00;31:*.bz=00;31:*.tz=00;31:*.rpm=00;31:
*.cpio=00;31:*.jpg=00;35:*.gif=00;35:*.bmp=00;35:*.xbm=00;35:
*.xpm=00;35:*.png=00;35:*.tif=00;35:
MAIL=/var/spool/mail/rich
PATH=/usr/kerberos/bin:/usr/libccache:/usr/local/bin:/bin:/usr/bin:
/home/rich/bin
INPUTRC=/etc/inputrc
PWD=/home/rich
LANG=en_US.UTF-8
SSH_ASKPASS=/usr/libexec/openssh/gnome-ssh-askpass
SHLVL=1
HOME=/home/rich
LOGNAME=rich
CVS_RSH=ssh
SSH_CONNECTION=192.168.1.2 1358 192.168.1.4 22
LESSOPEN=||/usr/bin/lesspipe.sh %s G_BROKEN_FILENAMES=1
_=/usr/bin/printenv
$
```




正如你所见，Bash shell 中会设置许多全局环境变量。其中大多数是在登录过程中由系统设置的。

要显示单个环境变量的值，请使用 echo 命令。引用环境变量时，必须在环境变量名称前加上美元符号：

```
$ echo $HOME
/home/rich
$
```

如前所述，全局环境变量也可用于当前 shell 会话下运行的子进程：

```
$ bash
$ echo $HOME
/home/rich
$
```

在此示例中，使用 bash 命令启动新 shell 后，我们显示了 HOME 环境变量的当前值（系统在你登录主 shell 时设置该变量）。果然，该值也可从子 shell 进程中获取。

## 本地环境变量

顾名思义，本地环境变量只能在定义它们的本地进程中查看。不过，不要对本地环境变量感到困惑；它们与全局环境变量同样重要。事实上，Linux 系统也会默认为你定义标准的本地环境变量。

想要查看本地环境变量列表有点棘手。不幸的是，没有命令只显示本地环境变量。set 命令显示特定进程设置的所有环境变量。然而，这也包括全局环境变量。

### 清单 9.2: set 命令的输出

```
$ set
BASH=/bin/bash
BASH_ARGC=()
BASH_ARGV=()
BASH_LINENO=()
BASH_SOURCE=()
BASH_VERSINFO=([0]="3" [1]="2" [2]="9" [3]="1" [4]="release"
[5]="i686-redhat-linux-gnu")
BASH_VERSION='3.2.9(1)-release'
COLORS=/etc/DIR_COLORS.xterm
COLUMNS=80
CVS_RSH=ssh
DIRSTACK=()
EUID=500
GROUPS=()
_G_BROKEN_FILENAMES=1
HISTFILE=/home/rich/.bash_history
HISTFILESIZE=1000
HISTSIZE=1000
HOME=/home/rich
HOSTNAME=testbox.localdomain
HOSTTYPE=i686
IFS=$' \t\n'
INPUTRC=/etc/inputrc
LANG=en_US.UTF-8
LESSOPEN='| /usr/bin/lesspipe.sh %s'
LINES=24
LOGNAME=rich
LS_COLORS='no=00:fi=00:di=00;34:ln=00;36:pi=40;33:so=00;35:bd=40;33;
01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=00;32:*.cmd=00;32:
*.exe=00;32:*.com=00;32:*.btm=00;32:*.bat=00;32:*.sh=00;32:
*.csh=00;32:*.tar=00;31:*.tgz=00;31:*.arj=00;31:*.taz=00;31:
*.lzh=00;31:*.zip=00;31:*.z=00;31:*.Z=00;31:*.gz=00;31:*.bz2=00;31:
*.bz=00;31:*.tz=00;31:*.rpm=00;31:*.cpio=00;31:*.jpg=00;35:
*.gif=00;35:*.bmp=00;35:*.xbm=00;35:*.xpm=00;35:*.png=00;35:
*.tif=00;35:'
MACHTYPE=i686-redhat-linux-gnu
MAIL=/var/spool/mail/rich
MAILCHECK=60
OPTERR=1
OPTIND=1
OSTYPE=linux-gnu
PATH=/usr/kerberos/bin:/usr/lib/ccache:/usr/local/bin:/bin:/usr/bin:
/home/rich/bin
PIPESTATUS=([0]="0")
PPID=3702
PROMPT_COMMAND='echo -ne "\033]0;${USER}@${HOSTNAME%%.*}:${PWD/#$HOME/~}"; echo -ne "\007"'
PS1='[\u@\h \W]\$ '
PS2='> '
PS4='+ '
PWD=/home/rich
SHELL=/bin/bash
SHELLOPTS=braceexpand:emacs:hashall:histexpand:history:
interactive-comments:monitor
SHLVL=2
SSH_ASKPASS=/usr/libexec/openssh/gnome-ssh-askpass
SSH_CLIENT='192.168.1.2 1358 22'
SSH_CONNECTION='192.168.1.2 1358 192.168.1.4 22'
SSH_TTY=/dev/pts/0
TERM=xterm
UID=500
USER=rich
_=-H
consolertype=pty
$
```

你会发现，printenv 命令中看到的所有全局环境变量都出现在 set 命令的输出中。然而，现在出现了许多额外的环境变量——这些就是本地环境变量。

### 清单 9.2: set 命令的输出（续）

## 设置本地环境变量

启动 Bash shell（或产生 shell 脚本）时，你可以创建仅在 shell 进程中可见的本地变量。你可以通过使用等号将变量赋值给某个值，为环境变量赋予数值或字符串值：

```
$ test=testing
$ echo $test
testing
$
```

任何时候你需要引用 test 环境变量的值时，只需通过名称 $test 来引用它。

如果你需要赋予包含空格的字符串值，则需要使用引号来界定字符串的开头和结尾：

```
$ test=testing a long string
-bash: a: command not found
$ test='testing a long string'
$ echo $test
testing a long string
$
```

如果没有单引号，Bash shell 会假设下一个字符是另一个要处理的命令。请注意，对于我们定义的本地环境变量，我们使用了小写字母，而目前为止看到的系统环境变量都使用了大写字母。

这是 Bash shell 中的标准惯例。如果你创建新的环境变量，建议（但非强制）使用小写字母。这有助于将你的个人环境变量与众多系统环境变量区分开来。

至关重要的是，环境变量名称、等号和值之间不能有空格。如果在赋值中加入任何空格，Bash shell 会将值解释为单独的命令：

```
$ test2 = test
-bash: test2: command not found
$
```

设置本地环境变量后，它可以在 shell 进程中的任何地方使用。但是，如果你产生另一个 shell，则在子 shell 中不可用：

```
$ bash
$ echo $test

$ exit
exit
$ echo $test
testing a long string
$
```

在此示例中，我们启动了一个子 shell，正如你所见，test 环境变量在子 shell 中不可用（其值为空白）。退出子 shell 并返回原始 shell 后，test 本地环境变量仍然可用。

同样，如果你在子进程中设置本地环境变量，当你离开子进程时，本地环境变量将不再可用：

```
$ bash
$ test=testing
$ echo $test
testing
$ exit
exit
$ echo $test

$
```

当我们返回父 shell 时，在子 shell 中设置的 test 环境变量不存在。

## 设置全局环境变量

全局环境变量对设置全局环境变量的进程创建的任何子进程都可见。创建全局环境变量的方法是先创建一个本地环境变量，然后将其导出到全局环境中。这是通过使用 export 命令完成的：

```
$ echo $test
testing a long string
$ export test
$ bash
$ echo $test
testing a long string
$
```

导出本地环境变量 test 后，我们启动了一个新 shell 进程并查看了 test 环境变量的值。这次，环境变量保留了其值，因为 export 命令使其变为全局。

> 注意：导出本地环境变量时，不需要使用美元符号来引用变量名。

## 查找系统环境变量

Linux 系统使用环境变量在程序和脚本中识别自身。这为你的程序获取系统信息提供了一种便捷方式。关键在于这些环境变量是如何设置的。

当你通过登录 Linux 系统启动 Bash shell 时，默认情况下 Bash 会检查多个文件中的命令。这些文件称为启动文件。Bash 处理的启动文件取决于你启动 Bash shell 的方式。启动 Bash shell 有三种方式：

- 作为登录时的默认登录 shell
- 作为不是登录 shell 的交互式 shell
- 作为用于运行脚本的非交互式 shell

以下部分描述了 Bash shell 在每种启动方式下执行的启动文件。

### 登录 shell

当你登录 Linux 系统时，Bash shell 作为登录 shell 启动。登录 shell 会查找四个不同的启动文件来处理命令。Bash shell 处理文件的顺序如下：

- `/etc/profile`
- `$HOME/.bash_profile`
- `$HOME/.bash_login`
- `$HOME/.profile`

`/etc/profile` 文件是 Bash shell 的主要默认启动文件。每次你登录 Linux 系统时，Bash 都会执行 `/etc/profile` 启动文件中的命令。不同的 Linux 发行版会将不同的命令放入此文件中。

> 大多数 Linux 发行版还会在启动时检查 `/etc/profile.d` 目录中的文件。这使得单个应用程序可以更轻松地设置环境变量，而无需修改主 `/etc/profile` 文件。

其余三个启动文件均用于相同功能——提供用于定义用户特定环境变量的用户特定启动文件。大多数 Linux 发行版仅使用这三个启动文件中的一个：

- `$HOME/.bash_profile`
- `$HOME/.bash_login`
- `$HOME/.profile`

请注意，这三个文件都以点开头，因此是隐藏文件（它们不会出现在普通 `ls` 命令列表中）。由于它们位于用户的 HOME 目录中，每个用户都可以编辑这些文件并添加自己的环境变量，这些变量在他们启动的每个 Bash shell 会话中都有效。

> 虽然不常这样做，但你也可以使用 `$HOME/.bash_logout` 文件在从 Linux 会话注销时运行脚本。

### 交互式 shell

如果你在未登录系统的情况下启动 Bash shell（例如，在 CLI 提示符下键入 `bash` 或打开 GUI 终端），则会启动所谓的交互式 shell。交互式 shell 的行为与登录 shell 不同，但它仍然提供一个 CLI 提示符供你输入命令。




如果 Bash 作为交互式 Shell 启动，它不会处理 /etc/profile 文件。相反，它会检查用户 HOME 目录中的 .bashrc 文件（通常称为 ~/.bashrc）。

.bashrc 文件有两个作用。首先，它会检查公共 /etc/bash.bashrc 文件。公共 bash.bashrc 文件为你提供了一种方法，用于设置所有启动交互式 Shell 的用户都会使用的脚本和变量。其次，它为用户提供了一个输入个人别名和私有脚本函数的地方。

### 非交互式 Shell

最后，最后一种 shell 是非交互式 shell。这是系统用于执行 shell 脚本时启动的 shell。它的不同之处在于没有需要担心的 CLI 提示符。但是，你可能仍然希望在系统上每次启动脚本时运行某些特定的启动命令。

为了适应这种情况，Bash shell 提供了 BASH_ENV 环境变量。当 shell 启动非交互式 shell 进程时，它会检查此环境变量，以获取要执行的启动文件名。如果存在，shell 将执行文件中的命令。在大多数 Linux 发行版中，此环境变量默认未设置。

### 使用命令别名

尽管 shell 命令别名本身并非环境变量，但其行为方式大致相同。_命令别名_ 允许你为常用命令（及其参数）创建别名，以最大限度减少输入。

你的 Linux 发行版很可能已经为你设置了一些常用命令别名。要查看活动别名列表，请带 -p 参数使用 alias 命令：

```
$ alias -p
alias l.='ls -d .* --color=tty'
alias ll='ls -l --color=tty'
alias ls='ls --color=tty'
alias vi='vim'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
$
```

请注意，在此示例中，Linux 发行版使用别名覆盖了标准 ls 命令。它会自动提供 --color 参数，使用颜色来指示列表中的文件和目录对象。

你可以使用 alias 命令创建自己的别名：

```
$ alias li='ls -l'
$ li
total 52
4508 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Desktop
4512 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Documents
4509 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Downloads
401354 -rw-r--r-- 1 rich rich 8980 Jun 12 11:06 examples.desktop
388616 -rw-r--r-- 1 rich rich 10 Jul 5 11:41 file1
389657 -rw-r--r-- 1 rich rich 12 Jul 5 11:41 file2
4513 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Music
389659 -rwxr-xr-x 1 rich rich 200 Jul 5 11:41 myscript.sh
388895 -rw-r--r-- 1 rich rich 36 Jun 27 14:23 nc
4514 drwxr-xr-x 2 rich rich 4096 Jun 28 08:47 Pictures
4511 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Public
417850 drwxr-xr-x 3 rich rich 4096 Jun 28 08:46 snap
4510 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Templates
4515 drwxr-xr-x 2 rich rich 4096 Jun 12 11:21 Videos
$
```

定义别名值后，你可以随时在 shell 中使用它，包括在 shell 脚本中。

命令别名的行为类似于局部环境变量。它们仅在定义它们的 shell 进程中有效：

```
$ alias li='ls -il'
$ bash
$ li
bash: li: command not found
$
```

当然，现在你知道解决这个问题的方法了。由于 Bash shell 在启动新的交互式 shell 时始终会读取 $HOME/.bashrc 启动文件，只需将命令别名语句放在其中，它们对你启动的所有 shell 都有效。

## Shell 脚本基础

Shell 脚本允许你编写小型程序，以自动执行 Linux 系统上的活动。Shell 脚本可以节省你的时间，为你提供快速处理数据和生成报告的灵活性，而这些操作如果在命令提示符下手动输入多个命令会非常麻烦。你可以使用 shell 脚本自动化你在命令提示符下执行的几乎所有操作。

本节将介绍 shell 脚本是什么以及如何开始编写它们。

### 运行多个命令

迄今为止，本书中我们一直在命令提示符下输入单个命令并查看结果。Linux 命令行的一项令人兴奋的功能是，你可以在同一行命令行中输入多个命令，Linux 会全部处理它们。只需在每个输入的命令之间放置一个分号：

```
$ date ; who
Thu Feb 20 19:20:06 EST 2019
rich   :0   2019-02-20 19:15 (:0)
$
```

Linux Bash shell 运行第一个命令（date）并显示输出；然后运行第二个命令（who）并显示该命令的输出，紧随第一个命令的输出之后。虽然这可能看起来微不足道，但它是 shell 脚本工作原理的基础。

### 重定向输出

Shell 脚本的另一个基础是能够存储命令输出。当你运行命令时，通常希望保存输出以供将来参考。为此，Bash shell 提供了输出重定向。

输出重定向允许我们将命令的输出从显示器重定向到其他设备（例如文件）。当你需要记录非工作时间运行的 shell 脚本的数据时，此功能非常有用，以便你可以查看 shell 脚本运行时做了什么。

要重定向命令的输出，请在命令之后使用大于号 (>) 并指定要用于捕获重定向输出的文件名。清单 9.3 演示了此过程。

### 清单 9.3: 将输出重定向到文件

```
$ date > today.txt
$ cat today.txt
Thu Feb 20 19:21:12 EST 2019
$
```

清单 9.3 中所示的示例将 date 命令的输出重定向到名为 today.txt 的文件。请注意，当你重定向命令输出时，显示器上不会显示任何内容。输出的所有文本现在都在文件中，如使用 cat 命令显示文件内容所示。

大于号输出重定向运算符会自动为输出创建一个新文件，即使文件已存在。如果你愿意，可以使用双大于号 (>>) 将输出追加到现有文件，如清单 9.4 所示。

### 清单 9.4: 将命令输出追加到文件

```
$ who >> today.txt
$ cat today.txt
Thu Feb 20 19:21:12 EST 2019
rich   :0   2019-02-20 19:15 (:0)
$
```

today.txt 文件现在包含清单 9.3 中原始 date 命令的输出，以及在清单 9.4 中运行的 who 命令的输出。

在 Linux 中，一切都是文件，包括命令的输入和输出过程。Linux 使用文件描述符来标识文件，文件描述符是一个非负整数。Bash shell 为输入和输出保留了前三个文件描述符。文件描述符 0 称为 STDIN，指向 shell 的标准输入，通常是键盘。文件描述符 1 称为 STDOUT，指向 shell 的标准输出，通常是显示器。标准输出消息会发送到这里。文件描述符 2 称为 STDERR，shell 会将其识别为错误的消息发送到这里。默认情况下，它指向与 STDOUT 文件描述符相同的设备，即显示器。你可以使用 2> 而不是标准 > 输出重定向字符，仅将 shell 脚本中的错误重定向到与正常输出分离的文件。这样，你可以指定一个单独的文件来监控命令的错误消息。

输出重定向是 shell 脚本中至关重要的功能。借助它，我们可以从脚本生成日志文件，从而在脚本在 Linux 系统上后台运行时跟踪事情。

### 管道数据

与输出重定向允许我们将命令输出重定向到文件不同，管道允许我们将输出重定向到另一个命令。第二个命令将第一个命令的重定向输出用作输入数据。当使用处理数据的命令（例如 sort 命令）时，此功能非常有用。

管道符号是竖线 (|) 符号，它通常出现在美国键盘的反斜杠键上方。清单 9.5 展示了使用管道的示例。

### 清单 9.5: 将命令输出管道到另一个命令

```
$ ls | sort
Desktop
Documents
Downloads
Music
Pictures
Public
Templates
test.txt
today.txt
Videos
$





The output from the ls command is sent directly to the sort command as input but behind the scenes. You don't see the output from the ls command displayed on the monitor; you see only the output from the last command in the pipeline, which in this case is the sort command. There's no limit to how many commands you can link together with piping.

> The >, >>, and | symbols are part of a group of characters often referred to as metacharacters. Metacharacters are characters that have a special meaning when used in the Linux shell. If you need to use a metacharacter as a standard character (such as using the > character as a greater-than symbol in your output instead of as a redirect symbol), you must identify the metacharacter by either placing a backslash (\\) in front of it or enclosing the metacharacter in single (') or double quotes ("). This method is called escaping.

## The Shell Script Format

Placing multiple commands on a single line, either by using the semicolon or piping, is a great way to process data, but it can still get rather tedious. Each time you want to run the set of commands, you need to type them all at the command prompt.

However, Linux allows us to place multiple commands in a text file and then run the text file as a program from the command line. This is called a shell script because we're scripting out commands for the Linux shell to run.

Shell script files are plain text files. To create a shell script file, you just need to use any text editor that you're comfortable with. If you're working from a KDE-based graphical desktop, you can use the KWrite program, or if you're working from a GNOME-based graphical desktop, you can use the GEdit program.

> Shell script files must be text files. If you use a word processing program to create the file (or even copy and paste text from a word processing document into a file), the script will not run correctly in the shell.

If you're working directly in a command-line environment, you still have some options. Many Linux distributions include either the pico or nano editor to provide a graphical editor environment by using ASCII control characters to create a full-screen editing window.

If your Linux distribution doesn't include either the pico or nano editor, there is still one last resort: the vi editor. The vi editor is a text-based editor that uses simple single-letter commands. It's the oldest text editor in the Linux environment, dating back to the early days of Unix, which may be one reason why it's not overly elegant or user-friendly.

When you've chosen your text editor, you're ready to create your shell scripts. First, for your shell script to work you'll need to follow a specific format for the shell script file. The first line in the file must specify the Linux shell required to run the script. This is written in somewhat of an odd format:

```
#!/bin/bash
```

The Linux world calls the combination of the pound sign and the exclamation symbol (#!) the shebang. It signals to the operating system which shell to use to run the shell script. Most Linux distributions support multiple Linux shells, but the most common is the Bash shell. You can run shell scripts written for other shells, as long as that shell is installed on the Linux distribution.

After you specify the shell, you're ready to start listing the commands in your script. You don't need to enter all of the commands on a single line; Linux allows you to place them on separate lines. Also, the Linux shell assumes each line is a new command in the shell script, so you don't need to use semicolons to separate the commands. Listing 9.6 shows an example of a simple shell script file.

### Listing 9.6: A simple shell script file

```
$ cat test1.sh
#!/bin/bash
# This script displays the date and who's logged in
date
who
$
```

The test1.sh script file shown in Listing 9.6 starts out with the shebang line identifying the Bash shell, the standard shell in Linux. The second line in the code shown in Listing 9.6 demonstrates another feature in shell scripts. Lines that start with a pound sign are called comment lines. They allow you to embed comments into the shell script program to help you remember what the code is doing. The shell skips comment lines when processing the shell script. You can place comment lines anywhere in your shell script file after the opening shebang line.

Notice in Listing 9.6 we used the .sh filename extension on the shell script file. Although this is not required in Linux, it's become somewhat of a de facto standard among programmers. It helps identify that the text file is a shell script that can be run at the command line.

## Running the Shell Script

If you just enter a shell script file at the command prompt to run it, you may be a bit disappointed:

```
$ test1.sh
test1.sh: command not found
$
```

Unfortunately, the shell doesn't know where to find the test1.sh command in the virtual directory. The reason for this is the shell uses a special environment variable called PATH to list directories where it looks for commands. If your local HOME folder is not included in the PATH environment variable list of directories, you can't run the shell script file directly. Instead, you must use either a relative or an absolute path name to point to the shell script file. The easiest way to do that is by adding the ./ relative path shortcut to the file:

```
$ ./test1.sh
bash: ./test1.sh: Permission denied
$
```

Now the shell can find the program file, but there's still an error message. This time the error is telling us that we don't have permissions to run the shell script file. A quick look at the shell script file using the ls command with the -l option shows the permissions set for the file:

```
$ ls -l test1.sh
-rw-r--r-- 1 rich rich 73 Feb 20 19:37 test1.sh
$
```

By default, the Linux system didn't give anyone execute permissions to run the file. You can use the chmod command to add that permission for the file owner:

```
$ chmod u+x test1.sh
$ ls -l test1.sh
-rwxr--r-- 1 rich rich 73 Feb 20 19:37 test1.sh
$
```

The u+x option adds execute privileges to the owner of the file. You should now be able to run the shell script file and see the output:

```
$ ./test1.sh
Thu Feb 20 19:48:27 EST 2019
rich :0 2019-02-20 19:15 (:0)
$
```

Running the script directly from the command prompt spawns a new subshell for the script, making any local environment variables in the shell not available to the script. If you want to make any local environment variables available for use in the shell script, launch the script using the exec command:

```
$ exec ./test1.sh
Thu Feb 20 19:51:27 EST 2019
rich :0 2019-02-20 19:15 (:0)
$
```

The exec command runs the script in the current shell.

Now that you've seen the basics for creating and running shell scripts, the next section dives into some more advanced features you can add to make fancier shell scripts.

## Advanced Shell Scripting

The previous section walked through the basics of how to group normal command-line commands together in a shell script file to run in the Linux shell. This section adds to that by showing more features available in shell scripts to make them look and act more like real programs.

### Displaying Messages

When you string commands together in a shell script file, the output may be somewhat confusing to look at. It would help to be able to customize the output by separating the output and adding our own text between the output from the listed commands.

The echo command allows you to display text messages from the command line. When used at the command line, it's not too exciting:

```
$ echo This is a test
This is a test
$
```

But with echo, you can now insert messages anywhere in the output from the shell script file. Listing 9.7 demonstrates how this is done.

### Listing 9.7: Displaying messages from shell scripts

```
$ cat test1.sh
#!/bin/bash
# This script displays the date and who's logged in
echo The current date and time is:
date
echo
echo "Let's see who's logged into the system:"
who
$ ./test1.sh
The current date and time is:
Thu Feb 20 19:55:44 EST 2019

Let's see who's logged into the system:
rich   :0           2019-02-20 19:15 (:0)
$
```





Listing 9.7 中所示的 shell 脚本向 test1.sh 脚本添加了三个 echo 命令。请注意，第一个 echo 命令没有使用任何引号，但第三个命令使用了。这样做的原因是第三个 echo 命令的文本输出包含单引号。单引号也是 shell 中的元字符，会让 echo 命令感到困惑，因此你需要将文本放在双引号中。还要注意，第二个 echo 命令的这一行没有任何文本。这会输出一个空行，当你想要分隔多个命令的输出时，这很有用。

## 在脚本中使用变量

编程的一部分是能够暂时存储数据以便在程序稍后使用。如前面*“Shell 变量”*部分所述，你可以在 Bash shell 中使用全局变量或局部变量来实现这一点。这种方法在使用 shell 脚本时也适用。

### 使用全局环境变量

如前面*“使用变量”*部分所述，全局环境变量通常用于跟踪特定的系统信息，例如系统名称、登录到 shell 的用户名、用户的用户 ID (UID)、用户的默认主目录以及 shell 用于查找可执行程序的搜索路径。你可以在脚本中通过使用环境变量名（前面加上美元符号）来访问这些环境变量，如 Listing 9.8 所示。

Listing 9.8: 显示环境变量的 test2.sh shell 脚本文件

```bash
$ cat test2.sh
#!/bin/bash
# display user information from the system.
echo User info for userid: $USER
echo UID: $UID
echo HOME: $HOME
$
```

$USER、$UID 和 $HOME 环境变量通常用于显示已登录用户的信息。如果你运行 Listing 9.8 中所示的 test2.sh shell 脚本，输出应如下所示：

```bash
$ chmod u+x test2.sh
$ ./test2.sh
User info for userid: rich
UID: 1000
HOME: /home/rich
$
```

你看到的值应与你的用户帐户相关。这允许你动态检索运行 shell 脚本的用户帐户信息，以自定义输出。

### 定义局部变量

shell 脚本中使用的大多数变量都是用于在 shell 脚本中存储你自己的数据的局部变量。你使用等号为局部变量赋值。变量名、等号和值之间不得出现空格。以下是几个示例：

```bash
var1=10
var2=23.45
var3=testing
var4="Still more testing"
```

shell 脚本会自动确定变量值使用的数据类型。Listing 9.9 展示了使用局部变量编写 shell 脚本的示例。

### Listing 9.9: 在 shell 脚本中使用局部变量

```bash
$ cat test3.sh
#!/bin/bash
# testing variables
days=10
guest=Katie
echo $guest checked in $days days ago
$
```

运行 Listing 9.9 中的 test3.sh 脚本会产生以下输出：

```bash
$ chmod u+x test3.sh
$ ./test3.sh
Katie checked in 10 days ago
$
```

将数据存储在局部变量后，你可以在 shell 脚本的任何位置引用它。但是，请记住，shell 脚本中定义的局部变量默认只能从 shell 脚本内部访问。如果你希望 shell 脚本中的变量对启动 shell 脚本的父 shell 可见，请使用 export 命令。

> 在 echo 语句中使用变量时要小心。由于变量名只是文本值，如果你尝试向变量名追加文本，shell 会将该文本视为变量名的一部分，因此你无法得到预期的结果。如果需要这样做，可以将变量名用大括号括起来，例如 ${guest}。这确保追加到变量末尾的任何文本都与变量名分开。

## 命令行参数

shell 脚本最通用的功能之一是在运行脚本时能够向脚本传递数据。这允许你每次运行脚本时都用新数据自定义脚本。

向 shell 脚本传递数据的一种方法是使用命令行参数。命令行参数是你在运行命令时包含在命令行中的数据。只需在命令后开始列出它们，用空格分隔每个数据值，格式如下：

```bash
command argument1 argument2 ...
```

你可以在 shell 脚本代码中使用特殊的数字位置变量来检索这些值。使用变量 $1 检索第一个命令行参数，$2 检索第二个参数，依此类推。Listing 9.10 展示了如何在 shell 脚本中使用位置变量。

### Listing 9.10: 在 shell 脚本中使用命令行参数

```bash
$ cat test4.sh
#!/bin/bash
# Testing command line arguments
echo $1 checked in $2 days ago
$ chmod u+x test4.sh
$ ./test4.sh Barbara 4
Barbara checked in 4 days ago
$ ./test4.sh Jessica 5
Jessica checked in 5 days ago
$
```

test4.sh shell 脚本使用两个命令行参数。$1 变量保存人员姓名，$2 变量保存他们几天前入住的天数。运行 test4.sh shell 脚本时，请务必在命令行中包含这两个数据值。如果位置变量不存在，shell 不会生成错误消息；你只是无法得到预期的结果：

```bash
$ ./test4.sh rich
rich checked in days ago
$
```

由你来检查位置变量是否存在于你的程序代码中。我们将在讨论*“逻辑语句”*时稍后探讨如何执行此操作。

## 获取用户输入

提供命令行选项和参数是从脚本用户获取数据的好方法，但有时你的脚本需要更具交互性。在脚本运行时，有时你需要提出问题并等待运行脚本的人做出回应。Bash shell 提供了 read 命令，专门用于此目的。

### 基本读取

read 命令接受来自标准输入（键盘）或另一个文件描述符的输入。接收输入后，read 命令将数据放入标准变量中。Listing 9.11 展示了 read 命令最简单的形式。

### Listing 9.11: 在脚本中接受用户输入

```bash
$ cat test5.sh
#!/bin/bash
# testing the read command

echo -n "Enter your name: "
read name
echo "Hello $name, welcome to my program."
$ ./test5.sh
Enter your name: Rich Blum
Hello Rich Blum, welcome to my program.
$
```

请注意，生成提示的 echo 命令使用了 -n 选项。这会抑制字符串末尾的换行字符，允许脚本用户在字符串后立即输入数据，而不是在下一行输入。这使你的脚本看起来更像表单。

实际上，read 命令包含 -p 选项，允许你直接在 read 命令行中指定提示。Listing 9.12 演示了此功能。

### Listing 9.12: 使用带 -p 选项的 read 命令

```bash
$ cat test6.sh
#!/bin/bash
# testing the read -p option

read -p "Please enter your age:" age
days=$[ $age * 365 ]
echo "That makes you over $days days old!"
$ ./test6.sh
Please enter your age:10
That makes you over 3650 days old!
$
```

在 Listing 9.11 的示例中，当我输入姓名时，read 命令将我的名和姓都分配给了同一个变量。read 命令会将提示处输入的所有数据分配给单个变量，或者你可以指定多个变量。输入的每个数据值都分配给列表中的下一个变量。如果变量列表在数据之前用完，则剩余数据分配给最后一个变量：

```bash
$ cat test7.sh
#!/bin/bash
# entering multiple variables

read -p "Enter your name: " first last
echo "Checking data for $last, $first..."
$ ./test7.sh
Enter your name: Rich Blum
Checking data for Blum, Rich...
$
```

你也可以在 read 命令行中不指定任何变量。如果这样做，read 命令将接收到的任何数据都放在特殊环境变量 REPLY 中：

```bash
$ cat test8.sh
#!/bin/bash
# testing the REPLY environment variable

read -p "Enter a number: "
factorial=1
for (( count=1; count <= $REPLY; count++ ))
do
    factorial=$[ $factorial * $count ]
done
echo "The factorial of $REPLY is $factorial"
$ ./test8.sh
Enter a number: 5
The factorial of 5 is 120
$
```

此示例使用 for 循环（将在*“逻辑语句”*中稍后解释）来遍历从 1 到输入数字的数字。REPLY 环境变量将包含输入中的所有数据，并且可以像 shell 脚本中的任何其他变量一样使用。

### 超时处理

使用 read 命令时存在危险。你的脚本很有可能会卡住，等待脚本用户输入数据。如果脚本必须继续运行，无论是否输入了数据，你可以使用 -t 选项指定一个计时器。-t 选项指定

read 命令等待输入的秒数。当计时器到期时，read 命令将返回非零退出状态：

```
$ cat test9.sh
#!/bin/bash
# timing the data entry

if read -t 5 -p "Please enter your name: " name
then
    echo "Hello $name, welcome to my script"
else
    echo
    echo "Sorry, too slow!"
fi
$ ./test9.sh
Please enter your name: Rich
Hello Rich, welcome to my script
$ ./test9.sh
Please enter your name:
Sorry, too slow!
$
```

由于如果计时器到期 read 命令将以非零退出状态退出，因此可以轻松使用标准结构化语句（例如 if 语句或 while 循环）来跟踪发生的情况。在此示例中，当计时器到期时，if 语句失败，shell 将执行 else 部分中的命令。

除了对输入计时之外，您还可以将 read 命令设置为计算输入字符数。当输入了预设数量的字符后，它会自动退出，并将输入的数据分配给变量。如清单 9.13 所示。

## 清单 9.13：计算输入字符数

```
$ cat test10.sh
#!/bin/bash
# getting just one character of input

read -n1 -p "Do you want to continue [Y/N]? " answer
case $answer in
Y | y) echo
    echo "fine, continue on...";;
N | n) echo
    echo OK, goodbye
    exit;;
esac
echo "This is the end of the script"
$ ./test10.sh
Do you want to continue [Y/N]? Y
fine, continue on...
This is the end of the script
$ ./test10.sh
Do you want to continue [Y/N]? n
OK, goodbye
$
```

在清单 9.13 中，read 命令将 -n 选项的值设置为 1，指示其在接受单个字符后即退出。一旦您按下单个字符来回答，read 命令就会接受输入并将其传递给变量。无需按 Enter 键。

## 静默读取

有时您需要脚本用户的输入，但又不希望该输入显示在监视器上。典型的例子是输入密码时，但还有许多其他类型的数据需要隐藏。

-s 选项可防止 read 命令中输入的数据在监视器上显示（实际上，数据仍会显示，但 read 命令将文本颜色设置为与背景颜色相同）。以下是在脚本中使用 -s 选项的示例：

```
$ cat test11.sh
#!/bin/bash
# hiding input data from the monitor

read -s -p "Enter your password: " pass
echo
echo "Is your password really $pass?"
$ ./test11.sh
Enter your password:
Is your password really T3sting?
$
```

在输入提示符处键入的数据不会显示在监视器上，但会正常分配给变量以供脚本使用。

## 退出状态

当 shell 脚本结束时，它会将退出状态返回给启动它的父 shell。退出状态告诉我们 shell 脚本是否成功完成。

Linux 为我们提供了特殊的 $? 变量，用于保存上一条执行命令的退出状态值。要检查命令的退出状态，必须在命令结束后立即查看 $? 变量。它会根据 shell 执行的上一条命令的退出状态而更改值：

```
$ who
rich        :0           2019-02-20 23:16 (:0)
$ echo $?
0
$
```

按照惯例，成功完成的命令的退出状态为 0。如果命令在出错时完成，则退出状态将显示为正整数值。

您可以使用 exit 命令更改 shell 脚本的退出状态。只需在 exit 命令中指定所需的退出状态值即可：

```
$ /bin/bash
$ exit 120
exit
$ echo $?
120
$
```

在此示例中，我们使用 /bin/bash 命令启动了一个新的子 shell，然后使用 exit 命令以退出状态代码 120 退出子 shell。回到父 shell 中，我们显示了 $? 变量的值，以查看其是否与 exit 命令中设置的值匹配。在编写更复杂的脚本时，可以通过更改退出状态值来指示错误。这样，通过检查退出状态，您可以轻松调试 shell 脚本。

## 编写脚本程序

迄今为止，我们已探讨如何在 shell 脚本中组合常规命令行命令，以自动执行系统管理员可能执行的常见任务。但 shell 脚本允许我们做更多事情。Bash shell 提供了更多类似编程的命令，允许我们在 shell 脚本中编写功能齐全的程序，例如捕获命令输出、执行数学运算、检查变量和文件条件以及循环处理命令。本节将介绍 Bash shell 提供给我们的一些高级编程功能。

### 命令替换

shell 脚本最有用的功能之一很可能是存储和处理数据的能力。迄今为止，我们已讨论如何使用输出重定向将命令的输出存储到文件，以及使用管道将命令的输出重定向到另一个命令。然而，还有一种技术可以让您在脚本中存储和使用数据时更具灵活性。

*命令替换*允许您将命令的输出分配给 shell 脚本中的用户变量。输出存储在变量后，您可以使用标准 Linux 字符串操作命令（例如 sort 或 grep）在显示数据之前对其进行处理。

要将命令的输出重定向到变量，需要使用以下两种命令替换格式之一：

- 将反引号 (`) 放在命令周围
- 在 $() 函数中使用命令

两种方法产生相同的结果：将命令的输出重定向到用户变量。清单 9.14 演示了使用这两种方法。

## 清单 9.14：演示命令替换

```
$ var1=`date`
$ echo $var1
Fri Feb 21 18:05:38 EST 2019
$ var2=$(who)
$ echo $var2
rich :0 2019-02-21 17:56 (:0)
$
```

命令替换的输出存储在相应的变量中。然后，您可以在脚本程序中的任何位置将这些变量用作标准字符串值。

反引号字符与单引号不同。它通常是美国键盘上与波浪号字符 (~) 位于同一键上的字符。由于反引号与单引号之间容易混淆，在 Linux 世界中使用 $() 函数格式变得更加流行。

## 执行数学运算

最终，您会希望在 shell 脚本中做的不仅仅是操作文本字符串。世界围绕数字运转，到了某个时刻，您可能需要对数据进行一些数学运算。不幸的是，Bash shell 在此方面显得有些老旧。Bash shell 中的数学功能不如较新 shell（例如 Z shell）中的功能先进。然而，在 Bash shell 脚本中使用简单数学函数有几种方法。

要在 shell 脚本中包含数学表达式，请使用一种特殊格式。此格式将方程放在括号内：

```
result=$[ 25 * 5 ]
```

使用此方法可以对数据执行多种不同的数学运算，但存在限制。$[] 格式仅允许使用整数；它不支持浮点值。

如果需要进行浮点计算，在 Bash shell 中事情会复杂得多。一种解决方案是使用 bc 命令行计算器程序。bc 计算器是 Linux 中的一个工具，可以执行浮点运算：

```
$ bc
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type 'warranty'.
12 * 5.4
64.8
3.156 * (3 + 5)
25.248
quit
$
```

不幸的是，bc 计算器也有其自身的局限性。浮点运算由名为 scale 的内置变量控制。必须将此变量设置为所需的小数位数，否则无法获得想要的结果：

```
$ bc -q
3.44 / 5
0
scale=4
3.44 / 5
.6880
quit
$
```

要将 bc 计算嵌入到脚本中，事情会变得有点复杂。必须使用命令替换将计算输出捕获到变量中，但这里有个诀窍。需要使用的基本格式是：

```
variable=$(echo "options; expression" | bc)
```

第一个参数 options 允许我们设置 bc 变量，例如 scale 变量。expression 参数定义要使用 bc 计算的数学表达式。尽管这看起来很奇怪，但它有效：

```
$ var1=$(echo "scale=4; 3.44 / 5" | bc)
$ echo $var1
.6880
$
```




这并不理想，但对于小型项目来说可行。如果你有一个需要大量计算的较大编程项目，我们建议研究一下 Z shell。它支持许多高级数学函数和功能。有关 Z shell 的更多信息，请参阅我们由 Wiley Publishing 出版的《Linux 命令行与 Shell 脚本编程圣经，第 3 版》(2015)。

## 逻辑语句

迄今为止，我们介绍的所有 shell 脚本都是以线性方式处理命令的，即一条命令接一条命令。然而，并非所有编程都是线性的。有时候，你希望程序测试某些条件——例如文件是否存在或数学表达式是否为 0——并根据测试结果执行不同的命令。为此，Bash shell 提供了逻辑语句。

逻辑语句允许我们测试特定条件，然后根据条件的求值结果是 True 还是 False 逻辑值，分支到不同的代码部分。在 Bash 脚本中实现逻辑语句有几种不同的方法。

### if 语句

最基本的逻辑语句是 if 条件语句。if 条件语句的格式如下：

```bash
if [ condition ]
then
   commands
fi
```

if 语句中使用的方括号是使用 test 命令的简写形式。test 命令对条件进行求值，如果测试通过则返回 True 逻辑值，如果测试失败则返回 False 逻辑值。如果条件通过，shell 会运行 then 部分的代码命令；或者如果条件求值为 False 逻辑值，shell 脚本将跳过 then 部分的代码命令。

在 Bash shell 编程中，条件测试表达式有多种不同的格式。有针对数值、字符串值，甚至文件和目录的内置测试。表 9.1 列出了可用的不同内置测试。

| 测试 | 类型 | 描述 |
| :--- | :--- | :--- |
| n1 -eq n2 | 数值 | 检查 n1 是否等于 n2 |
| n1 -ge n2 | 数值 | 检查 n1 是否大于或等于 n2 |
| n1 -gt n2 | 数值 | 检查 n1 是否大于 n2 |
| n1 -le n2 | 数值 | 检查 n1 是否小于或等于 n2 |
| n1 -lt n2 | 数值 | 检查 n1 是否小于 n2 |
| n1 -ne n2 | 数值 | 检查 n1 是否不等于 n2 |
| str1 = str2 | 字符串 | 检查 str1 是否与 str2 相同 |
| str1 != str2 | 字符串 | 检查 str1 是否与 str2 不同 |
| str1 < str2 | 字符串 | 检查 str1 是否小于 str2 |
| str1 > str2 | 字符串 | 检查 str1 是否大于 str2 |
| -n str1 | 字符串 | 检查 str1 的长度是否大于零 |
| -z str1 | 字符串 | 检查 str1 的长度是否为零 |
| -d file | 文件 | 检查 file 是否存在且是目录 |
| -e file | 文件 | 检查 file 是否存在 |
| -f file | 文件 | 检查 file 是否存在且是文件 |
| -r file | 文件 | 检查 file 是否存在且可读 |
| -s file | 文件 | 检查 file 是否存在且非空 |
| -w file | 文件 | 检查 file 是否存在且可写 |
| -x file | 文件 | 检查 file 是否存在且可执行 |
| -O file | 文件 | 检查 file 是否存在且由当前用户拥有 |
| -G file | 文件 | 检查 file 是否存在且默认组与当前用户相同 |
| file1 -nt file2 | 文件 | 检查 file1 是否比 file2 新 |
| file1 -ot file2 | 文件 | 检查 file1 是否比 file2 旧 |

清单 9.15 展示了在 shell 脚本中使用 if 条件语句的示例。

### 清单 9.15：使用 if 条件语句

```bash
$ cat test12.sh
#!/bin/bash
# testing the if condition
if [ $1 -eq $2 ]
then
  echo "Both values are equal!"
  exit
fi
if [ $1 -gt $2 ]
then
  echo "The first value is greater than the second"
  exit
fi
if [ $1 -lt $2 ]
then
  echo "The first value is less than the second"
  exit
fi
$
```

清单 9.15 中所示的 test12.sh 脚本会对命令行输入的两个参数值进行求值：

```bash
$ chmod u+x test12.sh
$ ./test12.sh 10 5
The first value is greater than the second
$
```

shell 脚本仅处理求值为 True 逻辑值的 if 语句中的命令。

> 你可以使用布尔 AND (&&) 和 OR (||) 符号组合测试。

### case 语句

你常常会发现自己想要评估变量的值，在一组可能的值中查找特定值，这与我们在清单 9.15 中演示的类似。与其编写多个 if 语句来测试所有可能的条件，不如使用 case 语句。

case 语句允许你以面向列表的格式检查单个变量的多个值：

```bash
case variable in
pattern1) commands1;
pattern2 | pattern3) commands2;;
*) default commands;;
esac
```

case 语句会将指定的变量与不同的模式进行比较。如果变量与模式匹配，shell 将执行为该模式指定的命令。你可以在一行上列出多个模式，使用竖线运算符分隔每个模式。星号符号是不匹配任何列出模式的值的兜底选项。清单 9.16 展示了使用 case 语句的示例。

### 清单 9.16：使用 case 语句

```bash
$ cat test13.sh
#!/bin/bash
# using the case statement
case $USER in
rich | barbara)
    echo "Welcome, $USER"
    echo "Please enjoy your visit";;
testing)
    echo "Special testing account";;
jessica)
    echo "Don't forget to log off when you're done";;
*)
    echo "Sorry, you're not allowed here";;
esac
$ chmod u+x test6.sh
$ ./test13.sh
Welcome, rich
Please enjoy your visit
$
```

case 语句为指定每个可能变量值的各种选项提供了一种更简洁的方法。

## 循环

编写脚本时，你常常会遇到需要多次重复相同命令的情况，例如对目录中的所有文件应用某个命令。Bash shell 提供了一些基本的循环命令来满足这一需求。

### for 循环

for 语句遍历一系列中的每个元素，例如目录中的文件或文本文档中的行。for 命令的格式如下：

```bash
for variable in series ; do
commands
done
```

变量会变成占位符，在每次迭代中取系列中每个元素的值。命令可以像使用脚本中定义的任何其他变量一样使用该变量。清单 9.17 展示了如何使用 for 循环遍历目录中的所有文件。

### 清单 9.17：使用 for 循环

```bash
$ cat test14.sh
#!/bin/bash
# iterate through the files in the Home folder
for file in $(ls | sort) ; do
  if [ -d $file ]
  then
    echo "$file is a directory"
  fi
  if [ -f $file ]
  then
    echo "$file is a file"
  fi
done
$
```

如果运行 test14.sh shell 脚本，你应该会看到主目录中文件和目录的列表：

```bash
$ ./test14.sh
Desktop is a directory
Documents is a directory
Downloads is a directory
Music is a directory
Pictures is a directory
Public is a directory
Templates is a directory
test1.sh is a file
test2.sh is a file
test3.sh is a file
test4.sh is a file
test5.sh is a file
test6.sh is a file
test7.sh is a file
today.txt is a file
Videos is a directory
$
```

这省去了在一大堆 if 或 case 语句中手动检查每个文件的大量编码工作。

> for 语句的常见用途是遍历一系列数字。你不必逐个列出所有数字，可以使用 seq 命令。seq 命令输出一系列数字。只需指定系列所需的起始值、结束值和间隔值即可。

### while 循环

另一个有用的循环语句是 while 命令。其格式如下：

```bash
while [ condition ]; do
commands
done
```

while 循环会一直循环，只要指定的条件求值为 True 逻辑值。当条件求值为 False 逻辑值时，循环停止。while 循环中使用的条件与 if-then 语句相同，因此你可以测试数字、字符串和文件。清单 9.18 演示了如何使用 while 循环计算一个数字的阶乘。

### 清单 9.18：计算一个数字的阶乘

```bash
$ cat test15.sh
#!/bin/bash
number=$1
factorial=1
while [ $number -gt 0 ]; do
    factorial=$[ $factorial * $number ]
    number=$[ $number - 1 ]
done
echo The factorial of $1 is $factorial
```




Shell 脚本会检索传递给脚本的第一个参数，并在 while 循环中使用它。只要 $number 变量中存储的值大于 0，while 循环就会继续循环。在每次循环迭代中，该值都会减少 1，因此在某个时刻 while 条件会变为 False。当发生这种情况时，$factorial 变量中包含最终计算结果。当你运行 test15.sh 程序时，你应该得到以下结果：

```
$ ./test15.sh 5
The factorial of 5 is 120
$ ./test15.sh 6
The factorial of 6 is 720
$
```

while 循环承担了遍历数字序列的所有繁重工作。现在你可以将任何数字作为命令行参数传入，并计算阶乘值！

while 命令的对立面是 until 命令。它会遍历命令块，直到测试条件计算结果为 True 逻辑值。

## 函数

随着你开始编写更复杂的 shell 脚本，你会发现自己会重复使用执行特定任务的代码片段。有时这很简单，比如显示一条文本消息并从脚本用户那里获取答案。有时则是一个在脚本中作为更大流程一部分多次使用的复杂计算。

在这些情况下，在脚本中一遍又一遍地编写相同的代码块会变得很令人厌烦。最好只编写一次代码块，然后就能在脚本中的任何位置引用该代码块，而无需重新编写。

Bash shell 提供了一项功能，允许你正好做到这一点。函数是脚本代码块，你可以为其赋予名称，然后在代码中的任何位置重复使用。每当你需要在脚本中使用该代码块时，只需使用为其指定的函数名（称为调用函数）即可。本节描述了如何在 shell 脚本中创建和使用函数。

在 Bash shell 脚本中创建函数有两种可用格式。第一种格式使用关键字 function，加上你为代码块指定的函数名：

```
function name {
commands
}
```

name 属性定义分配给函数的唯一名称。你在脚本中定义的每个函数都必须分配一个唯一名称。

commands 是构成函数的一个或多个 Bash shell 命令。当你调用函数时，Bash shell 会按函数中出现的顺序执行每个命令，就像在普通脚本中一样。

在 Bash shell 脚本中定义函数的第二种格式更接近其他编程语言中定义函数的方式：

```
name() {
commands
}
```

函数名后面的空括号表示你正在定义一个函数。此格式中的命名规则与原始 shell 脚本函数格式相同。要在脚本中使用函数，请在一行上指定函数名，就像使用任何其他 shell 命令一样。清单 9.19 演示了如何执行此操作。

## 清单 9.19: 在 shell 脚本中使用函数

```
$ cat test16.sh
#!/bin/bash
# using a function in a script

function func1 {
echo "This is an example of a function"
}

count=1
while [ $count -le 5 ]
do
	func1
	count=$[ $count + 1 ]
done

echo "This is the end of the loop"
func1
echo "Now this is the end of the script"
$ ./test16.sh
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is an example of a function
This is the end of the loop
This is an example of a function
Now this is the end of the script
$
```

在清单 9.19 中，每次在代码中引用 func1 函数名时，Bash shell 都会返回到 func1 函数定义，并执行其中定义的命令。

Bash shell 使用 return 命令以特定退出状态退出函数。return 命令允许你指定一个整数值来定义函数退出状态，从而为你以编程方式设置函数退出状态提供了一种简便方法：

```
$ cat test17.sh
#!/bin/bash
# using the return command in a function

function dbl {
    read -p "Enter a value: " value
    echo "doubling the value"
    return $[ $value * 2 ]
}

dbl
echo "The new value is $?"
$
```

dbl 函数将用户输入提供的 $value 变量中包含的值加倍。然后它使用 return 命令返回结果，脚本使用 $? 变量显示该结果。

就像你可以将命令的输出捕获到 shell 变量中一样，你也可以将函数的输出捕获到 shell 变量中。你可以使用此技术从函数中检索任何类型的输出并将其分配给变量：

```
result=$(dbl)
```

## 在后台模式下运行脚本

直接从命令行界面运行 shell 脚本有时很不方便。有些脚本处理时间很长，你可能不想在等待时占用命令行界面。在脚本运行期间，你无法在终端会话中做任何其他事情。幸运的是，该问题有一个简单的解决方案。以下各节描述了如何在 Linux 系统上以后台模式运行脚本。

## 在后台运行

以后台模式运行 shell 脚本是一件相当容易的事情。要从命令行界面以后台模式运行 shell 脚本，只需在命令后放置一个 & 符号：

```
$ ./test18.sh &
[1] 19555
$ This is test program
Loop #1
Loop #2
```

```
$ ls -l
total 8
-rwxr--r-- 1 rich rich 219 Feb 26 19:27 test18.sh
Loop #3
```

当你在命令后放置 & 符号时，它会将命令与 Bash shell 分离，并在系统上作为单独的后台进程运行。首先显示的是该行

```
[1] 19555
```

方括号中的数字是 shell 分配给后台进程的*作业编号*。shell 为每个启动的进程分配一个唯一的作业编号。下一个数字是 Linux 系统本身为进程分配的进程 ID (PID)。因此，shell 中运行的每个进程都有唯一的作业编号，而 Linux 系统上运行的每个进程都有唯一的 PID。

系统显示这些项目后，会出现一个新的命令行界面提示符。你会返回到 shell，并且你执行的命令会安全地以后台模式运行。

此时，你可以在提示符处输入新命令（如示例所示）。但是，在后台进程仍在运行时，它仍然会使用你的终端显示器来输出消息。你会从示例中注意到，test1.sh 脚本的输出会与 shell 中运行的任何其他命令的输出混合在一起出现。

后台进程完成时，它会在终端上显示一条消息：

```
[1]+ Done ./test18.sh
```

这显示了作业编号和作业状态 (Done)，以及启动作业所使用的命令。

## 运行多个后台作业

你可以从命令行提示符同时启动任意数量的后台作业：

```
$ ./test18.sh &
[1] 19582
$ This is test program
Loop #1
$ ./test18.sh &
[2] 19597
$ This is test program
Loop #1
$ ./test18.sh &
[3] 19612
$ This is test program
Loop #1
Loop #2
Loop #2
Loop #2
```

每次启动新作业时，shell 都会为其分配一个新作业编号，Linux 系统会为其分配一个新 PID。你可以通过使用 ps 命令查看所有脚本都在运行：

| 用户 | PID | %CPU | %MEM | VSZ | RSS | TTY | STAT | START | TIME | 命令 |
|------|-----|------|------|-----|-----|-----|------|-------|------|------|
| rich | 19498 | 0.0 | 1.2 | 2688 | 1628 | pts/0 | S | 11:38 | 0:00 | -bash |
| rich | 19582 | 0.0 | 0.9 | 2276 | 1180 | pts/0 | S | 11:55 | 0:00 | /bin/bash ./test18.sh |
| rich | 19597 | 0.1 | 0.9 | 2276 | 1180 | pts/0 | S | 11:55 | 0:00 | /bin/bash ./test18.sh |
| rich | 19612 | 0.1 | 0.9 | 2276 | 1180 | pts/0 | S | 11:55 | 0:00 | /bin/bash ./test18.sh |
| rich | 19639 | 0.0 | 0.4 | 1564 | 552 | pts/0 | S | 11:56 | 0:00 | sleep 10 |
| rich | 19640 | 0.0 | 0.4 | 1564 | 552 | pts/0 | S | 11:56 | 0:00 | sleep 10 |
| rich | 19641 | 0.0 | 0.4 | 1564 | 552 | pts/0 | S | 11:56 | 0:00 | sleep 10 |
| rich | 19642 | 0.0 | 0.5 | 2588 | 744 | pts/0 | R | 11:56 | 0:00 | ps au |

你启动的每个后台进程都会出现在 ps 命令输出的运行进程列表中。如果所有进程都在终端会话中显示输出，情况会很快变得非常混乱。幸运的是，有一个简单的方法可以解决这个问题，我们将在下一节中讨论。




从终端会话使用后台进程时需要小心。请注意 ps 命令输出中，每个后台进程都与终端会话（pts/0）终端绑定。如果终端会话退出，后台进程也会退出。有些终端模拟器在终端关联有运行中的后台进程时会发出警告，而有些则不会。如果你希望在从控制台注销后脚本仍能在后台模式下继续运行，那么还需要做一些其他事情。下一节将讨论该过程。

## 在没有控制台的情况下运行脚本

有时你希望从终端会话启动 shell 脚本，并让脚本在后台模式下运行直到完成，即使你退出终端会话。可以通过使用 nohup 命令来实现这一点。

nohup 命令用于运行另一个命令，并阻止发送给进程的任何 SIGHUP 信号。这样可防止进程在你退出终端会话时退出。

可以将 nohup 命令与 & 符号结合使用，以在后台运行脚本且不允许其被中断：

```
$ nohup ./test18.sh &
[1] 19831
$ nohup: appending output to 'nohup.out'
$
```

与普通后台进程一样，shell 会为命令分配作业编号，Linux 系统会分配 PID 编号。不同之处在于，当你使用 nohup 命令时，如果关闭会话，脚本会忽略终端会话发送的任何 SIGHUP 信号。

因为 nohup 命令将进程与终端解绑，进程失去了到显示器的输出链接。为了容纳命令生成的任何输出，nohup 命令会自动将输出消息重定向到一个名为 nohup.out 的文件中。

nohup.out 文件包含通常会发送到终端显示器的所有输出。进程运行结束后，你可以查看 nohup.out 文件以获取输出结果：

```
$ cat nohup.out
This is a test program
Loop #1
Loop #2
Loop #3
Loop #4
Loop #5
Loop #6
Loop #7
Loop #8
Loop #9
Loop #10
This is the end of the test program
$
```

输出会出现在 nohup.out 文件中，就像进程在命令行上运行一样！

如果使用 nohup 运行另一个命令，输出会追加到现有的 nohup.out 文件中。从同一目录运行多个命令时要小心，因为所有输出都会发送到同一个 nohup.out 文件中，这可能会令人困惑。

## 发送信号

Bash shell 可以向系统上运行的进程发送控制信号。这样，你在必要时就可以停止或中断失控的应用程序进程。你可以使用键盘上的按键组合生成两种基本的 Linux 信号，用于中断或停止前台进程。

## 中断进程

Ctrl+C 按键组合会生成 SIGINT 信号，并将其发送到 shell 中当前正在运行的任何进程。SIGINT 信号会中断正在运行的进程，这会导致大多数进程停止（可以编写脚本忽略中断信号，但这种情况不常见）。你可以运行一个通常需要很长时间才能完成的命令，然后按 Ctrl+C 按键组合来测试这一点：

```
$ sleep 100
$
```

Ctrl+C 按键组合在显示器上不会产生任何输出；它只是停止 shell 中当前正在运行的进程。

## 暂停进程

与终止进程不同，你可以暂停进程在执行任何操作时的中途。有时这可能是一件危险的事情（例如，如果脚本在关键系统文件上打开了文件锁），但通常这允许你查看脚本正在做什么，而无需实际终止进程。

Ctrl+Z 按键组合会生成 SIGTSTP 信号，停止 shell 中运行的任何进程。停止进程与终止进程不同，因为停止进程会使程序仍驻留在内存中，并能够从停止处继续运行。在下一节“作业控制”中，你将学习如何重新启动已停止的进程。当你使用 Ctrl+Z 按键组合时，shell 会告知你进程已停止：

```
$ sleep 100
[1]+ Stopped sleep 100
$
```

方括号中的数字表示进程在 shell 中的作业编号。如果 shell 会话中有已停止的作业，当你尝试退出 shell 时，Bash 会发出警告：

```
$ exit
logout
There are stopped jobs.
$
```

可以使用 ps 命令查看已停止的作业：

```
$ ps au
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
rich 20560 0.0 1.2 2688 1624 pts/0 S 05:15 0:00 -bash
rich 20605 0.2 0.4 1564 552 pts/0 T 05:22 0:00 sleep 100
rich 20606 0.0 0.5 2584 740 pts/0 R 05:22 0:00 ps au
$
```

ps 命令将已停止作业的状态显示为 T，这表示命令正在被跟踪或已停止。

如果你确实希望在已停止的作业仍处于活动状态时退出 shell，只需再次输入 exit 命令即可。shell 将退出，终止已停止的作业。或者，既然你知道了已停止作业的 PID，可以使用 kill 命令发送 SIGKILL 信号来终止它：

```
$ kill -9 20605
$
[1]+ Killed sleep 100
$
```

当你终止作业时，最初不会收到任何响应。但是，下次执行产生 shell 提示符的操作时，你会看到一条指示作业已被终止的消息。每次 shell 生成提示符时，它还会显示 shell 中任何状态发生变化的作业的状态。

## 作业控制

在上一节中，你学习了如何使用 Ctrl+Z 按键组合停止 shell 中运行的作业。停止作业后，Linux 系统允许你终止或重新启动它。

重新启动已停止的进程需要向其发送 SIGCONT 信号。

启动、停止、终止和恢复作业的功能称为 _作业控制_。通过作业控制，你可以完全控制进程在 shell 环境中的运行方式。

本节介绍了用于查看和控制 shell 中运行的作业的命令。

### 查看作业

作业控制的关键命令是 _jobs_ 命令。jobs 命令允许你查看 shell 当前正在处理的作业。清单 9.20 使用 shell 脚本来演示查看已停止的作业。

清单 9.20: 停止正在运行的作业

```
$ cat test19.sh
#!/bin/bash
# testing job control

echo "This is a test program $$"
count=1
while [ $count -le 10 ]; do
 echo "Loop #$count"
 sleep 10
 count=$[ $count + 1 ]
done
echo "This is the end of the test program"
$ ./test19.sh
This is a test program 29011
Loop #1

[1]+ Stopped ./test19.sh
$ ./test19.sh > test19.sh.out &
[2] 28861
$
$ jobs
[1]+ Stopped ./test19.sh
[2]- Running ./test19.sh > test19.sh.out &
$
```

清单 9.20 中所示的脚本使用 $$ 变量显示 Linux 系统分配给脚本的 PID；然后进入循环，每次迭代休眠 10 秒。在示例中，我们从命令行界面启动第一个脚本，然后使用 Ctrl+Z 按键组合将其停止。接下来，使用 & 符号将另一个作业作为后台进程启动。为了方便起见，我们将该脚本的输出重定向到一个文件，以便它不会出现在显示器上。

启动两个作业后，我们使用了 jobs 命令来查看分配给 shell 的作业。jobs 命令显示已停止和正在运行的作业，以及它们的作业编号和作业中使用的命令。

jobs 命令使用几个不同的命令行参数，如表 9.2 所示。

| 参数 | 描述 |
|---|---|
| -l | 列出作业编号和进程的 PID。 |
| -n | 仅列出自上次 shell 通知以来状态已更改的作业。 |
| -p | 仅列出作业的 PID。 |
| -r | 仅列出正在运行的作业。 |
| -s | 仅列出已停止的作业。 |

你可能注意到了清单 9.20 输出中的加号和减号。带加号的作业被视为默认作业。如果命令行中未指定作业编号，则任何作业控制命令引用的都是该作业。带减号的作业是在当前默认作业处理完成后将成为默认作业的作业。无论 shell 中运行多少个作业，任何时候只会有一个带加号的作业和一个带减号的作业。

清单 9.21 展示了当默认作业被移除时，队列中的下一个作业如何接管默认状态。

清单 9.21: 演示作业控制

```
$ ./test19.sh
This is a test program 29075
Loop #1

[1]+ Stopped ./test19.sh
$ ./test19.sh
```





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





## 定期安排脚本

使用at命令在预设时间安排脚本运行非常棒，但如果你需要该脚本每天在同一时间运行，或者每周运行一次，或者每月运行一次，该怎么办？与其不断提交at作业，不如使用Linux系统的另一个功能。

Linux系统使用cron程序来允许你安排需要定期运行的作业。cron程序在后台运行，并检查名为cron表（也简称为crontab）的特殊表格，以查找计划运行的作业。

### cron表

cron表使用一种特殊格式来允许你指定作业应在何时运行。cron表的格式为

```
min hour dayofmonth month dayofweek command
```

cron表允许你将条目指定为特定值、值范围（例如1–5）或通配符字符（星号）。例如，如果你想在每天上午10:15运行命令，可以使用cron表条目

```
15 10 * * * command
```

在dayofmonth、month和dayofweek字段中使用的通配符字符表示cron将在每天上午10:15执行该命令（每个月的每一天）。要指定在每周一下午4:15运行命令，可以使用

```
15 16 * * 1 command
```

你可以将dayofweek条目指定为三个字符的文本值（mon、tue、wed、thu、fri、sat、sun）或数值，其中0表示周日，6表示周六。另一个示例：要在每个月的第一天中午12点执行命令，可以使用格式

```
00 12 1 * * command
```

dayofmonth条目指定月份的日期值（1–31）。

在指定要运行的命令或shell时，必须使用其全路径名。你可以添加任意命令行参数或重定向符号，就像常规命令行一样：

```
15 10 * * * /home/rich/test21.sh > test21out
```

cron程序使用提交作业的用户帐户运行脚本。因此，你必须拥有访问命令列表中指定的命令和输出文件的适当权限。

### 构建cron表

所有系统用户都可以拥有自己的cron表（包括root用户）来运行计划作业。Linux提供crontab命令来处理cron表。要列出现有的cron表，请使用-l参数：

```
$ crontab -l
no crontab for rich
$
```

默认情况下，每个用户的cron表文件不存在。要向cron表添加条目，请使用-e参数。执行此操作时，crontab命令会自动启动vi编辑器打开现有的cron表，如果尚不存在则打开空文件。

## 练习 9.1

### 编写Bash脚本以查看系统用户的密码信息

本练习将介绍如何编写Bash脚本来查看Linux系统上配置的所有用户帐户的密码信息。

- 1. 登录到你的Linux图形桌面并打开命令提示符窗口。
- 2. 在命令提示符下，打开你选择的文本编辑器，并键入 nano pwinfo.sh、pico pwinfo.sh 或 vi pwinfo.sh 来创建文本文件 pwinfo.sh。
- 3. 将以下代码输入新文本文件：

```
#!/bin/bash
# pwinfo.sh - display password information for all users
list=$(cut -d : -f 1 /etc/passwd)
for user in $list ; do
    echo Password information for $user
    sudo chage -l $user
    echo "--------"
done
```

- 4. 使用编辑器的相应保存命令保存文件。
- 5. 键入 chmod u+x pwinfo.sh 为自己授予文件执行权限。
- 6. 键入 ./pwinfo.sh 运行shell脚本。
- 7. 在sudo提示符处输入你的密码。

你应该会看到系统上配置的所有用户帐户的chage密码信息。

## 摘要

Bash shell提供局部环境变量和全局环境变量，你可以在shell脚本中访问这些变量。全局环境变量允许你获取脚本运行所在shell环境的信息，例如启动shell的用户帐户以及该用户帐户的信息。局部环境变量允许你在脚本中存储和检索数据，使其像真实程序一样运行。

使用基本shell脚本，你可以将多个命令组合在一起作为单个命令运行。你可以使用输出重定向将命令的输出重定向到日后可读取的文件，或者使用管道将一个命令的输出重定向为另一个命令的输入数据。

当你将多个命令添加到文本文件以供运行时，必须以shebang (#!)行开头，该行标识要使用的Linux shell。你还需要使用chmod命令和u+x选项为自己授予运行文件的执行权限。你还可能需要在从命令提示符运行时指定文件的全路径，或者修改系统上的PATH环境变量，以便shell能找到你的shell脚本文件。

Bash shell提供其他功能，你可以将其添加到shell脚本文件中，使其更像真实程序。echo语句允许你在脚本命令输出之间插入文本输出，以帮助修改脚本生成的输出。

Bash shell还提供可在shell脚本中使用的高级编程功能。命令替换允许你将命令的输出捕获到变量中，以便在shell脚本中从命令输出中提取信息。Bash shell支持基本的整数数学运算，但不太擅长处理浮点数。为此，你需要bc计算器等其他程序的帮助。

Bash shell还支持一些标准编程功能，例如if和case逻辑语句，允许你测试数字、字符串和文件的特定条件，并根据这些条件的结果运行命令。它还支持for和while循环，允许你遍历数据组并在一组命令中处理每个元素。这些功能可以帮助你的Bash shell脚本像真实程序一样运行。

默认情况下，当你在终端会话shell中运行脚本时，交互shell会被挂起，直到脚本完成。你可以在命令名称后添加&符号，使脚本或命令以后台模式运行。当以后台模式运行脚本或命令时，交互shell会返回，允许你继续输入更多命令。使用此方法运行的任何后台进程仍与终端会话绑定。如果你退出终端会话，后台进程也会退出。

除了控制运行中的进程外，你还可以确定进程何时在系统上启动。与其直接从命令行界面提示符运行脚本，不如安排进程在备选时间运行。有几种不同的方法可以实现这一点。at命令允许你在预设时间运行一次脚本。cron程序提供了一个接口，可以按定期计划运行脚本。

## 考试要点

**解释环境变量以及Linux如何使用它们。** 环境变量在内存中存储shell会话的数据。在shell中设置的环境变量被视为局部变量，仅由局部shell访问。使用export命令允许环境变量被其他shell会话全局访问。系统管理员可以通过将它们放在/etc/profile文件中为所有用户设置标准环境变量，单个用户可以在其HOME目录中的.bash_profile、.profile或.bashrc文件中设置自己的环境变量。

**描述如何在shell脚本中链接多个命令行命令。** Bash shell允许我们将多个命令按顺序放入文件中，然后在从命令行运行文件时处理每个命令。每个命令的输出将出现在命令行输出中。

**解释如何在Bash shell脚本中处理数据。** Bash shell提供两种在命令中处理数据的方式。输出重定向允许你将命令的输出重定向到文本文件，以便你或另一个命令日后读取。管道允许你将一个命令的输出重定向为另一个命令的输入数据。运行shell脚本时，输出永远不会显示在监视器上；数据传输在后台进行。

**解释可以从shell脚本中访问的数据类型。** Bash shell提供对环境变量的访问，这些变量包含脚本运行所在shell环境的信息。你可以获取系统信息以及运行shell脚本的用户帐户信息。shell脚本还可以访问位置变量，这些变量允许你在运行shell脚本时从命令行向shell脚本传递数据。

**描述如何在在shell脚本中将命令输出用于另一个命令之前操作输出数据。** 命令替换允许你将命令的输出重定向到shell脚本中的用户变量。然后，你可以使用标准Linux文本处理命令来操作数据，例如对其进行排序或从中提取数据记录，然后再将变量数据重定向到另一个命令。

**描述Bash shell如何执行数学运算。** Bash shell使用$[ ]符号定义要处理的数学方程式。Bash shell只能执行整数数学，因此此功能有些有限。





解释在 Bash shell 脚本中实现逻辑的不同方法。Bash shell 支持 if 语句和 case 语句。它们都允许你对数值、字符串值或文件进行测试，然后根据测试结果运行一段命令。

描述如何从控制台或终端会话中以后台模式运行 shell 脚本。要以后台模式运行 shell 脚本，请在命令行中的 shell 脚本命令后加上 & 符号。shell 将以后台模式运行该脚本，并为你生成另一个命令提示符，以便你继续在 shell 中操作。

解释如何将 shell 脚本与控制台或终端会话断开连接，以便会话关闭后它仍能继续运行。nohup 命令将 shell 脚本与 shell 会话断开连接，并将其作为独立进程运行。如果控制台或终端会话退出，shell 脚本将继续运行。

解释如何停止或暂停在控制台或终端会话前台运行的 shell 脚本。要停止在控制台或终端会话前台运行的 shell 脚本，请按 Ctrl+C 组合键。要暂停正在运行的 shell 脚本，请按 Ctrl+Z 组合键。

描述如何列出控制台或终端会话中以后台模式运行的 shell 脚本。jobs 命令允许你列出控制台或终端会话中正在运行的命令。jobs 命令的输出同时显示 shell 分配的作业号和 Linux 系统分配的进程 ID。

描述如何在特定时间运行 shell 脚本。at 命令允许你安排作业在特定时间运行。你可以使用精确值（如 10:00PM）来指定时间，也可以使用常见的日期和时间引用（如 10:00AM 明天）。

解释如何每天在设定时间自动运行 shell 脚本。cron 进程每分钟运行一次并检查待运行的作业。你必须使用 crontab 命令在 cron 表中定义要运行的作业。

## 复习问题

答案请参阅附录。

- 1. Linux 中用于定义 shell 脚本所用 shell 的 shebang 由哪个字符或哪些字符组成？
  - A. >>
  - B. #!
  - C. |
  - D. >
  - E. 2>

- 2. 你使用哪个字符或哪些字符将命令的所有输出重定向到新文件？
  - A. >>
  - B. #!
  - C. |
  - D. >
  - E. 2>

- 3. 应将哪个 chmod 权限分配给文件以将其作为 shell 脚本运行？
  - A. 644
  - B. u+r
  - C. u+x
  - D. u+w
  - E. u=wr

- 4. 哪个环境变量包含启动 shell 的用户的用户名？
  - A. $USER
  - B. $UID
  - C. $HOME
  - D. $BASH
  - E. $1

- 5. 如何将数值 10 赋给变量 var1？
  - A. var1=$(10)
  - B. var1 = 10
  - C. var1=10
  - D. var1="10"
  - E. var1=`10`

- 6. 应使用哪个 if 条件测试来确定对象是否存在且是文件？
  - A. -e
  - B. -f
  - C. -d
  - D. -x
  - E. -w

- 7. 你使用哪个字符或字符组合将一个命令的输出重定向到另一个命令？
  - A. >>
  - B. #!
  - C. |
  - D. >
  - E. 2>

- 8. 你使用哪个命令来更改 shell 脚本返回的退出状态？
  - A. #!
  - B. $?
  - C. $1
  - D. exit
  - E. while

- 9. 应使用哪个命令来执行命令替换，以将命令的输出赋给 shell 脚本中的变量？
  - A. >
  - B. >>
  - C. $[]
  - D. |
  - E. $()

- 10. 应使用哪个命令来在 shell 脚本中执行数学运算？
  - A. >
  - B. >>
  - C. $[]
  - D. |
  - E. $()

- 11. 你使用哪个命令从控制台会话以后台模式启动 shell 脚本？
  - A. >
  - B. &
  - C. |
  - D. >>
  - E. nohup

- 12. 你使用哪个命令将 shell 脚本与当前控制台断开连接，以便控制台退出后它仍能继续运行？
  - A. >
  - B. &
  - C. |
  - D. >>
  - E. nohup

- 13. 如何让失控的 shell 脚本在控制台会话中停止运行？
  - A. 使用 nohup 命令启动它。
  - B. 使用 & 命令启动它。
  - C. 在脚本运行时按 Ctrl+C。
  - D. 使用管道符号重定向输出。
  - E. 使用 kill 命令停止它。

- 14. 如何暂时暂停控制台会话中前台运行的 shell 脚本？
  - A. 按 Ctrl+Z 组合键。
  - B. 按 Ctrl+C 组合键。
  - C. 使用 nohup 命令启动该命令。
  - D. 使用 & 命令启动该命令。
  - E. 使用 fg 命令启动该命令。

- 15. 如何确定控制台会话中默认运行的作业？
  - A. 通过 PID 编号
  - B. 通过作业编号
  - C. 通过 jobs 输出中作业编号旁的加号
  - D. 通过 jobs 输出中作业编号旁的减号
  - E. 使用 ps 命令

- 16. 你使用哪个命令将以后台模式运行的 shell 脚本取回并在控制台会话的前台运行？
  - A. bg
  - B. fg
  - C. nohup
  - D. &
  - E. at

- 17. 哪个命令允许你在特定时间运行 shell 脚本？
  - A. nohup
  - B. &
  - C. at
  - D. |
  - E. >

- 18. 哪个程序会在多个预设时间自动运行 shell 脚本？
  - A. at
  - B. nohup
  - C. &
  - D. cron
  - E. atq

- 19. cron 表条目 10 5 * * * myscript 何时会运行指定的 shell 脚本？
  - A. 每天上午 10:05
  - B. 每年 5 月 10 日
  - C. 每年 10 月 5 日
  - D. 每天下午 5:10
  - E. 每天上午 5:10

- 20. 你使用哪个命令来列出用户帐户的 cron 表条目？
  - A. cron
  - B. at
  - C. crontab
  - D. jobs
  - E. nohup
