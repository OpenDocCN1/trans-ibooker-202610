

## 8 文件操作

> *走自己的路，让别人说去吧。*
> 
> ——但丁·阿利吉耶里

文件提供了信息的持久化存储。如果你希望数据的可用时间比应用运行所需的时间更长，甚至长达数年，那么你应该将这些数据存储在文件中。物理存储设备可能是当今手机、笔记本电脑和服务器中的硬盘驱动器（HDD）、固态驱动器（SSD）或闪存驱动器。

本书不涉及物理设备的工作原理，也不涉及对硬件的底层软件访问。相反，在本章中，我们探讨使用文件存储和检索信息的抽象概念。我们将文件分组到*目录*或*文件夹*中，这些目录或文件夹可以嵌套。文件和文件夹共同构成了*文件系统*。操作系统管理文件系统，这是操作系统最重要的角色之一。

### 本章涵盖的主题

- 8.1 路径与文件系统 ...... 306
- 8.2 在文件系统中移动 ...... 310
- 8.3 创建和删除目录 ...... 312
- 8.4 文件和文件夹的列表 ...... 313
- 8.5 名称与位置 ...... 316
- 8.6 文件类型 ...... 317

### 8.1 路径与文件系统

描述如何从文件系统顶部到达我们所需确切文件的文本字符串称为*路径名*。你可以将其视为导航到所需信息的一组方向。

图 8.1 中的树显示了我在编写本书时使用的文件夹和文件的子集。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_334_0.png)

图 8.1：包含文件夹和文件的目录树

在 Linux 和 macOS 上，文件 **Working-with-Files.html** 的完整路径的路径名为

```text
/src/part-I/Working-with-Files.html
```

在 Microsoft Windows 上，我们会有一个类似 “C:” 的驱动器盘符，并且使用反斜杠而不是斜杠。路径名的大小写在 Windows 上不敏感，但在其他操作系统上是敏感的。

```text
C:\src\part-I\Working-with-Files.html
```

我将使用斜杠，而大多数处理路径名的 Python 例程在 Windows 上也能处理这两种分隔符。你可能会发现，如 4.1 节所述，对 Windows 路径名使用原始字符串会很方便。原始字符串减少了需要转义的字符数量。

```
"C:\\src\\part-I\\Working-with-Files.html"
'C:\\src\\part-I\\Working-with-Files.html'
r"C:\src\part-I\Working-with-Files.html"
'C:\\src\\part-I\\Working-with-Files.html'
```

“/”目录是根目录。

我们使用 os 包和 os.path 模块来操作路径名。“os”代表“operating system”（操作系统）。

```
import os

pn = "/src/part-I/Working-with-Files.html"
```

```
os.path.split(pn)

('/src/part-I', 'Working-with-Files.html')
```

元组中的第二项是基本名称。

```
os.path.basename(pn)

'Working-with-Files.html'
```

用 splitext 拆分文件路径或基本名称来获取扩展名。

```
os.path.splitext(pn)

('/src/part-I/Working-with-Files', '.html')
```

```
os.path.splitext(os.path.basename(pn))

('Working-with-Files', '.html')
```

在 Windows 上，使用 splitdrive 获取驱动器盘符和后面的冒号。

```
os.path.splitdrive("C:\\src\\part-I\\Working-with-Files.html")

('C:', '\\src\\part-I\\Working-with-Files.html')
```

操作系统会跟踪大量关于文件的数据，os.path 提供了获取这些数据的方法。图 8.2 显示了 Windows 10 在开发过程中的某个时刻为本章的源文件提供的文件信息：

![](img/4ffeafa3e3f1373f02971e43b0755ea6_336_0.png)

图 8.2：Windows 资源管理器文件信息

getctime、getmtime 和 getatime 函数分别返回文件的创建时间、最后修改时间和最后访问时间。

```
mod_time = os.path.getmtime("src/part-I/Working-with-Files.html")
mod_time
```

1628111739.0

这个结果是某人最后一次修改文件的时间，它是自纪元以来的秒数。

纪元是一个特殊的计算机系统概念，对许多操作系统来说，第 0 秒是 1970 年 1 月 1 日午夜。如果使用 gmtime 和 asctime，我们可以更好地理解 mod_time。

```
import time

time.gmtime(mod_time)

time.struct_time(tm_year=2021, tm_mon=8, tm_mday=4, tm_hour=21,
tm_min=15, tm_sec=39, tm_wday=2, tm_yday=216, tm_isdst=0)
```

