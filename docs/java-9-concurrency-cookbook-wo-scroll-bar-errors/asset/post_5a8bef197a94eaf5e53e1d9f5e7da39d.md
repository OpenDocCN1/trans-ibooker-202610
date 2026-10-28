
2. Then, add a synchronized block of code using the `this` keyword to the `close()` method:

```java
public void close() {
    System.out.printf("Closing accounting");
    long totalAmount;
    synchronized (this) {
        totalAmount = cash;
        cash = 0;
    }
    System.out.printf("The total amount is : %d", totalAmount);
}
```

3. Now add two new attributes to the `ParkingStats` class and initialize them in the constructor of the class:

```java
private final Object controlCars, controlMotorcycles;

public ParkingStats(ParkingCash cash) {
    numberCars = 0;
    numberMotorcycles = 0;
    controlCars = new Object();
    controlMotorcycles = new Object();
    this.cash = cash;
}
```

4. Finally, modify the methods that increment and decrement the number of cars and motorcycles, including the `synchronized` keyword. The `numberCars` attribute will be protected by the `controlCars` object, and the `numberMotorcycles` attribute will be protected by the `controlMotorcycles` object. You must also synchronize the `getNumberCars()` and `getNumberMotorcycles()` methods with the associated reference object:

```java
public void carComeIn() {
    synchronized (controlCars) {
        numberCars++;
    }
}
```

```java
public void carGoOut() {
    synchronized (controlCars) {
        numberCars--;
    }
    cash.vehiclePay();
}
```

```java
public void motoComeIn() {
    synchronized (controlMotorcycles) {
        numberMotorcycles++;
    }
}

public void motoGoOut() {
    synchronized (controlMotorcycles) {
        numberMotorcycles--;
    }
    cash.vehiclePay();
}
```

5. Execute the example now and see the difference when compared to the previous version.

## How it works...

The following screenshot shows the output of the new version of the example. No matter how many times you execute it, you will always obtain the correct result:

![](img/cc06c59cd21f558f43e9475d7f212170_81_0.png)

Let's see the different uses of the `synchronized` keyword in the example:

- First, we protected the `vehiclePay()` method. If two or more `Sensor` tasks call this method at the same time, only one will execute it and the rest will wait for their turn; therefore, the final amount will always be correct.
- We used two different objects to control access to the car and motorcycle counters. This way, one `Sensor` task can modify the `numberCars` attribute and another `Sensor` task can modify the `numberMotorcycles` attribute at the same time; however, no two `Sensor` tasks will be able to modify the same attribute at the same time, so the final value of the counters will always be correct.

For these types of situations, Java provides the `wait()`, `notify()`, and `notifyAll()` methods implemented in the `Object` class. A thread can call the `wait()` method inside a synchronized block of code. If it calls the `wait()` method outside a synchronized block of code, JVM throws an `IllegalMonitorStateException` exception. When the thread calls the `wait()` method, JVM puts the thread to sleep and releases the object that controls the synchronized block of code that it's executing and allows other threads to execute other blocks of synchronized code protected by this object. To wake up the thread, you must call the `notify()` or `notifyAll()` methods inside a block of code protected by the same object.

In this recipe, you will learn how to implement the producer-consumer problem using the `synchronized` keyword and the `wait()`, `notify()`, and `notifyAll()` methods.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

1. Create a class named `EventStorage`. It has two attributes, namely an `int` attribute called `maxSize` and a `List<Date>` attribute called `storage`:

```java
public class EventStorage {

    private int maxSize;
    private Queue<Date> storage;
```

2. Implement the constructor of the class that initializes the attributes of the class:

```java
public EventStorage() {
    maxSize = 10;
    storage = new LinkedList<>();
}
```

5. Create a class named `Producer` and specify that it implements the `Runnable` interface. It will implement the producer of the example:

```java
public class Producer implements Runnable {
```

6. Declare an `EventStorage` object and implement the constructor of the class that initializes this object:

