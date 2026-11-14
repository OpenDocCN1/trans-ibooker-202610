
## Making Additional Streams

```
std::cout << "\[" << ss.str() << "\]" << std::endl;

ss.str("");

ss << std::quoted(str2);

std::cout << "\[" << ss.str() << "\]" << std::endl;
```

Now, we will have a different result:

```
\["String without quotes"\]

\["String with quotes \\"right here\\""\]
```

Did you notice that the first string is wrapped by quotes and that the substring "right here" from the second string is stored with escape characters?

Now, you know how to wrap any string in quotes. You can even write your own wrapper to decrease the number of lines when you use `std::quoted()`. For example, we moved the work with the stream to a separate function:

```
std::string quote(const std::string& str)
{
    std::ostringstream oss;
    oss << std::quoted(str);
    return oss.str();
}
```

Then, we do the following when we need to call our wrapper:

```
std::string str1("String without quotes");
std::string str2("String with quotes \\"right here\\"");
std::cout << "\[" << quote(str1) << "\]" << std::endl;
std::cout << "\[" << quote(str2) << "\]" << std::endl;
```

Now, it looks much better. The first topic has come to an end, so let's revise what we have just learned. In practice, we learned about the usage of predefined stream objects, I/O operations with files with inner memory, I/O formatting, and the I/O of user-defined types. Now that we have a complete understanding of how to work with the I/O library in C++, we will consider what to do when the standard stream is not enough.

## How to Make an Additional Stream – Composition

When the provided interface of streams is not enough for resolving your task, you may want to create an additional stream that will reuse one of the existing interfaces. You may need to output or provide input from a specific external device, or you may need to add the ID of the thread that invoked the I/O operation. There are a few ways to do that. You may create a new class that will aggregate one of the existing streams as a private member. It will implement all the needed functions, such as shift operators, through already existing stream functions. Another way to do this is to inherit one of the existing classes and override all the virtual functions in a way you need them.

First of all, you have to choose the appropriate class to be used. Your choice should depend on which modification you want to add. Choose `std::basic_istream`, `std::basic_ostream`, and `std::basic_iostream` if you need to modify input or output operations. Choose `std::ios_base` if you want to modify the state information, control information, private storage, and so on. Choose `std::basic_ios` if you want to modify something related to the stream buffer. After you choose the correct base class, inherit one of the aforementioned classes to create an additional stream.

There's one more thing you have to know – how to initialize standard streams correctly. In terms of the initialization of a file or string stream and basic stream classes, there are some big differences. Let's review them. To initialize the object of a class that is derived from the file stream class, you need to pass the file name. To initialize the object of the class that is derived from the string stream class, you need to call the default constructor. Both of them have their own stream buffers, so they don't need additional manipulation on initialization. To initialize the object of the class that is derived from the basic stream class, you need to pass a pointer to a stream buffer. You can create a variable of the buffer or you may use the buffer of the predefined stream objects, such as `std::cout` or `std::cerr`.

Let's review these two methods of creating additional streams in detail.

### How to Make an Additional Stream – Composition

Composition means that you declare some of the standard stream objects in the private section of your class as a class member. When you choose an appropriate standard stream class, go to its header and notice which constructor it has. Then, you need to correctly initialize this member in the constructor of your class. To use your class as a stream object, you need to implement basic functions such as the shift operator, `str()`, and so on. As you may remember, every stream class has overloaded shift operators for built-in types. They also have overloaded shift operators for predefined functions such as `std::endl`. You need to be able to use your class as a real stream object. Instead of declaring all 18 overloaded shift operators, we just need to create one template. Also, to allow for the use of predefined manipulators, we must declare a shift operator that takes a pointer to a function.

This doesn't look very hard, so let's try to implement such a "wrapper" for the `std::ostream` object.

## Exercise 6: Composing the Standard Stream Object in the User-Defined Class

In this exercise, we will create our own stream object that wraps the `std::ostream` object and adds additional features. We will create a class called `extendedOstream` that will output data to the Terminal and insert the following data in front of each piece of output: date and time and thread ID. To complete this exercise, perform the following steps:

1.  Include the required headers: `<iostream>` for `std::endl` support, `<sstream>` for `std::ostream` support, `<thread>` for `std::this_thread::get_id()` support, `<chrono>` for `std::chrono::system_clock::now()`, and `<ctime>` for converting timestamps into readable representations:

    ### Note

    Don't forget to add the `pthread` linker flag to the Eclipse project settings for threading support: **Project** -> **Properties** -> **C/C++ Build** -> **Settings** -> **G++ Linker** -> **Miscellaneous** -> **Linker flags** enter '`-pthread`'. This path is valid for Eclipse Version: 3.8.1; it may vary for different versions.

    ```
    #include <iostream>
    #include <sstream>
    #include <thread>
    #include <chrono>
    #include <ctime>
    ```

2.  Next, declare the `extendedOstream` class. Declare the `std::ostream` variable called `m_oss` and the `bool` variable called `writeAdditionalInfo`. This `bool` variable will be used to indicate whether extended data should be printed or not:

    ```
    class extendedOstream
    {
    private:
        std::ostream& m_oss;
        bool writeAdditionalInfo;
    };
    ```

3.  Next, in the public section, define a default constructor and initialize `m_oss` with `std::cout` to redirect output to the Terminal. Initialize `writeAdditionalInfo` with `true`:

    ```
    extendedOstream()
        : m_oss(std::cout)
        , writeAdditionalInfo(true)
    {
    }
    ```

4.  Define a template overloaded left shift operator, `<<`, that returns a reference to `extendedOstream` and takes a template parameter called `value`. Then, if `writeAdditionalInfo` is `true`, output the time, thread ID, and the given value, and then set `writeAdditionalInfo` to `false`. If `writeAdditionalInfo` is `false`, output only the given value. This function will be used for the output of all built-in types:

    ```
    template<typename T>
    extendedOstream& operator<<(const T& value)
    {
        if (writeAdditionalInfo)
        {
            std::string time = fTime();
            auto id = threadId();
            m_oss << time << id << value;
            writeAdditionalInfo = false;
        }
        else
        {
            m_oss << value;
        }
        return *this;
    }
    ```

5.  Define another overloaded left shift operator that takes a pointer to the function as a parameter and returns the reference to `std::ostream`. In the function body, set `writeAdditionalInfo` to `true`, call the given function, and pass `m_oss` as an argument. This overloaded operator will be used for predefined functions such as `std::endl`:

    ```
    extendedOstream&
    operator<<(std::ostream& (*pfn)(std::ostream&))
    {
        writeAdditionalInfo = true;
    }
    ```
