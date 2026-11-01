# EXERCISE 4.7

Implement `sequence` and `traverse` for `Either`. These should return the first error that’s encountered, if there is one.

```
fun <E, A, B> traverse(
    xs: List<A>,
    f: (A) -> Either<E, B>
): Either<E, List<B>> =
    when (xs) {
        is Nil -> Right(Nil)
        is Cons ->
            map2(f(xs.head), traverse(xs.tail, f)) { b, xb ->
                Cons(b, xb)
            }
    }

fun <E, A> sequence(es: List<Either<E, A>>): Either<E, List<A>> =
    traverse(es) { it }
```

# EXERCISE 4.8

In the implementation found in Listing 4.8, `map2` is only able to report one error, even if both the name and the age are invalid. What would you need to change in order to report both errors? Would you change `map2` or the signature of `mkPerson`? Or could you create a new data type that captures this requirement better than `Either` does, with some additional structure? How would `orElse`, `traverse`, and `sequence` behave differently for that data type?

There are a number of variations on `Option` and `Either`. If we want to accumulate multiple errors, a simple approach is a new data type that lets us keep a list of errors in the data constructor that represents failures:

```
sealed class Partial<out A, out B>

data class Failures<out A>(val get: List<A>) : Partial<A, Nothing>()
data class Success<out B>(val get: B) : Partial<Nothing, B>()
```

There is a type very similar to this called `Validated` in the Arrow library. You can implement `map`, `map2`, `sequence`, and so on for this type in such a way that errors are accumulated when possible (`flatMap` is unable to accumulate errors—can you see why?). This idea can even be generalized further—we don’t need to accumulate failing values into a list; we can accumulate values using any user-supplied binary function. It’s also possible to use `Either<List<E>, _>` directly to accumulate errors, using different implementations of helper functions like `map2` and `sequence`.