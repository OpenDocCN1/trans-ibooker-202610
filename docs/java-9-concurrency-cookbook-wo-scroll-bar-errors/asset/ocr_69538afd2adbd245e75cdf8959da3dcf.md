```
value=false;
}
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

# How it works...

The Java Concurrency API provides a class that can be used to implement synchronization mechanisms with features of locks or semaphores. It's called AbstractQueuedSynchronizer, and as the name suggests, it's an abstract class. It provides operations to control access to a critical section and manage a queue of threads that are blocked and are awaiting access to the section. The operations are based on two abstract methods:

+   - tryAcquire(): This method is called to try and get access to a critical section. If the thread that calls it can access the critical section, the method returns the true value. Otherwise, it returns the false value.
+   - tryRelease(): This method is called to try and release access to a critical section. If the thread that calls it can release access, the method returns the true value. Else, it returns the false value.

In these methods, you have to implement the mechanism you use to control access to a critical section. In this case, you implemented the MyAbstractQueuedSynchonizer class that extends the AbstractQueuedSynchrhonizer class and implements the abstract methods using an AtomicInteger variable to control access to the critical section. This variable will store the value 0 if the lock is free, so a thread can have access to the critical section, and the value 1 if the lock is blocked, so a thread doesn't have access to the critical section.

You used the compareAndSet() method provided by the AtomicInteger class that tries to change the value you specify as the first parameter with the value you specify as the second parameter. To implement the tryAcquire() method, you try to change the value of the atomic variable from zero to one. Similarly, to implement the tryRelease() method, you try to change the value of the atomic variable from one to zero.