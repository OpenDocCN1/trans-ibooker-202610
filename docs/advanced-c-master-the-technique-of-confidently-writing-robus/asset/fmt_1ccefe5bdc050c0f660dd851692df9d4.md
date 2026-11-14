
最后要记住的是，在 I/O 操作完成之前，不能移动或删除 **OVERLAPPED** 对象或含数据的缓冲区。每个 I/O 操作都应创建一个新的 `OVERLAPPED` 对象。

最后，让我们看看系统通知我们 I/O 操作完成的几种方式。主要有以下机制：释放设备、释放事件、产生警报、使用 I/O 完成端口。

## “糟糕”的方法

`WriteFile` 和 `ReadFile` 函数将设备置为“占用”状态。I/O 操作完成后，驱动程序将设备置为“空闲”状态。我们可以通过调用 `WaitForSingleObject` 或 `WaitForMultipleObjects` 函数来检查 I/O 操作是否完成。下面的示例演示了这种方法：

```cpp
#include <Windows.h>
#include <WinError.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                              FILE_SHARE_READ | FILE_SHARE_WRITE, NULL,
                              OPEN_ALWAYS, FILE_FLAG_OVERLAPPED, NULL);
    BYTE bBuffer[100];
    OVERLAPPED o = { 0 };
    BOOL bResult = ReadFile(hFile, bBuffer, 100, NULL, &o);
    DWORD dwError = GetLastError();
    if (bResult && (dwError == ERROR_IO_PENDING))
    {
        WaitForSingleObject(hFile, INFINITE);
        bResult = TRUE;
    }
    CloseHandle(hFile);
    return 0;
}
```

这是检查 I/O 操作是否完成的最简单方法。但这种方法会让调用线程在 `WaitForSingleObject` 调用上等待，从而变成同步调用。此外，你可以为该设备发起多个 I/O 操作，但无法确保线程会在所需的设备释放时唤醒。

## “稍好但非最佳”的方法

还记得 `OVERLAPPED` 结构的最后一个成员吗？你可以通过调用 `CreateEvent` 函数创建一个事件，并将其赋给 `OVERLAPPED` 对象。当 I/O 操作完成时，系统会调用 `SetEvent` 函数来释放该事件。随后，当调用线程需要获取正在执行的 I/O 操作的结果时，可以调用 `WaitForSingleObject` 并传入该事件的句柄。下面的示例演示了这种方法：

```cpp
#include <Windows.h>
#include <synchapi.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                              FILE_SHARE_READ | FILE_SHARE_WRITE,
                              NULL, OPEN_ALWAYS, FILE_FLAG_OVERLAPPED, NULL);
    BYTE bInBuffer[10];
    OVERLAPPED o = { 0 };
    o.hEvent = CreateEvent(NULL, TRUE, FALSE, "IOEvent");
    ReadFile(hFile, bInBuffer, 10, NULL, &o);
    ///// do some work
    HANDLE hEvent = o.hEvent;
    WaitForSingleObject(hEvent, INFINITE);
    CloseHandle(hFile);
    return 0;
}
```

如果你想通知调用线程 I/O 操作结束，这是一种相当简单的方法。但这并非理想方式，因为当有大量此类操作时，你需要为每个操作创建一个事件对象。

## “另一种非最佳”的方法

可提醒 I/O 的工作方式如下。我们调用 `ReadFileEx` 和 `WriteFileEx` 进行输入/输出。它们类似于标准的 `ReadFile` 和 `WriteFile`，但不传递存储读/写字符数的变量，而是传递回调函数的地址。该回调函数称为完成例程，其声明如下：

```cpp
VOID WINAPI CompletionRoutine(DWORD dwError,
                              DWORD dwNumBytes,
                              OVERLAPPED* po);
```

`ReadFileEx` 和 `WriteFileEx` 将回调函数的地址传递给设备驱动程序。当设备上的操作完成时，驱动程序将回调函数的地址和指向 `OVERLAPPED` 结构的指针添加到 APC 队列中。然后，操作系统调用该函数，并传递读/写的字节数、错误代码和指向 `OVERLAPPED` 结构的指针。

这种方法的主要缺点是需要编写回调函数，且由于回调函数上下文中包含的信息很少，往往需要使用大量全局变量。不采用这种方法的另一个原因是：只有调用线程能接收完成通知。

既然已经了解了不好的方法，现在让我们看看处理 I/O 结果的最佳方法——I/O 完成端口。I/O 完成端口设计用于配合线程池使用。要创建此类端口，我们使用 `CreateIoCompletionPort`。其声明如下：

```cpp
HANDLE CreateIoCompletionPort(HANDLE hFile,
                              HANDLE hExistingCompletionPort,
                              ULONG_PTR CompletionKey,
                              DWORD dwNumberOfConcurrentThreads);
```

此函数创建一个 I/O 完成端口，并将设备与该端口关联。要完成此操作，我们需要调用它两次。第一次调用 `CreateIoCompletionPort` 创建新的完成端口：传入 `INVALID_HANDLE_VALUE` 作为第一个参数，`NULL` 作为第二个参数，`0` 作为第三个参数，并传入该端口的线程数作为第四个参数。将第四个参数传为 `0` 会将线程数设置为处理器数量。

> **注意**
>
> 对于 I/O 完成端口，建议使用的线程数等于处理器数量的两倍。

接下来，我们需要将此端口与输入/输出设备关联。因此，我们第二次调用 `CreateIoCompletionPort`，传入设备句柄、创建的完成端口句柄、指示对设备进行读或写操作的常量，以及 `0` 作为线程数。然后，当我们需要获取完成结果时，从端口句柄调用 `GetQueuedCompletionStatus`。如果操作已完成，该函数立即返回结果；否则，线程等待完成。下面的示例演示了这种方法：

```cpp
#include <Windows.h>
#include <synchapi.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                              FILE_SHARE_READ | FILE_SHARE_WRITE,
                              NULL, OPEN_ALWAYS, FILE_FLAG_OVERLAPPED, NULL);
    HANDLE m_hIOcp = CreateIoCompletionPort(INVALID_HANDLE_VALUE, NULL, 0, 0);
    CreateIoCompletionPort(hFile, m_hIOcp, 1, 0);
    BYTE bInBuffer[10];
    OVERLAPPED o = { 0 };
    ReadFile(hFile, bInBuffer, 10, NULL, &o);
    DWORD dwNumBytes;
    ULONG_PTR completionKey;
    GetQueuedCompletionStatus(m_hIOcp, &dwNumBytes, &completionKey, (OVERLAPPED**) &o, INFINITE);
    CloseHandle(hFile);
    return 0;
}
```

## Linux 平台上的异步 I/O

Linux 上的异步 I/O 支持套接字、管道和 TTY 等不同设备的输入和输出，**但不支持文件**。这听起来很奇怪，但 Linux 开发者认为文件 I/O 操作足够快。

要打开 I/O 设备，我们使用 `open()` 函数。其声明如下：

```c
int open(const char *filename, int flags[, mode_t mode])
```

第一个参数是文件名，第二个参数是控制文件打开方式的位掩码。如果系统无法打开设备，`open()` 返回 `-1`；成功时返回设备描述符。`open` 模式的可能标志有 `O_RDONLY`、`O_WRONLY` 和 `O_RDWR`。
