
## Listing 8.2 Define Gen by wrapping a state transition over a random number generator

```
interface RNG {
    fun nextInt(): Pair<Int, RNG>
}

data class State<S, out A>(val run: (S) -> Pair<A, S>)

data class Gen<A>(val sample: State<RNG, A>)
```

## EXERCISE 8.4

Implement Gen.choose using this representation of Gen. It should generate integers in the range start to stopExclusive. Feel free to use functions you've already written.

```
fun choose(start: Int, stopExclusive: Int): Gen<Int> = TODO()
```

## EXERCISE 8.5

Let's see what else we can implement using this representation of Gen. Try implementing unit, boolean, and listOfN with the following signatures, once again drawing on functions previously written:

```
fun <A> unit(a: A): Gen<A> = TODO()

fun boolean(): Gen<Boolean> = TODO()

fun <A> listOfN(n: Int, ga: Gen<A>): Gen<List<A>> = TODO()
```

As discussed in chapter 7, we're interested in understanding which operations are primitive and which are derived, and in finding a small yet expressive set of primitives. A good way to explore what is possible with a given set of primitives is to pick some concrete examples you'd like to express and see if you can assemble the functionality you want. As you do so, look for patterns, try factoring out these patterns into combinators, and refine your set of primitives. We encourage you to stop reading here and simply play with the primitives and combinators we've written so far. If you want some concrete examples to inspire you, here are a few ideas:

- If we can generate a single Int in some range, do we also need a new primitive to generate a Pair<Int, Int> in some range?
- Can we produce a Gen<Option<A>> from a Gen<A>? What about a Gen<A> from Gen<Option<A>>?
- Can we generate strings using our existing primitives?

## SIDEBAR

### The importance of play

You don't have to wait around for a concrete example to explore the problem domain of your library. In fact, if you rely exclusively on such useful or important examples to design your API, you'll often miss out on crucial design aspects and end up writing APIs with overly specific features.

We don’t want to *overfit* our design to the particular examples we happen to think of right now. We want to reduce the problem to its essence, and sometimes the best way to do this is by *playing*. Don’t try to solve important problems or produce useful functionality. At least, not right away. Just experiment with different representations, primitives and operations. Let questions naturally arise and explore whatever piques your interest.

Observations like "These two functions seem similar. I wonder if there’s some more general operation hiding inside," or "Would it make sense to make this data type polymorphic?", or "What would it mean to change this aspect of the representation from a single value to a `List` of values?" will begin to surface.

There is no right or wrong way to do this, but there are so many different design choices that it’s impossible *not* to run headlong into fascinating questions to play with. It doesn’t matter where you begin—if you keep playing, the domain will inexorably guide you to make all the design choices that are required.

## 8.2.4 Generators that depend on generated values

Suppose we’d like a `Gen<Pair<String, String>>` that generates pairs where the second string contains only characters from the first. Or that we had a `Gen<Int>` that chooses an integer between 0 and 11, and we’d like to make a `Gen<List<Double>>` that then generates lists of whatever length is chosen. In both of these cases there’s a dependency—we generate a value, and then use that value to determine what generator to use next. For this we need `flatMap`, which lets one generator depend on another.

## EXERCISE 8.6

Implement flatMap, and then use it to implement this more dynamic version of listOfN. Place flatMap and listOfN in the Gen data class as shown.

```
data class Gen<A>(val sample: State<RNG, A>) {
    companion object {
        fun <A> listOfN(gn: Gen<Int>, ga: Gen<A>): Gen<List<A>> = TODO()
    }
    fun <B> flatMap(f: (A) -> Gen<B>): Gen<B> = TODO()
}
```

## EXERCISE 8.7

Implement union for combining two generators of the same type into one by pulling values from each generator with equal likelihood.

```
fun <A> union(ga: Gen<A>, gb: Gen<A>): Gen<A> = TODO()
```

## EXERCISE 8.8

Implement weighted, a version of union that accepts a weight for each Gen and generates values from each Gen with probability proportional to its weight.

```
fun <A> weighted(
    pga: Pair<Gen<A>, Double>,
    pgb: Pair<Gen<A>, Double>
): Gen<A> = TODO()
```

## 8.2.5 Refining the property data type

