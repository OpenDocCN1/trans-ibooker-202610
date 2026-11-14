
## Thread Detachment and Daemon Threads

```cpp
thread2.detach();
thread3.detach();
thread4.detach();

using namespace std::chrono_literals;

std::this_thread::sleep_for(2s);

return 0;
```

Let's see what happens here. We used four functions from the previous examples, but they were changed a little bit. We added the print of the thread's unique ID by using the `std::this_thread::get_id()` function. This function returns the `std::thread::id` object, which represents the unique ID of the thread. This class has overloaded operators for output and comparison, so we can use it in a different manner. For example, we can check the thread ID and if it is the ID of the main thread, we can execute a special job. In our example, we can print the thread ID to the terminal. Next, we created four threads and detached them. This means that no thread will wait for the other to finish working. From this moment, they become **daemon threads**.

They will continue their job, but nobody knows about that. Then, we used the `std::this_thread::sleep_for(2s)` function to make the main thread wait for two seconds. We did that because when the main thread finishes its job, the application will stop, and we won't be able to view the output of the detached threads in the terminal. The following screenshot is an example of the output to the terminal:

![Figure 5.6: The result of an example execution](image/C14583_05_06.jpg)

**Figure 5.6: The result of an example execution**

In your IDE, the output can change as the order of execution is undefined. A real-world example of asynchronous execution can be an internet browser wherein you can open multiple tabs. When a new tab is opened, the application starts a new thread and detaches them. Although the threads work independently, they can share some resources, such as a file handler, to write logs or do something else.

### Note: `std::thread::get_id()`

`std::thread` has a member function called `get_id()` that returns the unique ID of the `std::thread` instance. If the `std::thread` instance wasn't initialized or was joined or detached, `get_id()` returns a default `std::thread::id` object. This means that no thread of execution is associated with the current `std::thread` instance.

## Asynchronous Execution Example: Currency Exchange

Let's use some pseudocode to show an example where computations are done by another computational unit. For example, let's say we develop an application that performs calculations with currency exchange. The user inputs an amount in one currency, chooses another currency to exchange, and the application shows them the amount in that currency. The application, in the background, sends a request to a remote server that holds all the currency exchange rates.

The remote server calculates the amount of the given currency and sends the result back. Your application shows a progress bar at that time and allows the user to perform other operations. When it receives the results, it displays them on the window. Let's take a look at the following code:

```cpp
#include <thread>

void runMessageLoop()
{
    while (true)
    {
        if (message)
        {
            std::thread procRes(processResults, message);
            procRes.detach();
        }
    }
}

void processResults(Result res)
{
    display();
}

void sendRequest(Currency from, Currency to, double amount)
{
    send();
}

void displayProgress()
{
}

void getUserInput()
{
    Currency from;
    Currency to;
    double amount;
    std::thread progress(displayProgress);
    progress.detach();
    std::thread request(sendRequest, from, to, amount);
    request.detach();
}

int main()
{
    std::thread messageLoop(runMessageLoop);
    messageLoop.detach();
    std::thread userInput(getUserInput);
    userInput.detach();    
    return 0;
}
```

Let's see what happens here. In the `main()` function, we created a thread called `messageLoop` that executes the `runMessageLoop()` function. Some code that checks if there are any new results from the server can be placed in this function. If a new result is received, it creates a new thread, `procRes`, that will display the results in a window. We also created another thread, `userInput`, in the `main()` function that gets currencies and the amount from the user and creates a new thread, `request`, that will send a request to the remote server. After sending the request, it creates a new thread, `progress`, that will display a progress bar until the results are received. Since all the threads were detached, they were able to work independently. Sure, this is just pseudocode, but the main idea is clear – our application sends a request to the remote server, which performs calculations for our application.

## Concurrency Concepts: Daily Life Analogy

Let's revise what we have learned about concurrency concepts using an example from daily life. Here is a background wherein you've to write an application and provide all the documentation and architectural concepts related to it:

- **Single-threaded work**: You write it yourself.
- **Multi-threaded work**: You invite your friends and write a project together. Somebody writes an architectural concept, somebody takes care of documentation work, and you focus on the coding part. All participants communicate with each other to clarify any questions and share documentation, such as questions about specifications.
- **Parallel work**: The tasks are divided. Someone writes the documentation for the project, someone designs the diagrams, someone writes the test cases, and you work independently. Participants don't communicate at all.
- **Synchronized work**: In this case, each of you are unable to understand what they are supposed to do. Thus, you all decide to work one after the other. When the architectural work is finished, the developer starts to write the code. Then, when the development work is finished, someone starts to write the documentation.
- **Asynchronous work**: In this case, you hire an outsource company to complete the project. While they are developing the project, you'll be engaged in some other task.

Now, let's apply our knowledge in practice and solve an exercise to see how all it works.

### Exercise 1: Creating Threads in a Different Way

In this exercise, we'll write a simple application that creates four threads; two of them will work in a synchronized way and two of them will work asynchronously. All of them will print some symbols to the terminal, so that we can see how the operating system switches thread execution.

#### Note: Linker Flags

Add the `pthread` linker flag in your project settings to let the compiler know that you will use threading libraries. For Eclipse IDE you can do this following this path: **Project** -> **Properties** -> **C/C++ Build** -> **Settings** -> **G++ Linker** -> **Miscellaneous** -> **Linker flags enter '-pthread'**. This path is valid for **Eclipse Version: 3.8.1**, it may vary in different versions.

#### Steps

1. Include some headers for threading support, namely `<thread>`, streaming support, namely `<iostream>`, and functional objects support, namely `<functional>`:

```cpp
#include <iostream>
#include <thread>
#include <functional>
```

2. Implement a free function, `printNumbers()`, that prints numbers from 0 to 100 in a `for` loop:

```cpp
void printNumbers()
{
    for(int i = 0; i < 100; ++i)
    {
        std::cout << i << " ";
    }
    std::cout << std::endl;
}
```

3. Implement a callable object, that is, a `Printer` class with an overloaded `operator()` that prints a `"*"` symbol from 0 to 100000 in a `for` loop. For every `200` iterations, print a new line symbol for more readable output:

```cpp
class Printer
{
public:
    void operator()()
    {
        for(int i = 0; i < 100000; ++i)
        {
            if (!(i % 200))
            {
                std::cout << std::endl;
            }
            std::cout << "*";
        }
    }
};
```
