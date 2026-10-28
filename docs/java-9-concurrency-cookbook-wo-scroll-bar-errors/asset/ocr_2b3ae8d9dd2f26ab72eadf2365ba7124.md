# Using atomic variables instead of synchronization

When you have to share data between multiple threads, you have to protect access to that piece of data using a synchronization mechanism. You can use the `synchronized` keyword in the declaration of the method that modifies the data so that only one thread can modify data at a time. Another possibility is the utilization of a `Lock` class to create a critical section with instructions that modify data.

Since version 5, Java includes atomic variables. When a thread is doing an operation with an atomic variable, the implementation of the class includes a mechanism to check that the operation is done in one step. Basically, the operation gets the value of the variable, changes the value in a local variable, and then tries to change the old value with the new one. If the old value is still the same, it does the change. If not, the method begins the operation again. Java provides the following types of atomic variables:

+   - `AtomicBoolean`
+   - `AtomicInteger`
+   - `AtomicLong`
+   - `AtomicReference`

In some cases, Java's atomic variables offer a better performance than solutions based on synchronization mechanisms (specially when we care about atomicity within each separate variable). Some classes of the `java.util.concurrent` package use atomic variables instead of synchronization. In this recipe, you will develop an example that shows how an atomic attribute provides better performance than synchronization.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.