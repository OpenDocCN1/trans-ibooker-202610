
- **Numerical update access mode**: This is to modify numerical values in an atomic way.

## See also
- The *Using atomic variables* and *Using atomic arrays* recipe in this chapter

## 8
## Customizing Concurrency Classes

In this chapter, we will cover the following topics:
- Customizing the ThreadPoolExecutor class
- Implementing a priority-based Executor class
- Implementing the ThreadFactory interface to generate custom threads
- Using our ThreadFactory in an Executor object
- Customizing tasks running in a scheduled thread pool
- Implementing the ThreadFactory interface to generate custom threads for the fork/join framework
- Customizing tasks running in the fork/join framework
- Implementing a custom Lock class
- Implementing a transfer queue-based on priorities
- Implementing your own atomic object
- Implementing your own stream generator
- Implementing your own asynchronous stream

## Introduction

The Java Concurrency API provides a lot of interfaces and classes to implement concurrent applications. They provide low-level mechanisms, such as the `Thread` class, the `Runnable` or `Callable` interfaces, or the `synchronized` keyword. They also provide high-level mechanisms, such as the `Executor` framework and the fork/join framework added in the Java 7 release, or the `Stream` framework added in Java 8, to process big sets of data. Despite this, you may find yourself developing a program where the default configuration and/or implementation of the Java API doesn't meet your needs.

In this case, you may need to implement your own custom concurrent utilities, based on the ones provided by Java. Basically, you can:
- Implement an interface to provide the functionality defined by that interface, for example, the `ThreadFactory` interface.
- Override some methods of a class to adapt its behavior to your needs. For example, overriding the `onAdvance()` method of the `Phaser` class that, by default, does nothing useful and is supposed to be overridden to offer some functionality.

Through the recipes of this chapter, you will learn how to change the behavior of some Java concurrency API classes without the need to design a concurrency framework from scratch. You can use these recipes as an initial point to implement your own customizations.

## Customizing the ThreadPoolExecutor class

The `Executor` framework is a mechanism that allows you to separate thread creation from its execution. It's based on the `Executor` and `ExecutorService` interfaces with the `ThreadPoolExecutor` class that implements both the interfaces. It has an internal pool of threads and provides methods that allow you to send two kinds of tasks and execute them in the pooled threads. These tasks are:
- The `Runnable` interface to implement tasks that don't return a result
- The `Callable` interface to implement tasks that return a result

In both cases, you only send the task to the executor. The executor uses one of its pooled threads or creates a new one to execute those tasks. It also decides the moment in which the task is executed.

```
System.out.printf("MyExecutor: Running tasks: %d\n", getActiveCount());
System.out.printf("MyExecutor: Pending tasks: %d\n", getQueue().size());
super.shutdown();
}
```

5. Override the shutdownNow() method. Write in the console information about the executed, running, and pending tasks. Then, call the shutdownNow() method of the parent class using the super keyword:

```
@Override
public List<Runnable> shutdownNow() {
    System.out.printf("MyExecutor: Going to immediately shutdown.\n");
    System.out.printf("MyExecutor: Executed tasks: %d\n", getCompletedTaskCount());
    System.out.printf("MyExecutor: Running tasks: %d\n", getActiveCount());
    System.out.printf("MyExecutor: Pending tasks: %d\n", getQueue().size());
    return super.shutdownNow();
}
```

6. Override the beforeExecute() method. Write a message in the console with the name of the thread that is going to execute the task and the hash code of the task. Store the start date in HashMap using the hash code of the task as the key:

```
@Override
protected void beforeExecute(Thread t, Runnable r) {
    System.out.printf("MyExecutor: A task is beginning: %s : %s\n", t.getName(), r.hashCode());
    startTimes.put(r, new Date());
}
```

7. Override the afterExecute() method. Write a message in the console with the result of the task and calculate the running time of the task after subtracting the start date of the task stored in HashMap of the current date:

```
@Override
protected void afterExecute(Runnable r, Throwable t) {
    Future<?> result=(Future<?>)r;
    try {
        System.out.printf("***********************************\n");
        System.out.printf("MyExecutor: A task is finishing.\n");
```

```
System.out.printf("MyExecutor: Result: %s\n", result.get());
Date startDate=startTimes.remove(r);
Date finishDate=new Date();
long diff=finishDate.getTime()-startDate.getTime();
System.out.printf("MyExecutor: Duration: %d\n",diff);
System.out.printf("***********************************************\n");
} catch (InterruptedException | ExecutionException e) {
 e.printStackTrace();
}
}
```

8. Create a class named SleepTwoSecondsTask that implements the Callable interface parameterized by the String class. Implement the call() method. Put the current thread to sleep for 2 seconds and return the current date converted into a String type:

```
public class SleepTwoSecondsTask implements Callable<String> {

    public String call() throws Exception {
        TimeUnit.SECONDS.sleep(2);
        return new Date().toString();
    }

}
```

9. Implement the main class of the example by creating a class named Main with a main() method:

```
public class Main {
    public static void main(String[] args) {
```

10. Create a MyExecutor object named myExecutor:

```
MyExecutor myExecutor=new MyExecutor(4, 8, 1000,
    TimeUnit.MILLISECONDS,
    new LinkedBlockingDeque<Runnable>());
```

11. Create a list of Future objects parameterized by the String class to store the resultant objects of the tasks you're going to send to the executor:

```
List<Future<String>> results=new ArrayList<>();
```

12. Submit 10 Task objects:

```
for (int i=0; i<10; i++) {
    SleepTwoSecondsTask task=new SleepTwoSecondsTask();
    Future<String> result=myExecutor.submit(task);
    results.add(result);
}
```

13. Get the result of the execution of the first five tasks using the get() method. Write them in the console:

```
for (int i=0; i<5; i++){
    try {
        String result=results.get(i).get();
        System.out.printf("Main: Result for Task %d : %s\n", i,result);
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
```

14. Finish the execution of the executor using the shutdown() method:

```
myExecutor.shutdown();
```

15. Get the result of the execution of the last five tasks using the get() method. Write them in the console:

```
for (int i=5; i<10; i++){
    try {
        String result=results.get(i).get();
        System.out.printf("Main: Result for Task %d : %s\n", i,result);
    } catch (InterruptedException | ExecutionException e) {
        e.printStackTrace();
    }
}
```

16. Wait for the completion of the executor using the awaitTermination() method:

```
try {
    myExecutor.awaitTermination(1, TimeUnit.DAYS);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

17. Write a message indicating the end of the execution of the program:

```
System.out.printf("Main: End of the program.\n");
```

## How it works...

In this recipe, we implemented our custom executor by extending the ThreadPoolExecutor class and overriding four of its methods. The beforeExecute() and afterExecute() methods were used to calculate the execution time of a task. The beforeExecute() method is executed before the execution of a task; in this case, we used HashMap to store the start date of the task in it. The afterExecute() method is executed after the execution of the task. You got startTime of the task that had finished from HashMap and then calculate the difference between the actual date and and the startTime to get the execution time of the task. You also overrode the shutdown() and shutdownNow() methods to write statistics about the tasks executed in the executor to the console. These tasks included:
- The executed tasks, using the getCompletedTaskCount() method
- The tasks that were running at the current time, using the getActiveCount() method
- The pending tasks, using the size() method of the blocking queue where the executor stores the pending tasks

The SleepTwoSecondsTask class that implements the Callable interface puts its execution thread to sleep for 2 seconds and the Main class, where you send 10 tasks to your executor, uses it and the other classes to demo their features.

Execute the program and you will see how the program shows the time span of each task that is running and the statistics of the executor upon calling the shutdown() method.

## See also
- The Creating a thread executor and controlling its rejected tasks recipe in Chapter 4, Thread Executors
- The Using our ThreadFactory in an Executor object recipe in this chapter
