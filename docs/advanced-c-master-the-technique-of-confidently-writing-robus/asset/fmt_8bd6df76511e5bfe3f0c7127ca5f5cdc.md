
![Figure 5.9: The result of the correct exercise execution](image/C14583_05_09.jpg)

**Figure 5.9: The result of the correct exercise execution**

Now, we will continue and make changes that will demonstrate race conditions.

6. Go back to the function and set the sleeping time in `2s` for the child thread after the `if` condition. Add the logs:

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

7. Go back to the `main()` function and set the sleeping time in `1s` for the main thread. After that, set the `dividend` variable to `0`. Add the logs:

```
std::cout << "Main thread goes sleep" << std::endl;
using namespace std::chrono_literals;
std::this_thread::sleep_for(1s);
std::cout << "Main thread woke up" << std::endl;
dividend = 0;
std::cout << "Main thread set dividend to 0" << std::endl;
```

> **Note**
>
> The `std::chrono_literals` namespace contains literals for time representations: `h` for **hours**, `min` for **minutes**, `s` for **seconds**, `ms` for **milliseconds**, `us` for **microseconds**, and `ns` for **nanoseconds**. To use them, you should just add them to the end of the number, for example, `1s`, `1min`, `1h`, and so on.

8. Add the `std::getchar()` function call before the exit of the `main()` function. This avoids us closing the application and we will have the possibility to see how detached threads work:

```
std::getchar();
```

9. Run this code in your editor. You will see that the main thread goes to sleep for `1s`. Then, the child thread enters the `if` condition and goes to sleep for `2s`, which means that it validates a `dividend` and it is not equal to `0`. Then, the main thread wakes up and sets a `dividend` variable to `0`. Then, the child thread wakes up and performs the division. But because the `dividend` is equal to `0` now, the application crashes. If you run this example in Debug mode, you will see a **SIGFPE exception** with a message: "Arithmetic exception". You will get the following output:

![Figure 5.10: The result of the exercise's execution with race conditions](image/C14583_05_10.jpg)

**Figure 5.10: The result of the exercise's execution with race conditions**

In this exercise, we considered "check then act" kinds of race conditions. We've set periods of sleep for threads to emulate the OS interruption, but in real-world programs, this situation may well happen but may not. It all depends on the OS and its scheduler. This makes it enormously difficult to debug and fix race conditions. To avoid race conditions in this example, we can act in a few ways:

* Pass copies of variables to the threaded function instead of passing references.
* Synchronize access to the shared variables between threads using Standard Library primitives.
* Join child thread before the main thread changes a `dividend` value to `0`.

Let's look at a few more ways to fix this race condition. All of them depend on a task that you try to implement. In the next section, we will consider synchronization primitives that are provided by the C++ Standard Library.

## Data Hazards

Previously, we considered the most harmless example, but sometimes, there are situations where the data is damaged, and this leads to undefined program behavior or abnormal termination. Such damage to data, as a result of race conditions or simply wrong design, are known as **data hazards**. In general, this term implies that the final result of a piece of work depends on the order of the thread's execution. If different threads work with shared data or global variables, it may happen that, due to an incorrect order of task execution by different threads, the result will vary from time to time. This happens due to the dependencies between the data being multi-threaded. Such dependency issues are conditionally divided into three groups:

* A **true dependency**: **Read After Writing** (RAW)
* An **anti-dependency**: **Write After Reading** (WAR)
* An **output dependency**: **Write After Writing** (WAW)

### RAW Dependency

A **RAW dependency** occurs when one thread calculates the value that is used by another thread. For example, **Thread A** should do its job and write the results to a variable. **Thread B** must read the value of this variable and do its job. In pseudocode, this looks as follows:

```
Thread A: a = doSomeStuff();
Thread B: b = a - doOtherStuff();
```

Difficulties will arise if **Thread B** executes first. It will lead to **Thread B** reading an invalid value. The order of the execution of threads should be strictly guaranteed. **Thread B** must read the value of the variable, but only after **Thread A** has written it. Otherwise, it will lead to undefined behavior. The following diagram will help you clarify the RAW data dependency that leads to data hazards:

![Figure 5.11: RAW data dependency between two threads](image/C14583_05_11.jpg)

**Figure 5.11: RAW data dependency between two threads**

### WAR Dependency

A **WAR dependency** occurs when one thread changes the data that is used by another thread. For example, **Thread A** must read the value of a variable and do its job. After that, **Thread B** should do its job and write the results to a variable. In pseudocode, this looks as follows:

```
Thread A: b = a - doSomeStuff();
Thread B: a = doOtherStuff();
```

Difficulties will arise if **Thread B** executes first. It will lead to **Thread B** changing the value before **Thread A** reads it. The order of the execution of threads should be strictly guaranteed. **Thread B** should write the new value to a variable only after **Thread A** reads its value. The following diagram will help you clarify the WAR data dependency that leads to data hazards:

![Figure 5.12: WAR data dependency between two threads](image/C14583_05_12.jpg)

**Figure 5.12: WAR data dependency between two threads**

### WAW Dependency

A **WAW dependency** occurs when several threads change the value of the same variable, and some thread reads for its calculations. For example, **Thread A** executes its job and writes the results to a variable. **Thread B** reads the value of the variable and executes its job. **Thread C** executes its job and writes the results to the same variable. In pseudocode, this looks as follows:

```
Thread A: a = doSomeStuff();
Thread B: b = a - doOtherStuff();
Thread C: a = doNewStuff();
```

Difficulties will arise if **Thread C** executes before threads A and B. This leads to **Thread B** reading the value that it is not expected to be read. The order of the execution of threads should be strictly guaranteed. **Thread C** must write a new value to a variable, but only after **Thread A** has written its value and **Thread B** has read it. The following diagram will help you clarify the WAW data dependency that leads to data hazards:

![Figure 5.13: WAW data dependency between two threads](image/C14583_05_13.jpg)

**Figure 5.13: WAW data dependency between two threads**

## Resource Synchronization
