
![](img/f9269002f2414fff96e5e6b4dc20839f_239_0.png)

But both the filter and map methods are lazy to the bone. As the execution goes through the chain, the filter and map methods store the lambda expressions and pass on a façade to the next call in the chain. The evaluations start only when findFirst, a terminal operation, is called.

The order of evaluation is different as well, as we see in the following figure. The filter method doesn't plow through all the elements in the collection in one shot. Instead, it runs until it finds the first element that satisfies the condition given in the attached lambda expression. As soon as it finds an element, it passes that to the next method in the chain. This next method, map in this example, does its part on the given input and passes it down the chain. When the evaluation reaches the end, the terminal operation checks to see if it has received the result it's looking for.

If the terminal operation got what it needed, the computation of the chain terminates. If the terminal operation isn't satisfied, it'll ask for the chain of operations to be carried out for more elements in the collection.

By examining the logic of this sequencing of operations, we can see that the execution will iterate over only essential elements in the collection. We can see evidence of this behavior by running the code.

```
getting length for Brad
getting length for Kate
getting length for Kim
converting to uppercase: Kim
KIM
```

From the output, we can see that most of the elements in the example list were not evaluated once the candidate name we're looking for was found.

The logical sequence of operations we saw in the previous example is achieved under the hood in the JDK using a fusing operation—all the functions in the intermediate operations are fused together into one function that's evaluated for each element, as appropriate, until the terminal operation is satisfied. In essence, there's only one pass on the data—filtering, mapping, and selecting the element all happen in one shot.

## Peeking into the Laziness

Writing the series of operations as a chain is the preferred and natural way in Java. But to really see that the lazy evaluations didn’t start until we reached the terminal operation, let’s break the chain from the previous code into steps.

```
lazy/fpij/LazyStreams.java
Stream<String> namesWith3Letters =
names.stream()
.filter(name -> length(name) == 3)
.map(name -> toUpper(name));

System.out.println("Stream created, filtered, mapped...");
System.out.println("ready to call findFirst...");

final String firstNameWith3Letters =
namesWith3Letters.findFirst()
.orElse("");

System.out.println(firstNameWith3Letters);
```

We transformed the collection into a stream, filtered the values, and then mapped the resulting collection. Then, separately, we called the terminal operation. Let’s run the code to see the sequence of evaluation.

```
Stream created, filtered, mapped...
ready to call findFirst...
getting length for Brad
getting length for Kate
getting length for Kim
converting to uppercase: Kim
KIM
```

From the output, we can clearly see that the intermediate operations delayed their real work until the last responsible moment when the terminal operation was invoked. And even then, they only did the minimum work necessary to satisfy the terminal operation. Pretty efficient, eh?

This example helped us uncover Stream’s true power. Next, we'll use Streams to create infinite collections.

## Creating Infinite, Lazy Collections

Using infinite collections can make the code that creates a growing series (such as the Fibonacci numbers) clearer and easier to express. But from our experience in Java, we might think a series can’t be infinite due to practical memory limits. The laziness of Streams comes in again here.

In Java, collections are still required to be finite, but streams can be infinite. We will see here how laziness becomes an essential workhorse to make infinite streams possible. We’ll use them to create highly expressive, easy-to-understand code to produce an infinitely growing series.

### A Desperate Attempt

We'll use a series of prime numbers, 2, 3, 5, 7,... as an example to explore the concepts here. Let's first create a helper function to determine if a number is prime.

```
public static boolean isPrime(final int number) {
    return number > 1 &&
    IntStream.rangeClosed(2, (int) Math.sqrt(number))
    .noneMatch(divisor -> number % divisor == 0);
}
```

A number greater than 1 is prime if it’s not divisible by any number between 2 and its square root. Normally, we’d use an external iterator to search for a divisor in that range. Instead, we use more Java goodness here—the rangeClosed static method of the IntStream interface.

The closed suffix in the method name emphasizes that the range of values will include the second parameter. For example, rangeClosed(1, 10) will return the range of values 1, 2,..., 10 packed into a Stream. In contrast, the range method, also a static method in the interface, will return a range of values, up to (but not including) the value in the second parameter.

In the `isPrime` method, we use the short and sweet `noneMatch` method on the stream returned by the `rangeClosed` method. The `noneMatch` method takes a `Predicate` as its parameter and we use this to determine if there's a divisor for the given number. The `noneMatch` method will yield a `boolean` `true` if the lambda expression returned `false` for all values in the range—that is, if there are no divisors.

On our first attempt, we'll use the `isPrime` method to create a series of prime numbers starting at any given `number`.

```
//don't try this at the office
public static List<Integer> primes(final int number) {
    if(isPrime(number)) {
        return concat(number, primes(number + 1));
    }

    return primes(number + 1);
}
```

If a given `number` is prime, we include it in the list of primes that follow the number. Otherwise, we omit it and move on to get that list.

Hold your tweets; no, your humble author hasn't gone mad—that code is enticingly elegant but, sadly, won't work. If we implement the `concat` method and run the code, we'll enter into a never-ending recursion and end up with a `java.lang.StackOverflowError`.

Let's drill into the code just a bit more to see if we can salvage anything from it. The `StackOverflowError` is from the recursive calls to the `primes` method. If we can be lazy about this call, then we won't run into issues.

## Reaching for the Stars

Earlier we saw how lazy Streams are. They don’t do any real work until we ask them for the results—kind of like my kids. We can rely on that laziness to easily create a lazy, infinite collection.

When we create a Stream, from a collection or through other means, we quickly receive a façade that has the potential to return an infinite list. But it’s wickedly clever; it returns to us only as many elements as we ask for, producing the elements just in time. We can use that capability to express an infinite collection and generate as many (finite) elements as we like from that list. Let’s see how.

The Stream interface has a static method iterate that can create an infinite Stream. It takes two parameters, a seed value to start the collection, and an instance of a UnaryOperator interface, which is the supplier of data in the collection. The Stream the iterate method returns will postpone creating the elements until we ask for them using a terminating method. To get the first element, for example, we could call the findFirst method. To get ten elements we could call the limit method on the Stream, like so: limit(10).

Let’s see how all these ideas shape up in code.

```
lazy/fpij/Primes.java

public class Primes {
  private static int primeAfter(final int number) {
    if(isPrime(number + 1)) {
      return number + 1;
    }

    return primeAfter(number + 1);
  }
}
