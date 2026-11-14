
```cpp
char a;

std::cin >> a;

return 0;

}
```

24. Compile and run the application. In the terminal, you will see logs from different threads about creating and moving people from one list to another. Your output will be similar to the following screenshot:

![Figure 5.27: The result of the application's execution](image/C14583_05_27.jpg)

*Figure 5.27: The result of the application's execution*

As you can see, all the threads communicate with each other in a very easy and clean way. We protected our shared data by using `mutexes` so that we can avoid race conditions. Here, we used an exception to warn about the empty lists and caught them in the thread's functions so that our threads handle exceptions on their own. We also checked if the thread is joinable before joining it in the destructor. This allowed us to avoid the unexpected termination of the program. Thus, this small project demonstrates our skills when it comes to working with threads.

## Chapter 6 – Streams and I/O

### Activity 1: The Logging System for The Art Gallery Simulator

The thread-safe logger allows us to output data to the Terminal simultaneously. We implement this logger by inheriting from the `std::ostringstream` class and using a `mutex` for synchronization. We will implement a class that provides an interface for the formatted output and our logger will use it to extend the basic output. We define macro definitions for different logging levels to provide an interface that will be easy and clear to use. Follow these steps to complete this activity:

1.  Open the project from `Lesson6`.

2.  Create a new directory called `logger` inside the `src/` directory. You will get the following hierarchy:

    ![Figure 6.25: The hierarchy of the project](image/C14583_06_25.jpg)

    *Figure 6.25: The hierarchy of the project*

3.  Create a header and source file called `LoggerUtils`. In `LoggerUtils.hpp`, add include guards. Include the `<string>` header to add support for working with strings. Define a namespace called `logger` and then define a nesting namespace called `utils`. In the `utils` namespace, declare the `LoggerUtils` class.

4.  In the public section, declare the following static functions: `getDateTime`, `getThreadId`, `getLoggingLevel`, `getFileAndLine`, `getFuncName`, `getInFuncName`, and `getOutFuncName`. Your class should look as follows:

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

5.  In `LoggerUtils.cpp`, add the required includes: the `LoggerUtils.hpp` header, `<sstream>` for `std::stringstream` support, and `<ctime>` for date and time support:

    ```cpp
    #include "LoggerUtils.hpp"

    #include <sstream>

    #include <ctime>

    #include <thread>
    ```

6.  Enter the `logger` and `utils` namespaces. Write the required function definitions. In the `getDateTime()` function, get the local time using the `localtime()` function. Format it into a string using the `strftime()` function. Convert it into the desired format using `std::stringstream`:

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

7.  In the `getThreadId()` function, get the current thread ID and convert it into the desired format using `std::stringstream`:

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

8.  In the `getLoggingLevel()` function, convert the given string into the desired format using `std::stringstream`:

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

9.  In the `getFileAndLine()` function, convert the given file and line into the desired format using `std::stringstream`:

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

10. In the `getFuncName()` function, convert the function name into the desired format using `std::stringstream`:

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

11. In the `getInFuncName()` function convert the function name to the desired format using `std::stringstream`.

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

12. In the `getOutFuncName()` function, convert the function name into the desired format using `std::stringstream`:

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

13. Create a header file called `LoggerMacroses.hpp`. Add include guards. Create macro definitions for each `LoggerUtils` function: `DATETIME` for the `getDateTime()` function, `THREAD_ID` for the `getThreadId()` function, `LOG_LEVEL` for the `getLoggingLevel()` function, `FILE_LINE` for the `getFileAndLine()` function, `FUNC_NAME` for the `getFuncName()` function, `FUNC_ENTRY_NAME` for the `getInFuncName()` function, and `FUNC_EXIT_NAME` for the `getOutFuncName()` function. As a result, the header file should look as follows:

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

14. Create a header and source file called `StreamLogger`. In `StreamLogger.hpp`, add the required include guards. Include the `LoggerMacroses.hpp` and `LoggerUtils.hpp` header files. Then, include the `<sstream>` header for `std::ostringstream` support, the `<thread>` header for `std::thread` support, and the `<mutex>` header for `std::mutex` support:
