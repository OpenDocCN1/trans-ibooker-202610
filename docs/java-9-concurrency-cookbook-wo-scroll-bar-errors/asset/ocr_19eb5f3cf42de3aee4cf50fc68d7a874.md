# See also

+   - The *Using atomic variables* recipe in `Chapter 7`, *Concurrent Collections*

# Implementing your own stream generator

A stream is a sequence of data that allows you to apply a sequence of operations (usually represented with lambda expressions) to it in a sequential or parallel way in order to filter, transform, sort, reduce, or construct a new data structure. It was introduced in Java 8 and was one of the most important features introduced in that version.

Streams are based on the `Stream` interface and some related classes and interfaces included in the `java.util.stream` package. They have also provoked the introduction of new methods in a lot of classes to generate streams from different data structures. You can create a `Stream` interface from every data structure that implements the `Collection` interface: from `File`, `Directory`, `Array`, and a lot of other sources.

Java also included different mechanisms to create streams from your own sources. The most important ones are:

+   - The `Supplier` interface: This interface defines the `get()` method. It will be called by `Stream` when it needs to process another object. You can create `Stream` from a `Supplier` interface using the `generate()` static method of the `Stream` class. Take into account that this source is potentially infinite, so you must use a method such as `limit()` or similar to limit the number of elements in `Stream`.
+   - The `Stream.Builder` interface: This interface provides the `accept()` and `add()` elements to add elements to `Stream` and the `build()` method, which returns the `Stream` interface created with the elements added before.
+   - The `Spliterator` interface: This interface defines the necessary methods to traverse and split the elements of a source. You can use the `stream()` method of the `StreamSupport` class to generate the `Stream` interface to process the elements of `Spliterator`.

In this chapter, you will learn how to implement your own `Spliterator` interface and how to create a `Stream` interface to process its data. We will work with a matrix of elements. A normal `Stream` interface should process one element at a time, but we will use the `Spliterator` class to implement one row at a time.