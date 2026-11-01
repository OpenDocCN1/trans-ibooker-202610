
要切换到当前工作目录的父目录，请使用 .. 文件夹名称：

```
C:\Users\AI>cd ..
C:\Users>
```

## 使用 dir 和 ls 列出文件夹内容

在 Windows 上，dir 命令显示当前工作目录中的文件夹和文件。ls 命令在 macOS 和 Linux 上执行相同的操作。您可以通过运行 dir [another folder] 或 ls [another folder] 来显示另一个文件夹的内容。

-l 和 -a 开关是 ls 命令的有用参数。默认情况下，ls 仅显示文件和文件夹的名称。要显示包含文件大小、权限、最后修改时间戳和其他信息的长列表格式，请使用 -l。根据惯例，macOS 和 Linux 操作系统将以句点开头的文件视为配置文件并将其隐藏在正常命令之外。您可以使用 -a 使 ls 显示所有文件，包括隐藏文件。要同时显示长列表格式和所有文件，请将开关组合为 ls -al。以下是在 macOS 或 Linux 终端窗口中的示例：

```
al@ubuntu:~$ ls
Desktop  Downloads  mu_code  Pictures  snap  Videos
Documents  examples.desktop  Music  Public  Templates
al@ubuntu:~$ ls -al
total 112
drwxr-xr-x 18 al  al  4096 Aug  4 18:47 .
drwxr-xr-x  3 root root 4096 Jun 17 18:11 ..
-rw-------  1 al  al  5157 Aug  2 20:43 .bash_history
-rw-r--r--  1 al  al   220 Jun 17 18:11 .bash_logout
-rw-r--r--  1 al  al  3771 Jun 17 18:11 .bashrc
drwx------ 17 al  al  4096 Jul 30 10:16 .cache
drwx------ 14 al  al  4096 Jun 19 15:04 .config
drwxr-xr-x  2 al  al  4096 Aug  4 17:33 Desktop
--snip--
```

ls -al 在 Windows 中的对应命令是 dir。以下是在 Windows 终端窗口中的示例：

```
C:\Users\AI>dir
    Volume in drive C is Windows
    Volume Serial Number is DFF3-8658

    Directory of C:\Users\AI

06/12/2019  05:18 PM    <DIR>          .
06/12/2019  05:18 PM    <DIR>          ..
12/04/2018  07:16 PM    <DIR>          .android
--snip--
08/31/2018  12:47 AM                 14,618 projectz.ipynb
10/29/2014  04:34 PM                121,474 foo.jpg
```

## 使用 dir /s 和 find 列出子文件夹内容

在 Windows 上，运行 dir /s 会显示当前工作目录的文件夹及其子文件夹。例如，以下命令会显示我的 C:\github\ezgmail 文件夹及其所有子文件夹中的每个 .py 文件：

```
C:\github\ezgmail>dir /s *.py
Volume in drive C is Windows
Volume Serial Number is DEE0-8982

Directory of C:\github\ezgmail

06/17/2019  06:58 AM            1,396 setup.py
               1 File(s)            1,396 bytes

Directory of C:\github\ezgmail\docs

12/07/2018  09:43 PM            5,504 conf.py
               1 File(s)            5,504 bytes

Directory of C:\github\ezgmail\src\ezgmail

06/23/2019  07:45 PM           23,565 __init__.py
12/07/2018  09:43 PM               56 __main__.py
               2 File(s)           23,621 bytes

    Total Files Listed:
               4 File(s)           30,521 bytes
               0 Dir(s)  505,407,283,200 bytes free
```

find . -name 命令在 macOS 和 Linux 上执行相同的操作：

```
al@ubuntu:~/Desktop$ find . -name "*.py"
./someSubFolder/eggs.py
./someSubFolder/bacon.py
./spam.py
```

. 告诉 find 从当前工作目录开始搜索。-name 选项告诉 find 按名称查找文件夹和文件名。"*.py" 告诉 find 显示名称与 *.py 模式匹配的文件夹和文件。请注意，find 命令要求 -name 之后的参数用双引号括起来。

## 使用 copy 和 cp 复制文件和文件夹

要在不同目录中创建文件或文件夹的副本，请运行 copy [source file or folder] [destination folder] 或 cp [source file or folder] [destination folder]。以下是在 Linux 终端窗口中的示例：

```
al@ubuntu:~/someFolder$ ls
hello.py  someSubFolder
al@ubuntu:~/someFolder$ cp hello.py someSubFolder
al@ubuntu:~/someFolder$ cd someSubFolder
al@ubuntu:~/someFolder/someSubFolder$ ls
hello.py
```

## 简短的命令名称

当我开始学习 Linux 操作系统时，我惊讶地发现我熟悉的 Windows copy 命令在 Linux 上名为 cp。“copy” 这个名称比“cp”可读性高得多。简短、晦涩的名称真的值得节省两个字符的输入吗？

