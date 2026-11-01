Sequence consisting of one `Prop` per size using the previously defined `forAll` function from Listing 8.5. Lastly, it combines them all into a single property using an updated version of our previously defined `and` function. At the end of all this, the remaining reduced property is checked.

# 8.4 Using the library and improving user experience

We’ve now converged on what seems like a reasonable API. We could keep tinkering with it, but at this point let’s try *using* it instead. We will do this by constructing tests and seeing if we notice any deficiencies, either in what it can express or in its general usability. Usability is somewhat subjective, but we generally like to have convenient syntax and appropriate helper functions for common usage patterns. We aren’t necessarily aiming to make the library more expressive, but we do want to make it pleasant to use.

# 8.4.1 Some simple examples

Let’s revisit an example that we mentioned at the start of this chapter—specifying the behavior of a function `max`, available as a method on `List<Int>`. The maximum of a list should be greater than or equal to every other element in the list. Let’s specify this:

# Listing 8.8 Property specifying maximum value in a list

```
val smallInt = Gen.choose(-10, 10)

val maxProp = forAll(SGen.listOf(smallInt)) { ns ->
    val mx = ns.max()
    ?: throw IllegalStateException("max on empty list")
    !ns.exists { it > mx } ①
}
```

① No value greater than mx should exist in ns

At this point, calling `check` directly on a `Prop` is rather cumbersome. We can introduce a helper function for running property values and printing their result to the console in a useful format. Let’s simply call it `run`.

# Listing 8.9 A convenience method for running properties using sensible defaults

```
fun run(
    p: Prop,
    maxSize: Int = 100, ①
    testCases: Int = 100, ②
    rng: RNG = SimpleRNG(System.currentTimeMillis()) ③
): Unit =
    when (val result = p.check(maxSize, testCases, rng)) {
        is Falsified -> ④
            println(
                "Falsified after ${result.successes}" +
                "passed tests: ${result.failure}"
            )
        is Passed -> ⑤
            println("OK, passed $testCases tests.")
    }
```