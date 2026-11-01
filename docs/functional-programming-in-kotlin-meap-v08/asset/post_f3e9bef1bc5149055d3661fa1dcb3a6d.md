
```
override val nil: Boolean = false
}

val booleanAnd: Monoid<Boolean> = object : Monoid<Boolean> {
    override fun combine(a1: Boolean, a2: Boolean): Boolean = a1 && a2
    override val nil: Boolean = true
}
```

## EXERCISE 10.2

Give a Monoid instance for combining Option values.

```
fun <A> optionMonoid(): Monoid<Option<A>> = object : Monoid<Option<A>> {
    override fun combine(a1: Option<A>, a2: Option<A>): Option<A> =
        a1.orElse { a2 }
    override val nil: Option<A> = None
}

fun <A> dual(m: Monoid<A>): Monoid<A> = object : Monoid<A> {
    override fun combine(a1: A, a2: A): A = m.combine(a2, a1)
    override val nil: A = m.nil
}

fun <A> firstOptionMonoid() = optionMonoid<A>()
fun <A> lastOptionMonoid() = dual(firstOptionMonoid<A>())
```

Notice that we have a choice in how we implement op. We can compose the options in either order. Both of the implementations satisfy the monoid laws, but they are not equivalent. This is true in general—that is, every monoid has a dual where the op combines things in the opposite order. Monoids like booleanOr and intAddition are equivalent to their duals because their op is commutative as well as associative.

## EXERCISE 10.3

A function having the same argument and return type is sometimes called an endofunction. 54 Write a monoid for endofunctions.

```
fun <A> endoMonoid(): Monoid<(A) -> A> =
    object : Monoid<(A) -> A> {
        override fun combine(a1: (A) -> A, a2: (A) -> A): (A) -> A =
            { a -> a1(a2(a)) }
        override val nil: (A) -> A
            get() = { a -> a }
    }

fun <A> endoMonoidComposed(): Monoid<(A) -> A> =
    object : Monoid<(A) -> A> {
        override fun combine(a1: (A) -> A, a2: (A) -> A): (A) -> A =
            a1 compose a2
        override val nil: (A) -> A
            get() = { it }
    }
```

## EXERCISE 10.4

Use the property-based testing framework we developed in chapter 8 to implement properties for the monoid laws of associativity and identity. Use your properties to test some of the monoids we’ve written so far.

```
fun <A> monoidLaws(m: Monoid<A>, gen: Gen<A>) =
    forAll(
        gen.flatMap { a ->
            gen.flatMap { b ->
                gen.map { c ->
                    Triple(a, b, c)
                }
            }
        }
    ) { (a, b, c) ->
        m.combine(a, m.combine(b, c)) == m.combine(m.combine(a, b), c) &&
            m.combine(m.nil, a) == m.combine(a, m.nil) &&
            m.combine(m.nil, a) == a
    }

class Exercise4 : WordSpec({
    val max = 100
    val count = 100
    val rng = SimpleRNG(42)
    val intGen = Gen.choose(-10000, 10000)

    "law of associativity" should {
        "be upheld using existing monoids" {
            monoidLaws(intAdditionMonoid, intGen)
                .check(max, count, rng) shouldBe Passed

            monoidLaws(intMultiplicationMonoid, intGen)
                .check(max, count, rng) shouldBe Passed
        }
    }
})
```

## EXERCISE 10.5

The function foldMap is used to align the types of the list elements so that a Monoid instance may be applied to the list. Implement this function.

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B =
    la.foldLeft(m.nil, { b, a -> m.combine(b, f(a)) })
```

## EXERCISE 10.6 (HARD)

The foldMap function can be implemented using either foldLeft or foldRight. But you can also write foldLeft and foldRight using foldMap. Give it a try for fun!

```
fun <A, B> foldRight(la: Sequence<A>, z: B, f: (A, B) -> B): B =
    foldMap(la, endoMonoid()) { a: A -> { b: B -> f(a, b) } }(z)
```

The function type (A, B) B, when curried is (A) (B) B. And of course (B) B is a monoid for any B (via function composition).

```
fun <A, B> foldLeft(la: Sequence<A>, z: B, f: (B, A) -> B): B =
    foldMap(la, dual(endoMonoid())) { a: A -> { b: B -> f(b, a) } }(z)
```

Folding to the left is the same except we flip the arguments to the function f to put the B on the correct side. Then we have to also "flip" the monoid so that it operates from left to right.

## EXERCISE 10.7

Implement foldMap based on the balanced fold technique. Your implementation should use the strategy of splitting the sequence in two, recursively processing each half, and then adding the answers together using the provided monoid.

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B =
    when {
        la.size >= 2 -> {
            val (lal, la2) = la.splitAt(la.size / 2)
            m.combine(foldMap(lal, m, f), foldMap(la2, m, f))
        }
        la.size == 1 ->
            f(la.first())
        else -> m.nil
    }
```

## EXERCISE 10.8 (HARD/OPTIONAL)

Also implement a parallel version of foldMap called parFoldMap using the library we developed in chapter 7.

