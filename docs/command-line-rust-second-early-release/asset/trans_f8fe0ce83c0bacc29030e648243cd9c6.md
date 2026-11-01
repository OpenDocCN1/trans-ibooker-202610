
如果你的程序偏离需求或破坏了设计。

最后，我想分享一下，我探索了几种编写此算法的其他方法，其中一种是将输入文件的所有行读入一个向量，并使用 `Vec::windows` 查看行对。这很有趣，但在输入文件大小超过我机器可用内存时可能会失败。此处提供的解决方案只会为当前行和上一行分配内存，因此应可扩展到任何大小的文件。

## 更进一步

如往常一样，BSD 和 GNU 版本的 `uniq` 都具有比我在挑战中选择包含的功能更多的功能。我鼓励你在你的版本中添加所有你想要的功能。考虑阅读原始源代码和测试，看看可以借鉴什么。务必为每个功能添加测试，并始终运行整个测试套件以验证所有先前功能仍然正常工作。

在我看来，`uniq` 与 `sort` 紧密相关，因为我经常一起使用它们。考虑实现你自己的 `sort` 版本，至少能够按字典顺序（词典顺序）或数值对值进行排序。

## 总结

我希望你写这个挑战的体验和我一样有趣。让我们回顾一下你学到的一些内容：

- 你现在可以打开一个新文件进行写入或使用 `STDOUT`。
- DRY 的思想是将任何重复的代码移动到一个单一的抽象中，如函数或闭包。
- 你已经看到闭包可用于捕获封闭作用域中的值。
- 你了解了 `Write` trait 以及实现此 trait 的值如何与 `write!` 和 `writeln!` 宏一起使用。
- 有时你会发现需要写入一个临时文件。你现在使用了一个 Rust crate，它可以帮助找到唯一的文件名并为你打开文件句柄。
- Rust 编译器有时可能要求你指明变量的生命周期，即它相对于其他变量存在的时间。

1 名称 *closure* 指的是这种捕获或 *包围* 词法作用域绑定的行为。

## 第7章 寻宝者

## 早期发布读者的说明

在早期发布电子书中，你可以获得书籍的最早形式——作者在写作时的原始未编辑内容——因此你可以在这些标题正式发布之前很久就利用这些技术。

这将是最终书籍的第7章。请注意，GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何意见，或者在本章中发现缺失的材料，请联系作者 kyle.clark@gmail.com。

> *然后我也许应该把它写下来，但当我环顾四周找笔时，我又试图回想你说的话*
> 
> —They Might Be Giants

`find` 实用程序不出所料会查找事物。如果没有选项，它将递归搜索当前工作目录中的所有条目，包括文件、符号链接、套接字、目录等。你可以限制它搜索一个或多个路径，并包含限制以查找匹配各种限制的条目，例如名称、文件大小、文件类型、修改时间、权限等。你编写的挑战程序将定位一个或多个目录中名称匹配一个或多个正则表达式的文件、目录或链接。

你将学习：

- 如何使用 `clap` 来限制命令行参数的可能值
- 如何将正则表达式锚定到字符串的末尾
- 如何创建枚举类型（`enum`）
- 如何使用 `walkdir` crate 递归搜索文件路径
- 如何使用 `Iterator::any`
- 如何链接多个 `filter` 和 `map` 操作

## find 的工作原理

`find` 的手册页真是令人惊叹。它长达约500行，详细说明了查找文件和目录的所有选项。下面只是开头，展示了 BSD `find` 的大致风格：

```
FIND(1)                                 BSD General Commands Manual
FIND(1)

NAME
        find -- walk a file hierarchy

SYNOPSIS
        find [-H | -L | -P] [-EXdsx] [-f path] path ...
[expression]
        find [-H | -L | -P] [-EXdsx] -f path [path ...]
[expression]

DESCRIPTION
        The find utility recursively descends the directory
tree for each path
        listed, evaluating an expression (composed of the
''primaries'' and
        ''operands'' listed below) in terms of each file in
the tree.
```

## GNU find 类似：

```
$ find --help
Usage: find [-H] [-L] [-P] [-Olevel]
[-D help|tree|search|stat|rates|opt|exec] [path...]
[expression]
```

默认路径是当前目录；默认表达式是 -print
表达式可能由以下组成：运算符、选项、测试和操作：

