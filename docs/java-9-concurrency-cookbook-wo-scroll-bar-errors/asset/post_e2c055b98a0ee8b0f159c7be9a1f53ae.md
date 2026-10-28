
16. Create and launch 100 Producer objects and store the threads in the array created earlier:

```
for (int i=0; i<THREADS; i++) {
    Producer producer=new Producer("Producer: "+ i , buffer);
    producerThreads[i]=new Thread(producer);
    producerThreads[i].start();
}
```

17. Wait for the finalization of the threads using the join() method:

```
for (int i=0; i<THREADS; i++){
    try {
        producerThreads[i].join();
        consumerThreads[i].join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

18. Write a message to the console with the size of the buffer:

```
System.out.printf("Main: Size of the buffer: %d\n", buffer.size());
System.out.printf("Main: End of the example\n");
```

## How it works...

In this recipe, you have used the LinkedTransferQueue class parameterized with the String class to implement the producer/consumer problem. This LinkedTransferQueue class is used as a buffer to share the data between producers and consumers.

You have implemented a Producer class that adds strings to the buffer using the put() method. You have executed 100 producers and every producer inserts in the buffer 10,000 strings, so you insert 1,000,000 strings in the buffer. The put() method adds the element at the end of the buffer.

You also have implemented a Consumer class, which gets a string from the buffer using the take() method. This method returns and deletes the first element of the buffer. If the buffer is empty, the method blocks the thread that makes the call until there are strings in the buffer to consume. You have executed 100 consumers, and every consumer gets 10,000 strings from the buffer.

In the example, first, you have launched the consumers and then the producers, so, as the buffer is empty, all the consumers will be blocked until the producers begin their execution and stores strings in the list.

The following screenshot shows part of the output of an execution of this example:

```
<terminated> Main (71) [Java Application] C:\Program Files\Java\jdk-9\bin\javaw.exe (20 nov. 2016 1:22:08)
Producer: Producer: 62: Producer done
Producer: Producer: 60: Producer done
Producer: Producer: 57: Producer done
Main: Size of the buffer: 0
Main: End of the example
```

To write the number of elements of the buffer, you have used the size() method. You have to take into account that this method can return a value that is not real, if you use them when there are threads adding or deleting data in the list. The method has to traverse the entire list to count the elements and the contents of the list can change for this operation. Only if you use them when there aren't any threads modifying the list, you will have the guarantee that the returned result is correct.

## There's more...

The LinkedTransferQueue class provides other useful methods. The following are some of them:

- getWaitingConsumerCount(): This method returns the number of consumers that are blocked in the take() method or poll (long timeout, TimeUnit unit) because the LinkedTransferQueue object is empty.
- hasWaitingConsumer(): This method returns true if the LinkedTransferQueue object has consumers waiting, or false otherwise.
- offer(E e): This method adds the element passed as a parameter at the end of the LinkedTransferQueue object and returns the true value. E represents the class used to parameterize the declaration of the LinkedTransferQueue class or a subclass of it.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or any other IDE such as NetBeans, open it and create a new Java project.

## How to do it...

Perform the following steps to implement the example:

- 1. Create a class named Task that implements the Runnable interface:
- 2. Implement the run() method of the task:
- 3. Create a loop with 100 steps:
- 4. In each step, put the thread to sleep for 100 milliseconds:
- 5. Write a message in the console with the name of the thread and the step number:
- 6. Create the main class of the example. Create a class named Main with a main() method:

```
public class Task implements Runnable {
```

```
@Override
public void run() {
```

```
for (int i=0; i<100; i++) {
```

```
try {
    TimeUnit.MILLISECONDS.sleep(100);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

```
System.out.printf("%s: %d\n",Thread.currentThread()
    .getName(),i);
}
```

```
public class Main {
    public static void main(String[] args) throws Exception{
```

13. Put the thread to sleep for one second and close the loop and the class:

```
TimeUnit.SECONDS.sleep(1);
}
}
```

## How it works...

In this recipe, you have used the following methods to get information about a Thread class:

- getId(): This method returns the ID of a thread. It's a unique long number and it can't be changed.
- getName(): This method returns the name of a thread. If you don't establish the name of the thread, Java gives it a default name.
- getPriority(): This method returns the priority of execution of a thread. Threads with higher priority are executed in preference to threads with lower priority. It's an int value that has a value between the MIN_PRIORITY and MAX_PRIORITY constants of the Thread class. By default, threads are created with the same priority that specified by the constant NORM_PRIORITY of the Thread class.
- getState(): This method returns the status of a thread. It's a Thread.State object. The Thread.State enumeration has all the possible states of a thread.
- getThreadGroup(): This method returns the ThreadGroup object of a thread. By default, threads belong to the same thread group, but you can establish a different one in the constructor of a thread.
- getStackTrace(): This method returns an array of StackTraceElement objects. Each of these objects represent a call to a method that begins with the run() method of a thread and includes all the methods that have been called until the actual execution point. When a new method is called, a new stack trace element is added to the array. When a method finishes its execution, its stack trace element is removed from the array.

## There's more...

The Thread class includes other methods that provide information about it that can be useful. These methods are as follows:

- `activeCount()`: This method returns the number of active threads in a group of threads.
- `dumpStack()`: This method prints the stack trace of a thread to the standard error output.

## See also

- The Creating, running, and setting the characteristics of a thread recipe in Chapter 1, Thread Management
- The Using a ThreadFactory interface in an Executor framework and Implementing a ThreadFactory interface to generate custom threads for the fork/join framework recipes in Chapter 8, Customizing Concurrency Classes

## Monitoring a Semaphore class

A semaphore is a counter that protects the access to one or more shared resources.

![](img/cc06c59cd21f558f43e9475d7f212170_528_0.png)

The concept of semaphore was introduced by Edsger Dijkstra in 1965 and was used for the first time in the THEOS operating system.

When a thread wants to use shared resources, it must acquire a semaphore. If the internal counter of the semaphore is greater than 0, the semaphore decrements the counter and allows the access to the shared resource. If the counter of the semaphore is 0, the semaphore blocks the thread until the counter is greater than 0. When the thread has finished using the shared resource, it must release the semaphore. That operation increases the internal counter of the semaphore.

In Java, semaphores are implemented in the Semaphore class.

In this recipe, you will learn what information you can obtain about the status of a semaphore and how to obtain it.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or any other IDE such as NetBeans, open it and create a new Java project.

## How to do it...

Perform the following steps to implement the example:

- 1. Create a class named Task that implements the Runnable interface:
  ```
  public class Task implements Runnable {
  ```
- 2. Declare a private Semaphore attribute named semaphore:
  ```
  private final Semaphore semaphore;
  ```
- 3. Implement the constructor of the class to initialize its attribute:
  ```
  public Task(Semaphore semaphore){
      this.semaphore=semaphore;
  }
  ```
- 4. Implement the run() method. First, acquire permit for the semaphore attribute writing a message in the console to indicate that circumstance:
  ```
  @Override
  public void run() {
      try {
          semaphore.acquire();
          System.out.printf("%s: Get the semaphore.\n",
              Thread.currentThread().getName());
  ```
- 5. Then, put the thread to sleep for two seconds using the sleep() method. Finally, release the permit and write a message in the console to indicate that circumstance:
  ```
  TimeUnit.SECONDS.sleep(2);
  System.out.println(Thread.currentThread().getName()+":
      Release the semaphore.");
  } catch (InterruptedException e) {
      e.printStackTrace();
  } finally {
      semaphore.release();
  }
  ```
