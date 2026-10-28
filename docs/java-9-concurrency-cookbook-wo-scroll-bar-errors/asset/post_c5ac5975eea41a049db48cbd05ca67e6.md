
7. Finally, we'll learn how to manage the situations where an intermediate operation returns Stream. In this case, we'll work with a Stream of streams, but we can concatenate all these Stream objects into a unique Stream using the flatMap() method. Generate List<String> with 100 elements using the FileGenerator class. Then, create a parallel stream with the parallelStream() method. We'll split each line to get its words using the split() method, and with the of() method of the Stream class, we convert the resultant Array into Stream. If we use the map() method, we are generating a Stream of streams, but using the flatMap() method we'll get a unique Stream of String objects with all the words of the whole List. Then, we get the words with a length greater than zero with the filter() method, sort the stream with the sorted() method, and collect it to Map using the groupingByConcurrent() method where the keys are the words and the values are the number of times each word appears in the stream:

```
List<String> file = FileGenerator.generateFile(100);
Map<String, Long> wordCount = file.parallelStream()
    .flatMap(line -> Stream.of(line.split("[ ,.]")))
    .filter(w -> w.length() > 0).sorted()
    .collect(Collectors.groupingByConcurrent(e -> e, Collectors
        .counting()));

wordCount.forEach((k, v) -> {
    System.out.printf("%s: %d\n", k, v);
});
```

8. Finally, we have to implement the getAge() method used previously in the code. This method receives the date of birth of a Person object and returns its age:

```
private static long getAge(Date birthDate) {
    LocalDate start = birthDate.toInstant()
        .atZone(ZoneId.systemDefault()).toLocalDate();
    LocalDate now = LocalDate.now();
    long ret = ChronoUnit.YEARS.between(start, now);
    return ret;
}
```

## How it works...

In this recipe you learnt how to convert the elements of the stream using an intermediate operation and an expression that makes the conversion between the source and the destination types. We used three different methods in our example:

- `mapToDouble()`: We used this method to convert `Stream` of objects into `DoubleStream` with double numbers as elements. This method receives as parameter a lambda expression or an implementation of the `ToDoubleFunction` interface. This expression receives an element of `Stream` and has to return a double value.
- `map()`: We can use this method when we have to convert the elements of `Stream` to a different class. For example, in our case, we convert the `Person` class to a `BasicPerson` class. This method receives as parameter a lambda expression or an implementation of the `Function` interface. This expression must create the new object and initialize its attributes.
- `flatMap()`: This method is useful in a more complex situation when you have to work with a `Stream` of `Stream` objects and you want to convert them to a unique `Stream`. This method receives as parameter a lambda expression or an implementation of the `Function` interface as the `map()` function, but in this case, this expression has to return a `Stream` object. The `flatMap()` method will automatically concatenate all those streams into a unique `Stream`.

## There's more...

The `Stream` class provides other methods to transform the elements of a `Stream`:

- `mapToInt(), mapToLong()`: These methods are identical to the `mapToDouble()` method, but they generate `IntStream` and `LongStream` objects, respectively.
- `flatMapToDouble(), flatMapToInt(), flatMapToLong()`: These methods are identical to the `flatMap()` method, but they work with `DoubleStream`, `IntStream`, and `LongStream`, respectively.

## See also

- The *Creating streams from different sources*, *Reducing the elements of a stream* and *Collecting the elements of a stream* recipes in this chapter

## Sorting the elements of a stream

Another typical operation you will want to do with a Stream is sorting its elements. For example, you may want to sort the elements of the Stream by name, postal code, or any other numeric value.

With streams, we have other considerations with the so-called encounter order. Some streams may have a defined encounter order (it depends on the source of the Stream). Some operations work with the elements of the stream in its encountered ordered, such as limit(), skip(), and others. This makes that parallel computation for this methods doesn't give us good performance. In these cases, you can speed-up the execution of these methods by deleting the ordering constraint.

In this recipe, you will learn how to sort the elements of Stream and how to delete the ordering constraint in situations where we don't need the encounter order of Stream.

