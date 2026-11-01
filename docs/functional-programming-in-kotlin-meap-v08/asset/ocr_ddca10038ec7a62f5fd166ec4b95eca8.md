# EXERCISE 5.1

Write a function to convert a Stream to a List, which will force its evaluation to let you look at the result in the REPL. You can convert to the singly-linked List type that we developed in chapter 3, and you can implement this and other functions that operate on a Stream using extension methods.

```
fun <A> Stream<A>.toList(): List<A> = TODO()
```

# TIP

Think about stack safety when implementing this function. Consider tail call elimination and the use of another method that you implemented on List.

# EXERCISE 5.2

Write the function take(n) for returning the first n elements of a Stream, and drop(n) for skipping the first n elements of a Stream.

```
fun <A> Stream<A>.take(n: Int): Stream<A> = TODO()
```

```
fun <A> Stream<A>.drop(n: Int): Stream<A> = TODO()
```

# EXERCISE 5.3

Write the function takeWhile for returning all starting elements of a Stream that match the given predicate.

```
fun <A> Stream<A>.takeWhile(p: (A) -> Boolean): Stream<A> = TODO()
```

# TIP

You can use take and toList together to inspect streams in the REPL. For example, try printing Stream.of(1,2,3).take(2).toList(). This is also of great use during assertion expressions in unit tests.

# 5.3 Separating program description from evaluation

A major theme in functional programming is separation of concerns. We want to separate the description of computations from actually running them. We’ve already touched on this theme in previous chapters in different ways. For example, first-class functions capture some computation in their bodies but only execute it once they receive their arguments. And we used Option to capture the fact that an error occurred, where the decision of what to do about it became a separate concern. With Stream, we’re able to build up a computation that produces a sequence of elements without running the steps of that computation until we actually need those elements.