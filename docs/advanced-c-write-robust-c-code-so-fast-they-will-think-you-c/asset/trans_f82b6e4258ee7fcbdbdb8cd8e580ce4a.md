

21. 声明 runAdd() 函数。在这里，我们创建了一个无限循环。在循环中，我们正在等待一个条件变量。当条件变量发出通知时，我们从 m_CreatedPeople 列表中取出人员并将它们移动到适当的列表中，即 m_PeopleInside，或者如果超过了限制则移动到 m_PeopleInQueue。然后，我们检查 m_PeopleInQueue 列表中是否有人员，并且如果 m_PeopleInside 未满，则将它们移入该列表：

```
void Watchman::runAdd()
{
    while (true)
    {
        std::unique_lock<std::mutex> locker(m_AddMux);
        while(!m_AddNotified)
        {
            std::cerr << "Watchman | runAdd | false awakening" <<
            std::endl;
            m_CondVarAddPerson.wait(locker);
        }
        std::cout << "Watchman | runAdd | new person came" << std::endl;
        m_AddNotified = false;
        while (m_CreatedPeople.size() > 0)
        {
            try
            {
                auto person = m_CreatedPeople.get();
                if (m_PeopleInside.size() < CountPeopleInside)
                {
                    std::cout << "Watchman | runAdd | welcome in our The Art Gallery" << std::endl;
                    m_PeopleInside.add(std::move(person));
                }
                else
                {
                    std::cout << "Watchman | runAdd | Sorry, we are full. Please wait" << std::endl;
                    m_PeopleInQueue.add(std::move(person));
                }
            }
            catch(const std::string& e)
            {
                std::cout << e << std::endl;
            }
        }
        std::cout << "Watchman | runAdd | check people in queue" << std::endl;
        if (m_PeopleInQueue.size() > 0)
        {
            while (m_PeopleInside.size() < CountPeopleInside)
            {
                try
                {
                    auto person = m_PeopleInQueue.get();
                    std::cout << "Watchman | runAdd | welcome in our The Art Gallery" << std::endl;
                    m_PeopleInside.add(std::move(person));
                }
                catch(const std::string& e)
                {
                    std::cout << e << std::endl;
                }
            }
        }
    }
}
```

22. 接下来，声明 runRemove() 函数，在其中我们将从 m_PeopleInside 中移除访客。在这里，也在无限循环中，我们等待 m_CondVarRemovePerson 条件变量。当它通知线程时，我们从访客列表中移除人员。接下来，我们将检查 m_PeopleInQueue 列表中是否有任何人，并且如果未超过限制，则将他们添加到 m_PeopleInside：

```
void Watchman::runRemove()
{
    while (true)
    {
        std::unique_lock<std::mutex> locker(m_RemoveMux);
        while(!m_RemoveNotified)
        {
            std::cerr << "Watchman | runRemove | false awakening" << std::endl;
            m_CondVarRemovePerson.wait(locker);
        }
    }
```

```
std::mutex g_AddMux;
std::mutex g_RemoveMux;
bool g_AddNotified = false;;
bool g_RemoveNotified = false;
Persons g_PeopleInside;
Persons g_PeopleInqueue;
Persons g_CreatedPersons;
PersonGenerator generator(g_CreatedPersons, g_CondVarAddPerson, g_CondVarRemovePerson, g_AddMux, g_RemoveMux, g_AddNotified, g_RemoveNotified);
Watchman watchman(g_CondVarAddPerson, g_CondVarRemovePerson, g_AddMux, g_RemoveMux, g_AddNotified, g_RemoveNotified, g_PeopleInside, g_PeopleInqueue, g_CreatedPersons);
}

char a;
std::cin >> a;
return 0;
}
```

24. 编译并运行应用程序。在终端中，你将看到来自不同线程的日志，关于创建人员以及人员从一个列表移动到另一个列表。你的输出将类似于以下截图：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_717_0.png)

图 5.27：应用程序执行的結果

正如你所见，所有线程以非常简单和清晰的方式相互通信。我们通过使用互斥锁保护共享数据，以避免竞态条件。在这里，我们使用异常来警告空列表，并在线程的函数中捕获它们，以便我们的线程自行处理异常。我们还在析构函数中加入线程之前检查线程是否可加入。这使我们能够避免程序的意外终止。因此，这个小型项目展示了我们在处理线程方面的技能。

# 第 6 章 – 流和 I/O

## 活动 1 The Art Gallery 模拟器的日志系统

线程安全的记录器允许我们同时向终端输出数据。我们通过继承 std::ostringstream 类并使用互斥锁进行同步来实现此记录器。我们将实现一个类，为格式化输出提供接口，我们的记录器将使用它来扩展基本输出。我们为不同的日志级别定义宏定义，以提供易于且清晰使用的接口。按照以下步骤完成此活动：

- 1. 打开 Lesson6 中的项目。
- 2. 在 src/ 目录内创建一个名为 logger 的新目录。你将得到以下层次结构：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_718_0.png)

图 6.25：项目的层次结构

- 3. 创建一个名为 LoggerUtils 的头文件和源文件。在 LoggerUtils.hpp 中，添加包含保护。包含 <string> 头文件以添加对字符串操作的支持。定义一个名为 logger 的命名空间，然后在其中定义一个名为 utils 的嵌套命名空间。在 utils 命名空间中，声明 LoggerUtils 类。

- 4. 在 public 部分，声明以下静态函数：getDateTime、getThreadId、getLoggingLevel、getFileAndLine、getFuncName、getInFuncName 和 getOutFuncName。你的类应如下所示：

```
#ifndef LOGGERUTILS_HPP_
#define LOGGERUTILS_HPP_

#include <string>
```

```
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

5. 在 LoggerUtils.cpp 中，添加所需的包含： "LoggerUtils.hpp" 头文件、<sstream> 用于 std::stringstream 支持，以及 <ctime> 用于日期和时间支持：

```
#include "LoggerUtils.hpp"
#include <sstream>
#include <ctime>
#include <thread>
```

6. 进入 logger 和 utils 命名空间。编写所需的函数定义。在 getDateTime() 函数中，使用 localtime() 函数获取本地时间。使用 strftime() 函数将其格式化为字符串。使用 std::stringstream 将其转换为所需格式：

```
std::string LoggerUtils::getDateTime()
{
    time_t rawtime;
    struct tm * timeinfo;
    char buffer[80];

    time (&rawtime);
    timeinfo = localtime(&rawtime);

strftime(buffer,sizeof(buffer),"%d-%m-%Y T%H:%M:%S",timeinfo);
std::stringstream ss;
ss << "[";
ss << buffer;
ss << "]";
return ss.str();
}
```

7. 在 getThreadId() 函数中，获取当前线程 ID 并使用 std::stringstream 将其转换为所需格式：

```
std::string LoggerUtils::getThreadId()
{
    std::stringstream ss;
    ss << "[";
    ss << std::this_thread::get_id();
    ss << "]";
    return ss.str();
}
```

8. 在 getLoggingLevel() 函数中，使用 std::stringstream 将给定的字符串转换为所需格式：

```
std::string LoggerUtils::getLoggingLevel(const std::string& level)
{
    std::stringstream ss;
    ss << "[";
    ss << level;
    ss << "]";
    return ss.str();
}
```

9. 在 getFileAndLine() 函数中，使用 std::stringstream 将给定的文件和行号转换为所需格式：

```
std::string LoggerUtils::getFileAndLine(const std::string& file, const int& line)
{
    std::stringstream ss;
    ss << " ";
    ss << file;
    ss << ":";
```

