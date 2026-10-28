The following screenshot shows part of the output of the example:

![](bbox=[0.27, 0.155, 0.73, 0.455])

You can see that when you call the `limit(1)` method in the parallel stream generated from `TreeSet`, you always obtain the same result because the Stream API respects the encounter order of that structure. But when we include a call to the `unordered()` method, the encounter order is not taken into account and the result obtained should vary, as in this case.

# There's more...

When you use the `unordered()` method, you're not executing any code that internally changes the order of the elements in the data structure. You're only deleting a condition that would be taken into account for some methods otherwise. It's possible that the results of a stream with the `unordered()` method and the results of the same stream without the method are equal. Its use may have consequences in possibly giving different processing results for parallel streams. For example, if you try our example using a `List` of `Person` objects instead of `personSet` a `TreeSet`, you will always obtain the same result in both the cases.

As we mentioned before, the main purpose of the `unordered()` method is to delete a constraint that limits the performance of parallel streams.