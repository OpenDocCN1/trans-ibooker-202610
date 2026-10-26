

Linux 系统中的异步 I/O 就介绍到这里。

> **注意**
> 
> 你可以在 Linux 的 man 手册中找到有关 Linux 中异步 IO 的更多信息：[http://man7.org/linux/man-pages/man7/aio.7.html](http://man7.org/linux/man-pages/man7/aio.7.html)。

现在，让我们来学习一下我们可以用于跨平台应用程序的东西。

## 异步跨平台 I/O 库

我们已经考虑了异步 I/O 的平台相关决策。现在，要编写跨平台应用程序，你可以使用这些平台相关的方案并通过预处理器指令使用它们；例如：

```
```cpp
#ifdef WIN

#include <WinAIO.hpp>

#else

#include <LinAIO.hpp>

#endif
```
```

在两个头文件中，你可以为平台相关的实现声明相同的接口。你也可以实现自己的 AIO 库，它将在单独的线程中使用一些状态机或队列。此外，你可以使用一些实现了必要功能的免费库。最受欢迎的库是 **Boost.Asio**。它提供了大量用于异步工作的接口，例如以下内容：

- 无线程的并发
- 线程
- 缓冲区
- 流
- 协程
- TCP、UDP 和 ICMP
- 套接字
- SSL
- 定时器
- 串口

让我们简要考虑一下它对 I/O 操作的接口。我们可以使用 **Asio** 库的接口进行同步和异步操作。所有 I/O 操作都从 **io_service** 类开始，它提供了核心 I/O 功能。它在 **<boost/asio/io_service.hpp>** 头文件中声明。同步 I/O 调用 **io_service** 对象的 **run()** 函数，用于单次操作，该操作会阻塞调用线程直到作业完成。异步 I/O 使用 **run()**、**run_one()**、**poll()** 和 **poll_one()** 函数。**run()** 函数运行事件循环以处理请求处理器。**run_one()** 函数做同样的事情，但事件循环应仅处理一个处理器。**poll()** 函数运行事件循环以执行所有就绪的处理器。**poll_one()** 做同样的事情但仅针对一个处理器。以下示例演示了所有这些函数的使用：

```
boost::asio::io_service io_service1;

io_service1.run();

boost::asio::io_service io_service2;

io_service2.run_one();

boost::asio::io_service io_service3;

io_service3.poll();

boost::asio::io_service io_service4;

io_service4.poll_one();
```

可以在实际调用 I/O 操作之前运行事件处理器。使用工作类与 **io_service** 类一起在代码中实现此功能。工作类保证 run 函数不会返回，直到你决定不再有未来的 I/O 操作为止。例如，你可以将工作类作为另一个类的成员，并在析构函数中将其移除。因此，在你的类生命周期内，**io_service** 将一直运行：

```
boost::asio::io_service io_service1;
boost::asio::io_service::work work(io_service1);
io_service1.run();

boost::asio::io_service io_service2;
boost::asio::io_service::work work(io_service2);
io_service2.poll();
```

接下来，要执行任何 I/O 操作，我们需要确切的 I/O 设备，例如文件、套接字等。有许多类实现了与不同 I/O 设备的交互，例如来自 `<boost/asio/ip/tcp.hpp>` 头文件的 **boost::asio::ip::tcp::socket**。接下来，要读取和写入套接字，我们使用 **boost::asio::async_read** 和 **boost::asio::async_write**。它们接受套接字、**boost::asio::buffer** 和回调函数作为参数。当异步操作执行时，回调函数将被调用。我们可以传递一个 lambda 函数作为回调函数，也可以使用 boost::bind 函数绑定现有函数。**boost::bind** 创建一个可调用对象。以下示例演示了如何使用 Boost::Asio 写入套接字：

```
boost::asio::io_service ioService;

tcp::socket socket;

int length = 15;

char* msg = new char(length);

msg = "Hello, world!";

auto postHandler = [=]()
{
    auto writeHandler = [=](boost::system::error_code ec, std::size_t length)
    {
        if (ec)
        {
            socket_.close();
        }
        else
        {
```

```
// wrote length characters
}
};
boost::asio::async_write(socket, boost::asio::buffer(msg, length),
writeHandler);
};

ioService.post(postHandler);
```

这里，我们使用了 lambda 函数作为异步 I/O 操作的回调。

> **注意**
> 
> **Boost.Asio** 在 https://www.boost.org/doc/libs/1_63_0/doc/html/boost_asio.html 上有很好的文档说明。有大量使用不同 IO 设备和不同方法的示例。如果你决定在项目中使用 **Boost.Asio**，可以参考此文档。

这里，我们考虑了可以实现异步 I/O 操作的不同方法。根据你的需求、环境和允许的实用程序，你可以选择合适的方式在应用程序中实现异步 I/O。请记住，如果你选择执行许多快速的 I/O 操作，最好以同步方式执行，因为它不会占用太多系统资源。现在我们知道了如何利用异步 I/O，让我们学习如何在多线程应用程序中使用 I/O。

## 线程与 I/O 的交互

I/O 标准库不是线程安全的。在标准库的文档中，我们可以找到解释，说明对流或流缓冲区的并发访问可能导致数据竞争，从而导致未定义行为。为了避免这种情况，我们应该使用我们在 *第 5 章，哲学家晚餐 – 线程与并发* 中学到的技术来同步对流和缓冲区的访问。

让我们谈谈 **std::cin** 和 **std::cout** 对象。每次调用它们都是线程安全的，但让我们考虑以下示例：

```
```cpp
std::cout << "Counter: " << counter << std::endl;
```
```

在这行代码中，我们看到 **std::cout** 被调用一次，但每次对移位运算符的调用实际上是对 **std::cout** 对象的不同调用。因此，我们可以重写此行如下：

```
```cpp
std::cout << "Counter: ";
std::cout << counter;
std::cout << std::endl;
```
```

此代码与之前的单行完全相同，即，如果你从不同的线程调用此单行，你的输出将被混合且不清晰。你可以修改它使其真正线程安全，如下所示：

```
```cpp
std::stringstream ss;
ss << "Counter: " << counter << std::endl;
std::cout << ss.str();
```
```

因此，如果你使用第二种方法向终端输出，你的输出将清晰且线程安全。此行为可能因编译器或 std 库版本而异。你还必须知道 **std::cout** 和 **std::cin** 之间是同步的。这意味着调用 **std::cout** 总是会刷新 **std::cin** 流，而调用 **std::cin** 总是会刷新 **std::cout** 流。

最佳方法是使用守卫类包装所有 I/O 操作，该类将使用互斥锁控制对流的访问。如果你需要从多个线程使用 **std::cout** 向终端输出，你可以实现一个非常简单的类，它除了锁定互斥锁并调用 **std::cout** 之外什么都不做。让我们完成一个练习并创建此类。

## 练习 9：为 std::cout 开发线程安全包装器

在本练习中，我们将开发一个简单的 **std::cout** 包装器以产生线程安全的输出。我们将编写一个小的测试函数来检查其工作原理。让我们开始并执行以下步骤：

- 1. 包含所有必需的头文件：

```
```cpp
#include <iostream>  // for std::cout
#include <thread>   // for std::thread
#include <mutex>    // for std::mutex
#include <sstream>  // for std::ostringstream
```
```

现在，让我们思考一下我们的包装器。我们可以在某个地方创建一个此类变量并将其传递给每个创建的线程。然而，这是个坏主意，因为在复杂的应用中，这将需要大量的精力。我们也可以将其作为单例实现，以便我们可以从任何地方访问它。接下来，我们必须考虑我们类的内容。实际上，我们可以使用我们在 *练习 7，继承标准流对象* 中创建的类。在该练习中，我们重载了 `std::basic_streambuf` 和 `std::basic_ostream` 并将 `std::cout` 设置为输出设备。我们可以在重载函数中添加一个互斥锁并照常使用它。请注意，我们不需要任何额外的逻辑——只是使用 `std::cout` 输出数据。为此，我们可以创建一个更简单的类。如果我们没有设置输出设备，应用左移运算符将不会生效并将数据存储在内部缓冲区中。太好了！现在，我们需要考虑如何将此缓冲区使用 `std::cout` 输出。

- 2. 实现一个 `write()` 函数，它将锁定互斥锁并从内部缓冲区输出到 `std::cout`。此函数的使用方式如下：
  
  ```cpp
  mtcout cout;
  cout << msg << std::endl;
  cout.write();
  ```

- 3. 我们有一个函数将始终自动调用，我们可以将 write 函数的代码放入其中。它是析构函数。在这种情况下，我们将创建和销毁合并为一行。此类对象的使用方式如下：
  
  ```cpp
  mtcout{} << msg << std::endl;
  ```

- 4. 现在，让我们定义我们的 `mtcout`（多线程 cout）类。它有一个公共默认构造函数。在私有部分，它有一个静态互斥变量。正如你可能回忆的那样，静态变量在类的所有实例之间共享。在析构函数中，我们锁定互斥锁并使用 cout 输出。添加前缀到输出——当前线程的 ID 和一个空格字符：

```
class mtcout : public std::ostringstream
{
public:
    mtcout() = default;
    ~mtcout()
    {
```

