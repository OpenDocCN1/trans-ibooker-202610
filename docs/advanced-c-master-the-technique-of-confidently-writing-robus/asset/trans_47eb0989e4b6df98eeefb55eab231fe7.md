

```cpp
char a;

std::cin >> a;

return 0;

}
```

24. 编译并运行应用程序。在终端中，你将看到来自不同线程的日志，内容涉及创建人员并将其从一个列表移动到另一个列表。你的输出将类似于以下屏幕截图：

![图 5.27：应用程序执行的结果](image/C14583_05_27.jpg)

*图 5.27：应用程序执行的结果*

正如你所见，所有线程都以一种非常简单且清晰的方式相互通信。我们通过使用 `mutexes` 保护了共享数据，从而避免了竞争条件。在这里，我们使用异常来警告空列表，并在线程函数中捕获它们，以便我们的线程自行处理异常。我们还在析构函数中连接线程之前检查了线程是否可连接。这使我们能够避免程序意外终止。因此，这个小项目展示了我们在处理线程方面的技能。

## 第 6 章 – 流与 I/O

### 活动 1：艺术画廊模拟器的日志系统

线程安全的记录器允许我们同时向终端输出数据。我们通过继承 `std::ostringstream` 类并使用 `mutex` 进行同步来实现此记录器。我们将实现一个提供格式化输出接口的类，我们的记录器将使用它来扩展基本输出。我们为不同的日志级别定义宏定义，以提供一个易于使用且清晰的接口。请按照以下步骤完成此活动：

1.  打开 `Lesson6` 中的项目。

2.  在 `src/` 目录内创建一个名为 `logger` 的新目录。你将获得以下层级结构：

    ![图 6.25：项目的层级结构](image/C14583_06_25.jpg)

    *图 6.25：项目的层级结构*

3.  创建一个名为 `LoggerUtils` 的头文件和源文件。在 `LoggerUtils.hpp` 中，添加包含保护符。包含 `<string>` 头文件以添加对字符串操作的支持。定义一个名为 `logger` 的命名空间，然后定义一个嵌套命名空间 `utils`。在 `utils` 命名空间中，声明 `LoggerUtils` 类。

4.  在公共部分，声明以下静态函数：`getDateTime`、`getThreadId`、`getLoggingLevel`、`getFileAndLine`、`getFuncName`、`getInFuncName` 和 `getOutFuncName`。你的类应如下所示：

    ```cpp
    #ifndef LOGGERUTILS_HPP_
    #define LOGGERUTILS_HPP_

    #include <string>

    namespace logger
    {

    namespace utils
    {

    class LoggerUtils
    {

    public:

        static std::string getDateTime();

        static std::string getThreadId();

        static std::string getLoggingLevel(const std::string& level);

        static std::string getFileAndLine(const std::string& file, const int& line);

        static std::string getFuncName(const std::string& func);

        static std::string getInFuncName(const std::string& func);

        static std::string getOutFuncName(const std::string& func);

    };

    } // namespace utils

    } // namespace logger

    #endif /* LOGGERUTILS_HPP_ */
    ```

5.  在 `LoggerUtils.cpp` 中，添加所需的包含：`LoggerUtils.hpp` 头文件、用于 `std::stringstream` 支持的 `<sstream>` 以及用于日期和时间支持的 `<ctime>`：

    ```cpp
    #include "LoggerUtils.hpp"

    #include <sstream>

    #include <ctime>

    #include <thread>
    ```

6.  进入 `logger` 和 `utils` 命名空间。编写所需的函数定义。在 `getDateTime()` 函数中，使用 `localtime()` 函数获取本地时间。使用 `strftime()` 函数将其格式化为字符串。使用 `std::stringstream` 将其转换为所需格式：

    ```cpp
    std::string LoggerUtils::getDateTime()
    {

        time_t rawtime;

        struct tm * timeinfo;

        char buffer[80];

        time (&rawtime);

        timeinfo = localtime(&rawtime);

        strftime(buffer,sizeof(buffer),"%d-%m-%YT%H:%M:%S",timeinfo);

        std::stringstream ss;

        ss << "[";

        ss << buffer;

        ss << "]";

        return ss.str();

    }
    ```

