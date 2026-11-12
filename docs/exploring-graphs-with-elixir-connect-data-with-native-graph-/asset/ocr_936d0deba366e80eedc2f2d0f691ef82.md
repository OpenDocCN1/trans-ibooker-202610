create type-specialized streams such as `IntStream` and `DoubleStream`), and then we *reduced* the resulting length to the sum value.

Instead of using the `sum` method, we could use a variety of methods like `max` to find the longest length, `min` to find the shortest length, `sorted` to sort the lengths, `average` to find the average of the length, and so on.

The hidden charm in the preceding example is the increasingly popular *MapReduce* pattern,[5] with the `map` method being the spread operation and the `sum` method being the special case of the more general reduce operation. In fact, the implementation of the `sum` method in the JDK uses a `reduce` method. Let's look at the more general form of reduce.

As an example, let's read over the given collection of names and display the longest one. If there's more than one name with the same longest length, we'll display the first one we find. One way we could do that is to figure out the longest length and then pick the first element of that length. But that'd require going over the list twice—not efficient. This is where a `reduce` method comes into play.

We can use the `reduce` method to compare two elements against each other and pass along the result for further comparison with the remaining elements in the collection. Much like the other higher-order functions on collections we've seen so far, the `reduce` method iterates over the collection. In addition, it carries forward the result of the computation that the lambda expression returned. An example will help clarify this, so let's get down to the code.

```
collections/fpij/PickALongest.java
```