```
fun choose(start: Int, stopExclusive: Int): Gen<Int> =
    Gen(State { rng: RNG -> nonNegativeInt(rng) }
        .map { start + (it % (stopExclusive - start)) })
```

# EXERCISE 8.5

Let’s see what else we can implement using this representation of Gen. Try implementing unit, boolean, and listOfN with the following signatures, once again drawing on functions previously written:

```
fun <A> unit(a: A): Gen<A> = Gen(State.unit(a))

fun boolean(): Gen<Boolean> =
    Gen(State { rng -> nextBoolean(rng) })

fun <A> listOfN(n: Int, ga: Gen<A>): Gen<List<A>> =
    Gen(State.sequence(List(n) { ga.sample }))
```

This solution draws heavily on the State API that was developed in chapter 6. We have hinged our solution on the State.sequence() function, which is able to convert a List<State<S, A>> into a State<A, List<A>>. When applying the list containing n and the wrapped sample to this state transition, we get back a new State which can subsequently be wrapped up again as a new Gen.

# EXERCISE 8.6

Implement flatMap, and then use it to implement this more dynamic version of listOfN. Place flatMap and listOfN in the Gen data class as shown.

```
data class Gen<A>(val sample: State<RNG, A>) {

    companion object {
        fun <A> listOfN(gn: Gen<Int>, ga: Gen<A>): Gen<List<A>> =
            gn.flatMap { n -> listOfN(n, ga) }
    }

    fun <B> flatMap(f: (A) -> Gen<B>): Gen<B> =
        Gen(sample.flatMap { a -> f(a).sample })
}
```

# EXERCISE 8.7

Implement union for combining two generators of the same type into one by pulling values from each generator with equal likelihood.

```
fun <A> union(ga: Gen<A>, gb: Gen<A>): Gen<A> =
    boolean().flatMap { if (it) ga else gb }
```

# EXERCISE 8.8

Implement weighted, a version of union that accepts a weight for each Gen and generates values from each Gen with probability proportional to its weight.

```
fun <A> weighted(
```