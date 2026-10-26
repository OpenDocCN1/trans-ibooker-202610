
Now that we've gone over the bad, let's look at the best approach for handling I/O results – I/O ports. The I/O completion ports are developed to be used with thread pools. To create such a port, we use CreateIoCompletionPort. The declaration of this function looks as follows:

```
HANDLE
CreateIoCompletionPort(HANDLE hFile,
    HANDLE hExistingCompletionPort,
    ULONG_PTR CompletionKey,
    DWORD dwNumberOfConcurrentThreads);
```

This function creates an I/O completion port and associates the device with this port. To complete this action, we need to call it twice. To create the new completion port, we invoke the CreateIoCompletionPort function and pass INVALID_HANDLE_VALUE as the first parameter, NULL as a second parameter, 0 as a third parameter, and pass the number of the threads for this port. Passing 0 as the fourth parameter will set the number of threads equal to the number of processors.

> Note
> 
> For the I/O completion port, it is recommended to use the number of threads that is equal to the number of processors, multiplied twice.

Next, we need to associate this port with the input/output device. So, we invoke the CreateIoCompletionPort function for the second time and pass a descriptor of the device, a descriptor of the created completion port, the constant that will indicate reading or writing to the device, and 0 as the number of threads. Then, when we need to get the result of completion, we call GetQueuedCompletionStatus from our port descriptor. If the operation completes, the function returns a result immediately. If it doesn't, then the thread waits to complete. The following example demonstrates this approach:

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

The asynchronous I/O on Linux supports input and output to different devices such as sockets, pipes, and TTYs, except files. Yes, this is pretty strange, but Linux developers decided that I/O operations with files are fast enough.

To open an I/O device, we use the open() function. It has the following declaration:

```
int open (const char *filename, int flags[, mode_t mode])
```

The first parameter is a filename, while the second parameter is a bitmask that controls how the file should be opened. If the system cannot open the device, open() returns a value of -1. In the case of success, it returns a device descriptor. The possible flags for open mode are O_RDONLY, O_WRONLY, and O_RDWR.

To perform input/output operations, we use the POSIX interface called aio. They have a defined set of functions such as aio_read, aio_write, aio_fsync, and so on. They are used to initiate the asynchronous operations. To get the result of execution, we can use signal notification or the instantiation of a thread. Alternatively, we can choose not to be notified at all. All of them are declared in the <aio.h> header.

Almost all of these take the aiocb structure (asynchronous IO control block) as a parameter. It controls the IO operations. The declaration of this structure looks as follows:

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

The aio_fildes member is a descriptor to the opened device, while the aio_offset member is an offset in the device where read or write operations should be done. The aio_buf member is a pointer to the buffer to read or write from. The aio_nbytes member is the size of the buffer. The aio_reqprio member is the priority of the execution of this IO operation. The aio_sigevent member is a structure that points out how the calling thread should be notified about the finish. The aio_lio_opcode member is a type of I/O operation. The following example demonstrates how to initialize the aiocb structure:

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

2. Create a bool variable called **isDone** that will indicate when the operation has been completed:

```
bool isDone{};
```

3. Define the function that will be our signal handler, that is, **aioSigHandler**. It will be called when the async operation is done. Signal handlers should have the following signature:

```
void name(int number, siginfo_t* si, void* additional)
```

4. The first parameter is a signal number, the second parameter is a structure that contains information about why the signal was generated, and the last parameter is additional information. It can be cast to a pointer of the **ucontext_t** structure so that we can receive the thread context that was interrupted by this signal. In **aioSigHandler**, check whether the signal regarding the async I/O operation is constant using **SI_ASYNCIO**. If so, output a message. Next, set **isDone** to **true**:

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

5. Define another helping function called **initSigAct**. It will initialize the **sigaction** structure. This structure defines which signal will be sent on the I/O operation's finish and which handler should be called. Here, we chose **SIGUSR1** – a user-defined signal. In **sa_flags**, set that we want this signal to be delivered on action restart or information received:

```
bool
initSigAct(struct sigaction& item)
{
    item.sa_flags = SA_RESTART | SA_SIGINFO;
    item.sa_sigaction = aioSigHandler;
```

- 8. Create a variable called **fileName** that holds a file called "Test.txt". Then, open this file with read-only access:

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

- 9. Create a **sigaction** structure and initialize it using the **initSigAct** function:

```
struct sigaction sa;
if (!initSigAct(sa))
{
    std::cerr << "failed registering signal" << std::endl;
    return -1;
}
```

- 10. Create an **aiocb** structure and initialize it using the **fillAiocb** function:

```
aiocb aiocbObj;
fillAiocb(aiocbObj, descriptor, buffer, bufSize);
```

- 11. Perform a **read** operation using the **aio_read** function:

```
if (-1 == aio_read(&aiocbObj))
{
    std::cerr << "aio_read failed" << std::endl;
}
```

- 12. Next, in the loop, evaluate the **isDone** variable. If it is false, make the thread sleep for 3ms. By doing this, we will wait for the I/O operation to finish:

```
while (!isDone)
{
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(3ms);
}
std::cout << "Successfully finished read operation. Buffer: " << std::endl
<< buffer;
```

- 13. Before running this exercise, create a `Test.txt` file in the project directory and write different symbols. For example, our file contains the following data:

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

Here, there's alphabetical characters, numerical characters, special symbols, spaces, tabulation characters, and newline characters.

- 14. Now, build and run this exercise in your IDE. Your output will be similar to the following:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_480_0.png)

You can see that the file was successfully opened for reading and that we successfully set the **SIGUSR1** signal and handler for it. Then, we received signal number 30, that is, the **SI_ASYNCIO** signal. Finally, we can output what we have read and compare it with the file content. By doing this, we can ensure that all the data was read correctly.
