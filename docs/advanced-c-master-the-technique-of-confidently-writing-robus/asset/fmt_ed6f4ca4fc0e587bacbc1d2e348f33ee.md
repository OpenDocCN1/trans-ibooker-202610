
## 处理多线程应用程序中的异常

```cpp
bufferOut += static_cast<char>(static_cast<int>(*iter) - 32);
            }
            else
            {
                bufferOut += *iter;
                getException();
            }
        }
    }
    catch(const std::exception& ex)
    {
        result.set_exception(std::make_exception_ptr(ex));
    }
    result.set_value(bufferOut);
}
```

### Note

有几种方法可以将异常设置给 promise。首先，我们可以捕获 `std::exception` 并使用 `std::make_exception_ptr()` 函数将其转换为 `std::exception_ptr`。也可以使用 `std::current_exception()` 函数，它返回 `std::exception_ptr` 对象。

在 `main()` 函数中，我们创建一个整数类型的 promise，称为 `upperResult`。我们创建一个名为 `futureRes` 的 future 结果，并从 `upperResult` promise 的值设置它。接下来，我们创建一个新线程 `thr`，将 `toUppercase()` 函数传递给它，并移动 `upperResult` promise。然后，我们调用 `futureRes` 对象的 `wait()` 函数，这会使调用线程等待直到结果可用。接着，在 `try-catch` 块中，我们调用 `futureRes` 对象的 `get()` 函数，它会重新抛出异常：

```cpp
#include <iostream>
#include <future>

int main()
{
    std::promise<std::string> upperResult;
    std::future<std::string> futureRes = upperResult.get_future();
    std::thread thr(toUppercase, "please, make it uppercase", std::move(upperResult));
    thr.detach();
    futureRes.wait();
    try
    {
        std::cout << "Result = " << futureRes.get() << std::endl;
    }
    catch(...)
    {
        std::cout << "Caught an exception" << std::endl;
    }
    return 0;
}
```

### Note

当我们创建 `std::promise<>` 对象时，我们承诺将强制设置值或异常。如果我们不执行其中任何操作，`std::promise` 的析构函数将抛出异常，即 `std::future_error – std::future_errc::broken_promise`。

如果在 IDE 中运行此代码，将获得以下输出：

![Figure 5.22: The result of this example's execution](image/C14583_05_22.jpg)

**Figure 5.22: The result of this example's execution**

这就是多线程应用程序中异常处理的全部内容。正如你所见，这与单线程中的做法非常相似。现在，让我们将所学知识付诸实践，编写一个简单的应用程序示例，演示如何使用不同的 future 结果进行同步。

## Exercise 5: Synchronization with Future Results

在本练习中，我们将编写一个简单的应用程序，演示如何使用 future 结果从单独的线程接收值。我们将运行 `ToUppercase()` 可调用对象三次。我们将使用 `std::async()` 函数执行第一个任务，使用 `std::packaged_task<>` 模板类执行第二个任务，使用 `std::thread` 和 `std::promise` 执行最后一个任务。

执行以下步骤完成此练习：

1.  包含线程支持的头文件 `<thread>`、流支持的头文件 `<iostream>` 以及 future 结果支持的头文件 `<future>`：

```cpp
#include <iostream>
#include <thread>
#include <future>
```

2.  实现一个 `ToUppercase` 类，将给定字符串转换为大写。它有两个重载的 `operator()`。第一个 `operator()` 接受要转换的字符串并返回大写的结果值。第二个 `operator()` 接受要转换的字符串和一个 `std::promise`，并将返回值存储在 promise 中：

```cpp
class ToUppercase
{
public:
    std::string operator()(const std::string& bufIn)
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
        return bufferOut;
    }
    void operator()(const std::string& bufIn, std::promise<std::string> result)
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
};
```

3.  现在，创建一个 `ToUppercase` 对象 `ptConverter`，并创建一个 `std::packaged_task` `upperCaseResult1`，它接受 `ptConverter` 对象作为参数。创建一个 `std::future` 值并从 `upperCaseResult1` 设置它。在单独的线程中运行此任务：

```cpp
ToUppercase ptConverter;
std::packaged_task<std::string(const std::string&)> upperCaseResult1(ptConverter);
std::future<std::string> futureUpperResult1 = upperCaseResult1.get_future();
std::thread thr1(std::move(ptConverter), "This is a string for the first asynchronous task");
thr1.detach();
```

4.  现在，创建第二个 `ToUppercase` 对象 `fConverter`。创建一个名为 `futureUpperResult2` 的 `std::future` 对象并从 `std::async()` 设置它：

```cpp
ToUppercase fConverter;
std::future<std::string> futureUpperResult2 = std::async(fConverter, "This is a string for the asynchronous task");
```

5.  现在，创建第三个 `ToUppercase` 对象 `pConverter`。创建一个名为 `promiseResult` 的 `std::promise` 值。然后，创建一个名为 `futureUpperResult3` 的 `std::future` 值并从 `promiseResult` 设置它。现在，在单独的线程中运行 `pConverter` 任务并将 `promiseResult` 作为参数传递：

```cpp
ToUppercase pConverter;
std::promise<std::string> promiseResult;
std::future<std::string> futureUpperResult3 = promiseResult.get_future();
std::thread thr2(pConverter, "This is a string for the task that returns a promise", std::move(promiseResult));
thr2.detach();
```

6.  现在，为了从所有线程接收结果，等待 `futureUpperResult3` 准备就绪，然后获取所有三个结果并打印它们：

```cpp
futureUpperResult3.wait();
std::cout << "Converted strings: "
          << futureUpperResult1.get() << std::endl
          << futureUpperResult2.get() << std::endl
          << futureUpperResult3.get() << std::endl;
```

7.  在编辑器中运行此代码。你将看到来自所有三个线程的转换后的字符串。

    你将获得以下输出：

![Figure 5.23: The result of this exercise's execution](image/C14583_05_23.jpg)

**Figure 5.23: The result of this exercise's execution**

那么，我们在这里做了什么？我们将大计算拆分为较小的部分并在单独的线程中运行它们。对于长时间计算，这将提高性能。在本练习中，我们学习了如何从线程接收结果。在本节中，我们还学习了如何将在单独线程中抛出的异常传递给调用线程。我们还学习了如何通过事件同步多个线程的工作，不仅使用条件变量，还使用 future 结果。

## Activity 1: Creating a Simulator to Model the Work of the Art Gallery
