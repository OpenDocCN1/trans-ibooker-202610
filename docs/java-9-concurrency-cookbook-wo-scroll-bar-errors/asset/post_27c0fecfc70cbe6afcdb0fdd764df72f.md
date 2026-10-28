
12. Implement another version of the tryLock() method with two parameters: a long parameter named time and a TimeUnit parameter named unit. Call the tryAcquireNanos() method of the sync object:

```
@Override
public boolean tryLock(long time, TimeUnit unit) throws InterruptedException {
    return sync.tryAcquireNanos(1, TimeUnit.NANOSECONDS
        .convert(time, unit));
}
```

13. Implement the unlock() method. Call the release() method of the sync object:

```
@Override
public void unlock() {
    sync.release(1);
}
```

14. Implement the newCondition() method. Create a new object of the internal class of the sync object, namely ConditionObject:

```
@Override
public Condition newCondition() {
    return sync.newCondition();
}
```

15. Create a class named Task and specify that it implements the Runnable interface:

```
public class Task implements Runnable {
```

16. Declare a private MyLock attribute named lock:

```
private final MyLock lock;
```

17. Declare a private String attribute called name:

```
private final String name;
```

18. Implement the constructor of the class to initialize its attributes:

```
public Task(String name, MyLock lock){
    this.lock=lock;
    this.name=name;
}
```

19. Implement the run() method of the class. Acquire the lock, put the thread to sleep for 2 seconds, and then release the lock object:

```
@Override
public void run() {
    lock.lock();
    System.out.printf("Task: %s: Take the lock\n",name);
    try {
        TimeUnit.SECONDS.sleep(2);
        System.out.printf("Task: %s: Free the lock\n",name);
    } catch (InterruptedException e) {
        e.printStackTrace();
    } finally {
        lock.unlock();
    }
}
```

20. Implement the main class of the example by creating a class named Main with a main() method:

```
public class Main {
    public static void main(String[] args) {
```

21. Create a MyLock object named lock:

```
MyLock lock=new MyLock();
```

22. Create and execute 10 Task tasks:

```
for (int i=0; i<10; i++){
    Task task=new Task("Task-"+i,lock);
    Thread thread=new Thread(task);
    thread.start();
}
```

23. Try to get the lock using the tryLock() method. Wait for a second, and if you don't get the lock, write a message and try again:

```
boolean value;
do {
    try {
        value=lock.tryLock(1,TimeUnit.SECONDS);
        if (!value) {
            System.out.printf("Main: Trying to get the Lock\n");
        }
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    value=false;
} while (!value);
```

24. Write a message indicating that you got the lock and release it:

```
System.out.printf("Main: Got the lock\n");
lock.unlock();
```

25. Write a message indicating the end of the program:

```
System.out.printf("Main: End of the program\n");
```

## How it works...

The Java Concurrency API provides a class that can be used to implement synchronization mechanisms with features of locks or semaphores. It's called AbstractQueuedSynchronizer, and as the name suggests, it's an abstract class. It provides operations to control access to a critical section and manage a queue of threads that are blocked and are awaiting access to the section. The operations are based on two abstract methods:

- tryAcquire(): This method is called to try and get access to a critical section. If the thread that calls it can access the critical section, the method returns the true value. Otherwise, it returns the false value.
- tryRelease(): This method is called to try and release access to a critical section. If the thread that calls it can release access, the method returns the true value. Else, it returns the false value.

In these methods, you have to implement the mechanism you use to control access to a critical section. In this case, you implemented the MyAbstractQueuedSynchronizer class that extends the AbstractQueuedSynchronizer class and implements the abstract methods using an AtomicInteger variable to control access to the critical section. This variable will store the value 0 if the lock is free, so a thread can have access to the critical section, and the value 1 if the lock is blocked, so a thread doesn't have access to the critical section.

You used the compareAndSet() method provided by the AtomicInteger class that tries to change the value you specify as the first parameter with the value you specify as the second parameter. To implement the tryAcquire() method, you try to change the value of the atomic variable from zero to one. Similarly, to implement the tryRelease() method, you try to change the value of the atomic variable from one to zero.

You have to implement `AtomicInteger` class because other implementations of the `AbstractQueuedSynchronizer` class (for example, the one used by `ReentrantLock`) are implemented as private classes internally. This is carried out in the class that uses it, so you don't have access to it.

Then, you implemented the `MyLock` class. This class implements the `Lock` interface and has a `MyQueuedSynchronizer` object as an attribute. To implement all the methods of the `Lock` interface, you used methods of the `MyQueuedSynchronizer` object.

Finally, you implemented the `Task` class that implements the `Runnable` interface and uses a `MyLock` object to get access to the critical section. This critical section puts the thread to sleep for 2 seconds. The `main` class creates a `MyLock` object and runs 10 `Task` objects that share the lock. The `main` class also tries to get access to the lock using the `tryLock()` method.

When you execute the example, you can see how only one thread has access to the critical section, and when that thread finishes, another one gets access to it.

You can use your own `Lock` interface to write log messages about its utilization, control the time that it's locked, or implement advanced synchronization mechanisms to control, for example, access to a resource so that it's only available at certain times.

## There's more...

The `AbstractQueuedSynchronizer` class provides two methods that can be used to manage the state of the lock. They are the `getState()` and `setState()` methods. These methods receive and return an integer value with the state of the lock. You could have used them instead of the `AtomicInteger` attribute to store the state of the lock.

The Java Concurrency API provides another class to implement synchronization mechanisms. It's the `AbstractQueuedLongSynchronizer` class, which is equivalent to `AbstractQueuedSynchronizer` but uses a long attribute to store the state of the threads.

## See also

- The *Synchronizing a block of code with locks* recipe in `Chapter 2`, *Basic Thread Synchronization*

## Implementing a transfer queue-based on priorities

The Java 9 API provides several data structures to work with concurrent applications. From these, we want to highlight the following two data structures:

- LinkedTransferQueue: This data structure is supposed to be used in programs that have a producer/consumer structure. In such applications, you have one or more producers of data and one or more consumers of data, and a data structure is shared by all of them. Producers put data in the data structure and consumers take it from there. If the data structure is empty, consumers are blocked until they have data to consume. If it is full, producers are blocked until they have space to put data.
- PriorityBlockingQueue: In this data structure, elements are stored in an ordered way. They have to implement the Comparable interface with the compareTo() method. When you insert an element in the structure, it's compared to the elements of the structure until it finds its position.

Elements of LinkedTransferQueue are stored in the same order as they arrive, so the ones that arrived earlier are consumed first. It may be the case when you want to develop a producer/consumer program, where data is consumed according to some priority instead of arrival time. In this recipe, you will learn how to implement a data structure to be used in the producer/consumer problem whose elements will be ordered by priority; elements with higher priority will be consumed first.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

1. Create a class named MyPriorityTransferQueue that extends the PriorityBlockingQueue class and implements the TransferQueue interface:

```
public class MyPriorityTransferQueue<E> extends
    PriorityBlockingQueue<E> implements TransferQueue<E> {
```
