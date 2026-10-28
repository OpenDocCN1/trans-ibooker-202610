# Configuring NetBeans for debugging concurrency code

Software is necessary to develop applications that work properly, meet the quality standards of the company, and could be easily modified in future (in limited time and cost as low as possible). To achieve this goal, it is essential to use an IDE that can integrate several tools (compilers and debuggers) that facilitate the development of applications under one common interface.

If you work with Java, NetBeans is one of the most popular IDEs. It has an integrated debugger that allows you to test your application.

In this recipe, you will learn how to change the configuration of the Netbeans debugger to help you test concurrent applications.

# Getting ready

You should have the NetBeans IDE installed. Open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named Task1 and specify that it implements the Runnable interface:

```
public class Task1 implements Runnable {
```

+   2. Declare two private Lock attributes, named lock1 and lock2:

```
private Lock lock1, lock2;
```

+   3. Implement the constructor of the class to initialize its attributes:

```
public Task1 (Lock lock1, Lock lock2) {
    this.lock1=lock1;
    this.lock2=lock2;
}
```