随着我在命令行中获得更多经验，我意识到答案是肯定的“是”。我们阅读源代码的次数比编写源代码多，因此为变量和函数使用冗长的名称是有帮助的。但我们输入命令到命令行的次数比阅读它们多，所以在这种情况下，情况恰恰相反：简短的命令名称使命令行更易于使用，并减轻手腕的负担。

## 使用 move 和 mv 移动文件和文件夹

在 Windows 上，您可以通过运行 move [source file or folder] [destination folder] 将源文件或文件夹移动到目标文件夹。mv [source file or folder] [destination folder] 命令在 macOS 和 Linux 上执行相同的操作。

以下是在 Linux 终端窗口中的示例：

```
al@ubuntu:~/someFolder$ ls
hello.py  someSubFolder
al@ubuntu:~/someFolder$ mv hello.py someSubFolder
al@ubuntu:~/someFolder$ ls
someSubFolder
al@ubuntu:~/someFolder$ cd someSubFolder/
al@ubuntu:~/someFolder/someSubFolder$ ls
hello.py
```

hello.py 文件已从 ~/someFolder 移动到 ~/someFolder/someSubFolder，并且不再出现在其原始位置。

## 使用 ren 和 mv 重命名文件和文件夹

运行 ren [file or folder] [new name] 可在 Windows 上重命名文件或文件夹，而 mv [file or folder] [new name] 在 macOS 和 Linux 上执行相同的操作。请注意，您可以在 macOS 和 Linux 上使用 mv 命令移动和重命名文件。如果为第二个参数提供现有文件夹的名称，mv 命令会将文件或文件夹移动到该位置。如果提供一个与现有文件或文件夹不匹配的名称，mv 命令会重命名文件或文件夹。

以下是在 Linux 终端窗口中的示例：

```
al@ubuntu:~/someFolder$ ls
hello.py  someSubFolder
al@ubuntu:~/someFolders$ mv hello.py goodbye.py
al@ubuntu:~/someFolders$ ls
goodbye.py  someSubFolder
```

*hello.py* 文件现在名称为 *goodbye.py*。

## 使用 del 和 rm 删除文件和文件夹

要在 Windows 上删除文件或文件夹，请运行 `del [file or folder]`。要在 macOS 和 Linux 上执行此操作，请运行 `rm [file]`（`rm` 是 remove 的缩写）。

这两个删除命令有一些细微差别。在 Windows 上，对文件夹运行 `del` 会删除其所有文件，但不会删除其子文件夹。`del` 命令也不会删除源文件夹；您必须使用 `rd` 或 `rmdir` 命令来执行此操作，我将在第 34 页的“使用 rd 和 rmdir 删除文件夹”中解释。此外，运行 `del [folder]` 不会删除源文件夹的子文件夹中的任何文件。您可以通过运行 `del /s /q [folder]` 来删除文件。`/s` 在子文件夹上运行 `del` 命令，而 `/q` 基本上意味着“保持安静，不要要求我确认”。图 2-4 说明了这种差异。

del delicious   del /s /q delicious

![](img/ab52fe24e511e29be73786864ebe0410_59_0.png)

图 2-4：运行 del delicious（左）或 del /s /q delicious（右）时，这些示例文件夹中的文件将被删除。

在 macOS 和 Linux 上，您不能使用 `rm` 命令删除文件夹。但您可以运行 `rm -r [folder]` 来删除文件夹及其所有内容。在 Windows 上，`rd /s /q [folder]` 会执行相同的操作。图 2-5 说明了此任务。

```
rd /s /q delicious
rm -r delicious
```

![](img/ab52fe24e511e29be73786864ebe0410_60_0.png)

图 2-5：运行 rd /s /q delicious 或 rm -r delicious 时，这些示例文件夹中的文件将被删除。

## 使用 md 和 mkdir 创建文件夹

运行 md [new folder] 可在 Windows 上创建新的空文件夹，而运行 mkdir [new folder] 可在 macOS 和 Linux 上执行相同的操作。mkdir 命令在 Windows 上也有效，但 md 更容易输入。

以下是在 Linux 终端窗口中的示例：

```
al@ubuntu:~/Desktop$ mkdir yourScripts
al@ubuntu:~/Desktop$ cd yourScripts
al@ubuntu:~/Desktop/yourScripts$ ls
al@ubuntu:~/Desktop/yourScripts$
```

请注意，新创建的 yourScripts 文件夹是空的；当我们运行 ls 命令列出文件夹内容时，什么也没有出现 ①。

## 使用 rd 和 rmdir 删除文件夹

运行 rd [source folder] 可在 Windows 上删除源文件夹，而 rmdir [source folder] 可在 macOS 和 Linux 上删除源文件夹。与 mkdir 类似，rmdir 命令在 Windows 上也有效，但 rd 更容易输入。在删除之前，文件夹必须为空。

以下是在 Linux 终端窗口中的示例：

```
al@ubuntu:~/Desktop$ mkdir yourScripts
al@ubuntu:~/Desktop$ ls
yourScripts
```
