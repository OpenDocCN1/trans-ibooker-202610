# EXERCISE 5.4

Implement `forAll`, which checks that all elements in the `Stream` match a given predicate. Your implementation should terminate the traversal as soon as it encounters a non-matching value.

```
fun <A> Stream<A>.forAll(p: (A) -> Boolean): Boolean = TODO()
```

# EXERCISE 5.5

Use `foldRight` to implement `takeWhile`.

# EXERCISE 5.6

Implement `headOption` using `foldRight`.

# EXERCISE 5.7

Implement `map`, `filter`, `append`, and `flatMap` using `foldRight`. The `append` method should be non-strict in its argument.

TIP  Consider using previously defined methods where applicable.

Note that these implementations are *incremental*—they don’t fully generate their answers. It’s not until some other computation looks at the elements of the resulting `Stream` that the computation to generate that `Stream` actually takes place—and then it will do just enough work to generate the requested elements. Because of this incremental nature, we can call these functions one after another without fully instantiating the intermediate results.

Let’s look at a simplified program trace for a fragment of the motivating example we started this chapter with, `Stream.of(1, 2, 3, 4).map { it + 10 }.filter { it % 2 == 0 }`. We leave off the final transformation, `.map { it * 3 }` for the sake of simplicity. We’ll convert this expression to a `List` to force evaluation. Take a minute to work through this trace to understand what’s happening. It’s a bit more challenging than the trace we looked at earlier in this chapter. Remember, a trace like this is just the same expression over and over again, evaluated by one more step each time.

# Listing 5.4 Trace representing evaluation order of operations on a Stream

```
import chapter3.Cons as ConsL
import chapter3.Nil as NilL
```

```
Stream.of(1, 2, 3, 4).map { it + 10 }
    .filter { it % 2 == 0 }
```