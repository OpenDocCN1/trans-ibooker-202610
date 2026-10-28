
6. Create the main class of the example. Create a class called `Main` and implement the `main()` method:

```
public class Main {
    public static void main(String[] args) {
```
7. Create a `MyThreadFactory` object and a `Task` object:

```
MyThreadFactory factory=new MyThreadFactory("MyThreadFactory");
Task task=new Task();
```
8. Create 10 `Thread` objects using the `MyThreadFactory` object and start them:

```
Thread thread;
System.out.printf("Starting the Threads\n");
for (int i=0; i<10; i++){
    thread=factory.newThread(task);
    thread.start();
}
```
9. Write the statistics of the thread factory in the console:

```
System.out.printf("Factory stats:\n");
System.out.printf("%s\n",factory.getStats());
```
10. Run the example and see the results.

## How it works...

The `ThreadFactory` interface has only one method, called `newThread()`. It receives a `Runnable` object as a parameter and returns a `Thread` object. When you implement a `ThreadFactory` interface, you have to implement it and override the `newThread` method. The most basic `ThreadFactory` has only one line:

```
return new Thread(r);
```

## 2

## Basic Thread Synchronization

In this chapter, we will cover the following topics:
- Synchronizing a method
- Using conditions in synchronized code
- Synchronizing a block of code with a lock
- Synchronizing data access with read/write locks
- Using multiple conditions in a lock
- Advanced locking with the StampedLock class

## Introduction

One of the most common situations in concurrent programming occurs when more than one execution thread shares a resource. In a concurrent application, it is normal for multiple threads to read or write the same data structure or have access to the same file or database connection. These shared resources can provoke error situations or data inconsistency, and we have to implement mechanisms to avoid these errors. These situations are called race conditions and they occur when different threads have access to the same shared resource at the same time. Therefore, the final result depends on the order of the execution of threads, and most of the time, it is incorrect. You can also have problems with change visibility. So if a thread changes the value of a shared variable, the changes would only be written in the local cache of that thread; other threads will not have access to the change (they will only be able to see the old value).

The solution for these problems lies in the concept of critical section. A critical section is a block of code that accesses a shared resource and can't be executed by more than one thread at the same time.

To help programmers implement critical sections, Java (and almost all programming languages) offers synchronization mechanisms. When a thread wants access to a critical section, it uses one of these synchronization mechanisms to find out whether there is any other thread executing the critical section. If not, the thread enters the critical section. If yes, the thread is suspended by the synchronization mechanism until the thread that is currently executing the critical section ends it. When more than one thread is waiting for a thread to finish the execution of a critical section, JVM chooses one of them and the rest wait for their turn. This chapter presents a number of recipes that will teach you how to use the two basic synchronization mechanisms offered by the Java language:
- The `synchronized` keyword
- The `Lock` interface and its implementations

## Synchronizing a method

In this recipe, you will learn how to use one of the most basic methods of synchronization in Java, that is, the use of the `synchronized` keyword to control concurrent access to a method or a block of code. All the `synchronized` sentences (used on methods or blocks of code) use an object reference. Only one thread can execute a method or block of code protected by the same object reference.

When you use the `synchronized` keyword with a method, the object reference is implicit. When you use the `synchronized` keyword in one or more methods of an object, only one execution thread will have access to all these methods. If another thread tries to access any method declared with the `synchronized` keyword of the same object, it will be suspended until the first thread finishes the execution of the method. In other words, every method declared with the `synchronized` keyword is a critical section, and Java only allows the execution of one of the critical sections of an object at a time. In this case, the object reference used is the own object, represented by the `this` keyword. Static methods have a different behavior. Only one execution thread will have access to one of the static methods declared with the `synchronized` keyword, but a different thread can access other non-static methods of an object of that class. You have to be very careful with this point because two threads can access two different `synchronized` methods if one is static and the other is not. If both methods change the same data, you can have data inconsistency errors. In this case, the object reference used is the class object.

When you use the `synchronized` keyword to protect a block of code, you must pass an object reference as a parameter. Normally, you will use the `this` keyword to reference the object that executes the method, but you can use other object references as well. Normally, these objects will be created exclusively for this purpose. You should keep the objects used for synchronization private. For example, if you have two independent attributes in a class shared by multiple threads, you must synchronize access to each variable; however, it wouldn't be a problem if one thread is accessing one of the attributes and the other accessing a different attribute at the same time. Take into account that if you use the own object (represented by the `this` keyword), you might interfere with other synchronized code (as mentioned before, the `this` object is used to synchronize the methods marked with the `synchronized` keyword).

In this recipe, you will learn how to use the `synchronized` keyword to implement an application simulating a parking area, with sensors that detect the following: when a car or a motorcycle enters or goes out of the parking area, an object to store the statistics of the vehicles being parked, and a mechanism to control cash flow. We will implement two versions: one without any synchronization mechanisms, where we will see how we obtain incorrect results, and one that works correctly as it uses the two variants of the `synchronized` keyword.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:
1. First, create the application without using any synchronization mechanism. Create a class named `ParkingCash` with an internal constant and an attribute to store the total amount of money earned by providing this parking service:

```
public class ParkingCash {
    private static final int cost=2;
    private long cash;

    public ParkingCash() {
        cash=0;
    }
}
```

```
public void motoComeIn() {
    numberMotorcycles++;
}

public void motoGoOut() {
    numberMotorcycles--;
    cash.vehiclePay();
}
```

6. Finally, implement two methods to obtain the number of cars and motorcycles in the parking area, respectively.

7. Create a class named Sensor that will simulate the movement of vehicles in the parking area. It implements the Runnable interface and has a ParkingStats attribute, which will be initialized in the constructor:

```
public class Sensor implements Runnable {

    private ParkingStats stats;

    public Sensor(ParkingStats stats) {
        this.stats = stats;
    }
}
```

8. Implement the run() method. In this method, simulate that two cars and a motorcycle arrive in and then leave the parking area. Every sensor will perform this action 10 times:

```
@Override
public void run() {
    for (int i = 0; i < 10; i++) {
        stats.carComeIn();
        stats.carComeIn();
        try {
            TimeUnit.MILLISECONDS.sleep(50);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        stats.motoComeIn();
        try {
            TimeUnit.MILLISECONDS.sleep(50);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```
