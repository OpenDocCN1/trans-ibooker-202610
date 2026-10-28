# Using variable handles

Variable handles are a new feature of Java 9 that allow you to get a typed reference to a variable (attribute, static field, or array element) in order to access it in different modes. You can, for example, protect access to this variable in a concurrent application by allowing atomic access to the variable. Until now, you could only obtain this behavior with atomic variables, but now, you can use variable handles to obtain the same functionality without using any synchronization mechanism. A variable handle also allows you to get additional access modes to a variable.

In this recipe, you will learn how to obtain and use a variable handle and the benefits you obtain using it.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named `Account` with two double public attributes named `amount` and `unsafeAmount`. Implement the constructor to initialize its values:
+   ```
+   public class Account {
+       public double amount;
+       public double unsafeAmount;
+   
+       public Account() {
+           this.amount=0;
+           this.unsafeAmount=0;
+       }
+   }
+   ```

+   2. Create a class named `Decrementer` and specify that it implements the `Runnable` interface. It has a private `Account` attribute initialized in the constructor of the class:
+   ```
+   public class Decrementer implements Runnable {
+       private Account account;
+   ```