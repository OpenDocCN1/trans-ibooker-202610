Now, you know how to wrap any string in the quotes. You can even write your own wrapper to decrease the number of lines when you use std::quoted(). For example, we moved the work with the stream to a separate function:

```
std::string quote(const std::string& str)
{
    std::ostringstream oss;
    oss << std::quoted(str);
    return oss.str();
}
```

Then, we do the following when we need we call our wrapper:

```
std::string str1("String without quotes");
std::string str2("String with quotes \"right here\"");

std::cout << "[" << quote(str1) << "]" << std::endl;
std::cout << "[" << quote(str2) << "]" << std::endl;
```

Now, it looks much better. The first topic has come to an end, so let's revise what we have just learned. In practice, we learned about the usage of predefined stream objects, I/O operations with files with inner memory, I/O formatting, and the I/O of user-defined types. Now that we have a complete understanding of how to work with the I/O library in C++, we will consider what to do when the standard stream is not enough.

# Making Additional Streams

When the provided interface of streams is not enough for resolving your task, you may want to create an additional stream that will reuse one of the existing interfaces. You may need to output or provide input from a specific external device, or you may need to add the Id of the thread that invoked the I/O operation. There are a few ways to do that. You may create a new class that will aggregate one of the existing streams as a private member. It will implement all the needed functions, such as shift operators, through already existing stream functions. Another way to do this is to inherit one of the existing classes and override all the virtual functions in a way you need them.