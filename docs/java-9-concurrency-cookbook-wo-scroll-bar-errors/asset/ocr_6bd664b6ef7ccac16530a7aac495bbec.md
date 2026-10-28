The ForkJoinWorkerThread adds new methods to the Thread class, such as the onStart() method that's executed when the thread is created and the onTermination() method that's called to clean up the resources used by the thread. The ForkJoinPool class uses an implementation of the ForkJoinWorkerThreadFactory interface to create the worker threads it uses.

In this recipe, you will learn how to implement a customized worker thread to be used in a ForkJoinPool class and how to use it with a factory extending the ForkJoinPool class and implementing the ForkJoinWorkerThreadFactory interface.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named MyWorkerThread that extends the ForkJoinWorkerThread class:
+   ```
+   public class MyWorkerThread extends ForkJoinWorkerThread {
+   ```
+   2. Declare and create a private ThreadLocal attribute parameterized by the Integer class named taskCounter:
+   ```
+   private final static ThreadLocal<Integer> taskCounter=
+       new ThreadLocal<Integer>();
+   ```
+   3. Implement a constructor of the class:
+   ```
+   protected MyWorkerThread(ForkJoinPool pool) {
+       super(pool);
+   }
+   ```