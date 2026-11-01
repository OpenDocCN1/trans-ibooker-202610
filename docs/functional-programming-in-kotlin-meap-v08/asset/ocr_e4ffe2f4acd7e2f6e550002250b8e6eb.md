# EXERCISE 6.6

Write the implementation of map2 based on the following signature. This function takes two actions, ra and rb, and a function f for combining their results, and returns a new action that combines them:

```
fun <A, B, C> map2(
    ra: Rand<A>,
    rb: Rand<B>,
    f: (A, B) -> C
): Rand<C> = TODO()
```

We only have to write the map2 combinator once, and then we can use it to combine arbitrary RNG state actions. For example, if we have an action that generates values of type A and an action to generate values of type B, then we can combine them into one action that generates pairs of both A and B:

```
fun <A, B> both(ra: Rand<A>, rb: Rand<B>): Rand<Pair<A, B>> =
    map2(ra, rb) { a, b -> Pair(a, b) }
```

We can use both to reimplement intDouble and doubleInt from exercise 6.3 more succinctly. We do this by using the Rand values intR and doubleR developed earlier in the chapter:

```
val intR: Rand<Int> = { rng -> rng.nextInt() }

val doubleR: Rand<Double> =
    map(::nonNegativeInt) { i ->
        i / (Int.MAX_VALUE.toDouble() + 1)
    }

val intDoubleR: Rand<Pair<Int, Double>> = both(intR, doubleR)

val doubleIntR: Rand<Pair<Double, Int>> = both(doubleR, intR)
```

# EXERCISE 6.7 (Hard)

If you can combine two RNG transitions, you should be able to combine a whole list of them. Implement sequence for combining a List of transitions into a single transition. Use it to reimplement the ints function you wrote before. For the sake of simplicity in this exercise, it is acceptable to write ints with recursion to build a list with x repeated n times.

```
fun <A> sequence(fs: List<Rand<A>>): Rand<List<A>> = TODO()
```

Once you're done implementing sequence(), try reimplementing it using a fold.