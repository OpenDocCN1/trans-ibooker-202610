# How to do it...

Follow these steps to implement the example:

+   1. Create a class named TaskAtomic and specify that it implements the Runnable interface:
+   ```
+   public class TaskAtomic implements Runnable {
+   ```
+   2. Declare a private AtomicInteger attribute named number:
+   ```
+   private final AtomicInteger number;
+   ```
+   3. Implement the constructor of the class to initialize its attributes:
+   ```
+   public TaskAtomic () {
+       this.number=new AtomicInteger();
+   }
+   ```
+   4. Implement the run() method. In a loop with 1,000,000 steps, assign the number of steps to the atomic attribute as a value, using the set() method:
+   ```
+   @Override
+   public void run() {
+       for (int i=0; i<1000000; i++) {
+           number.set(i);
+       }
+   }
+   ```
+   5. Create a class named TaskLock and specify that it implements the Runnable interface:
+   ```
+   public class TaskLock implements Runnable {
+   ```
+   6. Declare a private int attribute named number and a private Lock attribute named lock:
+   ```
+   private Lock lock;
+   private int number;
+   ```
+   7. Implement the constructor of the class to initialize its attributes:
+   ```
+   public TaskLock() {
+       this.lock=new ReentrantLock();
+   }
+   ```