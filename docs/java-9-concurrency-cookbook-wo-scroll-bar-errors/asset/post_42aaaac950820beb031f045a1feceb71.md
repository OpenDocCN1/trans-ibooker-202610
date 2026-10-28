
You can see how the students finished the first exercise at different times. When all of them finish the first exercise, `phaser` calls the `onAdvance()` method that writes the log messages in the console, then all the students start the second exercise at the same time.

## See also

- The *Running concurrent-phased tasks* recipe in this chapter
- The *Monitoring a Phaser class* recipe in `Chapter 9`, *Testing Concurrent Applications*

## Exchanging data between concurrent tasks

The Java concurrency API provides a synchronization utility that allows interchange of data between two concurrent tasks. In more detail, the `Exchanger` class allows you to have a definition of a synchronization point between two threads. When the two threads arrive at this point, they interchange a data structure such that the data structure of the first thread goes to the second one and vice versa.

This class may be very useful in a situation similar to the producer-consumer problem. This is a classic concurrent problem where you have a common buffer of data, one or more producers of data, and one or more consumers of data. As the `Exchanger` class synchronizes only two threads, you can use it if you have a producer-consumer problem with one producer and one consumer.

In this recipe, you will learn how to use the `Exchanger` class to solve the producer-consumer problem with one producer and one consumer.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. First, begin by implementing the producer. Create a class named `Producer` and specify that it implements the `Runnable` interface:
  
  ```
  public class Producer implements Runnable {
  ```
- 2. Declare a `List<String>` field named `buffer`. This will be the data structure that the producer will interchange with the consumer:
  
  ```
  private List<String> buffer;
  ```
- 3. Declare an `Exchanger<List<String>>` field named `exchanger`. This will be the exchanger object that will be used to synchronize the producer and consumer:
  
  ```
  private final Exchanger<List<String>> exchanger;
  ```
- 3. Implement the constructor of the class that will initialize the two attributes:
  
  ```
  public Producer (List<String> buffer, Exchanger<List<String>>
      exchanger){
      this.buffer=buffer;
      this.exchanger=exchanger;
  }
  ```
- 4. Implement the `run()` method. Inside it, implement 10 cycles of interchange:
  
  ```
  @Override
  public void run() {
      for (int cycle = 1; cycle <= 10; cycle++){
          System.out.printf("Producer: Cycle %d\n",cycle);
  ```
- 5. In each cycle, add 10 strings to the buffer:
  
  ```
  for (int j=0; j<10; j++){
      String message="Event "+((cycle-1)*10)+j);
      System.out.printf("Producer: %s\n",message);
      buffer.add(message);
  }
  ```

## Thread Synchronization Utilities

12. In each cycle, begin with a call to the `exchange()` method to synchronize with the producer. The consumer needs data to consume. As this method can throw an `InterruptedException` exception, you have to add some code to process it:

```
```java
try {
    buffer=exchanger.exchange(buffer);
} catch (InterruptedException e) {
    e.printStackTrace();
}
```
```

13. Write the 10 strings the producer sent in its buffer to the console and delete them from the buffer to leave it empty:

```
```java
System.out.println("Consumer: "+buffer.size());
for (int j=0; j<10; j++) {
    String message=buffer.get(0);
    System.out.println("Consumer: "+message);
    buffer.remove(0);
}
```
```

14. Now, implement the `main` class of the example by creating a class named `Main` and adding the `main()` method to it:

```
```java
public class Main {
    public static void main(String[] args) {
```
```

15. Create two buffers that will be used by the producer and consumer:

```
```java
List<String> buffer1=new ArrayList<>();
List<String> buffer2=new ArrayList<>();
```
```

16. Create the `Exchanger` object that will be used to synchronize the producer and consumer:

```
```java
Exchanger<List<String>> exchanger=new Exchanger<>();
```
```

17. Create the `Producer` and `Consumer` objects:

```
```java
Producer producer=new Producer(buffer1, exchanger);
Consumer consumer=new Consumer(buffer2, exchanger);
```
```

18. Create the threads to execute the producer and consumer and start the threads:

```
Thread threadProducer=new Thread(producer);
Thread threadConsumer=new Thread(consumer);

threadProducer.start();
threadConsumer.start();
```

## How it works...

The consumer begins with an empty buffer and calls Exchanger to synchronize with the producer. It needs data to consume. The producer begins its execution with an empty buffer. It creates 10 strings, stores them in the buffer, and uses the Exchanger to synchronize with the consumer.

At this point, both the threads (producer and consumer) are in Exchanger, which changes the data structures. So when the consumer returns from the exchange() method, it will have a buffer with 10 strings. When the producer returns from the exchange() method, it will have an empty buffer to fill again. This operation will be repeated 10 times.

If you execute the example, you will see how producer and consumer do their jobs concurrently and how the two objects interchange their buffers in every step. As it occurs with other synchronization utilities, the first thread that calls the exchange() method is put to sleep until the other threads arrive.

## There's more...

The Exchanger class has another version of the exchange method: exchange(V data, long time, TimeUnit unit). where, V is the type used as a parameter in the declaration of Phaser (List<String> in our case). The thread will sleep until it's interrupted, another thread arrives, or the specified time passes. In this case, a TimeoutException is thrown. The TimeUnit class is an enumeration with the following constants: DAYS, HOURS, MICROSECONDS, MILLISECONDS, MINUTES, NANOSECONDS, and SECONDS.

## Completing and linking tasks asynchronously

Java 8 Concurrency API includes a new synchronization mechanism with the `CompletableFuture` class. This class implements the `Future` object and the `CompletionStage` interface that gives it the following two characteristics:

- As the `Future` object, a `CompletableFuture` object will return a result sometime in future
- As the `CompletionStage` object, you can execute more asynchronous tasks after the completion of one or more `CompletableFuture` objects

You can work with a `CompletableFuture` class in different ways:

- You can create a `CompletableFuture` object explicitly and use it as a synchronization point between tasks. One task will establish the value returned by `CompletableFuture`, using the `complete()` method, and the other tasks will wait for this value, using the `get()` or `join()` methods.
- You can use a static method of the `CompletableFuture` class to execute `Runnable` or `Supplier` with the `runAsync()` and `supplyAsync()` methods. These methods will return a `CompletableFuture` object that will be completed when these tasks end their execution. In the second case, the value returned by `Supplier` will be the completion value of `CompletableFuture`.
- You can specify other tasks to be executed in an asynchronous way after the completion of one or more `CompletableFuture` objects. This task can implement the `Runnable`, `Function`, `Consumer` or `BiConsumer` interfaces.

## How to do it...

Follow these steps to implement the example:

- 1. First, we're going to implement the auxiliary tasks we will use in the example. Create a class named `SeedGenerator` that implements the `Runnable` interface. It will have a `CompletableFuture` object as an attribute, and it will be initialized in the constructor of the class:
  
  ```java
  public class SeedGenerator implements Runnable {
      private CompletableFuture<Integer> resultCommunicator;
      public SeedGenerator (CompletableFuture<Integer> completable) {
          this.resultCommunicator=completable;
      }
  }
  ```
- 2. Then, implement the `run()` method. It will sleep the current thread for 5 seconds (to simulate a long operation), calculate a random number between 1 and 10, and then use the `complete()` method of the `resultCommunicator` object to complete CompletableFuture:
  
  ```java
  @Override
  public void run() {
      System.out.printf("SeedGenerator: Generating seed...\n");
      // Wait 5 seconds
      try {
          TimeUnit.SECONDS.sleep(5);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      int seed=(int) Math.rint(Math.random() * 10);
      System.out.printf("SeedGenerator: Seed generated: %d\n", seed);
      resultCommunicator.complete(seed);
  }
  ```
- 3. Create a class named `NumberListGenerator` that implements the `Supplier` interface parameterized with the `List<Long>` data type. This means that the `get()` method provided by the `Supplier` interface will return a list of large numbers. This class will have an integer number as a private attribute, which will be initialized in the constructor of the class:
