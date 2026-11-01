```
unit(2)
)
)
```

With all these stepwise improvements, our property has become easier to understand and use. These might seem like minor changes, but such refactoring and cleanup has a huge effect on the usability of our library. The helper functions we’ve written make the properties easier to read and more pleasant to work with.

Let’s look at some other properties from chapter 7. Recall that we generalized our test case:

```
map(unit(x), f) == unit(f(x))
```

We then simplified it to the law that mapping the identity function over a computation should have no effect:

```
map(y, id) == y
```

Can we express this? Not exactly. This property implicitly states that the equality holds for all choices of y, for all types. We’re forced to pick particular values for y:

```
val pint: Gen<Par<Int>> = {
    Gen.choose(0, 10).map {
        unit(it)
    }
}

val p = forAllPar(pint) { n ->
    equal(map(n) { it }, n)
}
```

We can certainly range over more choices of y, but what we have here is probably good enough. The implementation of map doesn’t care about the values of our parallel computation, so there isn’t much point in constructing the same test for Double, String, and so on. What can affect map is the structure of the parallel computation. If we wanted greater assurance that our property held, we could provide richer generators for the structure. Here, we’re only supplying Par expressions with one level of nesting.

**EXERCISE 8.15**

Write a richer generator for `Par<Int>` which builds more deeply nested parallel computations than the simple variant we've provided so far.

**EXERCISE 8.16**

Express the property about `fork` from chapter 7 that `fork(x) == x`.