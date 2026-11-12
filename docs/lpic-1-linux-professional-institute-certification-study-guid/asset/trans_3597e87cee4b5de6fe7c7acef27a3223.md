
在清单 4.25 中，首先使用超级用户权限将 /var/log/wtmp 文件复制到本地目录。然后制作此文件的四个副本。使用 ls -lh 命令，您可以以人类可读的格式看到 wtmp 文件的大小为 210K。接下来，使用各种压缩实用程序。注意，使用 zip 命令时，必须指定存档文件的名称 wtmp4.zip，然后跟上任何文件名。在此例中，仅将 wtmp4 放入 zip 存档中。

在使用各种实用程序压缩文件后，清单 4.25 中又发出了一个 ls -lh 命令。注意各种文件扩展名以及文件的压缩后大小。您可以看到 xz 程序对此文件产生的压缩率最高，因为其文件大小最小。清单中的最后一个命令显示，除 zip 外，所有压缩程序都删除了原始文件。

> 对于前面的数据压缩实用程序，您可以通过 -# 选项指定压缩级别并控制速度。# 是 1 到 9 之间的数字，其中 1 最快但提供的压缩率最低。9 最慢但压缩率最高。zip 实用程序目前尚不支持这些压缩级别，但支持解压缩。通常，实用程序使用 -6 作为默认压缩级别。建议查看每个实用程序的 man 页面中这些级别规范——它们存在有用但细微的差别。

如果需要查看压缩文件的内容，可以在不将文件切换回未压缩格式的情况下完成此任务。cat 命令的三个变体可以帮助完成此任务：bzcat、xzcat 和 zcat。这些实用程序会临时解压缩文件，并将其内容显示到 STDOUT。清单 4.26 展示了创建 xz 压缩文件并使用 xzcat 显示其内容的示例。

### 清单 4.26: 使用 xzcat 命令查看 xz 压缩文件的内容

```
$ xz alphabet.txt
$
$ ls alphabet*
alphabet.txt.xz
$
$ xzcat alphabet.txt.xz
Alpha
Tango
Bravo
Echo
Foxtrot
$
$ ls alphabet*
alphabet.txt.xz
$
```

请记住，显示 xz 压缩文件需要 xzcat。您不能使用它来显示 bzip2 压缩文件。为了帮助理清思路，以及了解模仿这些各种压缩文件显示命令的其他命令，请参见表 4.5。

### 表 4.5 压缩文件显示命令

| 命令 | 等效命令 | 说明 |
| --- | --- | --- |
| bzcat | bzip2 -dc | 用于显示 bzip2 压缩文件。 |
| xzcat | xz --decompress --stdout | 显示 xz 压缩文件的内容。 |
| zcat | gunzip -c | 用于显示 gzip 压缩文件。某些类 Unix 系统改用 gzcat 命令。 |

请注意，zcat 实用程序显示 gzip 压缩文件，而不是 zip 文件。这可能会令人困惑。

> 如果您有一个没有正确文件扩展名的数据文件，请记住可以使用 file 命令来查明其类型。例如，您可以键入 file filename 来确定它是文本文件、存档文件还是其他类型。如果它是压缩数据文件，您甚至会了解用于创建它的压缩方法。这很有用！

## 存档文件命令

有多种程序可用于管理备份。一些更流行的产品包括 Amanda、Bacula、Bareos、Duplicity 和 BackupPC。然而，这些 GUI 和/或 Web 程序的核心通常是命令行实用程序。我们在此关注其中的一些命令行实用程序：

- cpio
- dd
- tar

### 使用 cpio 进行复制

cpio 实用程序的名称代表 "copy in and out"。它将文件副本收集在一起并将其存储在存档文件中。该程序有几个很好的选项。更常用的选项在表 4.6 中有描述。

### 表 4.6 cpio 命令的常用选项

| 短选项 | 长选项 | 说明 |
| --- | --- | --- |
| -I | N/A | 指定要使用的存档文件。 |
| -i | --extract | 从存档中复制文件或显示存档中的文件，具体取决于所使用的其他选项。称为复制-入模式。 |
| N/A | --no-absolute-filenames | 指定仅使用相对路径名。（默认值为使用绝对路径名。） |
| -o | --create | 通过将文件复制到其中来创建存档。称为复制-出模式。 |
| -t | --list | 显示存档中的文件列表。此列表称为目录。 |
| -v | --verbose | 在处理每个文件时显示该文件的名称。 |

