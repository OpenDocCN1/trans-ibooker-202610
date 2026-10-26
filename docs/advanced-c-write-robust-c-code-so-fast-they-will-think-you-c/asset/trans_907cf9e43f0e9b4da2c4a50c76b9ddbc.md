

FILE_ATTRIBUTE_OFFLINE // 文件已传输到归档介质
FILE_ATTRIBUTE_READONLY // 仅读访问
FILE_ATTRIBUTE_SYSTEM // 系统文件
FILE_ATTRIBUTE_TEMPORARY // 临时文件

最后一个参数 hFileTemplate 接受一个打开文件的句柄或 NULL 作为参数。如果传递了文件句柄，CreateFile 函数会忽略所有属性和标志，并使用打开文件的属性和标志。

关于 CreateFile 参数就这些了。如果它无法打开设备，则返回 INVALID_HANDLE_VALUE。以下示例演示如何打开文件进行读取：

```
#include <iostream>
#include <Windows.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                            FILE_SHARE_READ | FILE_SHARE_WRITE,
                            NULL, OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL,
NULL);

    if (INVALID_HANDLE_VALUE == hFile)
        std::cout << "Failed to open file for reading" << std::endl;
    else
        std::cout << "Successfully opened file for reading" << std::endl;

    CloseHandle(hFile);

    return 0;
}
```

接下来，要执行输入操作，我们使用 **ReadFile** 函数。它将文件描述符作为第一个参数，源缓冲区作为第二个参数，要读取的最大字节数作为第三个参数，读取字节数作为第四个参数，以及用于同步执行时的 **NULL** 值或指向有效且唯一的 OVERLAPPED 结构的指针作为最后一个参数。如果操作成功，**ReadFile** 返回 true，否则返回 false。以下示例演示如何从之前打开的用于读取的文件中输入：

```
BYTE pb[20];

DWORD dwNumBytes;

ReadFile(hFile, pb, 20, &dwNumBytes, NULL);
```

要执行输出操作，我们使用 **WriteFile** 函数。它的声明与 **ReadFile** 相同，但第三个参数设置要写入的字节数，第五个参数是已写入的字节数。以下示例演示如何向之前打开的用于写入的文件输出：

```
BYTE pb[20] = "Some information\0";

DWORD dwNumBytes;

WriteFile(hFile, pb, 20, &dwNumBytes, NULL);
```

要将缓存的数据写入设备，请使用 **FlushFileBuffer** 函数。它接受一个参数——文件描述符。让我们转向异步 I/O。为了让操作系统知道你计划异步地与设备工作，你需要使用 **FILE_FLAG_OVERLAPPED** 标志打开它。现在，为写入或读取打开文件如下所示：

```
#include <iostream>

#include <Windows.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                            FILE_SHARE_READ | FILE_SHARE_WRITE,
                            NULL, OPEN_ALWAYS, FILE_FLAG_OVERLAPPED,
NULL);

    if (INVALID_HANDLE_VALUE == hFile)
        std::cout << "Failed to open file for reading" << std::endl;
    else
        std::cout << "Successfully opened file for reading" << std::endl;

    CloseHandle(hFile);

    return 0;
}
```

我们使用相同的操作来执行文件的读取或写入，即 **ReadFile** 和 **WriteFile**，唯一的区别是读取或写入的字节数设置为 NULL，并且我们必须传递一个有效且唯一的 **OVERLAPPED** 对象。让我们考虑 OVERLAPPED 对象的结构是什么：

```
typedef struct _OVERLAPPED {
DWORD Internal; // for error code
DWORD InternalHigh; // for number of read bytes
DWORD Offset;
DWORD OffsetHigh;
HANDLE hEvent; // handle to an event
} OVERLAPPED, *LPOVERLAPPED;
```

Internal 成员设置为 **STATUS_PENDING**，这意味着操作尚未开始。读取或写入的字节数将写入 **InternalHigh** 成员。**Offset** 和 **OffsetHigh** 在异步操作中被忽略。**hEvent** 成员用于接收关于异步操作完成的事件。

> **Note**
> 
> I/O 操作的顺序不能保证，因此你不能依赖它。如果你计划在一个地方写入文件，在另一个地方从文件读取，你不能依赖顺序。

