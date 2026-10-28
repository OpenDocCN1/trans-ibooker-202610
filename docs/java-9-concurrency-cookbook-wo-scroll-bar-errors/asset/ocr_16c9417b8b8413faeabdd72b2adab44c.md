+   - `exec()`: This method implements the logic of the task. In this case, you delegated the logic to the abstract `compute()` method (as the `RecursiveAction` and `RecursiveTask` classes). However, in the `exec()` method, you measure the execution time of the method, writing it in the console.

Finally, in the main class of the example, you created an array of 10,000 elements, a `ForkJoinPool` executor, and a `Task` object to process the whole array. Execute the program and you'll see how the different tasks that are executed write their execution time in the console.

# See also

+   - The Creating a fork/join pool recipe in Chapter 5, Fork/Join Framework
+   - The Implementing the ThreadFactory interface to generate custom threads for the fork/join framework recipe in this chapter

# Implementing a custom Lock class

Locks are one of the basic synchronization mechanisms provided by the Java Concurrency API. They allow programmers to protect a critical section of code so only one thread can execute that block of code at a time. It provides the following two operations:

+   - `lock()`: You call this operation when you want to access a critical section. If there is another thread running this critical section, other threads are blocked until they're woken up by the lock to get access to the critical section.
+   - `unlock()`: You call this operation at the end of a critical section to allow other threads to access it.

In the Java Concurrency API, locks are declared in the `Lock` interface and implemented in some classes, for example, the `ReentrantLock` class.

In this recipe, you will learn how to implement your own `Lock` object by implementing a class that implements the `Lock` interface, which can be used to protect a critical section.