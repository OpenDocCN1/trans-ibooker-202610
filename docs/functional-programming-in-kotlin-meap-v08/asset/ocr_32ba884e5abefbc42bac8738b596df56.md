```
fun <A, B> foldRight(z: B, f: (A, B) -> B): B

fun <A, B> foldLeft(z: B, f: (B, A) -> B): B
```

We see the initializer z, the combining function (A, B) -> B and the result type of B carried through from the initializer.

What happens if we turn A and B into a single type by calling it A?

```
fun <A> foldRight(z: A, f: (A, A) -> A): A

fun <A> foldLeft(z: A, f: (A, A) -> A): A
```

The components of a monoid fit these argument types like a glove. So if we had a List<String>, words, we could simply pass the combine and nil of the stringMonoid in order to reduce the list with the monoid and concatenate all the strings. Let’s try this in the REPL:

```
>>> val words = listOf("Hic", "Est", "Index")
res0: kotlin.collections.List<kotlin.String> = [Hic, Est, Index]

>>> words.foldRight(stringMonoid.nil, stringMonoid::combine)
res1: kotlin.String = HicEstIndex

>>> words.foldLeft(stringMonoid.nil, stringMonoid::combine)
res2: kotlin.String = HicEstIndex
```

Note that it doesn’t matter if we choose foldLeft or foldRight when folding with a monoid. We should get the same result in both cases. This is precisely because the laws of associativity and identity hold. A left fold associates operations to the left, whereas a right fold associates to the right, with the identity element on the far left and right respectively:

```
>>> words.foldLeft("") { a, b -> a + b } == ("" + "Hic") + "Est") + "Index"
res3: kotlin.Boolean = true

>>> words.foldRight("") { a, b -> a + b } == "Hic" + ("Est" + ("Index" + ""))
res4: kotlin.Boolean = true
```

Armed with this knowledge, we can now write a function called concatenate that folds a list with a monoid.

```
fun <A> concatenate(la: List<A>, m: Monoid<A>): A =
  la.foldLeft(m.nil, m::combine)
```

In some circumstances the element type might not have a Monoid instance. In cases like this we could always map over the list to turn it into a type that does have an associated instance.