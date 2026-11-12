# Peeking into the Laziness

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