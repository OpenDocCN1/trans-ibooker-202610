We need *non-strict* evaluation to support early termination—we discuss this in chapter 5.

# EXERCISE 3.7

See what happens when you pass Nil and Cons themselves to foldRight, like this:

```
fun <A, B> foldRight(xs: List<A>, z: B, f: (A, B) -> B): B =
    when (xs) {
        is Nil -> z
        is Cons -> f(xs.head, foldRight(xs.tail, z, f))
    }

val f = { x: Int, y: List<Int> -> Cons(x, y) }
val z = Nil as List<Int>

val trace = {
    foldRight(List.of(1, 2, 3), z, f)
    Cons(1, foldRight(List.of(2, 3), z, f))
    Cons(1, Cons(2, foldRight(List.of(3), z, f)))
    Cons(1, Cons(2, Cons(3, foldRight(List.empty(), z, f))))
    Cons(1, Cons(2, Cons(3, Nil)))
}
```

What do you think this says about the relationship between foldRight and the data constructors of List?

*Replacing z and f with Nil and Cons respectively when invoking foldRight results in xs being copied.*

# EXERCISE 3.8

Compute the length of a list using foldRight.

```
fun <A> length(xs: List<A>): Int =
    foldRight(xs, 0, { _, acc -> 1 + acc })
```

# EXERCISE 3.9

Our implementation of foldRight is not tail-recursive and will result in a StackOverflowError for large lists (we say it’s not stack-safe). Convince yourself that this is the case, and then write another general list-recursion function, foldLeft, that is tail-recursive, using the techniques we discussed in the previous chapter.

```
tailrec fun <A, B> foldLeft(xs: List<A>, z: B, f: (B, A) -> B): B =
    when (xs) {
        is Nil -> z
        is Cons -> foldLeft(xs.tail, f(z, xs.head), f)
    }
```

# EXERCISE 3.10

Write sum, product, and a function to compute the length of a list using foldLeft.

```
fun sumL(xs: List<Int>): Int =
    foldLeft(xs, 0, { x, y -> x + y })
```