运算符（优先级递减；-and 在没有其他运算符时隐含）：
    ( EXPR )  ! EXPR  -not EXPR  EXPR1 -a EXPR2
EXPR1 -and EXPR2
    EXPR1 -o EXPR2  EXPR1 -or EXPR2  EXPR1 ,  EXPR2

位置选项（始终为真）：-daystart -follow -regexptype

普通选项（始终为真，指定在其他表达式之前）：
    -depth --help -maxdepth LEVELS -mindepth LEVELS -mount -noleaf
    --version -xautofs -xdev -ignore_readir_race -noignore_readir_race

测试（N 可以是 +N 或 -N 或 N）：-amin N -anewer FILE -atime N -cmin N
    -cnewer FILE -ctime N -empty -false -fstype TYPE -gid N -group NAME
    -ilname PATTERN -iname PATTERN -inum N -iwholename PATTERN
    -iregex PATTERN -links N -lname PATTERN -mmin N -mtime N
    -name PATTERN -newer FILE -nouser -nogroup -path PATTERN
    -perm [-/]MODE -regex PATTERN -readable -writable -executable
    -wholename PATTERN -size N[bcwkMG] -true -type [bcdp flsD] -uid N
    -used N -user NAME -xtype [bcdp fls] -context CONTEXT

```
actions: -delete -print0 -printf FORMAT -fprintf FILE
FORMAT -print
    -fprint0 FILE -fprint FILE -ls -fls FILE -prune -quit
    -exec COMMAND ; -exec COMMAND {} + -ok COMMAND ;
    -execdir COMMAND ; -execdir COMMAND {} + -okdir
COMMAND ;
```

如往常一样，挑战程序只会尝试实现这些选项的一个子集，我将使用 07_findr/tests/inputs 中的文件立即演示。以下 tree 的输出显示了目录和文件结构。请注意 -> 表示 d/b.csv 是指向文件 a/b/b.csv 的链接。链接是指向另一个文件或目录的指针或快捷方式：

```
$ cd 07_findr/tests/inputs
$ tree
.
├── a
│   ├── a.txt
│   └── b
│       ├── b.csv
│       └── c
│           └── c.mp3
├── d
│   ├── b.csv -> ../a/b/b.csv
│   ├── d.tsv
│   ├── d.txt
│   └── e
│       └── e.mp3
├── f
│   └── f.txt
└── g.csv

6 directories, 9 files
```

## 注意

Windows 不像 Unix 那样具有符号链接（AKA symlink），因此有四个测试会失败，因为路径 tests\inputs\d\b.csv 存在为普通文件而不是链接。我建议 Windows 用户探索在 Windows Subsystem for Linux 中编写和测试此程序。

首先，find 会接受一个或多个位置参数作为起始路径。对于每个路径，find 将递归搜索其中找到的所有文件和目录。如果我在 tests/inputs 目录中并指示 . 表示当前工作目录，find 将列出所有内容。请注意，我将展示在 macOS 上运行 find 的输出，其条目顺序与 Linux 上显示的顺序不同：

```
$ find .
.
./g.csv
./a
./a/a.txt
./a/b
./a/b/b.csv
./a/b/c
./a/b/c/c.mp3
./f
./f/f.txt
./d
./d/b.csv
./d/d.txt
./d/d.tsv
./d/e
./d/e/e.mp3
```

使用 -type 选项¹，我可以指定“f”仅查找文件：

```
$ find . -type f
./g.csv
./a/a.txt
./a/b/b.csv
./a/b/c/c.mp3
./f/f.txt
./d/d.txt
./d/d.tsv
./d/e/e.mp3
```

我可以使用“l”仅查找 *链接*：

```
$ find . -type l
./d/b.csv
```

我也可以使用“d”仅查找 *目录*：

```
$ find . -type d
.
./a
./a/b
./a/b/c
./f
./d
./d/e
```

虽然挑战程序只会尝试查找这些类型，但根据手册页，`find` 会接受更多 `-type` 值：

```
-type t
    True if the file is of the specified type.  Possible
file types
    are as follows:

    b    block special
    c    character special
    d    directory
    f    regular file
    l    symbolic link
    p    FIFO
    s    socket
```

如果你给出此列表中找不到的 `-type` 值，`find` 将会报错：
