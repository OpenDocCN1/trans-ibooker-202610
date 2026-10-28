# See also

+   - The *Using non-blocking thread-safe deques* recipe in this chapter

# Using thread-safe HashMaps

A hash table is a data structure that allows you to map a key to a value. Internally, it usually uses an array to store the elements and a hash function to calculate the position of the element in the array, using its key. The main advantage of this data structure is that the insert, delete, and search operations are very fast here, so it's very useful in situations when you have to carry out a lot of search operations.

The Java API provides different hash table implementations through the `Map` and `ConcurrentMap` interfaces. The `ConcurrentMap` interface provides thread-safety and atomic guarantees to all the operations, so you can use them in concurrent applications. The `ConcurrentHashMap` class implements the `ConcurrentMap` interface and adds some more methods to the ones defined in the interface. This class supports the following:

+   - Full concurrency of read operations
+   - High expected concurrency for insert and delete operations

Both the elements (class and interface) were introduced in Java version 5, but in version 8, a lot of new methods similar to the ones provided by the stream API were developed.

In this recipe, you will learn how to use the `ConcurrentHashMap` class in your application and the most important methods it provides.

# Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or a different IDE, such as NetBeans, open it and create a new Java project.