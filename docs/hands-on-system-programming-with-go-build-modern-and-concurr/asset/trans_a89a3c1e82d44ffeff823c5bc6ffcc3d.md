
我们可以使用一些类别来划分操作系统提供的众多功能。这些类别包括正在运行的应用程序及其流程的控制、文件系统访问以及网络。

## 进程控制

此类服务包括 `load`，它将程序加载到内存中并准备执行，然后将控制权传递给程序本身；或 `execute`，它在预先存在的进程上下文中运行可执行文件。属于此类别的其他操作如下：

*   `end` 和 `abort`——前者要求应用程序退出，后者强制其退出。
*   `CreateProcess`，在 Unix 系统上也称为 `fork`，在 Windows 中称为 `NtCreateProcess`。
*   终止进程。
*   获取/设置进程属性。
*   等待时间、等待事件或发送事件信号。
*   分配和释放内存。

## 文件管理

文件和文件系统的处理属于文件管理系统调用。有用于从文件系统添加或移除条目的 `create` 和 `delete` 文件操作，以及用于获取文件控制权以执行 `read` 和 `write` 操作的 `open` 和 `close` 操作。也可以读取和更改文件属性。

## 设备管理

设备管理处理除文件系统之外的所有其他设备，例如帧缓冲区或显示器。它包括从请求设备开始的所有操作，包括与其通信（`read`、`write`、`seek`）及其释放。它还包括更改设备属性以及逻辑上连接和断开设备的所有操作。

## 信息维护

读写系统日期和时间属于信息维护类别。该类别还负责其他系统数据，例如环境变量。属于此处的另一组重要操作是进程、文件和设备属性的请求与操作。

## 通信

从处理套接字到接受连接的所有网络操作都属于通信类别。这包括连接的创建、删除和命名，以及 `send` 和 `receive` 消息。

# 操作系统之间的差异

Windows 拥有一系列不同的系统调用，涵盖所有内核操作。其中许多与 Unix 对应物完全一致。以下是一些重叠系统调用的列表：

| 类别 | Windows | Unix |
| --- | --- | --- |
| 进程控制 | `CreateProcess()`<br>`ExitProcess()`<br>`WaitForSingleObject()` | `fork()`<br>`exit()`<br>`wait()` |
| 文件操作 | `CreateFile()`<br>`ReadFile()`<br>`WriteFile()`<br>`CloseHandle()` | `open()`<br>`read()`<br>`write()`<br>`close()` |
| 文件保护 | `SetFileSecurity()`<br>`InitializeSecurityDescriptor()`<br>`SetSecurityDescriptorGroup()` | `chmod()`<br>`umask()`<br>`chown()` |
| 设备管理 | `SetConsoleMode()`<br>`ReadConsole()`<br>`WriteConsole()` | `ioctl()`<br>`read()`<br>`write()` |
| 信息维护 | `GetCurrentProcessID()`<br>`SetTimer()`<br>`Sleep()` | `getpid()`<br>`alarm()`<br>`sleep()` |
| 通信 | `CreatePipe()`<br>`CreateFileMapping()`<br>`MapViewOfFile()` | `pipe()`<br>`shmget()`<br>`mmap()` |

# 理解 POSIX 标准

为了确保操作系统之间的一致性，IEEE 为操作系统制定了一些标准。这些标准将在以下章节中描述。

## POSIX 标准与特性

用于 Unix 的 `Portable Operating System Interface` (`POSIX`) 代表了一系列操作系统接口标准。第一个版本可追溯至 1988 年，涵盖文件名、Shell 和正则表达式等一系列主题。

`POSIX` 定义了许多特性，它们被组织在四个不同的标准中，每个标准专注于 Unix 合规性的不同方面。它们都以 `POSIX` 后跟一个数字命名。

## POSIX.1 – 核心服务

`POSIX.1` 是 1988 年的原始标准，最初命名为 `POSIX`，但后来重命名以便在不放弃名称的情况下向该系列添加更多标准。它定义了以下特性：

*   进程创建与控制
*   信号
*   浮点异常
*   分段/内存违规
*   非法指令
*   总线错误
*   定时器
*   文件与目录操作
*   管道
*   C 库（标准 C）
*   I/O 端口接口与控制
*   进程触发器

## POSIX.1b 和 POSIX.1c – 实时与线程扩展

`POSIX.1b` 专注于实时应用程序和需要高性能的应用程序。它关注以下方面：

*   优先级调度
*   实时信号
*   时钟与定时器
*   信号量
*   消息传递
*   共享内存
*   异步与同步 I/O
*   内存锁定接口

`POSIX.1c` 引入了多线程范式并定义了以下内容：

*   线程创建、控制与清理
*   线程调度
*   线程同步
*   信号处理

## POSIX.2 – Shell 与工具

`POSIX.2` 为命令行解释器和实用程序（如 `cd`、`echo` 或 `ls`）指定了标准。

## 操作系统遵循情况

并非所有操作系统都符合 `POSIX` 标准。例如，Windows 诞生于该标准之后，并不合规。从认证角度来看，macOS 比 Linux 更合规，因为后者使用的是构建在 `POSIX` 之上的另一个标准。

## Linux 与 macOS

大多数 Linux 发行版遵循 `Linux Standard Base` (`LSB`)，这是另一个包含 `POSIX` 及更多内容的标准，专注于维护不同 Linux 发行版之间的互兼容性。它不被视为官方合规，因为开发者未经历认证过程。

然而，macOS 自 2007 年 Snow Leopard 发行版起成为完全兼容，并自那时起获得了 `POSIX` 认证。

## Windows

Windows 不符合 `POSIX` 标准，但有许多尝试使其合规。存在开源项目如 Cygwin 和 MinGW，它们提供了一个不太符合 `POSIX` 标准的开发环境，并支持使用 Microsoft Visual C 运行时库的 C 应用程序。Microsoft 本身也尝试过 `POSIX` 兼容性，例如 Microsoft POSIX 子系统。Microsoft 制作的最新兼容层是 Windows Linux Subsystem，这是一项可在 Windows 10 中激活的可选功能，深受开发者好评（包括我自己）。

# 总结

在本章中，我们了解了什么是系统编程——编写具有严格要求的系统软件，例如与硬件绑定、使用低级语言、在资源受限的环境中工作。在构建通常需要优化资源使用的分布式系统时，其实践非常有用。我们讨论了 API，即允许软件被其他软件使用的定义，并列出了不同类型——操作系统中的 API、库和框架、远程和 Web API。

我们分析了操作系统中对资源的访问如何按称为保护环的分层级别排列，以防止不受控制的使用，从而提高安全性并避免应用程序故障。Linux 模型将此层次结构简化为仅两个级别，称为用户空间和内核空间。所有应用程序都在用户空间运行，为了访问机器资源，它们需要内核介入。

然后我们看到了一种特定类型的 API，称为系统调用，它允许应用程序向内核请求资源，并调解进程控制、文件的访问与管理、设备和网络通信。