```java
private EventStorage storage;

public Producer(EventStorage storage) {
    this.storage = storage;
}
```

7. Implement the `run()` method that calls the `set()` method of the `EventStorage` object 100 times:

```java
@Override
public void run() {
    for (int i = 0; i < 100; i++) {
        storage.set();
    }
}
```

8. Create a class named `Consumer` and specify that it implements the `Runnable` interface. It will implement the consumer of the example:

```java
public class Consumer implements Runnable {
```

9. Declare an `EventStorage` object and implement the constructor of the class that initializes this object:

```java
private EventStorage storage;

public Consumer(EventStorage storage) {
    this.storage = storage;
}
```

## How it works...

The key to this example is the `set()` and `get()` methods of the `EventStorage` class. First of all, the `set()` method checks whether there is free space in the storage attribute. If it's full, it calls the `wait()` method to wait for free space. When the other thread calls the `notify()` method, this thread wakes up and checks the condition again. The `notify()` method doesn't guarantee that the condition is met. This process is repeated until there is free space in storage and it can generate a new event and store it.

The behavior of the `get()` method is similar. First, it checks whether there are events in the storage attribute. If the storage is empty, it calls the `wait()` method to wait for events. When the other thread calls the `notify()` method, this thread wakes up and checks the condition again until there are some events in storage.

![](img/cc06c59cd21f558f43e9475d7f212170_88_0.png)

You have to keep checking the conditions and calling the `wait()` method in a `while` loop. You will not be able to continue until the condition is `true`.

If you run this example, you will find that although the producer and consumer are setting and getting events, storage never has the capacity to include more than 10 events.

## There's more...

There are other important uses of the `synchronized` keyword. See the See also section of this recipe that explains the use of this keyword.

## See also

- The *Synchronizing a method* recipe in this chapter

## Synchronizing a block of code with a lock

Java provides another mechanism for synchronizing blocks of code. It's a more powerful and flexible mechanism than the `synchronized` keyword. It's based on the `Lock` (of the `java.util.concurrent.locks` package) interface and classes that implement it (as `ReentrantLock`). This mechanism presents some advantages, which are as follows:

- It allows you to structure synchronized blocks in a more flexible way. With the `synchronized` keyword, you only have control over a synchronized block of code in a structured way. However, the `Lock` interface allows you to get more complex structures to implement your critical section.
- The `Lock` interface provides additional functionalities over the `synchronized` keyword. One of the new functionalities is implemented by the `tryLock()` method. This method tries to get control of the lock, and if it can't, because it's used by another thread, it returns `false`. With the `synchronized` keyword, if thread A tries to execute a synchronized block of code when thread B is executing it, thread A is suspended until thread B finishes the execution of the synchronized block. With a lock, you can execute the `tryLock()` method. This method returns a Boolean value indicating whether there is another thread running the code protected by this lock.
- The `ReadWriteLock` interface allows a separation of read and write operations with multiple readers and only one modifier.
- The `Lock` interface offers better performance than the `synchronized` keyword.

The constructor of the `ReentrantLock` class admits a boolean parameter named `fair`; this parameter allows you to control its behavior. The `false` value is the default value and it's called the non-fair mode. In this mode, if some threads are waiting for a lock and the lock has to select one of these threads to get access to the critical section, it randomly selects any one of them. The `true` value is called the fair mode. In this mode, if some threads are waiting for a lock and the lock has to select one to get access to a critical section, it selects the thread that has been waiting for the longest period of time. Take into account that the behavior explained previously is only used in the `lock()` and `unlock()` methods. As the `tryLock()` method doesn't put the thread to sleep if the `Lock` interface is used, the fair attribute doesn't affect its functionality.

In this recipe, you will learn how to use locks to synchronize a block of code and create a critical section using the `Lock` interface and the `ReentrantLock` class that implements it, implementing a program that simulates a print queue. You will also learn how the fair parameter affects the behavior of `Lock`.
