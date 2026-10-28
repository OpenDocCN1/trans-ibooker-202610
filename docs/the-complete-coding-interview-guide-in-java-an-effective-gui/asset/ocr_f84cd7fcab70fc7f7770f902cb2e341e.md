Notice that, depending on the stream's source and the intermediate operations, streams may or may not have a defined encounter order.

# Coding challenge 15 – Converting arrays to streams

Problem: How would you convert an array to a stream?

Solution: Converting an array of objects into a stream can be done in at least three ways, as follows:

+   1. The first is via Arrays#stream():
+   2. Second, we can use Stream#of():
+   3. The last technique is via List#stream():

```
public static <T> Stream<T> toStream(T[] arr) {
    return Arrays.stream(arr);
}
```

```
public static <T> Stream<T> toStream(T[] arr) {
    return Stream.of(arr);
}
```

```
public static <T> Stream<T> toStream(T[] arr) {
    return Arrays.asList(arr).stream();
}
```

Converting an array of primitives (for example, integers) into a stream can be done in at least two ways, as follows:

+   1. Firstly, via Arrays#stream():

```
public static IntStream toStream(int[] arr) {
    return Arrays.stream(arr);
}
```