在异步模式下使用 **ReadFile** 和 **WriteFile** 时有一个不寻常的事情。它们在 I/O 请求同步执行时返回非零值。如果它们返回 **FALSE**，你需要调用 **GetLastError** 函数来检查为什么返回了 **FALSE**。如果错误代码是 **ERROR_IO_PENDING**，这意味着 I/O 请求已成功处理，处于挂起状态，将在以后执行。

你应该记住的最后一件事是，在 I/O 操作完成之前，你不能移动或删除 **OVERLAPPED** 对象或包含数据的缓冲区。对于每个 I/O 操作，你应该创建一个新的 **OVERLAPPED** 对象。

最后，让我们考虑系统通知我们 I/O 操作完成的方式。有几种这样的机制：释放设备、释放事件、产生警报和使用 I/O 端口。

**“糟糕”的方法：** **WriteFile** 和 **ReadFile** 函数将设备设置为“占用”状态。当 I/O 操作完成时，驱动程序将设备设置为“空闲”状态。我们可以检查完成的 I/O 操作是否调用 **WaitForSingleObject** 或 **WaitForMultipleObject** 函数。以下示例演示了这种方法：

```
```cpp
#include <Windows.h>

#include <WinError.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                               FILE_SHARE_READ | FILE_SHARE_WRITE,
    NULL,
                               OPEN_ALWAYS, FILE_FLAG_OVERLAPPED,
    NULL);

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
```

这是检查 I/O 操作是否已完成的最简单方法。但这种方法使调用线程在 `WaitForSingleObject` 调用上等待，因此它成为同步调用。此外，你可以为该设备启动多个 I/O 操作，但你不能确定线程会在设备释放的所需时刻唤醒。

**有点好，但不是最好的方法：** 你还记得重叠结构的最后一个成员吗？你通过调用 `CreateEvent` 函数创建一个事件并将其设置为 `OVERLAPPED` 对象。然后，当 I/O 操作完成时，系统通过调用 `SetEvent` 函数释放此事件。接下来，当调用线程需要获取正在执行的 I/O 操作的结果时，你调用 `WaitForSingleObject` 并传递此事件的描述符。以下示例演示了这种方法：

```
```c
#include <Windows.h>
#include <synchapi.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
                              FILE_SHARE_READ | FILE_SHARE_WRITE,
                              NULL, OPEN_ALWAYS, FILE_FLAG_OVERLAPPED,
    NULL);

    BYTE bInBuffer[10];
    OVERLAPPED o = { 0 };
    o.hEvent = CreateEvent(NULL,TRUE,FALSE,"IOEvent");
    ReadFile(hFile, bInBuffer, 10, NULL, &o);

    ///// do some work

    HANDLE hEvent = o.hEvent;

    WaitForSingleObject(hEvent, INFINITE);

    CloseHandle(hFile);

    return 0;
}
```
```

如果你希望通知调用线程 I/O 操作的结束，这是一个相当简单的方法。但这不是理想的方法，因为当有大量此类操作时，你需要为每个操作创建一个事件对象。

**另一种不是最好的方法：** 可警报的 I/O 工作方式如下。我们调用 **ReadFileEx** 和 **WriteFileEx** 进行输入/输出。它们与标准的 **ReadFile** 和 **WriteFile** 类似，但我们不传递存储读取或写入字符数的变量，并传递回调函数的地址。此回调函数称为完成例程，其声明如下：

```
```cpp
VOID WINAPI

CompletionRoutine(DWORD dwError,
                  DWORD dwNumBytes,
                  OVERLAPPED* po);
```
```

**ReadFileEx** 和 **WriteFileEx** 将回调函数的地址传递给设备驱动程序。当设备上的操作完成时，驱动程序将回调函数的地址添加到 APC 队列并指向 OVERLAPPED 结构的指针。然后，操作系统调用此函数并传递读取或写入的字节数、错误代码和指向 OVERLAPPED 结构的指针。

这种方法的主要缺点是编写回调函数并使用大量全局变量，因为回调函数在上下文中只有少量信息。另一个不使用这种方法的原因是，只有调用线程可以接收关于完成的通知。