```
time.asctime(time.gmtime(mod_time))

'Wed Aug  4 21:15:39 2021'
```

文件创建、修改和访问时间，以及说明谁可以对该文件执行哪些操作的权限数据，共同构成文件元数据的一部分。

要获取文件中的字节数，请使用 getsize。

```
f'{os.path.getsize("src/part-I/Working-with-Files.html"):,} bytes'

'53,449 bytes'
```

我使用了 f-string 来格式化输出，使其更美观。

如果你尝试访问不存在的文件或路径，大多数 os.path 函数会引发 OSError。

```
os.path.getsize("src/part-I/Working-with-Flies.html")

FileNotFoundError: The system cannot find the file specified:
'src/part-I/Working-with-Flies.html'
```

使用 exists 来测试文件或目录是否存在。

```
os.path.exists("src/part-I/Working-with-Files.html")

True
```

```
os.path.exists("src/part-I")

True
```

isfile 测试文件是否存在并且不是目录。

```
os.path.isfile("src/part-I/Working-with-Files.html")

True
```

```
os.path.isfile("src/part-I/Working-with-Flies.html")

False
```

```
os.path.isfile("src/part-I")

False
```

要查看路径是否指向目录或文件夹，请使用 isdir。

```
os.path.isdir("src/part-I/Working-with-Files.html")

False
```

```
os.path.isdir("src/part-I")

True
```

### 练习 8.1

编写一个函数，该函数接受两个文件路径名，验证它们指向存在的文件，并打印最近修改的文件的名称。

### 8.2 在文件系统中移动

如果你在 Windows、macOS 或 Linux 中使用过命令行，你就会了解“当前工作目录”的概念，以及如何移动到另一个目录。Python 也有同样的概念和功能。

图 8.3 是构成这本书的文件目录树的另一个部分视图。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_338_0.png)

图 8.3：显示当前目录的目录树

如果我位于 src 目录中，我可以在操作系统命令行上发出 pwd 命令，看到类似如下的内容：

```text
Path
----
C:\src
```

在 Windows 上是这样，在 macOS 或 Linux 上则是 /src。“pwd” 是 “print working directory” 的缩写。对应的 Python 函数是 getcwd，即 “get the current working directory” 的缩写，

```
import os

os.getcwd()

'C:\\src'
```

我在命令行上发出 cd code 命令以切换到 code 子目录。在 Python 中，对应的函数是 chdir。

```
os.chdir("code")
os.getcwd()

'C:\\src\\code'
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_339_0.png)

图 8.4：显示新的当前目录的目录树

当前目录的简写是 “.”。使用 “..” 可以移动到当前目录之上的目录，即父目录。将其与其他目录名组合，可以“向上、跨越、向下”。这里，我们使用了一个相对目录路径。

```
os.chdir("../../final/images")
os.getcwd()

'C:\\final\\images'
```

使用绝对路径可以转到完整指定的目录。

```
os.chdir("/src/frontmatter")
os.getcwd()

'C:\\src\\frontmatter'
```

### 8.4 文件和文件夹的列表

如果我有五个文件名

guitar_01.py, guitar_02.py, unipoly_01.py, unipoly_02.py, and unipoly_10.py,

那么我可以使用*通配符*来匹配其中一些名称。下面是一些示例：

- 模式 "*.py" 匹配所有这些文件，因为该模式表示“名称可以以任何字符开头，但必须以 '.py' 结尾”。
- 模式 "*01*" 匹配 **guitar_01.py** 和 **unipoly_01.py**。
- "unipoly_0?.py" 匹配 **unipoly_01.py** 和 **unipoly_02.py**。“?” 是单字符通配符，匹配该位置上的任意字符。

使用这样的模式来匹配文件名称为 “globbing”（文件名通配匹配），其历史可以追溯到 20 世纪 60 年代末和 70 年代初 UNIX 的早期。Python 中的 **glob** 模块实现了 globbing。

该模块的两个最重要的函数是 *glob*（返回匹配列表）和 *iglob*（生成一个迭代器，按需提供匹配项）。除非你需要列表，否则我建议你在循环中使用 *iglob*。

下面是我列出本书第一部分所有源 HTML 文件的方法：

```
import glob

for html_file in glob.iglob("src/part-I/*.html"):
    html_file = os.path.normcase(html_file)
    print(html_file)
```

```text
src\part-i\collecting-things-together.html
src\part-i\computing-and-calculating.html
src\part-i\defining-and-using-functions.html
src\part-i\organizing-objects-into-classes.html
```

