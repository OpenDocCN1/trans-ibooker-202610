# There's more...

The `DelayQueue` class has other interesting methods, which are as follows:

+   - `clear()`: This method removes all the elements of the queue.
+   - `offer(E e)`: Here, E represents the class used to parameterize the `DelayQueue` class. This method inserts the element that is passed as a parameter into the queue.
+   - `peek()`: This method retrieves but doesn't remove the first element of the queue.
+   - `take()`: This method retrieves and removes the first element of the queue. If there aren't any active elements, the thread that is executing the method will be blocked until the thread gets some active elements.

# See also

+   - The *Using blocking thread-safe deques* recipe in this chapter

# Using thread-safe navigable maps

The `ConcurrentNavigableMap` is an interface that defines interesting data structures provided by the Java API that you can use in your concurrent programs. The classes that implement the `ConcurrentNavigableMap` interface stores elements in two parts:

+   - A **key** that uniquely identifies an element
+   - The rest of the data that defines the element, called **value**

The Java API also provides a class that implements `ConcurrentSkipListMap`, which is the interface that implements a non-blocking list with the behavior of the `ConcurrentNavigableMap` interface. Internally, it uses a **Skip List** to store data. A Skip List is a data structure based on parallel lists that allow us to get the kind of efficiency that is associated with a binary tree. You can get more information about Skip Lists at https://en.wikipedia.org/wiki/Skip_list. With it, you can get a sorted data structure, instead of a sorted list, with better access time to insert, search, or delete elements.

![](bbox=[0.12, 0.85, 0.2, 0.95])

Skip List was introduced by William Pugh in 1990.