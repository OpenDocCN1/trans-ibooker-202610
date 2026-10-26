
Perform the following steps to complete this exercise:

+   1. Include some headers for threading support, namely `<thread>`, streaming support, namely `<iostream>`, and functional objects support, namely `<functional>`:

```
```cpp
#include <iostream>
#include <thread>
#include <functional>
```
```

+   2. Implement a free function, **printNumbers()**, that prints numbers from 0 to 100 in a **for** loop:

```
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
```

+   3. Implement a callable object, that is, a **Printer** class with an overloaded **operator()** that prints a "*" symbol from 0 to 100000 in a **for** loop. For every **200** iterations, print a new line symbol for more readable output:

```
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
```

8. Create an instance of the Printer class called print. Create a third thread, thr3, and initialize it with the print object. Detach it using the detach() method:

```
Printer print;
std::thread thr3(print);
thr3.detach();
```

9. Create the last thread, thr4, and initialize it with the printOther object. Detach it:

```
std::thread thr4(printOther);
thr4.detach();
```

10. Add the std::getchar() function call before the exit of the main() function. This avoids closing the application. We'll have the possibility to see how detached threads work:

```
std::getchar();
```

11. Run this code in your editor. You will see that thr1 starts execution and the program waits. After thr1 has finished, thr2 starts execution and the program waits. This is an example of synchronous execution. After thr2 has finished its work, threads thr3 and thr4 start execution. They are detached, so the program can proceed with the execution. In the following output, you will see that the symbols are mixed. This happens because the operating system performs interruptions and the threads work at the same time.

Your output will be similar to the following:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_361_0.png)

Figure 5.7: The result of the exercise's execution

Thread A continues the increment:

+   1. Writes the new value (var = 1).

Thus, we expect the variable to be equal to 2 after the completion of the work, but in fact, it is equal to 1. Have a look at the following diagram to get a better understanding of this example:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_363_0.png)

Figure 5.8: Two threads increment the same shared variable

```
else
{
    std::cout << "Error: dividend = 0" << std::endl;
}
```

3. Enter the main() function, create two integers called divisor and dividend, and initialize them with any non-zero values:

```
int main()
{
    int divisor = 15;
    int dividend = 5;
    return 0;
}
```

4. Create the thr1 thread, pass the divide function, use divisor and dividend by reference, and then detach the thread:

```
std::thread thr1(divide, std::ref(divisor), std::ref(dividend));
thr1.detach();
std::getchar();
```

Note

In the std::this_thread namespace there is a function called sleep_for that blocks threads for a given period of time. As a parameter, it takes std::chrono::duration – a template class to represent a time interval.

5. Run this code in your editor. You will see that the divide() function works correctly in thr1. The output looks as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_365_0.png)

Figure 5.9: The result of the correct exercise execution

Now, we will continue and make changes that will demonstrate race conditions.

+   6. Go back to the function and set the sleeping time in **2s** for the child thread after the **if** condition. Add the logs:

```
if (0 != dividend)
{
    std::cout << "Child thread goes sleep" << std::endl;
    using namespace std::chrono_literals;
    std::this_thread::sleep_for(2s);
    std::cout << "Child thread woke up" << std::endl;
    std::cout << "Dividend = " << dividend << std::endl;
    std::cout << (divisor / dividend) << std::endl;
}
```

+   7. Go back to the **main()** function and set the sleeping time in **1s** for the main thread. After that, set the **dividend** variable to **0**. Add the logs:

```
std::cout << "Main thread goes sleep" << std::endl;
using namespace std::chrono_literals;
std::this_thread::sleep_for(1s);
std::cout << "Main thread woke up" << std::endl;
dividend = 0;
std::cout << "Main thread set dividend to 0" << std::endl;
```

# Note

The **std::chrono_literals** namespace contains literals for time representations: `h` for **hours**, `min` for **minutes**, `s` for **seconds**, `ms` for **milliseconds**, `us` for **microseconds**, and `ns` for **nanoseconds**. To use them, you should just add them to the end of the number, for example, 1s, 1min, 1h, and so on.

+   8. Add the **std::getchar()** function call before the exit of the **main()** function. This avoids us closing the application and we will have the possibility to see how detached threads work:

```
std::getchar();
```

9. Run this code in your editor. You will see that the main thread goes to sleep for 1s. Then, the child thread enters the if condition and goes to sleep for 2s, which means that it validates a dividend and it is not equal to 0. Then, the main thread wakes up and sets a dividend variable to 0. Then, the child thread wakes up and performs the division. But because the dividend is equal to 0 now, the application crashes. If you run this example in Debug mode, you will see a SIGFPE exception with a message: "Arithmetic exception". You will get the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_367_0.png)

Figure 5.10: The result of the exercise's execution with race conditions

In this exercise, we considered "check then act" kinds of race conditions. We've set periods of sleep for threads to emulate the OS interruption, but in real-world programs, this situation may well happen but may not. It all depends on the OS and its scheduler. This makes it enormously difficult to debug and fix race conditions. To avoid race conditions in this example, we can act in a few ways:

+   - Pass copies of variables to the threaded function instead of passing references.
+   - Synchronize access to the shared variables between threads using Standard Library primitives.
+   - Join child thread before the main thread changes a dividend value to 0.

Let's look at a few more ways to fix this race condition. All of them depend on a task that you try to implement. In the next section, we will consider synchronization primitives that are provided by the C++ Standard Library.

Difficulties will arise if Thread B executes first. It will lead to Thread B changing the value before Thread A reads it. The order of the execution of threads should be strictly guaranteed. Thread B should write the new value to a variable only after Thread A reads its value. The following diagram will help you clarify the RAW data dependency that leads to data hazards:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_370_0.png)

Figure 5.12: WAR data dependency between two threads

# WAW Dependency

A WAW dependency occurs when several threads change the value of the same variable, and some thread reads for its calculations. For example, Thread A executes its job and writes the results to a variable. Thread B reads the value of the variable and executes its job. Thread C executes its job and writes the results to the same variable. In pseudocode, this looks as follows:

```
Thread A: a = doSomeStuff();
Thread B: b = a - doOtherStuff();
Thread C: a = doNewStuff();
```

Difficulties will arise if **Thread C** executes before threads A and B. This leads to **Thread B** reads the value that it is not expected to be read. The order of the execution of threads should be strictly guaranteed. **Thread C** must write a new value to a variable, but only after **Thread A** has written its value and **Thread B** has read it. The following diagram will help you clarify the WAW data dependency that leads to data hazards:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_371_0.png)

Figure 5.13: WAW data dependency between two threads

# Resource Synchronization

To prevent races and data hazards, there is a shared data locking mechanism where one of the streams intends to change or read these data. This mechanism is called **resource synchronization**. For synchronization, we need to allocate pieces of code that change or read shared resources. Such pieces of code are called **critical sections**. Synchronization consists of blocking critical sections when one of the threads enters it. Other threads that also intend to execute the code of this critical section will be blocked. When the thread executing the critical section leaves it, the lock is released. Then, the story will repeat with the next thread.
