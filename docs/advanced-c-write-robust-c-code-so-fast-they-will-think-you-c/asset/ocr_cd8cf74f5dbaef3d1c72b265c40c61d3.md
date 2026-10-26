15. Enter the namespace logger. Declare the StreamLogger class, which inherits from the std::ostringstream class. This inheritance allows us to use an overloaded left shift operator, <<, for logging. We don't set the output device, so the output will not be performed – just stored in the internal buffer. In the private section, declare a static std::mutex variable called m_mux. Declare constant strings so that you can store the logging level, file and line, and function name. In the public section, declare a constructor that takes the logging level, file and line, and function name as parameters. Declare a class destructor. The class declaration should look like as follows:

```
namespace logger
{
class StreamLogger : public std::ostringstream
{
public:
    StreamLogger(const std::string logLevel,
                 const std::string fileLine,
                 const std::string funcName);
    ~StreamLogger();
private:
    static std::mutex m_mux;
    const std::string m_logLevel;
    const std::string m_fileLine;
    const std::string m_funcName;
};
} // namespace logger
```

16. In StreamLogger.cpp, include the StreamLogger.hpp and <iostream> headers for std::cout support. Enter the logger namespace. Define the constructor and initialize all the members in the initializer list. Then, define the destructor and enter its scope. Lock the m_mux mutex. If the internal buffer is empty, output only the date and time, thread ID, logging level, file and line, and the function name. As a result, we will get the line in the following format: [dateTime][threadId][logLevel][file:line: ][name() --- ]. If the internal buffer contains any data, output the same string with the buffer at the end. As a result, we will get the line in the following format: [dateTime][threadId][logLevel][file:line: ][name() --- ] | message. The complete source file should look as follows:

```
#include "StreamLogger.hpp"
#include <iostream>

std::mutex logger::StreamLogger::m_mux;
```