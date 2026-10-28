# Generating concurrent random numbers

The Java concurrency API provides a specific class to generate pseudorandom numbers in concurrent applications. It's the ThreadLocalRandom class and it's new in Java 7 version. It works as the thread's local variables. Every thread that wants to generate random numbers has a different generator, but all of them are managed from the same class, in a transparent way to the programmer. With this mechanism, you will get a better performance than using a shared Random object to generate the random numbers of all the threads.

In this recipe, you will learn how to use the ThreadLocalRandom class to generate random numbers in a concurrent application.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or any other IDE such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named TaskLocalRandom and specify that it implements the Runnable interface:

```
public class TaskLocalRandom implements Runnable {
```

+   2. Implement the run() method. Get the name of the thread that is executing this task and write 10 random integer numbers to the console using the nextInt() method:

```
@Override
public void run() {
    String name=Thread.currentThread().getName();
    for (int i=0; i<10; i++){
        System.out.printf("%s: %d\n",name,
            ThreadLocalRandom.current().nextInt(10));
    }
}
```