## Getting ready

The example of this recipe has been implemented using the Eclipse IDE. If you use Eclipse or other IDE such as NetBeans, open it and create a new Java project.

## How to do it...

Follow these steps to implement the example:

- 1. First, we'll implement some auxiliary classes we will use in the example. First, implement the Person class, which stores the basic attributes of a person, and the PersonGenerator class, which generates a List of random Person objects. Please, check the recipe Apply an action to all the elements of a stream to see the source code of both the classes.
- 2. Now, implement the Main class with the main() method. First, we'll create an Array of int numbers. Then, we'll create a parallel stream with the parallelStream() method from this array, use the sorted() method to sort the elements of the array, and use the forEachOrdered() method to write the elements in an ordered way. Take into account that this operation won't use all the power of our multi-core processor as it has to write the elements in the specified order:

```
public class Main {
    public static void main args[] {
        int[] numbers={9,8,7,6,5,4,3,2,1,2,3,4,5,6,7,8,9};
        Arrays.stream(numbers).parallel().sorted().forEachOrdered
            (n -> {
        System.out.printf("%d\n", n);
    });
```

3. Now, let's try the same principles with a Stream of Person objects. Create a list of 10 random Person objects using the PersonGenerator class and use the same methods, sorted() and forEachOrdered(), to see how the persons are written in an ordered way:

```
List<Person> persons=PersonGenerator.generatePersonList(10);
persons.parallelStream().sorted().forEachOrdered(p -> {
    System.out.printf("%s, %s\n",p.getLastName(),p.getFirstName());
});
```

4. Finally, we'll see how to eliminate the encounter order of a data structure using the unordered() method. First, we'll create TreeSet from our List of random Person objects. We use TreeSet because it orders the elements internally. Then, we make a loop to repeat the operations ten times and see how there's a difference between the ordered and the unordered operations:

```
TreeSet<Person> personSet=new TreeSet<>(persons);
for (int i=0; i<10; i++) {
```

5. Then, we create a stream from PersonSet using the stream() method, convert it to parallel with the parallel() method, get the first element with the limit() method, and return the Person object, collecting it to a list and getting the first element:

```
Person person= personSet.stream().parallel().limit(1)
    .collect(Collectors.toList()).get(0);
System.out.printf("%s %s\n", person.getFirstName(),
    person.getLastName());
```

6. Now, we perform the same operation but remove the ordered constraint with the unordered() method between the stream() and parallel() methods:

```
person=personSet.stream().unordered().parallel().limit(1)
    .collect(Collectors.toList()).get(0);
System.out.printf("%s %s\n", person.getFirstName(),
    person.getLastName());
```

## How it works...

There are `Stream` objects that may have an encounter order depending on its source and the intermediate operations you have applied before. This encounter order imposes a restriction about the order in which the elements must be processed by certain methods. For example, if you use the `limit()` or `skip()` methods in `Stream` with an encounter order, they will get and ignore the first elements according to that encounter order. There are other operations, such as the `forEach()` method, that don't take into account the encounter order. If you apply the same operations to a stream with an encounter order, the result will always be the same. If the stream doesn't have an encounter order, the results may vary.

When you work with sequential streams, the encounter order doesn't have any impact on the performance of the application, but with parallel streams it can affect it greatly. Depending on the operations, it would be necessary to process more than once the elements of `Stream` or to store in a buffer a big amount of data. In this case, removing the encounter order using the `unordered()` method, as we did in this recipe, will significantly increase the performance of the application.

On the other hand, the `sorted()` method sorts the elements of the `Stream`. If you use this method, the elements of `Stream` must implement the `Comparable` interface. Otherwise, you can pass a `Comparator` as a parameter that will be used to sort the elements. If you use this method, you are creating an ordered stream, so all the things explained before to the streams with an encounter order are applicable to the resultant stream.

Finally, the `forEach()` method doesn't take into account the encounter order of the stream. If you want to take this encounter order into account, say, to write the elements of the stream order after sorting them, you can use the `forEachOrdered()` method.
