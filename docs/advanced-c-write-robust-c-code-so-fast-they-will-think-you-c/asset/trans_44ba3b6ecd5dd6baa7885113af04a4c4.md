

现在我们已经讨论了不好的做法，让我们来看一下处理 I/O 结果的最佳方法 – I/O 端口。I/O 完成端口是为与线程池一起使用而开发的。要创建这样一个端口，我们使用 CreateIoCompletionPort。该函数的声明如下：

```
HANDLE
CreateIoCompletionPort(HANDLE hFile,
    HANDLE hExistingCompletionPort,
    ULONG_PTR CompletionKey,
    DWORD dwNumberOfConcurrentThreads);
```

该函数创建一个 I/O 完成端口并将设备与此端口关联。要完成此操作，我们需要调用它两次。要创建新的完成端口，我们调用 CreateIoCompletionPort 函数并将 INVALID_HANDLE_VALUE 作为第一个参数传递，将 NULL 作为第二个参数传递，将 0 作为第三个参数传递，并传递此端口的线程数。将第四个参数传递为 0 将使线程数等于处理器数量。

> Note
> 
> 对于 I/O 完成端口，建议使用的线程数等于处理器数量的两倍。

接下来，我们需要将此端口与输入/输出设备关联。因此，我们第二次调用 CreateIoCompletionPort 函数并传递设备的描述符、已创建完成端口的描述符、将指示对设备进行读写操作的常量，以及 0 作为线程数。然后，当我们需要获取完成结果时，我们从端口描述符调用 GetQueuedCompletionStatus。如果操作完成，函数会立即返回结果。如果没有，则线程等待完成。以下示例演示了这种方法：

```
#include <Windows.h>

#include <synchapi.h>

int main()
{
    HANDLE hFile = CreateFile(TEXT("Test.txt"), GENERIC_READ,
        FILE_SHARE_READ | FILE_SHARE_WRITE,
```

```
NULL, OPEN_ALWAYS, FILE_FLAG_OVERLAPPED,
NULL);

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

## Asynchronous I/O on Linux Platforms

Linux 平台上的异步 I/O 支持对不同设备（如套接字、管道和 TTY）进行输入和输出，除了文件。是的，这很奇怪，但 Linux 开发者决定文件 I/O 操作已经足够快了。

要打开一个 I/O 设备，我们使用 open() 函数。它的声明如下：

```
int open (const char *filename, int flags[, mode_t mode])
```

第一个参数是文件名，而第二个参数是一个位掩码，控制文件应如何打开。如果系统无法打开设备，open() 返回 -1 的值。成功时，它返回设备描述符。open 模式的可能标志为 O_RDONLY、O_WRONLY 和 O_RDWR。

要执行输入/输出操作，我们使用名为 aio 的 POSIX 接口。它们有一组定义的函数，如 aio_read、aio_write、aio_fsync 等。它们用于启动异步操作。要获取执行结果，我们可以使用信号通知或线程的实例化。或者，我们也可以选择完全不接收通知。它们都声明在 <aio.h> 头文件中。

几乎所有这些都以 aiocb 结构（异步 IO 控制块）作为参数。它控制 IO 操作。此结构的声明如下：

```
struct aiocb
{
    int aio_fildes;
    off_t aio_offset;
    volatile void *aio_buf;
    size_t aio_nbytes;
    int aio_reqprio;
    struct sigevent aio_sigevent;
    int aio_lio_opcode;
};
```

aio_fildes 成员是打开设备的描述符，而 aio_offset 成员是设备中应进行读或写操作的偏移量。aio_buf 成员是指向要读写缓冲区的指针。aio_nbytes 成员是缓冲区的大小。aio_reqprio 成员是此 IO 操作执行的优先级。aio_sigevent 成员是一个结构，指出调用线程应如何被通知完成。aio_lio_opcode 成员是 I/O 操作的类型。以下示例演示如何初始化 aiocb 结构：

```
std::string fileContent;

constexpr int BUF_SIZE = 20;

fileContent.resize(BUF_SIZE, 0);

