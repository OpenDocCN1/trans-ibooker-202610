Collector has a set of Characteristics that define its behavior and can be defined or not for a specific collector. For us, the most important is the CONCURRENT one that indicates if the collector can work in a concurrent way or not. In this case, we can't take advantage of our multicore processor by creating only a parallel stream. If we use a collect operation with Collector, we have to also take into account the value of the CONCURRENT characteristic of that Collector. We will only have a concurrent reduction if the next three conditions are true:

+   - The Stream is parallel (we have used parallelStream() of the parallel() methods in the stream)
+   - The collector has the CONCURRENT characteristic
+   - Either the stream is unordered, or the collector has the UNORDERED characteristic

In our case, groupingByConcurrent() and toConcurrentMap() return collectors which have the CONCURRENT characteristic and the joining() and partitionBy() methods return collectors that don't have such characteristics.

However, there's another version of the collect() method that can be used with parallel streams. This version of the collect() method receives the following three parameters:

+   - A supplier function that generates a data structure of the type of the final result of the collect operation. With parallel streams, this function will be called as many times as there are threads executing the operation.
+   - An accumulator function that receives a data structure and an element of the stream and makes the process of the element.
+   - A combiner function that receives two data structures and generates a unique data structure of the same type.

You can use lambda expressions to implement these functions, but you can also implement the Supplier interface for the supplier function or the BiConsumer interface for the accumulator and combiner functions (always parameterized with the adequate data types). You can also use method references (Class::Method) if the input and output parameters are adequate. For example, we have used the List::new reference as the supplier function and the List::addAll method as the combiner function. We could use the List::add method as the accumulator function. There are more methods you can use as parameters to the collect() method.