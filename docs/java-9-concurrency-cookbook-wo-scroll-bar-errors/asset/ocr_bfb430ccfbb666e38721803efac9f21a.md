# See also

+   - The *Creating a fork/join pool* recipe in this chapter
+   - The *Monitoring a fork/join pool* recipe in Chapter 9, *Testing Concurrent Applications*

# Throwing exceptions in the tasks

There are two kinds of exceptions in Java:

+   - **Checked exceptions**: These exceptions must be specified in the `throws` clause of a method or caught inside them. For example, `IOException` or `ClassNotFoundException`.
+   - **Unchecked exceptions**: These exceptions don't have to be specified or caught. For example, `NumberFormatException`.

You can't throw any checked exception in the `compute()` method of the `ForkJoinTask` class because this method doesn't include any `throws` declaration in its implementation. You have to include the necessary code to handle the checked exceptions. On the other hand, you can throw (or it can be thrown by any method or object used inside the method) an unchecked exception. The behavior of the `ForkJoinTask` and `ForkJoinPool` classes is different from what you may expect. The program doesn't finish execution and you won't see any information about the exception in the console. It's simply swallowed as if it weren't thrown. Only when you call the `get()` method of the initial task, the exception will be thrown. You can, however, use some methods of the `ForkJoinTask` class to know if a task has thrown an exception and if so, what kind of exception it was. In this recipe, you will learn how to get that information.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or other IDE such as NetBeans, open it and create a new Java project.