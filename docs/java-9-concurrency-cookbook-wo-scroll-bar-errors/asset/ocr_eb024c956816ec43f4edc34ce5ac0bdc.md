In this recipe, you will learn how to use the `Executors` class to create a new `ThreadPoolExecutor` object, how to send tasks to the `Executor`, and how to control the rejected tasks of the `Executor` class.

# Getting ready

You should read the *Creating, running, and setting the characteristics of a thread* recipe in `Chapter 1, Thread Management`, to learn the basic mechanism of thread creation in Java. You can compare both the mechanisms and select one, depending on the problem.

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. First, implement the tasks that will be executed by the server. Create a class named `Task` that implements the `Runnable` interface:
+   2. Declare a `Date` attribute named `initDate` to store the creation date of the task and a `String` attribute called `name` to store the name of the task:
+   3. Implement the constructor of the class that initializes both the attributes:
+   4. Implement the `run()` method:

```
public class Task implements Runnable {
```

```
private final Date initDate;
private final String name;
```

```
public Task(String name) {
    initDate=new Date();
    this.name=name;
}
```

```
@Override
public void run() {
```