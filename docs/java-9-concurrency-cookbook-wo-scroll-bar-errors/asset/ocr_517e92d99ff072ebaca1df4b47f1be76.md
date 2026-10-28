# See also

+   - The *Creating a thread executor and controlling its rejected tasks* recipe in `Chapter 4`, Thread Executors
+   - The *Customizing the ThreadPoolExecutor class and Implementing a priority-based Executor class* recipes in `Chapter 8`, Customizing Concurrency Classes

# Monitoring a fork/join pool

The Executor framework provides a mechanism that allows you to separate task implementation from the creation and management of threads that execute the tasks. Java 9 includes an extension of the Executor framework for a specific kind of problem that will improve the performance of other solutions (using `Thread` objects directly or the Executor framework). It's the fork/join framework.

This framework is designed to solve problems that can be broken down into smaller tasks using the `fork()` and `join()` operations. The main class that implements this behavior is `ForkJoinPool`.

In this recipe, you will learn what information you can obtain about a `ForkJoinPool` class and how to obtain it.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named `Task` that extends the `RecursiveAction` class:

```
public class Task extends RecursiveAction{
```