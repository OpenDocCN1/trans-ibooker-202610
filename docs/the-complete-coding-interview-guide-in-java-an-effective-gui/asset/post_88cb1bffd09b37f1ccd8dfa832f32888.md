
**Solution:** As the following diagram reveals, a lambda has three main parts:

![](img/6709a71136a6b327c831c6c15aff2f6a_718_0.png)

Figure 17.1 – Lambda parts

The parts of a lambda expression are as follows:

- On the left of the arrow, there are the parameters of this lambda that are used in the lambda body. In this example, these are the parameters of the FilenameFilter.accept(File folder, String fileName) method.
- On the right of the arrow, there is the lambda body. In this example, the lambda body checks whether the folder (folder) in which the file (fileName) was found can be read and whether the name of this file is suffixed with the .pdf string.
- The arrow that sits between the list of parameters and the body of a lambda acts as a separator.

Next, let’s talk about the characteristics of a lambda expression. So, if we write the anonymous class version of the lambda from the preceding diagram, then it will be as follows:

```
FilenameFilter filter = new FilenameFilter() {

    @Override
    public boolean accept(File folder, String fileName) {
        return folder.canRead() && fileName.endsWith(".pdf");
    }
};
```

Now, if we compare the anonymous version and the lambda expression, then we notice that the lambda expression is a concise anonymous function that can be passed as a parameter to a method or be held in a variable.

The four words shown in the following diagram characterize a lambda expression:

![](img/6709a71136a6b327c831c6c15aff2f6a_719_0.png)

Figure 17.2 – Lambda characteristics

As a rule of thumb, keep in mind that lambdas sustain the Behavior Parameterization design pattern (a behavior is passed as parameter of a function) and it can be used only in the context of a functional interface.

## Coding challenge 2 – Functional interface

Problem: What is a functional interface?

Solution: In Java, a functional interface is an interface that contains only one abstract method. In other words, a functional interface contains only one method that is not implemented. So, a functional interface wraps a function as an interface and the function is represented by a single abstract method on the interface.

Optionally, besides this abstract method, a functional interface can have default and/or static methods as well. Commonly, a functional interface is annotated with @FunctionalInterface. This is just an informative annotation type that's used to mark a functional interface.

Here is an example of a functional interface:

```
@FunctionalInterface
public interface Callable<V> {
	V call() throws Exception;
}
```

As a rule of thumb, if an interface has more methods without implementation (that is, abstract methods), then it is no longer a functional interface. This means that such an interface cannot be implemented by a Java lambda expression.

## Coding challenge 3 – Collections versus streams

**Problem:** What are the main differences between collections and streams?

**Solution:** Collections and streams are quite different. Some of the differences are as follows:

- **Conceptual differences:** The main difference between collections and streams consists of the fact that they are conceptually two different things. While collections are meant to store data (for example, `List`, `Set`, and `Map`), streams are meant to apply operations (for example, *filtering*, *mapping*, and *matching*) on that data. In other words, streams apply complex operations on a view/source represented by data stored on a collection. Moreover, any modification/change performed on a stream is not reflected in the original collection.
- **Data modification:** While we can add/remove elements from a collection, we cannot add/remove elements from a stream. Practically, a stream consumes a view/source, performs operations on it, and returns a result without modifying the view/source.
- **Iteration:** While a stream consumes a view/source, it automatically and internally performs the iteration of that view/source. The iteration takes place depending on the chosen operations that should be applied to the view/source. On the other hand, collections must be iterated externally.
- **Traversal:** While collections can be traversed multiple times, streams can be traversed only once. So, by default, Java streams cannot be reused. Attempting to traverse a stream twice will lead to an error reading *Stream has already been operated on or closed.*
- **Construction:** Collections are eagerly constructed (all the elements are present right from the beginning). On the other hand, streams are lazily constructed (the so-called *intermediate* operations are not evaluated until a *terminal* operation is invoked).

## Coding challenge 4 – The map() function

**Problem:** What does the `map()` function do and why would you use it?

Solution: The map() function is an intermediate operation named mapping and available via the Stream API. It is used to transform a type of object to other type by simply applying the given function. So, map() traverses the given stream and transforms each element in a new version of it by applying the given function and accumulating the results in a new Stream. The given Stream is not modified. For example, transforming a List<String> into a List<Integer> via Stream#map() can be done as follows:

```
List<String> strList = Arrays.asList("1", "2", "3");
List<Integer> intList = strList.stream()
    .map(Integer::parseInt)
    .collect(Collectors.toList());
```

Challenge yourself to practice more examples. Try to apply map() to transform an array into another array.

## Coding challenge 5 – The flatMap() function

Problem: What does the flatMap() function do and why would you use it?

Solution: The flatMap() function is an intermediate operation named flattening and is available via the Stream API. This function is an extension of map(), meaning that apart from transforming the given object into another type of object, it can also flatten it. For example, having a List<List<Object>>, we can turn it into a List<Object> via Stream#flatMap() as follows:

```
List<List<Object>> list = ...
List<Object> flatList = list.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());
```

The next coding challenge is related to this one, so consider this as well.

## Coding challenge 6 – map() versus flatMap()

Problem: What's the difference between map() and flatMap() functions?

Solution: Both of these functions are intermediate operations capable of transforming a given type of object into another type of object by applying the given function. In addition, the flatMap() function is capable of flattening the given object as well. In other words, flatMap() can also flatten a Stream object.

Why does this matter? Well, map() knows how to wrap a sequence of elements in a Stream, right? This means that map() can produce streams such as Stream<String[]>, Stream<List<String>>, Stream<Set<String>>, or even Stream<Stream<R>>. But the problem is that these kinds of streams cannot be manipulated successfully (that is, as we expected) by stream operations such as sum(), distinct(), and filter().

For example, let's consider the following List:

```
List<List<String>> melonLists = Arrays.asList(
    Arrays.asList("Gac", "Cantaloupe"),
    Arrays.asList("Hemi", "Gac", "Apollo"),
    Arrays.asList("Gac", "Hemi", "Cantaloupe"));
```

We try to obtain the distinct names of melons from this list. If wrapping an array into a stream can be done via Arrays.stream(), for a collection, we have Collection.stream(). Therefore, the first attempt may look as follows:

```
melonLists.stream()
    .map(Collection::stream) // Stream<Stream<String>>
    .distinct();
```

But this will not work because map() will return Stream<Stream<String>>. The solution is provided by flatMap(), as follows:

```
List<String> distinctNames = melonLists.stream()
    .flatMap(Collection::stream) // Stream<String>
    .distinct()
    .collect(Collectors.toList());
```

The output is as follows: Gac, Cantaloupe, Hemi, Apollo.

Moreover, if you find trouble understanding these functional programming methods, then I strongly recommend you to read my other book, Java Coding Problems, available from Packt (https://www.packtpub.com/programming/java-coding-problems). That book contains two comprehensive chapters about Java functional-style programming that provide detailed explanations, diagrams, and applications useful for deep diving into this topic.
