# See also

+   - The *Implementing your own atomic object* recipe in `Chapter 8`, *Customizing Concurrency Classes*

# Holding locks for as short time as possible

Locks, just like other synchronization mechanisms, allow the definition of a critical section that only one thread can execute at a time. You must be very careful to define the critical section. It must only include those instructions that really need mutual exclusion. This is especially true if the critical section includes long operations. If the critical section includes lengthy operations that do not use shared resources, application performance will be worse than it could be.

In this recipe, you will implement an example to see the difference in the performance of a task with a long operation inside the critical section and a task with a long operation outside the critical section.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named `Operations`:
+   
+   ```
+   public class Operations {
+   ```
+   
+   2. Implement a `public static` method named `readData()`. It puts the current thread to sleep for 500 milliseconds:
+   
+   ```
+   public static void readData() {
+       try {
+           TimeUnit.MILLISECONDS.sleep(500);
+       } catch (InterruptedException e) {
+           e.printStackTrace();
+   ```