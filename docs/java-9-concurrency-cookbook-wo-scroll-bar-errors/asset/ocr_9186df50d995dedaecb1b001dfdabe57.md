For these types of situations, Java provides the wait(), notify(), and notifyAll() methods implemented in the Object class. A thread can call the wait() method inside a synchronized block of code. If it calls the wait() method outside a synchronized block of code, JVM throws an IllegalMonitorStateException exception. When the thread calls the wait() method, JVM puts the thread to sleep and releases the object that controls the synchronized block of code that it's executing and allows other threads to execute other blocks of synchronized code protected by this object. To wake up the thread, you must call the notify() or notifyAll() methods inside a block of code protected by the same object.

In this recipe, you will learn how to implement the producer-consumer problem using the synchronized keyword and the wait(), notify(), and notifyAll() methods.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named EventStorage. It has two attributes, namely an int attribute called maxSize and a List<Date> attribute called storage:

```
public class EventStorage {

private int maxSize;
private Queue<Date> storage;
```

+   2. Implement the constructor of the class that initializes the attributes of the class:

```
public EventStorage() {
maxSize=10;
storage=new LinkedList<>();
}
```