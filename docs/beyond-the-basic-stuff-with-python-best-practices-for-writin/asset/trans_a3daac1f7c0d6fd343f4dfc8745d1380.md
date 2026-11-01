
```
al@ubuntu:~/Desktop$ rmdir yourScripts
al@ubuntu:~/Desktop$ ls
al@ubuntu:~/Desktop$
```

在此示例中，我们创建了一个名为 *yourScripts* 的空文件夹然后将其删除。

要删除非空文件夹（连同其中包含的所有文件夹和文件），在 Windows 上运行 `rd /s/q [source folder]`，或在 macOS 和 Linux 上运行 `rm -rf [source folder]`。

## 使用 where 和 which 查找程序

在 Windows 上运行 `where [program]` 或在 macOS 和 Linux 上运行 `which [program]` 会告诉你程序的确切位置。当你在命令行输入命令时，你的计算机会在 PATH 环境变量列出的文件夹中查找程序（尽管 Windows 会先检查 cwd）。

这些命令可以告诉你输入 python 时 shell 运行的是哪个可执行 Python 程序。如果你安装了多个 Python 版本，你的计算机可能有多个同名可执行程序。实际运行的是哪个取决于 PATH 环境变量中文件夹的顺序，而 where 和 which 命令会输出它：

```
C:\Users\Al>where python
C:\Users\Al\AppData\Local\Programs\Python\Python38\python.exe
```

在此示例中，文件夹名称表明从 shell 运行的 Python 版本位于 *C:\Users\Al\AppData\Local\Programs\Python\Python38\*。

## 使用 cls 和 clear 清除终端

在 Windows 上运行 `cls` 或在 macOS 和 Linux 上运行 `clear` 将清除终端窗口中的所有文本。如果只是想以一个看起来全新的终端窗口开始，这很有用。

## 环境变量和 PATH

无论程序是用何种语言编写，所有正在运行的程序进程都有一组称为 *环境变量* 的变量，可以存储字符串。环境变量通常保存着每个程序都会觉得有用的系统范围设置。例如，TEMP 环境变量保存着任何程序都可以存储临时文件的路径。当操作系统运行一个程序（如命令行）时，新创建的进程会收到操作系统环境变量和值的自己的副本。你可以独立于操作系统的环境变量集更改进程的环境变量。但这些更改仅适用于该进程，不适用于操作系统或任何其他进程。

我在本章讨论环境变量，因为其中一个变量 PATH 可以帮助你从命令行运行程序。

## 查看环境变量

你可以通过从命令行运行 set（在 Windows 上）或 env（在 macOS 和 Linux 上）来查看终端窗口的环境变量列表：

```
C:\Users\Al>set
ALLUSERSPROFILE=C:\ProgramData
APPDATA=C:\Users\Al\AppData\Roaming
CommonProgramFiles=C:\Program Files\Common Files
--snip--
USERPROFILE=C:\Users\Al
VBOX_MSI_INSTALL_PATH=C:\Program Files\Oracle\VirtualBox\
WINDIR=C:\WINDOWS
```

等号 (=) 左侧的文本是环境变量名，右侧的文本是字符串值。每个进程都有自己的环境变量集，因此不同的命令行可以为其环境变量具有不同的值。

你也可以使用 echo 命令查看单个环境变量的值。在 Windows 上运行 echo %HOMEPATH% 或在 macOS 和 Linux 上运行 echo $HOME 分别查看 HOMEPATH 或 HOME 环境变量的值，其中包含当前用户的主文件夹。在 Windows 上，它看起来像这样：

```
C:\Users\Al>echo %HOMEPATH%
\Users\Al
```

在 macOS 或 Linux 上，它看起来像这样：

```
al@al-VirtualBox:~$ echo $HOME
/home/al
```

如果该进程创建另一个进程（如命令行运行 Python 解释器时），子进程会收到父进程环境变量的自己的副本。子进程可以更改其环境变量的值而不影响父进程的环境变量，反之亦然。

你可以将操作系统的环境变量集视为“主副本”，进程从中复制其环境变量。操作系统的环境变量比 Python 程序的环境变量更改频率更低。事实上，大多数用户从未直接触及他们的环境变量设置。

## 使用 PATH 环境变量

当你输入命令，如 Windows 上的 python 或 macOS 和 Linux 上的 python3 时，终端会在你当前所在的文件夹中查找具有该名称的程序。如果在那里找不到，它将检查 PATH 环境变量列出的文件夹。

