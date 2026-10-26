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

![](bbox=[0.155, 0.39, 0.905, 0.56])

Figure 5.21: The result of this example's execution

The std::promise<> class handles exceptions in another way. It allows us to store an exception manually using the set_exception() or set_exception_at_thread_exit() function. To set an exception in std::promise, we have to catch it. If we do not catch an exception, an error will be set in the destructor of std::promise as std::future_errc::broken_promise in future result. When you call the get() function, an exception will be rethrown. Let's consider an example. We will use a helper function from the previous example – getException(). It's staying the same. However, we will change the toUppercase() function and add the third parameter, std::promise. Now, we will call the getException() function in the try block, catch an exception, and set it to the std::promise value:

```
void toUppercase(const std::string& bufIn, std::promise<std::string> result)
{
    std::string bufferOut;
    try
    {
```