
@Override
public void subscribe(Subscriber<? super News> subscriber) {

    ConsumerData consumerData=new ConsumerData();
    consumerData.setConsumer((Consumer)subscriber);

    MySubscription subscription=new MySubscription();
    consumerData.setSubscription(subscription);

    subscriber.onSubscribe(subscription);

    consumers.add(consumerData);
}

16. Now implement the publish() method. This method receives a News parameter and sends it to the subscribers that meet the conditions explained before. To do this, we create a PublisherTask method per Subscriber and send these tasks to the executor:

```
public void publish(News news) {
    consumers.forEach( consumerData -> {
        try {
            executor.execute(new PublisherTask(consumerData, news));
        } catch (Exception e) {
            consumerData.getConsumer().onError(e);
        }
    });
}
```

17. Finally, implement the Main class of the example with its main() method. We create a publisher and two subscribers and subscribe them to the publisher:

```
public class Main {

    public static void main(String[] args) {

        MyPublisher publisher=new MyPublisher();

        Subscriber<News> consumer1, consumer2;
        consumer1=new Consumer("Consumer 1");
        consumer2=new Consumer("Consumer 2");

        publisher.subscribe(consumer1);
        publisher.subscribe(consumer2);
```

18. Then, create a News object, send it to the publisher, sleep the main thread for a second, create another News object, and send it to the publisher again:

```
System.out.printf("Main: Start\n");

News news=new News();
news.setTitle("My first news");
news.setContent("This is the content");
news.setDate(new Date());

publisher.publish(news);

try {
    TimeUnit.SECONDS.sleep(1);
} catch (InterruptedException e) {
    e.printStackTrace();
}

news=new News();
news.setTitle("My second news");
news.setContent("This is the content of the second news");
news.setDate(new Date());
publisher.publish(news);

System.out.printf("Main: End\n");
}
```

## How it works...

In this example, we implemented a reactive streams communication between a publisher and a subscriber using the interfaces provided by the Java 9 API and just followed the expected behavior defined in the reactive streams' specification.

We had a publisher implemented by the MyPublisher class and subscribers implemented by the Consumer class. There are subscriptions between the publishers, and each subscriber is implemented by the MySubscription object.

The cycle of the communication starts when a subscriber calls the `subscribe()` method of a publisher. The publisher has to create the subscription between them and send that subscription to the subscriber using the `onSubscribe()` method. The subscriber must use the `request()` method of the subscription to indicate that it's ready to process more elements from the publisher. When the publisher publishes an item, it will send it to all its subscribers who have requested elements from the publisher using the subscription between them.

We added all the necessary elements to guarantee this behavior in a concurrent way.

The following screenshot shows the output of an execution of this example:

![](img/cc06c59cd21f558f43e9475d7f212170_451_0.png)

## There's more...

The easiest way to create an application that uses reactive streams is to use the `SubmissionPublisher` class. This class implements the `Publisher` interface and provides the necessary methods to use it as the publisher part of the application.

## See also

- *The Reactive programming with reactive streams* recipe in `Chapter 6`, *Parallel and Reactive Streams*

## 9

## Testing Concurrent Applications

In this chapter, we will cover the following topics:

- Monitoring a Lock interface
- Monitoring a Phaser class
- Monitoring an Executor framework
- Monitoring a fork/join pool
- Monitoring a stream
- Writing effective log messages
- Analyzing concurrent code with FindBugs
- Configuring Eclipse for debugging concurrency code
- Configuring NetBeans for debugging concurrency code
- Testing concurrency code with MultithreadedTC
- Monitoring with JConsole

## Introduction

Testing an application is a critical task. Before you make an application ready for end users, you have to demonstrate its correctness. You use a test process to prove that correctness is achieved and errors are fixed. Testing is a common task in any software development and quality assurance process. You can find a lot of literature about testing processes and the different approaches you can apply to your developments. There are a lot of libraries as well, such as JUnit, and applications, such as Apache JMeter, that you can use to test your Java applications in an automated way. Testing is even more critical in concurrent application development.

The fact that concurrent applications have two or more threads that share data structures and interact with each other adds more difficulty to the testing phase. The biggest problem you will face when you test concurrent applications is that the execution of threads is non-deterministic. You can't guarantee the order of the execution of threads, so it's difficult to reproduce errors.

## Monitoring a Lock interface

A `Lock` interface is one of the basic mechanisms provided by the Java concurrency API to synchronize a block of code. It allows you to define a **critical section**. A critical section is a block of code that accesses a shared resource and can't be executed by more than one thread at the same time. This mechanism is implemented by the `Lock` interface and the `ReentrantLock` class.

In this recipe, you will learn what information you can obtain about a `Lock` object and how to obtain that information.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. Create a class named MyLock that extends the ReentrantLock class:

```
public class MyLock extends ReentrantLock {
```

- 2. Implement getOwnerName(). This method returns the name of the thread that has control of a lock (if any), using the protected method of the Lock class called getOwner():

```
public String getOwnerName() {
    if (this.getOwner()==null) {
        return "None";
    }
    return this.getOwner().getName();
}
```

- 3. Implement getThreads(). This method returns a list of threads queued in a lock, using the protected method of the Lock class called getQueuedThreads():

```
public Collection<Thread> getThreads() {
    return this.getQueuedThreads();
}
```

- 4. Create a class named Task that implements the Runnable interface:

```
public class Task implements Runnable {
```

- 5. Declare a private Lock attribute named lock:

```
private final Lock lock;
```

- 6. Implement a constructor of the class to initialize its attribute:

```
public Task (Lock lock) {
    this.lock=lock;
}
```

- 7. Implement the run() method. Create a loop with five steps:

```
@Override
public void run() {
    for (int i=0; i<5; i++) {
```

- 8. Acquire the lock using the lock() method and print a message:

```
lock.lock();
System.out.printf("%s: Get the Lock.\n",
                Thread.currentThread().getName());
```

- 9. Put the thread to sleep for 500 milliseconds. Free the lock using the unlock() method and print a message:

```
try {
    TimeUnit.MILLISECONDS.sleep(500);
    System.out.printf("%s: Free the Lock.\n",
        Thread.currentThread().getName());
} catch (InterruptedException e) {
    e.printStackTrace();
} finally {
    lock.unlock();
}
}
```

- 10. Create the main class of the example by creating a class named Main with a main() method:

```
public class Main {
    public static void main(String[] args) throws Exception {
```

- 11. Create a MyLock object named lock:

```
MyLock lock=new MyLock();
```

- 12. Create an array of five Thread objects:

```
Thread threads[]=new Thread[5];
```

- 13. Create and start five threads to execute five Task objects:

```
for (int i=0; i<5; i++) {
    Task task=new Task(lock);
    threads[i]=new Thread(task);
    threads[i].start();
}
```

- 14. Create a loop with 15 steps:

```
for (int i=0; i<15; i++) {
```

- 15. Write the name of the owner of the lock in the console:

```
System.out.printf("Main: Logging the Lock\n");
System.out.printf("***********************\n");
System.out.printf("Lock: Owner : %s\n",lock.getOwnerName());
```
