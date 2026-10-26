
There is one more important thing to take note of. If you don't want any results from the thread and would prefer to wait until the thread finishes its work, you can use **std::future<void>**. Now, when you call **future.get()**, your current thread will wait at this point. Let's consider an example:

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

The complete example of getting the result using promises is as follows:

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

Here is the main() function, where we create a new thread, thr, in the try-catch block. We catch an exception and print the message to the terminal:

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

If you run this code in your IDE, you will see the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_399_0.png)

Figure 5.19: The result of an example's execution

We can see that std::terminate() was called after throwing an exception. When you have lots of threads in your program, it's very hard to find the right place where the thread was terminated. Fortunately, we have a few mechanisms for catching an exception from another thread. Let's consider them all.

The std::async function uses future results for transferring exceptions to the calling thread. It stores std::exception_ptr in the future result and sets the ready flag. Then, when you call get(), std::future checks if there any std::exception_ptr stored and rethrows the exception. All we need to do is place a get() call in the try-catch block. Let's consider an example. We will use two helper functions from the previous example, that is, getException() and toUppercase(). They will stay the same. In the main() function, we create an std::future object called result and run the toUppercase() function using the std::async() function. Then, we call the get() function of the result object in the try-catch block and catch an exception:

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

If you run the preceding code in your IDE, you will get the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_400_0.png)

Figure 5.20: The result of the example's execution

As you can see, we caught an exception and now we can handle it in some way. The **std::packaged_task<>** class handles exceptions in the same way – it stores **std::exception_ptr** in the future result, sets the ready flag, and then **std::future** rethrows an exception in the **get()** call. Let's consider a small example. We will use two helper functions from the previous example – **getException()** and **toUppercase()**. They will stay the same. In the **main()** function, we create an **std::packaged_task** object called **task**. By using the type of our **toUppercase()** function, it returns an integer and takes two integers as parameters. We pass the **toUppercase()** function to the **task** object. Then, we create an **std::future** object called **result** and get the result from the task object using the **get_future()** function. Finally, we run the task object in the new thread, **thr**, and in the **try-catch** block call the **get()** function of the **result** variable:

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

If you run this code in your IDE, you will get the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_401_0.png)

Figure 5.21: The result of this example's execution

The std::promise<> class handles exceptions in another way. It allows us to store an exception manually using the set_exception() or set_exception_at_thread_exit() function. To set an exception in std::promise, we have to catch it. If we do not catch an exception, an error will be set in the destructor of std::promise as std::future_errc::broken_promise in future result. When you call the get() function, an exception will be rethrown. Let's consider an example. We will use a helper function from the previous example – getException(). It's staying the same. However, we will change the toUppercase() function and add the third parameter, std::promise. Now, we will call the getException() function in the try block, catch an exception, and set it to the std::promise value:

```
void toUppercase(const std::string& bufIn, std::promise<std::string> result)
{
    std::string bufferOut;
    try
    {
```