7.  在 `getThreadId()` 函数中，获取当前线程 ID 并使用 `std::stringstream` 将其转换为所需格式：

    ```cpp
    std::string LoggerUtils::getThreadId()
    {

        std::stringstream ss;

        ss << "[";

        ss << std::this_thread::get_id();

        ss << "]";

        return ss.str();

    }
    ```

8.  在 `getLoggingLevel()` 函数中，使用 `std::stringstream` 将给定的字符串转换为所需格式：

    ```cpp
    std::string LoggerUtils::getLoggingLevel(const std::string& level)
    {

        std::stringstream ss;

        ss << "[";

        ss << level;

        ss << "]";

        return ss.str();

    }
    ```

9.  在 `getFileAndLine()` 函数中，使用 `std::stringstream` 将给定的文件和行转换为所需格式：

    ```cpp
    std::string LoggerUtils::getFileAndLine(const std::string& file, const int& line)
    {

        std::stringstream ss;

        ss << " ";

        ss << file;

        ss << ":";

        ss << line;

        ss << ":";

        return ss.str();

    }
    ```

10. 在 `getFuncName()` 函数中，使用 `std::stringstream` 将函数名转换为所需格式：

    ```cpp
    std::string LoggerUtils::getFuncName(const std::string& func)
    {

        std::stringstream ss;

        ss << " --- ";

        ss << func;

        ss << "()";

        return ss.str();

    }
    ```

11. 在 `getInFuncName()` 函数中，使用 `std::stringstream` 将函数名转换为所需格式。

    ```cpp
    std::string LoggerUtils::getInFuncName(const std::string& func)
    {

        std::stringstream ss;

        ss << " --> ";

        ss << func;

        ss << "()";

        return ss.str();

    }
    ```

12. 在 `getOutFuncName()` 函数中，使用 `std::stringstream` 将函数名转换为所需格式：

    ```cpp
    std::string LoggerUtils::getOutFuncName(const std::string& func)
    {

        std::stringstream ss;

        ss << " <-- ";

        ss << func;

        ss << "()";

        return ss.str();

    }
    ```

13. 创建一个名为 `LoggerMacroses.hpp` 的头文件。添加包含保护符。为每个 `LoggerUtils` 函数创建宏定义：`DATETIME` 对应 `getDateTime()` 函数，`THREAD_ID` 对应 `getThreadId()` 函数，`LOG_LEVEL` 对应 `getLoggingLevel()` 函数，`FILE_LINE` 对应 `getFileAndLine()` 函数，`FUNC_NAME` 对应 `getFuncName()` 函数，`FUNC_ENTRY_NAME` 对应 `getInFuncName()` 函数，`FUNC_EXIT_NAME` 对应 `getOutFuncName()` 函数。结果头文件应如下所示：

    ```cpp
    #ifndef LOGGERMACROSES_HPP_
    #define LOGGERMACROSES_HPP_

    #define DATETIME \
        logger::utils::LoggerUtils::getDateTime()

    #define THREAD_ID \
        logger::utils::LoggerUtils::getThreadId()

    #define LOG_LEVEL( level ) \
        logger::utils::LoggerUtils::getLoggingLevel(level)

    #define FILE_LINE \
        logger::utils::LoggerUtils::getFileAndLine(__FILE__, __LINE__)

    #define FUNC_NAME \
        logger::utils::LoggerUtils::getFuncName(__FUNCTION__)

    #define FUNC_ENTRY_NAME \
        logger::utils::LoggerUtils::getInFuncName(__FUNCTION__)

    #define FUNC_EXIT_NAME \
        logger::utils::LoggerUtils::getOutFuncName(__FUNCTION__)

    #endif /* LOGGERMACROSES_HPP_ */
    ```

14. 创建一个名为 `StreamLogger` 的头文件和源文件。在 `StreamLogger.hpp` 中，添加所需的包含保护符。包含 `LoggerMacroses.hpp` 和 `LoggerUtils.hpp` 头文件。然后，包含用于 `std::ostringstream` 支持的 `<sstream>` 头文件、用于 `std::thread` 支持的 `<thread>` 头文件以及用于 `std::mutex` 支持的 `<mutex>` 头文件：

