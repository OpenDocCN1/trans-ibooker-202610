```
thr.detach();
std::cout << "Main thread is not blocked here" << std::endl;
futureRes.wait();

std::cout << "Result = " << futureRes.get() << std::endl;
return 0;
}
```

So, we covered almost everything that's required to write multithreaded applications, except one important thing – what would happen if an exception is thrown in the separate thread? For example, you pass a function in the thread and it throws an exception. In this case, std::terminate() will be called for this thread. Other threads will continue their job. Let's consider a simple example.

We have a getException() function that generates a message with a thread ID and throws std::runtime_error:

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

We also have the toUppercase() function. Which converts the given string into uppercase and calls the getException() function, which throws an exception:

```
std::string toUppercase(const std::string& bufIn)
{
    std::string bufferOut;
    for (std::string::const_iterator iter = bufIn.begin(); iter != bufIn.end(); iter++)
```