Implementation classes are logically divided into the following categories: **File I/O**, **String I/O**, **Synchronized I/O**, **I/O manipulators**, and predefined standard stream objects. All of them are derived from the aforementioned abstract classes. Let's consider each of them in detail in the upcoming sections.

# Predefined Standard Stream Objects

We will start our acquaintance with the I/O Standard Library with the already familiar **std::cout** class from the `<iostream>` header file. We use it for outputting the data to the Terminal. You may also be aware about the **std::cin** class for reading user input – but not everyone knows that **std::cout** and **std::cin** are predefined standard stream objects that are used for formatting input and output to the Terminal. The `<iostream>` header file also contains **std::cerr** and **std::clog** stream objects, which are used for logging errors. As usual, there are also their analogs for wide characters with a prefix of "w": **wcout**, **wcin**, **wcerr**, and **wclog**. All of these objects are automatically created and initialized at system startup. Although it is safe to use these objects from multiple threads, the output can be mixed. Let's revise how to use them. Since they are only overloaded for built-in types, we should write our own overrides for user-defined types.

The **std::cout** stream object is often used with the **std::endl** manipulator. It inserts a newline character in the output sequence and flushes it. Here is an example of using them:

```
std::string name("Marilyn Monroe");

int age = 18;

std::cout << "Name: " << name << ", age: " << age << std::endl;
```

Originally, the **std::cin** object reads all input character sequence, symbol by symbol. But it has overloads for built-in types and can read values such as **numbers**, **strings**, **characters**, and so on. There is a little trick in reading strings; **std::cin** reads the string until the next whitespace or newline symbol. So, if you need it to read a string, you have to do it in a loop, read it word by word, or use the **std::getline()** function, which takes the **std::cin** object as the first parameter and the destination string as the second.

> **Note**
> 
> The right shift operator, `>>`, of the **std::cin** stream object reads only one word from a line. Use **std::getline(std::cin, str)** to read the whole line.