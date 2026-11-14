
在下一节中，我们将探讨用于同步的 C++ 线程库原语以及死锁的另一个原因。

## 多线程闭包的移动语义

`std::thread` 类不可复制，但如果我们想存储几个线程，或者可能是 10 个或 20 个怎么办？当然，我们可以创建相应数量的线程，然后像这样 join 或 detach 它们：

```cpp
std::thread thr1(someFunc);
std::thread thr2(someFunc);
std::thread thr3(someFunc);
std::thread thr4(someFunc);
std::thread thr5(someFunc);

thr1.join();
thr2.join();
thr3.join();
thr4.join();
thr5.join();
```

但将一组线程存储在 `STL container`（例如线程向量）中会更方便：

```cpp
std::vector<std::thread> threads;
```

`STL containers` 无法用于不支持**复制语义**的对象。多亏了**移动语义**，我们可以将具有移动构造函数和移动赋值运算符的不可复制对象存储到容器中。然后，我们可以配合 `std::move()` 函数使用线程向量。要初始化容器中的线程，我们可以执行类似以下的操作：

```cpp
for (int i = 0; i < 10; i++) {
    auto t = std::thread([i]() {
        std::cout << "thread: " << i << "\n";
    });
    threads.push_back(std::move(t));
}
```

然后，我们可以 join 或 detach 它们所有：

```cpp
for (auto& thr : threads) {
    if (thr.joinable()) {
        thr.join();
    }
}
```

当我们将 `std::thread` 对象作为类成员存储时，移动语义也很有用。在这种情况下，我们应该仔细设计类，删除复制构造函数和赋值运算符，并实现新的移动构造函数和移动赋值运算符。让我们考虑这样一个类的代码示例：

```cpp
class Handler {
    std::thread threadHandler;

public:
    Handler(const Handler&) = delete;
    Handler& operator=(const Handler&) = delete;

    Handler(Handler&& obj)
        : threadHandler(std::move(obj.threadHandler)) {}

    Handler& operator=(Handler&& obj) {
        if (threadHandler.joinable()) {
            threadHandler.join();
        }
        threadHandler = std::move(obj.threadHandler);
        return *this;
    }

    ~Handler() {
        if (threadHandler.joinable()) {
            threadHandler.join();
        }
    }
};
```

在移动赋值运算符中，我们首先检查线程是否可 join。如果是，我们先 join 它，然后才执行赋值操作。

### 注意

我们永远不应该在不使用 `join()` 或 `detach()` 的情况下将一个线程对象赋值给另一个线程对象。这将导致调用 `std::terminate()` 函数。

也可以使用 `std::move()` 函数将对象移动到线程函数中。这对于处理大对象很有帮助，因为复制它们是不建议的。让我们做一个练习，以确保对象可以被移动到线程函数中。

## 练习 3：将对象移动到线程函数

在本练习中，我们将编写一个简单的应用程序，演示 `std::move()` 如何为 `std::thread` 类工作。我们将创建一个同时拥有复制构造函数和移动构造函数的类，以查看当我们将该类的对象移动到 `std::thread` 函数时会调用哪一个。执行以下步骤以完成此练习：

1.  包含线程支持的头文件 `<thread>` 和流支持的头文件 `<iostream>`：

    ```cpp
    #include <iostream>
    #include <thread>
    ```

2.  实现 `Handler` 类，它拥有默认构造函数、析构函数、复制构造函数、赋值运算符、移动构造函数和移动赋值运算符。它们除了打印日志外不做任何事：

    ```cpp
    class Handler {
    public:
        Handler() {
            std::cout << "Handler()" << std::endl;
        }

        Handler(const Handler&) {
            std::cout << "Handler(const Handler&)" << std::endl;
        }

        Handler& operator=(const Handler&) {
            std::cout << "Handler& operator=(const Handler&)" << std::endl;
            return *this;
        }

        Handler(Handler&& obj) {
            std::cout << "Handler(Handler && obj)" << std::endl;
        }

        Handler& operator=(Handler&& obj) {
            std::cout << "Handler & operator=(Handler && obj)" << std::endl;
            return *this;
        }

        ~Handler() {
            std::cout << "~Handler()" << std::endl;
        }
    };
    ```

3.  实现 `doSomeJob()` 函数，它实际上在这里什么都不做，只是打印一条日志消息：

    ```cpp
    void doSomeJob(Handler&& h) {
        std::cout << "I'm here" << std::endl;
    }
    ```

4.  进入 `main()` 函数并创建一个 `Handler` 类型的 `handler` 变量。创建 `thr1`，传递 `doSomeJob()` 函数，并移动 handler 变量：

    ```cpp
    Handler handler;
    std::thread thr1(doSomeJob, std::move(handler));
    ```

5.  分离 `thr1` 线程，并为主线程添加一小段睡眠时间以避免应用程序关闭。我们将能够看到来自分离线程的输出：

    ```cpp
    thr1.detach();
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(5s);
    ```

6.  在编辑器中运行此代码。在终端日志中，从默认构造函数开始，你将看到来自移动运算符的两条日志、一条来自析构函数的日志、来自 `doSomeJob()` 函数的消息，最后是来自析构函数的另外两条日志消息。我们可以看到移动构造函数被调用了两次。

    你将获得以下输出：

    ![](image/C14583_05_16.jpg)

    **图 5.16：** 练习执行的结果

正如你所见，`Handler` 对象被移动到了线程函数中。尽管如此，所有未使用 `std::ref()` 函数传递的参数都被复制到了线程的内存中。

让我们考虑一个有趣的问题。正如你可能记得的，当我们初始化 `std::thread` 时，所有构造函数参数都会被复制到线程内存中，包括可调用对象——lambda、函数或 `std::function`。但如果我们的可调用对象不支持复制语义怎么办？例如，我们创建了一个只有移动构造函数和移动赋值运算符的类：

```cpp
class Converter {
public:
    Converter(Converter&&) {}
    Converter& operator=(Converter&&) { return *this; }
    Converter() = default;
    Converter(const Converter&) = delete;
    Converter& operator=(const Converter&) = delete;
    void operator()(const std::string&) {
        // do nothing
    }
};
```

我们如何将其传递给线程构造函数？如果我们直接原样传递，会得到编译器错误；例如：

```cpp
int main() {
    Converter convert;
    std::thread convertThread(convert, "convert me");
    convertThread.join();
    return 0;
}
```

你将获得以下输出：

![](image/C14583_05_17.jpg)

**图 5.17：** 编译错误示例

这里有很多奇怪的错误。为了修复此问题，我们可以使用 `std::move()` 函数来移动可调用对象：

```cpp
std::thread convertThread(std::move(convert), "convert me");
```

现在，一切正常——代码编译通过并完全按我们想要的方式运行。

现在，让我们再考虑一个有趣的例子。例如，你有一个需要捕获不可复制对象（例如 `unique_ptr`）的 lambda 函数：

```cpp
auto unique = std::make_unique<Converter>();
```

从 C++14 开始，我们可以使用 `std::move()` 来捕获可移动对象。因此，要捕获 unique pointer，我们可以使用以下代码：

```cpp
std::thread convertThread([unique = std::move(unique)] {
    unique->operator()("convert me");
});
```

正如你所见，使用 `std::move` 在 lambda 中按值捕获非常有用。当我们不想复制某些对象（因为它们复制起来可能需要很长时间）时，这也很有用。