```
fun <A> par(m: Monoid<A>): Monoid<Par<A>> = object : Monoid<Par<A>> {
    override fun combine(pa1: Par<A>, pa2: Par<A>): Par<A> =
        map2(pa1, pa2) { a1: A, a2: A ->
            m.combine(a1, a2)
        }
    override val nil: Par<A>
        get() = unit(m.nil)
}

fun <A, B> parFoldMap(
    la: List<A>,
    pm: Monoid<Par<B>>,
    f: (A) -> B
): Par<B> =
    when {
        la.size >= 2 -> {
            val (lal, la2) = la.splitAt(la.size / 2)
            pm.combine(parFoldMap(lal, pm, f), parFoldMap(la2, pm, f))
        }
        la.size == 1 ->
            unit(f(la.first()))
        else -> pm.nil
    }

parFoldMap(
    listOf("lorem", "ipsum", "dolor", "sit"),
    par(stringMonoid),
    { it.toUpperCase() }
)(es).invoke { cb -> result.set(cb) }
```

- Use map2 from chapter 7 to combine two Par instances
- Use unit from chapter 7 to wrap zero in Par
- Promote a Monoid<A> to Monoid<Par<A>> using par
- Apply the executor service and invoke a callback function on Future

## EXERCISE 10.9 (HARD/OPTIONAL)

Use foldMap as developed in exercise 10.7 to detect ascending order of a List<Int>. This will require some creativity when deriving the appropriate Monoid instance.

```
typealias TrackingState = Triple<Int, Int, Boolean>

val m = object : Monoid<Option<TrackingState>> {
    override fun combine(
        a1: Option<TrackingState>,
        a2: Option<TrackingState>
    ): Option<TrackingState> =
        when (a1) {
            is None -> a2
            is Some ->
                when (a2) {
                    is None -> a1
                    is Some -> Some(
                        Triple(
                            min(a1.t.first, a2.t.first),
                            max(a1.t.second, a2.t.second),
                            a1.t.third &&
                            a2.t.third &&
                            a1.t.second <= a2.t.first
                        )
                    )
                }
        }

    override val nil: Option<TrackingState> = None
}

fun ordered(ints: Sequence<Int>): Boolean =
    foldMap(ints, m) { i: Int -> Some(TrackingState(i, i, true)) }
        .map { it.third }
        .getOrElse { true }
```

## EXERCISE 10.10

Write a monoid instance for WC and ensure that it meets both monoid laws.

```
val wcMonoid: Monoid<WC> = object : Monoid<WC> {
    override fun combine(a1: WC, a2: WC): WC =
        when (a1) {
            is Stub -> when (a2) {
                is Stub ->
                    Stub(a1.chars + a2.chars)
                is Part ->
                    Part(a1.chars + a2.ls, a2.words, a2.rs)
            }
            is Part -> when (a2) {
                is Stub ->
                    Part(a1.ls, a1.words, a1.rs + a2.chars)
                is Part ->
                    Part(
                        a1.ls,
                        a1.words + a2.words +
                        (if ((a1.rs + a2.ls).isEmpty()) 0 else 1),
                        a2.rs
                    )
            }
        }

    override val nil: WC
        get() = Stub("")
}
```

## EXERCISE 10.11

Use the wc monoid to implement a function that counts words in a String by recursively splitting it into substrings and counting the words in those substrings.

```
fun wordCount(s: String): Int {

    fun wc(c: Char): WC =
        if (c.isWhitespace()) Part("", 0, "")
        else Stub("$c")

    fun unstub(s: String): Int = min(s.length, 1)

    return when (val wc = foldMap(s.asSequence(), wcMonoid) { wc(it) }) {
        is Stub -> unstub(wc.chars)
        is Part -> unstub(wc.ls) + wc.words + unstub(wc.rs)
    }
}
```

## EXERCISE 10.12

Implement foldLeft, foldRight and foldMap on the Foldable<F> interface in terms of each other.

**NOTE**
Using all these functions in terms of each other could result in undesired effects like circular references. We will remedy this in exercise 10.13.

```
interface Foldable<F> {

    fun <A, B> foldRight(fa: Kind<F, A>, z: B, f: (A, B) -> B): B =
        foldMap(fa, endoMonoid()) { a: A -> { b: B -> f(a, b) } }(z)

    fun <A, B> foldLeft(fa: Kind<F, A>, z: B, f: (B, A) -> B): B =
        foldMap(fa, dual(endoMonoid())) { a: A -> { b: B -> f(b, a) } }(z)

    fun <A, B> foldMap(fa: Kind<F, A>, m: Monoid<B>, f: (A) -> B): B =
        foldRight(fa, m.nil, { a, b -> m.combine(f(a), b) })
}
```

## EXERCISE 10.13

Implement Foldable<ForList> using the Foldable<F> interface from the previous exercise.

**NOTE**
A foldable version of the list implementation we developed in chapter 3 has been provided in the chapter 10 exercise boilerplate code.

```
object ListFoldable : Foldable<ForList> {

    override fun <A, B> foldRight(
```
