In the case of the `acquire()`, `acquireUninterruptibly()`, and `tryAcquire()` methods, if the value of the counter is less than the number passed as parameter value, the thread will be blocked until the counter gets the same value or a greater one.

# Fairness in semaphores

The concept of fairness is used by the Java language in all classes that can have various threads blocked and are waiting for the release of a synchronization resource (for example, a semaphore). The default mode is called **non-fair mode**. In this mode, when the synchronization resource is released, one of the waiting threads is selected and is given this resource; however, it's selected without any criteria. Fair mode, on the other hand, changes this behavior and selects the thread that has been waiting for the longest period of time.

As it occurs with other classes, the `Semaphore` class admits a second parameter in its constructor. This parameter must take a Boolean value. If you give it a false value, you are creating a semaphore that will work in non-fair mode. You will get the same behavior if you don't use this parameter. If you give it a true value, you are creating a semaphore that will work in fair mode.

# See also

+   - The Monitoring a Lock interface recipe in Chapter 9, Testing Concurrent Applications
+   - The Synchronizing a block of code with a lock recipe in Chapter 2, Basic Thread Synchronization

# Waiting for multiple concurrent events

The Java concurrency API provides a class that allows one or more threads to wait until a set of operations are made. It's called the `CountDownLatch` class. This class is initialized with an integer number, which is the number of operations the threads are going to wait for. When a thread wants to wait for the execution of these operations, it uses the `await()` method. This method puts the thread to sleep until the operations are completed. When one of these operations finishes, it uses the `countDown()` method to decrement the internal counter of the `CountDownLatch` class. When the counter arrives at 0, the class wakes up all the threads that were sleeping in the `await()` method.