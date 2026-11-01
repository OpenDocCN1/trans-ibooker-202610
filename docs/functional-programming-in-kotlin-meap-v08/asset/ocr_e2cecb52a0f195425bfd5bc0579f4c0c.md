Par<A> being an alias for the function type (ExecutorService) -> Future<A>.

```
val es = Executors.newCachedThreadPool()
val pl = forAll(Gen.unit(Pars.unit(1))) { pi ->
  map(pi, { it + 1 })(es).get() == Pars.unit(2)(es).get()
}
```

The resulting test is verbose, cluttered, and the idea of the test is obscured by irrelevant detail. Notice that this isn’t a question of the API being expressive enough—yes, we can express what we want, but a combination of missing helper functions and poor syntax obscures the real intent.

# PROVING PROPERTIES

Next, let’s improve on this verbosity and clutter. Our first observation is that forAll is a bit too general for this test case. We aren’t varying the input to the test, we just have a hardcoded example which in turn should be as convenient to write as in any traditional unit testing framework. Let’s introduce a combinator for it on the Prop companion object:

```
fun check(p: () -> Boolean): Prop = TODO()
```

How would we implement this? One possible way is to use forAll:

```
fun check(p: () -> Boolean): Prop {
  val result by lazy { p() }
  return forAll(Gen.unit(Unit)) {
    result
  }
}
```

+   ① Pass in a non-strict value
+   ② Result is memoized to avoid recomputation

This doesn’t seem quite right. We’re providing a unit generator that only generates a single Unit value. Then we proceed by ignoring that value just to force evaluation of the given Boolean. Not great.

Even though we memoize the result so that it’s not evaluated more than once, the test runner will still generate multiple test cases and test the Boolean many times. For example, if we execute run(check(true)), this will test the property 100 times and print “OK, passed 100 tests.” But checking a property that is always true 100 times is a terrible waste of effort. What we need is a new primitive.

The representation of Prop that we have so far is just a function of type (MaxSize, TestCases, RNG) -> Result, where Result is either Passed or Falsified. A simple implementation of a check primitive is to construct a Prop that ignores the number of test cases:

```
fun check(p: () -> Boolean): Prop =
  Prop { _, _, _ ->
    if (p()) Passed
```