Now that we have explored the representation of our generators, let’s return to our definition of Prop. Our Gen representation has revealed information about the requirements for Prop. Our current definition of Prop looks like this, ignoring the and operator for now:

```
interface Prop {
    fun check(): Either<Pair<FailedCase, SuccessCount>, SuccessCount>
}
```

At this point Prop is nothing more than an Either, although it’s missing some vital information. We have the number of successful test cases in SuccessCount, but we haven’t specified how many test cases to examine before we consider the property to have passed the test. We could

certainly hardcode some value, but it would be far better to abstract over this detail. We will do so by injecting an integer aliased as TestCases. We will also turn Prop into a data class and make check a value instead of a method.

```
typealias TestCases = Int

typealias Result = Either<Pair<FailedCase, SuccessCount>, SuccessCount>

data class Prop(val check: (TestCases) -> Result)
```

Also, we’re recording the number of successful tests on both sides of Either. But when a property passes, it’s implied that the number of passed tests will be equal to the argument to check. So the caller of check learns nothing new by being told the success count. Since we don’t currently need any information in the Right case of that Either, we can turn it into an Option:

```
typealias Result = Option<Pair<FailedCase, SuccessCount>>

data class Prop(val check: (TestCases) -> Result)
```

This now seems a bit weird since None will mean that all tests succeeded, and Some will indicate a failure. Until now, we’ve only ever used the None case of Option to indicate failure, but in this case we’re using it to represent the absence of a failure. That is a perfectly legitimate use for Option, but its intent isn’t very clear. So let’s make a new data type equivalent to Option<Pair<FailedCase, SuccessCount>> that makes our intent more explicit.

## Listing 8.3 Model the possible results of a test run as an ADT

```
sealed class Result {
    abstract fun isFalsified(): Boolean
}

object Passed : Result() {
    override fun isFalsified(): Boolean = false
}

data class Falsified(
    val failure: FailedCase,
    val successes: SuccessCount
) : Result() {
    override fun isFalsified(): Boolean = true
}
```

- ① Sealed type of Result
- ② Subtype indicates that all tests passed
- ③ Subtype indicates that one of the test cases falsified the property

Is this a sufficient representation of Prop now? Let’s take another look at forAll. Can forAll be implemented? If not, why not?

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

As we can see, forAll doesn’t have enough information to return a Prop. Besides the number of test cases to try, check must have all the information needed to generate test cases. If it needs to generate random test cases using our current representation of Gen, it’s going to need an RNG. Let’s go ahead and supply this dependency to Prop:

## Listing 8.4 Supply an instance of RNG for Prop to allow test case generation

```
data class Prop(val check: (TestCases, RNG) -> Result)
```

If we think of other dependencies that it might need besides the number of test cases and the source of randomness, we can just add these as extra parameters to check later.

By supplying RNG as parameter to Prop, we now have enough information available to implement forAll. Here’s a first stab.

## Listing 8.5 Implementation of forAll using all the underlying building blocks

```
fun <A> forAll(ga: Gen<A>, f: (A) -> Boolean): Prop =
    Prop { n: TestCases, rng: RNG ->
        randomSequence(ga, rng).mapIndexed { i, a ->
            try {
                if (f(a)) Passed
                else Falsified(a.toString(), i)
            } catch (e: Exception) {
                Falsified(buildMessage(a, e), i)
            }
        }.take(n)
        .find { it.isFalsified() }
        .toOption()
        .getOrElse { Passed }
    }

private fun <A> randomSequence(
    ga: Gen<A>,
    rng: RNG
): Sequence<A> =
    sequence {
        val (a, rng2: RNG) = ga.sample.run(rng)
        yield(a)
        yieldAll(randomSequence(ga, rng2))
    }

private fun <A> buildMessage(a: A, e: Exception) =
    """
    |test case: $a
    |generated an exception: ${e.message}
    |stacktrace:
    |${e.stackTrace.joinToString("\n")}
    """.trimMargin()
```

- 1. Prepare a Sequence of indexes i mapped to generated values a
- 2. On test failure, record failed case and index exposing how many tests succeeded before failure.
- 3. In the case of an exception, record as a result with pretty message.
- 4. Generates infinite sequence of A recursively, sampling a generator.
