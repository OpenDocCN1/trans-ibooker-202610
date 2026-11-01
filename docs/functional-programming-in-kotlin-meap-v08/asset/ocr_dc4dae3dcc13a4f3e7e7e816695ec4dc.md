```
tail=Cons(head=0, tail=Nil)))
```

This example should be equivalent to the expression List.of(1+2+3+0, 2+3+0, 3+0, 0). Your function should reuse intermediate results so that traversing a Stream with n elements always takes time linear in n. Can it be implemented using unfold? How, or why not? Could it be implemented using another function we’ve written?

```
fun <A, B> Stream<A>.scanRight(z: B, f: (A, () -> B) -> B): Stream<B> =
    foldRight({ Pair(z, Stream.of(z)) },
        { a: A, p0: () -> Pair<B, Stream<B>> ->
            val p1: Pair<B, Stream<B>> by lazy { p0() }
            val b2: B = f(a) { p1.first }
            Pair<B, Stream<B>>(b2, cons({ b2 }, { p1.second }))
        }).second
```

# CHAPTER 6. PURELY FUNCTIONAL STATE

# EXERCISE 6.1

Write a function that uses RNG.nextInt to generate a random integer between 0 and Int.MAX_VALUE (inclusive). Make sure to handle the corner case when nextInt returns Int.MIN_VALUE, which doesn’t have a non-negative counterpart.

```
fun nonNegativeInt(rng: RNG): Pair<Int, RNG> {
    val (i1, rng2) = rng.nextInt()
    return Pair(if (i1 < 0) -(i1 + 1) else i1, rng2)
}
```

# EXERCISE 6.2

Write a function to generate a Double between 0 and 1, not including 1. In additon to the function you already developed, you can use Int.MAX_VALUE to obtain the maximum positive integer value, and you can use x.toDouble() to convert an x: Int to a Double.

```
fun double(rng: RNG): Pair<Double, RNG> {
    val (i, rng2) = nonNegativeInt(rng)
    return Pair(i / (Int.MAX_VALUE.toDouble() + 1), rng2)
}
```

# EXERCISE 6.3

Write functions to generate a Pair<Int, Double>, a Pair<Double, Int>, and a Triple<Double, Double, Double>. You should be able to reuse the functions you’ve already written.

```
fun intDouble(rng: RNG): Pair<Pair<Int, Double>, RNG> {
    val (i, rng2) = rng.nextInt()
    val (d, rng3) = double(rng2)
    return Pair(Pair(i, d), rng3)
}

fun doubleInt(rng: RNG): Pair<Pair<Double, Int>, RNG> {
    val (id, rng2) = intDouble(rng)
    val (i, d) = id
    return Pair(Pair(d, i), rng2)
}
```