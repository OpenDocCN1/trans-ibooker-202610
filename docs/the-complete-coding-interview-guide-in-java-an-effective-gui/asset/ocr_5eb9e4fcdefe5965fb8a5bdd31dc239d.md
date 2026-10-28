An Iterator can traverse the elements of a collection only via hasNext()/next() because it doesn't have a size. On the other hand, a Spliterator can provide the size of the collection either by approximating it via estimateSize() or exactly via getExactSizeIfKnown().

A Spliterator can use several flags for internally disabling unnecessary operations (for example, CONCURRENT, DISTINCT, and IMMUTABLE). An Iterator doesn't have such flags.

Finally, you can create a Spliterator around an Iterator as follows:

```
Spliterators.spliteratorUnknownSize(
    your_Iterator, your_Properties);
```

In the book Java Coding Problems (https://www.amazon.com/gp/product/B07Y9BPV4W/), you can find more details on this topic, including a complete guide for writing a custom Spliterator.

# Coding challenge 20 – Optional

Problem: What is the Optional class?

Solution: Inspired by Haskell and Scala, the Optional class was introduced in Java 8 with the main purpose of mitigating/avoiding NullPointerException. The Java language architect Brian Goetz's definition is as follows:

> Optional is intended to provide a limited mechanism for library method return types where there needed to be a clear way to represent no result, and using null for such was overwhelmingly likely to cause errors.

In a nutshell, you can think of Optional as a single value container that contains either a value or is empty. For example, an empty Optional looks like this:

```
Optional<User> userOptional = Optional.empty();
```

And a non-empty Optional looks like this:

```
User user = new User();
Optional<User> userOptional = Optional.of(user);
```

In Java Coding Problems (https://www.amazon.com/gp/product/B07Y9BPV4W/), you can find a complete chapter dedicated to best practices for using Optional. This is a must-read chapter for any Java developer.