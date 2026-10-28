# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named `Task` and specify that it implements the `Runnable` interface:
+   ```
+   public class Task implements Runnable {
+   ```
+   2. Declare a private `String` attribute called `name` that will store the name of the task:
+   ```
+   private final String name;
+   ```
+   3. Implement the constructor of the class that initializes the attribute:
+   ```
+   public Task(String name) {
+       this.name=name;
+   }
+   ```
+   4. Implement the `run()` method. Write a message to the console with the actual date to verify that the task is executed within the specified period:
+   ```
+   @Override
+   public void run() {
+       System.out.printf("%s: Executed at: %s\n",name,new Date());
+   }
+   ```
+   5. Implement the main class of the example by creating a class named `Main` and adding the `main()` method to it:
+   ```
+   public class Main {
+       public static void main(String[] args) {
+   ```
+   6. Create `ScheduledExecutorService` using the `newScheduledThreadPool()` method of the `Executors` class. Pass 1 to this method as a parameter:
+   ```
+   ScheduledExecutorService executor=Executors
+       .newScheduledThreadPool(1);
+   ```