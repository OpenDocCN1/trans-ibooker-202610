
还有一件重要的事情需要注意。如果你不想要线程的任何结果，而是希望等待线程完成其工作，你可以使用 **std::future<void>**。现在，当你调用 **future.get()** 时，当前线程将在此处等待。让我们看一个示例：

```
#include <iostream>
#include <future>

void toUppercase(const std::string& bufIn)
{
    std::string bufferOut;
    for (std::string::const_iterator iter = bufIn.begin(); iter != bufIn.end(); iter++)
    {
        if (*iter >= 97 && *iter <= 122)
        {
            bufferOut += static_cast<char>(static_cast<int>(*iter) - 32);
        }
        else
        {
            bufferOut += *iter;
        }
    }
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(2s);
    std::cout << bufferOut << std::endl;
}

int main()
{
    std::packaged_task<void(const std::string&)> task(toUppercase);
    auto futureResult = task.get_future();
    std::thread thr1(std::move(task), "please, make it uppercase");
```

使用 promise 获取结果的完整示例如下：

```
#include <iostream>
#include <future>

void toUppercase(const std::string& bufIn, std::promise<std::string> result)
{
    std::string bufferOut;
    for (std::string::const_iterator iter = bufIn.begin(); iter != bufIn.end(); iter++)
    {
        if (*iter >= 97 && *iter <= 122)
        {
            bufferOut += static_cast<char>(static_cast<int>(*iter) - 32);
        }
        else
        {
            bufferOut += *iter;
        }
    }

    result.set_value(bufferOut);
}

int main()
{
    std::promise<std::string> stringInUpper;
    std::future<std::string> futureRes = stringInUpper.get_future();
    std::thread thr(toUppercase, "please, make it uppercase", std::move(stringInUpper));
```

```
thr.detach();
std::cout << "Main thread is not blocked here" << std::endl;
futureRes.wait();

std::cout << "Result = " << futureRes.get() << std::endl;
return 0;
}
```

所以，我们几乎涵盖了编写多线程应用程序所需的所有内容，除了一件重要的事情——如果在单独的线程中抛出异常会发生什么？例如，你在线程中传递一个函数，它抛出了一个异常。在这种情况下，将为该线程调用 std::terminate()。其他线程将继续执行它们的工作。让我们考虑一个简单的示例。

我们有一个 getException() 函数，它生成一条包含线程 ID 的消息，并抛出 std::runtime_error：

```
#include <sstream>
#include <exception>
#include <iostream>
#include <future>

std::string getException()
{
    std::stringstream ss;
    ss << "Exception from thread: ";
    ss << std::this_thread::get_id();
    throw std::runtime_error(ss.str());
}
```

我们还有一个 toUppercase() 函数。它将给定的字符串转换为大写，并调用 getException() 函数，该函数会抛出异常：

```
std::string toUppercase(const std::string& bufIn)
{
    std::string bufferOut;
    for (std::string::const_iterator iter = bufIn.begin(); iter != bufIn.end(); iter++)
```

```
{
    if (*iter >= 97 && *iter <= 122)
    {
        bufferOut += static_cast<char>(static_cast<int>(*iter) - 32);
    }
    else
    {
        bufferOut += *iter;
        getException();
    }
}

    return bufferOut;
}
```

下面是 main() 函数，我们在 try-catch 块中创建了一个新线程 thr。我们捕获异常并将消息打印到终端：

```
int main()
{
    try
    {
        std::thread thr(toUppercase, "please, make it uppercase");
        thr.join();
    }
    catch(const std::exception& ex)
    {
        std::cout << "Caught an exception: " << ex.what() << std::endl;
    }
    return 0;
}
```

如果你在 IDE 中运行这段代码，你将看到以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_399_0.png)

图 5.19：示例执行的结果

我们可以看到，在抛出异常后调用了 std::terminate()。当你的程序中有很多线程时，很难找到线程被终止的正确位置。幸运的是，我们有几种机制可以从另一个线程捕获异常。让我们逐一考虑。

std::async 函数使用 future 结果将异常传递给调用线程。它将 std::exception_ptr 存储在 future 结果中，并设置 ready 标志。然后，当你调用 get() 时，std::future 会检查是否存储了任何 std::exception_ptr，并重新抛出异常。我们需要做的就是在 try-catch 块中放置一个 get() 调用。让我们看一个示例。我们将使用前面示例中的两个辅助函数，即 getException() 和 toUppercase()。它们保持不变。在 main() 函数中，我们创建一个名为 result 的 std::future 对象，并使用 std::async() 函数运行 toUppercase() 函数。然后，我们在 try-catch 块中调用 result 对象的 get() 函数并捕获异常：

```
#include <iostream>

#include <future>

int main()
{
    std::future<std::string> result = std::async(toUppercase, "please, make it uppercase");
    try
    {
        std::cout << "Future result = " << result.get() << std::endl;
    }
    catch(const std::exception& ex)
```

```
```cpp
{
    std::cout << "Caught an exception: " << ex.what() << std::endl;
}
return 0;
}
```
```

如果你在 IDE 中运行上述代码，你将获得以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_400_0.png)

图 5.20：示例执行的结果

正如你所看到的，我们捕获了一个异常，现在我们可以以某种方式处理它。**std::packaged_task<>** 类以同样的方式处理异常——它将 **std::exception_ptr** 存储在 future 结果中，设置 ready 标志，然后 **std::future** 在 **get()** 调用中重新抛出异常。让我们考虑一个小示例。我们将使用前面示例中的两个辅助函数——**getException()** 和 **toUppercase()**。它们保持不变。在 **main()** 函数中，我们创建一个名为 **task** 的 **std::packaged_task** 对象。通过使用我们的 **toUppercase()** 函数的类型，它返回一个整数并接受两个整数作为参数。我们将 **toUppercase()** 函数传递给 **task** 对象。然后，我们创建一个名为 **result** 的 **std::future** 对象，并使用 **get_future()** 函数从 task 对象获取结果。最后，我们在新线程 **thr** 中运行 task 对象，并在 **try-catch** 块中调用 **result** 变量的 **get()** 函数：

```
```cpp
#include <iostream>

#include <future>

int main()
{
    std::packaged_task<std::string(const std::string&)> task(toUppercase);
    auto result = task.get_future();
    std::thread thr(std::move(task), "please, make it uppercase");
    thr.detach();
```

```
try
{
    std::cout << "Future result = " << result.get() << std::endl;
}
catch(const std::exception& ex)
{
    std::cout << "Caught an exception: " << ex.what() << std::endl;
}
return 0;
}
```

如果你在 IDE 中运行这段代码，你将获得以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_401_0.png)

图 5.21：此示例执行的结果

**std::promise<>** 类以另一种方式处理异常。它允许我们使用 set_exception() 或 set_exception_at_thread_exit() 函数手动存储异常。要在 std::promise 中设置异常，我们必须捕获它。如果我们没有捕获异常，在 std::promise 的析构函数中，将在 future 结果中设置一个错误，即 std::future_errc::broken_promise。当你调用 get() 函数时，将重新抛出异常。让我们考虑一个示例。我们将使用前面示例中的辅助函数——getException()。它保持不变。但是，我们将更改 toUppercase() 函数并添加第三个参数 std::promise。现在，我们将在 try 块中调用 getException() 函数，捕获异常，并将其设置到 std::promise 值中：

```
void toUppercase(const std::string& bufIn, std::promise<std::string> result)
{
    std::string bufferOut;
    try
    {
```

