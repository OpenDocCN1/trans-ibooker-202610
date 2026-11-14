
Now, let's put our knowledge to practice and write an application example that demonstrates how we can use `std::move` with threads.

### Exercise 4: Creating and Working with an STL Container of Threads

In this exercise, we will write a simple application where we will use `std::move()` with threads. First of all, we will implement a class that is move constructible. This class will convert lowercase text into uppercase text. Then, we will create a vector of instances of this class. Next, we will create a vector of `std::thread` objects. Finally, we will initialize the threads with an object from the first vector.

Perform the following steps to complete this exercise:

1. Include headers for threading support, namely `<thread>`, streaming support, namely `<iostream>`, and `<vector>`:

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <string>
```

2. Implement the `Converter` class, which has the `m_bufferIn` private member of the `const std::vector<std::string>&` type. This is a reference to the original vector of strings in lowercase. It also has a user constructor, which takes the `bufferIn` variable. Then, we delete the copy constructor and assignment operators. Finally, we define the overloaded `operator()`, where we convert all lowercase symbols into uppercase. After conversion, we write the result to the result buffer:

```cpp
class Converter
{
public:
    Converter(std::vector<std::string>& bufferIn)
        : m_bufferIn(bufferIn)
    {
    }
    Converter(Converter&& rhs)
        : m_bufferIn(std::move(rhs.m_bufferIn))
    {
    }
    Converter(const Converter&) = delete;
    Converter& operator=(const Converter&) = delete;
    Converter& operator=(Converter&&) = delete;
    void operator()(const int idx, std::vector<std::string>& result)
    {
        try
        {
            std::string::const_iterator end = m_bufferIn.at(idx).end();
            std::string bufferOut;
            for (std::string::const_iterator iter = m_bufferIn.at(idx).begin(); iter != end; iter++)
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
            result[idx] = bufferOut;
        }
        catch(...)
        {
            std::cout << "Invalid index" << std::endl;
        }
    }
private:
    const std::vector<std::string>& m_bufferIn;
};
```

3. Enter the `main()` function, create a constant value called `numberOfTasks`, and set it to `5`. Then, create a vector of a `Converter` object and reserve its size with `numberOfTasks`. Then, create a vector of `std::thread` objects and reserve its size with `numberOfTasks`:

```cpp
const int numberOfTasks = 5;
std::vector<Converter> functions;
functions.reserve(numberOfTasks);
std::vector<std::thread> threads;
threads.reserve(numberOfTasks);
```

4. Create the vector of strings, `textArr`, and push five different big strings to be converted:

```cpp
std::vector<std::string> textArr;
textArr.emplace_back("In the previous topics, we learned almost all that we need to work with threads. But we still have something interesting to consider – how to synchronize threads using future results. When we considered condition variables we didn't cover the second type of synchronization with future results. Now it's time to learn that.");
textArr.emplace_back("First of all, let's consider a real-life example. Imagine, you just passed the exam at the university. You were asked to wait some amount of time for results. So, you have time to coffee with your mates, and every 10-15 mins you check are results available. Then, when you finished all your other activities, you just come to the door of the lecture room and wait for results.");
textArr.emplace_back("In this exercise, we will write a simple application where we will use std::move() with threads. First of all, we will implement a class that is move constructible. This class will convert lowercase text into uppercase text. Then we will create a vector of instances of this class. Next, we will create a vector of std::thread object. Finally, we will initialize threads with an object from the first vector");
textArr.emplace_back("Let's consider one interesting issue. As you remember when we initialize std::thread all constructor arguments are copied into thread memory, including a callable object – lambda, function, std::function. But what if our callable object doesn't support copy semantic? For example, we created a class that has only move constructor and a move assignment operator:");
textArr.emplace_back("Run this code in your editor. You will see in the terminal log from the default constructor, two logs from the move operator, then one log from a destructor, then message from the doSomeJob() function and, finally two other log messages from the destructor. We see that the move constructor is called twice. You will get the output like the following:");
```

5. Implement a `for` loop where we push `Converter` objects into the functions vector:

```cpp
for (int i = 0; i < numberOfTasks; ++i)
{
    functions.push_back(Converter(textArr));
}
```

6. Create a result vector of strings and push five empty strings. Then, create a variable that will be an index of the array element:

```cpp
std::vector<std::string> result;
for (int i = 0; i < numberOfTasks; ++i)
{
    result.push_back("");
}
int idx = 0;
```

7. Implement another `for` loop where we push `std::thread` objects into the threads vector:

```cpp
for (auto iter = functions.begin(); iter != functions.end(); ++iter)
{
    std::thread tmp(std::move(*iter), idx, std::ref(result));       
    threads.push_back(std::move(tmp));
    from = to;
    to += step;
}
```

8. Implement a third `for` loop where we detach `std::thread`s:

```cpp
for (auto iter = threads.begin(); iter != threads.end(); ++iter)
{
    (*iter).detach();
}
```

9. Add a small sleep for the main thread to avoid closing the application. Now, we can see how detached threads work:

```cpp
using namespace std::chrono_literals;
std::this_thread::sleep_for(5s);
```

10. Finally print the result into the terminal:

```cpp
for (const auto& str : result)
{
    std::cout << str;
}
```

11. Run this code in your editor. In the terminal, you can see that all strings are in uppercase, which means that all threads were moved and run successfully. You will get the following output:

![Figure 5.18: The result of the exercise's execution](image/C14583_05_18.jpg)

*Figure 5.18: The result of the exercise's execution*

In this exercise, we practiced how to create an STL container of move-only objects. We also considered how to pass non-copiable objects to a thread constructor. This knowledge will help us in the next section when we learn how to get the result from the thread.

## Futures, Promises, and Async

In the previous section, we learned almost all that we need to work with threads. But we still have something interesting to consider, that is, synchronizing threads using future results. When we considered condition variables, we didn't cover the second type of synchronization with future results. Now, it's time to learn about that.
