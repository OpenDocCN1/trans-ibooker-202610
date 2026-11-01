# EXERCISE C.3

You’re strongly encouraged to venture out and try using the library we’ve developed! See what else you can test with it, and see if you discover any new idioms for its use or perhaps ways it could be extended further or made more convenient. Here are a few ideas to get you started:

+   - Write properties to specify the behavior of some of the other functions we wrote for List and Stream, for instance, take, drop, filter, and unfold.
+   - Write a sized generator for producing the Tree data type defined in chapter 3, and then use this to specify the behavior of the fold function we defined for Tree. Can you think of ways to improve the API to make this easier?
+   - Write properties to specify the behavior of the sequence function we defined for Option and Either.

# C.3 Hints

# EXERCISE C.1

You will need to add to the representation of Gen. For example, Gen<Int> should be capable of generating random integers as well as generating a stream of all the integers from Int.MIN_VALUE to Int.MAX_VALUE. You may want to have the behavior depend on how many test cases were requested.

# EXERCISE C.2

If we are just looking at the random case, one way to have the generated Int depend on the String might be to set the seed of a new random number generator to be equal to the hashCode of the given input String.

# C.4 Answers

# C.4.1 Property based testing

# EXERCISE C.1

A detailed answer is to be found in the file Exhaustive.kt in the code accompanying this appendix.

# EXERCISE C.2

Let’s start by looking at the signature of our motivating example, generating a function from (String) -> Int given a Gen<Int>:

```
fun genStringInt(g: Gen<Int>): Gen<(String) -> Int> = TODO()
```

And let’s generalize this a bit to not be specialized to Int, because that would let us cheat a bit (by, say, returning the hashCode of the input String, which just so happens to be an Int).

```
fun <A> genStringFn(g: Gen<A>): Gen<(String) -> A> = TODO()
```