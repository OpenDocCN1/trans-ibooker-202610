All the elements you want to add to PriorityBlockingQueue have to implement the Comparable interface; alternatively, you can include Comparator in the queue's constructor. This interface has a method called compareTo() that receives an object of the same type. So you have two objects to compare: the one that is executing the method and the one that is received as a parameter. The method must return a number less than zero if the local object is less than the parameter. It should return a number bigger than zero if the local object is greater than the parameter. The number must be zero if both the objects are equal.

PriorityBlockingQueue uses the compareTo() method when you insert an element in it to determine the position of the element inserted. Bigger elements will either be the tail or head of the queue, depending on the compareTo() method.

Another important characteristic of PriorityBlockingQueue is that it's a blocking data structure. It has methods that, if unable to perform the operation immediately, will block the thread until they are able to do it.

In this recipe, you will learn how to use the PriorityBlockingQueue class by implementing an example, where you are going to store a lot of events with different priorities in the same list, to check that the queue will be ordered as you want.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.

# How to do it...

Follow these steps to implement the example:

+   1. Create a class named Event and specify that it implements the Comparable interface parameterized by the Event class:
+   
+   ```
+   public class Event implements Comparable<Event> {
+   ```
+   2. Declare a private int attribute named thread to store the number of threads that have created the event:
+   
+   ```
+   private final int thread;
+   ```