要使用 cpio 实用程序创建存档，必须先生成文件列表，然后将其管道传输到命令中。清单 4.27 显示了执行此任务的示例。

### 清单 4.27: 使用 cpio 创建存档

```
$ ls Project4?.txt
Project42.txt Project43.txt Project44.txt
Project45.txt Project46.txt
$
$ ls Project4?.txt | cpio -ov > Project4x.cpio
Project42.txt
Project43.txt
Project44.txt
Project45.txt
Project46.txt
59 blocks
$
$ ls Project4?.*
Project42.txt Project44.txt Project46.txt
Project43.txt Project45.txt Project4x.cpio
$
```

在清单 4.27 中，首先使用 ? 通配符和 ls 命令显示当前工作目录中的各种文本文件。然后使用该命令，并将其 STDOUT 作为 STDIN 管道传输到 cpio 实用程序。（如果您需要回顾 STDOUT 和 STDIN，请参见第 1 章。）cpio 命令使用的选项是 -ov，它会创建包含所列文件副本的存档，并在复制文件时显示文件名。使用的存档文件名为 Project4x.cpio。虽然不是必须的，但在 cpio 存档文件上使用 .cpio 扩展名被视为良好做法。

您可以通过 cpio 实用程序基于元数据（而非文件位置）备份数据。例如，假设您想要为 JKirk 用户帐户拥有的虚拟目录系统中的任何文件创建 cpio 档案。您可以使用 find / -user JKirk 命令（本章后面将介绍）并将其管道传输到 cpio 实用程序以创建存档文件。这是一个有用的功能。

您可以轻松查看 cpio 存档中存储的文件。只需再次使用 cpio 命令，并使用其 -itv 选项和 -I 选项来指定存档文件，如清单 4.28 所示。

### 清单 4.28: 使用 cpio 列出存档内容

```
$ cpio -itvI Project4x.cpio
-rw-r--r-- 1 Christina Christina 29900 Aug 19 17:37 Project42.txt
-rw-rw-r-- 1 Christina Christina 0 Aug 19 18:07 Project43.txt
-rw-rw-r-- 1 Christina Christina 0 Aug 19 18:07 Project44.txt
-rw-rw-r-- 1 Christina Christina 0 Aug 19 18:07 Project45.txt
-rw-rw-r-- 1 Christina Christina 0 Aug 19 18:07 Project46.txt
59 blocks
$
```

虽然清单 4.28 中未显示，但 cpio 实用程序会保留每个文件的绝对目录引用。因此，它常用于创建系统映像和完整备份。

要从存档中恢复文件，只需使用 -iv 选项。但是，由于 cpio 保留文件的绝对路径，如果您需要将文件恢复到另一个目录位置，这可能会很困难。为此，您需要使用 --no-absolute-filenames 选项，如清单 4.29 所示。

### 清单 4.29: 使用 cpio 将文件恢复到不同目录位置

```
$ ls -dF Projects
Projects/
$
$ mv Project4x.cpio Projects/
$
$ cd Projects
$ pwd
/home/Christine/Answers/Projects
$
$ ls Project4?.*
Project4x.cpio
$
$ cpio -iv --no-absolute-filenames -I Project4x.cpio
Project42.txt
Project43.txt
Project44.txt
Project45.txt
Project46.txt
59 blocks
$
$ ls Project4?.*
Project42.txt Project44.txt Project46.txt
Project43.txt Project45.txt Project4x.cpio
$
```

在清单 4.29 中，Project4.cpio 存档文件被移动到预先存在的子目录 Projects 中。通过使用 --no-absolute-filenames 选项从存档文件中剥离绝对路径名，您可以将文件恢复到新目录位置。如果您想将文件恢复到其原始位置，请保留该选项不使用，而只需使用清单 4.29 中显示的其他 cpio 开关即可。

## 使用 tar 进行存档

tar 实用程序的名称代表 tape archiver（磁带归档器），并常用于创建数据备份。与 cpio 类似，使用 tar 命令时，所选文件会被复制并存储在单个文件中。此文件称为 tar 存档文件。如果使用数据压缩实用程序压缩此存档文件，则压缩后的存档文件称为 tarball。

### 表 4.7 tar 命令的常用 tarball 创建选项

| 短选项 | 长选项 | 说明 |
|---|---|---|
| -c | --create | 创建 tar 存档文件。备份可以是完整备份或增量备份，具体取决于其他选定的选项。 |
| -u | --update | 将文件追加到现有 tar 存档文件中，但仅复制自原始存档文件创建以来被修改的文件。 |
