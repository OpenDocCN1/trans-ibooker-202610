
4. Implement the run() method. First, get control of the lock1 object using the lock() method and write a message in the console indicating that you have got it:

```
@Override
public void run() {
    lock1.lock();
    System.out.printf("Task 1: Lock 1 locked\n");
```

5. Then, get control of lock2 using the lock() method and write a message in the console indicating that you have got it:

```
lock2.lock();
System.out.printf("Task 1: Lock 2 locked\n");
```

6. Finally, release the two lock objects-first the lock2 object and then the lock1 object:

```
lock2.unlock();
lock1.unlock();
}
```

7. Create a class named Task2 and specify that it implements the Runnable interface:

```
public class Task2 implements Runnable{
```

8. Declare two private Lock attributes, named lock1 and lock2:

```
private Lock lock1, lock2;
```

9. Implement the constructor of the class to initialize its attributes:

```
public Task2(Lock lock1, Lock lock2) {
    this.lock1=lock1;
    this.lock2=lock2;
}
```

10. Implement the run() method. First, get control of the lock2 object using the lock() method and write a message in the console indicating that you have got it:

```
@Override
public void run() {
    lock2.lock();
    System.out.printf("Task 2: Lock 2 locked\n");
```

11. Then, get control of lock1 using the lock() method and write a message in the console indicating that you have got it:

```
lock1.lock();
System.out.printf("Task 2: Lock 1 locked\n");
```

12. Finally, release the two lock objects-first lock1 and then lock2:

```
lock1.unlock();
lock2.unlock();
}
```

13. Implement the main class of the example by creating a class named Main and adding the main() method to it:

```
public class Main {
```

14. Create two lock objects named lock1 and lock2:

```
Lock lock1, lock2;
lock1=new ReentrantLock();
lock2=new ReentrantLock();
```

15. Create a Task1 object named task1:

```
Task1 task1=new Task1(lock1, lock2);
```

16. Create a Task2 object named task2:

```
Task2 task2=new Task2(lock1, lock2);
```

17. Execute both the tasks using two threads:

```
Thread thread1=new Thread(task1);
Thread thread2=new Thread(task2);

thread1.start();
thread2.start();
```

18. When the two tasks finish their execution, write a message in the console every 500 milliseconds. Use the isAlive() method to check whether a thread has finished its execution:

```
while ((thread1.isAlive() &&(thread2.isAlive())) {
    System.out.println("Main: The example is"+ "running");
    try {
```

```
TimeUnit.MILLISECONDS.sleep(500);
} catch (InterruptedException ex) {
    ex.printStackTrace();
}
```

19. Add a breakpoint in the first call to the printf() method of the run() method of the Task1 class.

20. Debug the program. You will see the Debugging window in the top left-hand side corner of the main NetBeans window. The next screenshot illustrates the window with the thread that executes the Task1 object. The thread is waiting in the breakpoint. The other threads of the application are running:

![](img/cc06c59cd21f558f43e9475d7f212170_493_0.png)

21. Pause the execution of the main thread. Select the thread, right-click on it, and select the Suspend option. The following screenshot shows the new appearance of the Debugging window. Refer to the following screenshot:

![](img/cc06c59cd21f558f43e9475d7f212170_493_1.png)

22. Resume the two paused threads. Select each thread, right-click on them, and select the Resume option.

## How it works...

While debugging a concurrent application using NetBeans, when the debugger hits a breakpoint, it suspends the thread that hit the breakpoint and shows the **Debugging** window in the top left-hand side corner with the threads that are currently running. You can use the window to pause or resume the threads that are currently running, using the **Pause** or **Resume** options. You can also see the values of the variables or attributes of the threads using the **Variables** tab.

