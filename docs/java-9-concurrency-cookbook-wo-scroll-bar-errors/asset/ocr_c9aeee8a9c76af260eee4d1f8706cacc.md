# Waiting for the finalization of a thread

In some situations, we will have to wait for the end of the execution of a thread (the `run()` method ends its execution). For example, we may have a program that will begin initializing the resources it needs before proceeding with the rest of the execution. We can run initialization tasks as threads and wait for their finalization before continuing with the rest of the program.

For this purpose, we can use the `join()` method of the `Thread` class. When we call this method using a thread object, it suspends the execution of the calling thread until the object that is called finishes its execution.

In this recipe, we will learn the use of this method with an initialization example.

# Getting ready

The example for this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class called `DataSourcesLoader` and specify that it implements the `Runnable` interface:
+   ```
+   public class DataSourcesLoader implements Runnable {
+   ```
+   2. Implement the `run()` method. It writes a message to indicate that it starts its execution, sleeps for 4 seconds, and writes another message to indicate that it ends its execution:
+   ```java
+   @Override
+   public void run() {
+       System.out.printf("Beginning data sources loading: %s\n", new Date());
+       try {
+           TimeUnit.SECONDS.sleep(4);
+       } catch (InterruptedException e) {
+           e.printStackTrace();
+       }
+   ```