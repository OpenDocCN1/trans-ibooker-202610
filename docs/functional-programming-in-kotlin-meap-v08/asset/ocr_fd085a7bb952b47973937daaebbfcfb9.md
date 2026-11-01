5 Use string interpolation and margin trim to build a pretty message.

Notice that we’re catching exceptions and reporting them as test failures rather than letting `check` throw the exception. This is so that we don’t lose information about what argument potentially triggered the failure.

**NOTE** We are using the Kotlin standard library `Sequence` type that allows us to generate a lazy stream of values by using the `sequence`, `yield` and `yieldAll` functions. The detail of this is not very important, and all we need to know is that we take *n* elements from the `Sequence` and apply a terminal operation to `find` an occurrence that is falsified, else we report a pass.

**EXERCISE 8.9** Now that we have a representation of `Prop`, implement `and` and `or` for composing `Prop` values. Notice that in the case of an `or` failure, we don’t know which property was responsible, the left or the right. Can you devise a way of handling this?

```
```kotlin
data class Prop(val run: (TestCases, RNG) -> Result) {
    fun and(p: Prop): Prop = TODO()
    fun or(p: Prop): Prop = TODO()
}
```
```

# 8.3 Test case minimization

Earlier, we mentioned the idea of test case minimization. By this we mean that we’d like our framework to find the smallest or simplest failing test case to better illustrate a failure and facilitate debugging. Let’s see if we can tweak our representations to support this outcome. There are two general approaches we could take:

+   - *Shrinking* — After we’ve found a failing test case, we can run a separate procedure to minimize the test case by successively decreasing its “size” until it no longer fails. This is called *shrinking*, and it usually requires us to write separate code for each data type to implement this minimization process.
+   - *Sized generation* — Rather than shrinking test cases, we simply generate our test cases in order of *increasing* size and complexity. So we start small and increase the size until we find a failure. This idea can be extended in various ways to allow the test runner to make larger jumps in the space of possible sizes while still making it possible to find the smallest failing test.

KotlinTest, in addition to most of the popular property-based testing frameworks like ScalaCheck³⁹ and Haskell’s QuickCheck⁴⁰ take the first approach of shrinking. Due to the greater complexity involved in implementing the approach, we’ll choose to use the alternative