NetBeans also includes a deadlock detector. When you select the **Check for Deadlock** option in the **Debug** menu, NetBeans performs an analysis of the application that you're debugging to determine whether there's a deadlock situation. This example presents a clear deadlock. The first thread gets `lock1` first and then `lock2`. The second thread gets the locks in reverse manner. The breakpoint inserted provokes the deadlock, but if you use the NetBeans deadlock detector, you'll not find anything. Therefore, this option should be used with caution. Change the locks used in both the tasks by the `synchronized` keyword and debug the program again. The code of `Task1` is as follows:

```
@Override
public void run() {
    synchronized(lock1) {
        System.out.printf("Task 1: Lock 1 locked\n");
        synchronized(lock2) {
            System.out.printf("Task 1: Lock 2 locked\n");
        }
    }
}
```

The code of the `Task2` class will be analogous to this, but it changes the order of the locks. If you debug the example again, you will obtain a deadlock one more time. However, in this case, it's detected by the deadlock detector, as you can see in the following screenshot:

## There's more...

There are options to control the debugger. Select Options from the Tools menu. Then, select the Miscellaneous option and the Java Debugger tab. The following screenshot illustrates this window:

![](img/cc06c59cd21f558f43e9475d7f212170_495_0.png)

![](img/cc06c59cd21f558f43e9475d7f212170_495_1.png)

There are two options in the window that control the behavior described earlier:

+   - **New breakpoints suspend**: With this option, you can configure the behavior of NetBeans, which finds a breakpoint in a thread. You can suspend only that thread that has a breakpoint or all the threads of the application.

+   - **Steps resume**: With this option, you can configure the behavior of NetBeans when you resume a thread. You can resume only the current thread or all the threads.

Both the options have been marked in the screenshot presented earlier.

## See also

+   - The *Configuring Eclipse for debugging concurrency code* recipe in this chapter

## Testing concurrency code with MultithreadedTC

`MultithreadedTC` is a Java library for testing concurrent applications. Its main objective is to solve the problem of concurrent applications being non-deterministic. You can't control the order of execution of the different threads that form the application. For this purpose, it includes an internal **metronome**. These testing threads are implemented as methods of a class.

In this recipe, you will learn how to use the `MultithreadedTC` library to implement a test for `LinkedTransferQueue`.

## Getting ready

Download the MultithreadedTC library from https://code.google.com/archive/p/multithreadedtc/ and the JUnit library, version 4.10, from http://junit.org/junit4/. Add the `junit-4.10.jar` and `MultithreadedTC-1.01.jar` files to the libraries of the project.

## How to do it...

Follow these steps to implement the example:

+   1. Create a class named `ProducerConsumerTest` that extends the `MultithreadedTestCase` class:
+   
+   ```
+   public class ProducerConsumerTest extends MultithreadedTestCase {
+   ```
+   
+   2. Declare a private `LinkedTransferQueue` attribute parameterized by the `String` class named `queue`:
+   
+   ```
+   private LinkedTransferQueue<String> queue;
+   ```
+   
+   3. Implement the `initialize()` method. This method won't receive any parameters and will return no value. It will call the `initialize()` method of its parent class and then initialize the queue attribute:
+   
+   ```
+   @Override
+   public void initialize() {
+       super.initialize();
+       queue=new LinkedTransferQueue<String>();
+       System.out.printf("Test: The test has been initialized\n");
+   }
+   ```
+   
+   4. Implement the `thread1()` method. It will implement the logic of the first consumer. Call the `take()` method of the queue and then write the returned value in the console:
+   
+   ```
+   public void thread1() throws InterruptedException {
+       String ret=queue.take();
+       System.out.printf("Thread 1: %s\n",ret);
+   }
+   ```
+   
+   5. Implement the `thread2()` method. It will implement the logic of the second consumer. First wait until the first thread has slept in the `take()` method. To put the thread to sleep, use the `waitForTick()` method. Then, call the `take()` method of the queue and write the returned value in the console:
+   
+   ```
+   public void thread2() throws InterruptedException {
+       waitForTick(1);
+       String ret=queue.take();
+       System.out.printf("Thread 2: %s\n",ret);
+   }
+   ```
