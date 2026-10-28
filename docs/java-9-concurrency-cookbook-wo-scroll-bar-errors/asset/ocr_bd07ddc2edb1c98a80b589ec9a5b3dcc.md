Java provides data collection processes that you can use in your concurrent programs without any problems or inconsistency. Basically, Java provides two kinds of collections to use in concurrent applications:

+   - **Blocking collections**: This kind of collection includes operations to add and remove data. If the operation can't be done immediately, because the collection is either full or empty, the thread that makes the call will be blocked until the operation could be carried out.
+   - **Non-blocking collections**: This kind of collection also includes operations to add and remove data. But in this case, if the operation can't be done immediately, it returns a `null` value or throws an exception; the thread that makes the call won't be blocked here.

Through the recipes in this chapter, you will learn how to use some Java collections in your concurrent applications. These include:

+   - Non-blocking deques, using the `ConcurrentLinkedDeque` class
+   - Blocking deques, using the `LinkedBlockingDeque` class
+   - Blocking queues to be used with producers and consumers of data, using the `LinkedTransferQueue` class
+   - Blocking queues that order elements by priority, using the `PriorityBlockingQueue` class
+   - Blocking queues with delayed elements, using the `DelayQueue` class
+   - Non-blocking navigable maps, using the `ConcurrentSkipListMap` class
+   - Non-blocking hash tables, using the `ConcurrentHashMap` class
+   - Atomic variables, using the `AtomicLong` and `AtomicIntegerArray` classes
+   - Variables stored in fields marked with the `volatile` keyword
+   - Atomic operations on the fields of individual classes, using variable handles.

# Using non-blocking thread-safe deques

"List" is referred to as the most basic collection. It has an undetermined number of elements, and you can add, read, or remove an element from any position. Concurrent lists allow various threads to add or remove elements from the list at a time, without producing any data inconsistency errors. Similar to lists, we have deques. A deque is a data structure similar to a queue, but in a deque, you can add or remove elements from either the front (head) or back (tail).