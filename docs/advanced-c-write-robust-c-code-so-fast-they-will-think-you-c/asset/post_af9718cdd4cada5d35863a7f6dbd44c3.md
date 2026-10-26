
21. Declare the runAdd() function. Here, we create an infinite loop. In the loop, we are waiting for a condition variable. When the condition variable notifies, we take people from the m_CreatedPeople list and move them to the appropriate list, that is, m_PeopleInside, or m_PeopleInQueue if the limit has been exceeded. Then, we check if there are any people in the m_PeopleInQueue list and if m_PeopleInside is not full, we move them into this list:

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

22. Next, declare the runRemove() function, where we will remove visitors from m_PeopleInside. Here, also in the infinite loop, we are waiting for the m_CondVarRemovePerson condition variable. When it notifies the thread, we remove people from the list of visitors. Next, we will check if there's anybody in the m_PeopleInQueue list and if the limit is not exceeded, we add them to m_PeopleInside:

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

24. Compile and run the application. In the terminal, you will see logs from different threads about creating and moving people from one list to another. Your output will be similar to the following screenshot:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_717_0.png)

Figure 5.27: The result of the application's execution

As you can see, all the threads communicate with each other in a very easy and clean way. We protected our shared data by using mutexes so that we can avoid race conditions. Here, we used an exception to warn about the empty lists and caught them in the thread's functions so that our threads handle exceptions on their own. We also checked if the thread is joinable before joining it in the destructor. This allowed us to avoid the unexpected termination of the program. Thus, this small project demonstrates our skills when it comes to working with threads.

# Chapter 6 – Streams and I/O

## Activity 1 The Logging System for The Art Gallery Simulator

The thread-safe logger allows us to output data to the Terminal simultaneously. We implement this logger by inheriting from the std::ostringstream class and using a mutex for synchronization. We will implement a class that provides an interface for the formatted output and our logger will use it to extend the basic output. We define macro definitions for different logging levels to provide an interface that will be easy and clear to use. Follow these steps to complete this activity:

- 1. Open the project from Lesson6.
- 2. Create a new directory called logger inside the src/ directory. You will get the following hierarchy:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_718_0.png)

Figure 6.25: The hierarchy of the project

- 3. Create a header and source file called LoggerUtils. In LoggerUtils.hpp, add include guards. Include the <string> header to add support for working with strings. Define a namespace called logger and then define a nesting namespace called utils. In the utils namespace, declare the LoggerUtils class.

- 4. In the public section, declare the following static functions: getDateTime, getThreadId, getLoggingLevel, getFileAndLine, getFuncName, getInFuncName, and getOutFuncName. Your class should look as follows:

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

5. In LoggerUtils.cpp, add the required includes: the "LoggerUtils.hpp" header, <sstream> for std::stringstream support, and <ctime> for date and time support:

```
#include "LoggerUtils.hpp"
#include <sstream>
#include <ctime>
#include <thread>
```

6. Enter the logger and utils namespaces. Write the required function definitions. In the getDateTime() function, get the local time using the localtime() function. Format it into a string using the strftime() function. Convert it into the desired format using std::stringstream:

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

7. In the getThreadId() function, get the current thread ID and convert it into the desired format using std::stringstream:

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

8. In the getLoggingLevel() function, convert the given string into the desired format using std::stringstream:

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

9. In the getFileAndLine() function, convert the given file and line into the desired format using std::stringstream:

```
std::string LoggerUtils::getFileAndLine(const std::string& file, const int& line)
{
    std::stringstream ss;
    ss << " ";
    ss << file;
    ss << ":";
```