例如，在我的 Windows 计算机上，python.exe 程序文件位于 C:\Users\Al\AppData\Local\Programs\Python\Python38 文件夹中。要运行它，我必须输入 C:\Users\Al\AppData\Local\Programs\Python\Python38\python.exe，或先切换到该文件夹然后输入 python.exe。

这个冗长的路径名需要大量输入，因此我改为将此文件夹添加到 PATH 环境变量中。然后，当我输入 python.exe 时，命令行会在 PATH 列出的文件夹中搜索具有此名称的程序，从而免去我输入整个文件路径的麻烦。

因为环境变量只能包含单个字符串值，所以向 PATH 环境变量添加多个文件夹名称需要使用特殊格式。在 Windows 上，分号分隔文件夹名称。你可以使用 path 命令查看当前的 PATH 值：

```
C:\Users\Al>path
C:\Path;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;
--snip--
C:\Users\Al\AppData\Local\Microsoft\WindowsApps
```

在 macOS 和 Linux 上，冒号分隔文件夹名称：

```
al@ubuntu:~$ echo $PATH
/home/al/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

文件夹名称的顺序很重要。如果我在 C:\WINDOWS\system32 和 C:\WINDOWS 中有两个名为 someProgram.exe 的文件，输入 someProgram.exe 将运行 C:\WINDOWS\system32 中的程序，因为该文件夹在 PATH 环境变量中排在前面。

如果你输入的程序或命令不存在于 cwd 或 PATH 列出的任何目录中，命令行会给出错误，例如 command not found 或 not recognized as an internal or external command。如果没有打错字，请检查程序位于哪个文件夹，并查看它是否出现在 PATH 环境变量中。

## 更改命令行的 PATH 环境变量

你可以更改当前终端窗口的 PATH 环境变量以包含额外文件夹。在 Windows 和 macOS/Linux 之间添加文件夹到 PATH 的过程略有不同。在 Windows 上，你可以运行 path 命令将新文件夹添加到当前的 PATH 值：

```
1 C:\Users\Al>path C:\newFolder;%PATH%
```

```
2 C:\Users\Al>path
C:\newFolder;C:\Path;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;
--snip--
C:\Users\Al\AppData\Local\Microsoft\WindowsApps
```

%PATH% 部分 ① 会扩展为 PATH 环境变量的当前值，因此你是在现有 PATH 值的开头添加新文件夹和分号。你可以再次运行 path 命令来查看 PATH 的新值 ②。

在 macOS 和 Linux 上，你可以使用类似于 Python 中赋值语句的语法设置 PATH 环境变量：

```
① al@al-VirtualBox:~$ PATH=/newFolder:$PATH
② al@al-VirtualBox:~$ echo $PATH
/newFolder:/home/al/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

$PATH 部分 ① 会扩展为 PATH 环境变量的当前值，因此你是在现有 PATH 值中添加新文件夹和冒号。你可以再次运行 echo $PATH 命令来查看 PATH 的新值 ②。

但前面两种向 PATH 添加文件夹的方法仅适用于当前终端窗口以及在添加后从其中运行的任何程序。如果你打开一个新的终端窗口，它将不会有你的更改。永久添加文件夹需要更改操作系统的环境变量集。

## 在 Windows 上永久添加文件夹到 PATH

Windows 有两组环境变量：系统环境变量（适用于所有用户）和用户环境变量（覆盖系统环境变量但仅适用于当前用户）。要编辑它们，请单击开始菜单，然后输入“编辑此账户的环境变量”，这将打开环境变量窗口，如图 2-6 所示。

从用户变量列表（不是系统变量列表）中选择 Path，单击编辑，在出现的文本字段中添加新文件夹名称（不要忘记分号分隔符），然后单击确定。

这个界面不是最容易使用的，因此如果你经常在 Windows 上编辑环境变量，我建议安装免费的 Rapid Environment Editor 软件，网址为 https://www.rapidee.com/。请注意，安装后，你必须以管理员身份运行此软件才能编辑系统环境变量。单击开始菜单，输入 Rapid Environment Editor，右键单击软件图标，然后单击“以管理员身份运行”。

从命令提示符，你可以使用 setx 命令永久修改系统 PATH 变量：

```
C:\Users\Al>setx /M PATH "C:\newFolder;%PATH%"
```

你需要以管理员身份运行命令提示符才能运行 setx 命令。
