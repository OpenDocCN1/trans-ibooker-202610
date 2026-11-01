# The advantages of algebraic design

When you design the algebra of a library first, representations for the data types of the algebra don't matter as much. As long as they support the required laws and functions, you don't even need to make your representations public.

There's an underlying idea here that a type is given meaning based on its relationship to other types (which are specified by the set of functions and their laws), rather than its internal representation. This viewpoint is often associated with category theory, a branch of mathematics we've alluded to before.

# 9.2 One possible approach to designing an algebra

In this section, we’ll walk through the discovery process of a set of combinators for the parsing tasks mentioned earlier. If you worked through this design task yourself, you likely took a different path to what we will take. You may well have ended up with a different set of combinators, which is perfectly fine.

# 9.2.1 Counting character repetition

First, let’s consider the parser that recognizes zero or more repetitions of the character 'a' and returns the number of characters it has seen. We can start by adding a primitive combinator that takes us halfway there, let’s call it many:

```
fun <A> many(pa: Parser<A>): Parser<List<A>>
```

This isn’t exactly what we’re after—we need a Parser<Int> that counts the number of elements. We could change the many combinator to return a Parser<Int>, but that feels too specific. Undoubtedly there will be occasions where we care about more than just the list length. Better to introduce another combinator that should be familiar by now, map:

```
fun <A, B> map(pa: Parser<A>, f: (A) -> B): Parser<B>
```

We can now define our parser as follows:

```
map(many(char('a'))) { it.size }
```

Let’s transform these combinators into extension methods to make this a bit more pleasing to the eye.

```
fun <A> Parser<A>.many(): Parser<List<A>>
fun <A, B> Parser<A>.map(f: (A) -> B): Parser<B>
```

With these combinators in place, our new parser may be expressed as numA, followed by its