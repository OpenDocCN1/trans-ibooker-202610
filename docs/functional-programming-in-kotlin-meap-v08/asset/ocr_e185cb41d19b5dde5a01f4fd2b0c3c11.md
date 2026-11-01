```
is Cons ->
    Some(
        Pair(
            Pair(Some(ths.head()), Some(tht.head())),
            Pair(ths.tail(), tht.tail())
        )
    )
else ->
    Some(
        Pair(
            Pair(Some(ths.head()), None),
            Pair(ths.tail(), empty<B>())
        )
    )
}
else -> when (tht) {
    is Cons ->
        Some(
            Pair(
                Pair(None, Some(tht.head())),
                Pair(empty<A>(), tht.tail())
            )
        )
    else -> None
}
}
```

# EXERCISE 5.14 (HARD)

Implement startsWith using functions you’ve written. It should check if one Stream is a prefix of another. For instance, Stream(1,2,3) startsWith Stream(1,2) would be true.

```
fun <A> Stream<A>.startsWith(that: Stream<A>): Boolean =
    this.zipAll(that)
        .takeWhile { !it.second.isEmpty() }
        .forAll { it.first == it.second }
```

# EXERCISE 5.15

Implement tails using unfold. For a given Stream, tails returns the Stream of suffixes of the input sequence, starting with the original Stream. For example, given Stream.of(1,2,3), it would return Stream.of(Stream.of(1,2,3), Stream.of(2,3), Stream.of(3), Stream.empty()).

```
fun <A> Stream<A>.tails(): Stream<Stream<A>> =
    unfold(this) { s: Stream<A> ->
        when (s) {
            is Cons ->
                Some(Pair(s, s.tail()))
            else -> None
        }
    }
```

# EXERCISE 5.16 (HARD)

Generalize tails to the function scanRight, which is like a foldRight that returns a stream of the intermediate results. For example:

```
>>> Stream.of(1, 2, 3).scanRight(0, { a, b -> a + b }).toList()
res1: chapter3.List<kotlin.Int> = Cons(head=6, tail=Cons(head=5, tail=Cons(head=3,
```