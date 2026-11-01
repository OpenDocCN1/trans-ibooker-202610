# Listing 6.16 State declarations and combinators required to perform state propagation

```
val int: State<RNG, Int> = TODO()

fun ints(x: Int): State<RNG, List<Int>> = TODO()

fun <A, B> flatMap(
    s: State<RNG, A>,
    f: (A) -> State<RNG, B>
): State<RNG, B> = TODO()

fun <A, B> map(
    s: State<RNG, A>,
    f: (A) -> B
): State<RNG, B> = TODO()
```

+   1. is a `State<RNG, Int>` with the ability of generating a single random integer
+   2. returns a `State<RNG, List<Int>>` that can generate a list of x random integers
+   3. a `flatMap` function that operates on a `State<RNG, A>` with a function from A to `State<RNG, B>`
+   4. a `map` function that operates on a `State<RNG, A>` with a function that transforms A to B

Now we write some code utilizing these declarations that we subsequently have available to us from Listing 6.16.

# Listing 6.17 State propagation using a series of flatMap and mapS.

```
val ns: State<RNG, List<Int>> =
    flatMap(int) { x ->
        flatMap(int) { y ->
            map(ints(x)) { xs ->
                xs.map { it % y }
            }
        }
    }
```

+   1. int will generate a single random integer.
+   2. ints(x) generates a list of length x random integers
+   3. replaces every element in the list with its remainder when divided by y

It’s not clear what’s going on here due to all the nested flatMap and map calls. Let’s look for a simpler approach by turning our attention to the for-comprehension. This construct will unravel a series of flatMap calls, allowing us to rewrite this code in what seems to be a series of imperative declarations. The trick with this code lies in the destructuring that occurs in every step. Each time we see something being destructured, it implies a call to flatMap.

For example, a line that is written as flatMap(int) { x -> ... } could be rewritten as val (x: Int) = int within the confines of a for-comprehension. With this in mind, let’s try rewriting Listing 6.17 using this technique.