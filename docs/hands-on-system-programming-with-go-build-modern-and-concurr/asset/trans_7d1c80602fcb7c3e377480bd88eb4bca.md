
# Unix 组件概览

## 退出代码

```c
#define EX_OK 0 /* successful termination */
#define EX__BASE 64 /* base value for error messages */
#define EX_USAGE 64 /* command line usage error */
#define EX_DATAERR 65 /* data format error */
#define EX_NOINPUT 66 /* cannot open input */
#define EX_NOUSER 67 /* addressee unknown */
#define EX_NOHOST 68 /* host name unknown */
#define EX_UNAVAILABLE 69 /* service unavailable */
#define EX_SOFTWARE 70 /* internal software error */
#define EX_OSERR 71 /* system error (e.g., can't fork) */
#define EX_OSFILE 72 /* critical OS file missing */
#define EX_CANTCREAT 73 /* can't create (user) output file */
#define EX_IOERR 74 /* input/output error */
#define EX_TEMPFAIL 75 /* temp failure; user is invited to retry */
#define EX_PROTOCOL 76 /* remote error in protocol */
#define EX_NOPERM 77 /* permission denied */
#define EX_CONFIG 78 /* configuration error */
#define EX__MAX 78 /* maximum listed value */
```

其来源如下：[https://elixir.bootlin.com/linux/latest/source/fs/ext4/ext4.h#L212](https://elixir.bootlin.com/linux/latest/source/fs/ext4/ext4.h#L212)。

上一条命令的退出代码存储在 `$?` 变量中，可以对其进行测试以控制操作流程。常用的运算符是 `&&`（双与号），它仅在第一条指令的退出代码为 `0` 时才执行下一条指令，例如 `stat file && echo something >> file`，仅当文件存在时才向文件追加内容。

## 信号

退出代码将进程及其父进程连接起来，但信号使得任何进程都能与另一个进程（包括其自身）进行交互。它们也是异步且单向的，但它们代表来自进程外部的通信。

最常见的信号是 `SIGINT`，它通知应用程序终止，可以在 Shell 中通过 *Ctrl* + *C* 组合键发送给前台进程。不过，还有更多选项，如下表所示：

| 名称 | 编号 | 描述 |
|------|--------|-------------|
| `SIGHUP` | 1 | 控制终端关闭 |
| `SIGINT` | 2 | 中断信号（*Ctrl* + *C*） |
| `SIGQUIT` | 3 | 退出信号（*Ctrl* + *D*） |
| `SIGFPE` | 8 | 尝试非法数学运算 |
| `SIGKILL` | 9 | 立即退出应用程序 |
| `SIGALRM` | 14 | 闹钟信号 |

`kill` 命令允许你向任何应用程序发送信号，可用的信号完整列表可以通过 `-l` 标志显示：

![](assets/fe0c00b9-45c3-4c14-acc5-02d28aa7a744.png)

## 管道

管道是进程间最后一种单向通信方法。顾名思义，管道连接两端——一个进程的输入与另一个进程的输出——使得同一主机上的进程能够通信以交换数据。

这些分为匿名管道和命名管道：

*   匿名管道将一个进程的标准输出链接到另一个进程的标准输入。在 Shell 中可以使用 `|` 运算符轻松实现，将管道前命令的输出作为管道后命令的输入。`ls -l | grep "user"` 获取 `ls` 命令的输出并将其作为 `grep` 的输入。
*   命名管道使用特定文件来执行重定向。输出可以通过 `>`（大于号）运算符重定向到文件，而 `<`（小于号）符号允许你将文件作为另一个进程的输入。`ls -l > file.txt` 将命令输出保存到文件。`cat < file.txt` 将文件内容发送到命令的标准输入，标准输入再将其复制到标准输出。

也可以使用 `>>`（双大于号）运算符向命名管道追加内容，这将从文件末尾开始写入。

## 套接字

Unix 域套接字是同一机器上应用程序之间的双向通信方法。它们是由内核处理并管理数据交换的逻辑端点。

套接字的特性允许将其用作面向流或面向数据报。面向流的协议确保在移动到下一个数据块之前消息已送达，以保持消息完整性。相比之下，面向消息的协议忽略未接收到的数据并继续发送后续消息，这使其成为一种更快但可靠性较低、延迟极低的协议。

套接字分类如下：

*   `SOCK_STREAM`：面向连接、有序、可靠的数据流传输
*   `SOCK_SEQPACKET`：面向连接、有序、可靠的带记录边界的消息数据传输
*   `SOCK_DGRAM`：无序且不可靠的消息传输

## 总结

本章概述了主要 Unix 组件及其相互交互的方式。我们从内存管理及其在 Unix 中的工作原理开始，理解**分页**和**交换**等概念。

然后我们分析了文件系统，了解现代操作系统的支持，并解释了现有文件类型的区别：文件、目录、硬链接和软链接。

在了解 inode 概念后，我们查看了 Unix 操作系统中目录的结构，并解释了如何导航和与文件系统交互，以及如何挂载和卸载其他分区。

接着我们转到进程、Unix 中运行的应用程序及其结构和属性。我们分析了进程生命周期，从通过 `fork` 或 `exec` 创建，到通过 `kill` 命令结束或终止。

另一个重要主题是用户、组和权限。我们了解了什么是用户、什么是组、如何加入它们，以及这些概念如何用于将权限划分为三组：用户、组和其他人。这有助于更好地理解 Unix 权限模型，以及如何更改文件和目录的权限。

最后，我们了解了进程间通信如何通过单向通道（如信号和退出代码）或双向通信（如套接字）工作。

## 问题

1.  现代操作系统使用哪种文件系统？
2.  什么是 inode？Unix 中的 inode `0` 是什么？
3.  PID 和 PPID 有什么区别？
4.  如何终止在后台运行的进程？
5.  用户和组有什么区别？
6.  Unix 权限模型的范围是什么？
7.  你能解释信号和退出代码的区别吗？
8.  什么是交换文件？

# Go 概览

本章将提供 Go 语言及其基本功能的概览。我们将简要解释该语言及其特性，这些将在后续章节中详细阐述。这将帮助我们在使用所有特性和应用时更好地理解 Go。

本章将涵盖以下主题：

*   语言特性
*   包和导入
*   基本类型、接口和用户定义类型
*   变量和函数
*   流程控制
*   内置函数
*   并发模型
*   内存管理

## 技术要求

从本章开始，你需要在机器上安装 Go。请按以下步骤操作：

1.  从 [https://golang.org/dl/](https://golang.org/dl/) 下载最新版本的 Go。
2.  使用 `tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz` 解压。
3.  使用 `export PATH=$PATH:/usr/local/go/bin` 将其添加到 `PATH`。
4.  使用 `go version` 验证 Go 是否已安装。
5.  在你的 `.profile` 中添加 export 语句以自动添加。
6.  如果你想为代码使用不同的目录，也可以更改 `GOPATH` 变量（默认为 `~/go`）。
