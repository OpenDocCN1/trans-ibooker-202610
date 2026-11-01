
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

### EXERCISE 8.15

Write a richer generator for `Par<Int>` which builds more deeply nested parallel computations than the simple variant we've provided so far.

### EXERCISE 8.16

Express the property about `fork` from chapter 7 that `fork(x) == x`.

## 8.5 Generating higher-order functions and other possibilities

So far, our library seems quite expressive, but there is one area where it’s lacking: we don’t have a good way to test higher-order functions. While we have lots of ways of generating *data* using our generators, we don’t really have a good way of generating *functions*. In this section we deal with generating functions in order to test higher-order functions.

For instance, let’s consider the `takeWhile` function defined for `List` and `Sequence`. Recall that this function returns the longest prefix of its input whose elements all satisfy a predicate. For instance, `listOf(1,2,3).takeWhile { it < 3 }` results in `List(1,2)`. A simple property we’d like to check is that for any list, `s: List<A>`, and any `f: (A) -> Boolean`, the expression `s.takeWhile(f).forAll(f)` evaluates to `true`. That is, every element in the returned list satisfies the predicate.⁴¹

### EXERCISE 8.17

Come up with some other properties that `takeWhile` should satisfy. Can you think of a good property expressing the relationship between `takeWhile` and `dropWhile`?

We could certainly take the approach of only examining *particular* arguments when testing higher-order functions. For instance, here’s a more specific property for `takeWhile`:

```
val isEven = { i: Int -> i % 2 == 0 }

val takeWhileProp =
    Prop.forAll(Gen.listOfN(n, ga)) { ns ->
        ns.takeWhile(isEven).forAll(isEven)
    }
```

This works, but is there a way we could let the testing framework handle generating functions to use with `takeWhile` instead? Let’s consider our options. To make this concrete, let’s suppose we have a `Gen<Int>` and would like to produce a `Gen<(String) -> Int>`. What are some ways we could do that? Well, we could produce `(String) -> Int` functions that simply ignore their input and delegate to the underlying `Gen<Int>`:

```
fun genStringIntFn(g: Gen<Int>): Gen<(String) -> Int> =
    g.map { i -> { _: String -> i } }
```

This approach isn’t sufficient though. We’re simply generating constant functions that ignore their input. In the case of `takeWhile`, where we need a function that returns a `Boolean`, this will be a function that always returns `true` or `false` depending on what the underlying boolean generator passes it—clearly not very interesting for testing the behavior of our function.

```
fun genIntBooleanFn(g: Gen<Boolean>): Gen<(Int) -> Boolean> =
    g.map { b: Boolean -> { _: Int -> b } }
```

Now, let’s consider the following function that will return a function generator that will perform some logic based on a value passed to it. In this case a threshold t is passed, and any Int injected into the function will be tested to see if the value exceeds t.

```
fun genIntBooleanFn(t: Int): Gen<(Int) -> Boolean> =
    Gen.unit { i: Int -> i > t }
```

Now let’s put our new function generator to work. We begin by generating a List<Int> as well as a random threshold value. We pre-load our function generator with the given random threshold and let it produce its function of (Int) -> Boolean. Lastly, we apply this generated function to takeWhile on our generated list and then apply the same predicate to forAll which should always result in true.

```
val gen: Gen<Boolean> =
    Gen.listOfN(100, Gen.choose(1, 100)).flatMap { ls: List<Int> ->
        Gen.choose(1, ls.size / 2).flatMap { threshold: Int ->
            genIntBooleanFn(threshold).map { fn: (Int) -> Boolean ->
                ls.takeWhile({fn}).forAll(fn)
            }
        }
    }
```

When run in context of our test harness using Prop.forAll, we should always see the test passing.

```
run(Prop.forAll(gen) { success -> success })
```

Even though this example is somewhat contrived and trivial, it sufficiently demonstrates what is possible in terms of random function generators. We can take these ideas much further, and suggest that you explore the options by following the expansive exercises found in appendix C at the back of the book.

## 8.6 The laws of generators

As we’ve worked through the process of designing our library, we see patterns emerging that we’ve come across many times before in previous chapters. Many of the combinators we’ve discovered even have the same name and functionality. For example, many of the functions we’ve implemented for our Gen type look quite similar to other functions we defined on Par, List, Stream, and Option. Looking back at our implementation of Par in chapter 7 reveals that we defined the following combinator:

```
fun <A, B> map(a: Par<A>, f: (A) -> B): Par<B> = TODO()
```

And in this chapter we defined map for Gen (as a method on Gen<A>):

```
fun <A, B> map(a: Gen<A>, f: (A) -> B): Gen<B> = TODO()
```

We’ve also defined similar-looking functions for `Option`, `List`, `Stream`, and `State`. We have to wonder, is it merely that our functions share similar-looking signatures, or do they satisfy the same *laws* as well? Let’s look at a law we introduced for `Par` in chapter 7:

```
map(y, id) == y
```

Does this law hold true for our implementation of `Gen.map`? What about for `Stream`, `List`, `Option`, and `State`? Yes, it does! Try it and see. This indicates that not only do these functions share similar-looking signatures, they also in some sense have analogous meanings in their respective domains. It appears there are deeper forces at work! We’re uncovering some fundamental patterns that cut across all these domains. In part 3, we’ll learn the names for these patterns, discover the laws that govern them, and understand what it all means.

To reiterate, the goal was not necessarily to learn about property-based testing as such, but rather to highlight particular aspects of functional design. First, we saw that oscillating between the abstract algebra and the concrete representation lets the two inform each other. This avoids over-fitting the library to a particular representation, and also avoids a disconnected abstraction which is far removed from the end goal.

Second, we noticed that this domain led us to discover many of the same combinators we’ve now seen a few times before: `map`, `flatMap`, and so on. Not only are the signatures of these functions analogous, the *laws* satisfied by the implementations are analogous too. There are a great many seemingly distinct *problems* being solved in the world of software, yet the space of functional *solutions* is much smaller. Many libraries are just simple combinations of certain fundamental structures that appear over and over again across a variety of different domains. This is an opportunity for code reuse that we’ll exploit in part 3. We will learn both the names of these structures as well as how to spot more general abstractions.
