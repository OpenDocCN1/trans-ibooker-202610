```
}
else
{
    bufferOut += *iter;
}
}
return bufferOut;
}
```

Then, in the main() function, we create an std::future variable with a name result and initialize it using the std::async() return value. Then, we fetch the result by using the get() function of the result object:

```
#include <iostream>
#include <future>

int main()
{
    std::future<std::string> result = std::async(toUppercase, "please, make it uppercase");
    std::cout << "Main thread isn't locked" << std::endl;
    std::cout << "Future result = " << result.get() << std::endl;
    return 0;
}
```

Actually, here, we created a future object:

```
std::future<std::string> result = std::async(toUppercase, "please, make it uppercase");
```

As you can see, we didn't pass the launch mode to the std::async() function, which means that the default mode will be used: std::launch::deferred | std::launch::async. You can do this explicitly:

```
std::future<std::string> result = std::async(std::launch::async, toUppercase, "please, make it uppercase");
```