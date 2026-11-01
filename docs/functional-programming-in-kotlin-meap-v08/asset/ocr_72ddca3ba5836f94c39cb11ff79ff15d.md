```
//a better approach using foldRight
fun <A> sequence2(fs: List<Rand<A>>): Rand<List<A>> =
    foldRight(fs, unit(List.empty()), { f, acc ->
        map2(f, acc, { h, t -> Cons(h, t) })
    })

fun ints2(count: Int, rng: RNG): Pair<List<Int>, RNG> {
    fun go(c: Int): List<Rand<Int>> =
        if (c == 0) Nil
        else Cons({ r -> Pair(1, r) }, go(c - 1))
    return sequence2(go(count))(rng)
}
```

# EXERCISE 6.8

Implement flatMap, and then use it to implement nonNegativeLessThan.

```
fun <A, B> flatMap(f: Rand<A>, g: (A) -> Rand<B>): Rand<B> =
{ rng ->
    val (a, rng2) = f(rng)
    g(a)(rng2)
}

fun nonNegativeIntLessThan(n: Int): Rand<Int> =
    flatMap(::nonNegativeInt) { i ->
        val mod = i % n
        if (i + (n - 1) - mod >= 0) unit(mod)
        else nonNegativeIntLessThan(n)
    }
```

# EXERCISE 6.9

Reimplement map and map2 in terms of flatMap. The fact that this is possible is what we’re referring to when we say that flatMap is more powerful than map and map2.

```
fun <A, B> mapF(ra: Rand<A>, f: (A) -> B): Rand<B> =
    flatMap(ra) { a -> unit(f(a)) }

fun <A, B, C> map2F(
    ra: Rand<A>,
    rb: Rand<B>,
    f: (A, B) -> C
): Rand<C> =
    flatMap(ra) { a ->
        map(rb) { b ->
            f(a, b)
        }
    }
```

# EXERCISE 6.10

Generalize the functions unit, map, map2, flatMap, and sequence. Add them as methods on the State data class where possible. Otherwise you should put them in the State companion object.

```
data class State<S, out A>(val run: (S) -> Pair<A, S>) {

    companion object {
        fun <S, A> unit(a: A): State<S, A> =
            State { s: S -> Pair(a, s) }

        fun <S, A, B, C> map2(
            ra: State<S, A>,
```