aiocb aiocbObj;

aiocbObj.aio_fildes = open("test.txt", O_RDONLY);
if (aiocbObj.aio_fildes == -1)
{
```

```
#include <unistd.h>
#include <iostream>
#include <chrono>
#include <thread>
```

2. 创建一个名为 **isDone** 的 bool 变量，它将指示操作是否已完成：

```
bool isDone{};
```

3. 定义函数作为我们的信号处理程序，即 **aioSigHandler**。当异步操作完成时它将被调用。信号处理程序应具有以下签名：

```
void name(int number, siginfo_t* si, void* additional)
```

4. 第一个参数是信号号，第二个参数是包含信号生成原因信息的结构，最后一个参数是附加信息。它可以转换为 **ucontext_t** 结构的指针，以便我们接收被此信号中断的线程上下文。在 **aioSigHandler** 中，检查与异步 I/O 操作相关的信号是否使用 **SI_ASYNCIO** 为常量。如果是，则输出一条消息。接下来，将 **isDone** 设置为 **true**：

```
void
aioSigHandler(int no, siginfo_t* si, void*)
{
    std::cout << "Signo: " << no << std::endl;
    if (si->si_code == SI_ASYNCIO)
    {
        std::cout << "I/O completion signal received" << std::endl;
    }
    isDone = true;
}
```

5. 定义另一个帮助函数名为 **initSigAct**。它将初始化 **sigaction** 结构。此结构定义了在 I/O 操作完成时将发送哪个信号以及应调用哪个处理程序。这里，我们选择了 **SIGUSR1** – 一个用户定义的信号。在 **sa_flags** 中，设置我们希望此信号在操作重启或接收信息时被传递：

```
bool
initSigAct(struct sigaction& item)
{
    item.sa_flags = SA_RESTART | SA_SIGINFO;
    item.sa_sigaction = aioSigHandler;
```

- 8. 创建一个名为 **fileName** 的变量，它保存一个名为 "Test.txt" 的文件。然后，以只读访问权限打开此文件：

```
const std::string fileName("Test.txt");
int descriptor = open(fileName.c_str(), O_RDONLY);
if (-1 == descriptor)
{
    std::cerr << "Failed to open file for reading" << std::endl;
    return -1;
}
std::cout << "Successfully opened file for reading" << std::endl;
```

- 9. 创建一个 **sigaction** 结构并使用 **initSigAct** 函数初始化它：

```
struct sigaction sa;
if (!initSigAct(sa))
{
    std::cerr << "failed registering signal" << std::endl;
    return -1;
}
```

- 10. 创建一个 **aiocb** 结构并使用 **fillAiocb** 函数初始化它：

```
aiocb aiocbObj;
fillAiocb(aiocbObj, descriptor, buffer, bufSize);
```

- 11. 使用 **aio_read** 函数执行 **read** 操作：

```
if (-1 == aio_read(&aiocbObj))
{
    std::cerr << "aio_read failed" << std::endl;
}
```

- 12. 接下来，在循环中评估 **isDone** 变量。如果为 false，则使线程休眠 3ms。通过这样做，我们将等待 I/O 操作完成：

```
while (!isDone)
{
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(3ms);
}
std::cout << "Successfully finished read operation. Buffer: " << std::endl
<< buffer;
```

- 13. 在运行此练习之前，在项目目录中创建一个 `Test.txt` 文件并写入不同的符号。例如，我们的文件包含以下数据：

```
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
a1a"1 a1\\a1 a1    a1
```

这里有字母字符、数字字符、特殊符号、空格、制表符和换行符。

- 14. 现在，在你的 IDE 中构建并运行此练习。你的输出将类似于以下内容：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_480_0.png)

你可以看到文件已成功以只读方式打开，并且我们已成功为其设置 **SIGUSR1** 信号和处理程序。然后，我们接收到信号号 30，即 **SI_ASYNCIO** 信号。最后，我们可以输出读取的内容并与文件内容进行比较。通过这样做，我们可以确保所有数据都已正确读取。

