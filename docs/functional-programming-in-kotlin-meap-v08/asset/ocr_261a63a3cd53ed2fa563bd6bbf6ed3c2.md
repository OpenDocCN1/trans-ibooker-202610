# NOTE

Simply passing in `Nil` is not sufficient as we are lacking the type information of `A` in this context. As a result we need to express this as `Nil` as `List<Int>`. Since this is very verbose, a convenience method to circumvent this can be added to the companion object:

```
fun <A> empty(): List<A> = Nil
```

This method will be used in all subsequent listings and exercises to represent an empty `List`.

# EXERCISE 3.8

Compute the length of a list using `foldRight`.

```
fun <A> length(xs: List<A>): Int = TODO()
```

# WARNING

From this point on, the exercises will noticeably increase in difficulty. So much so that in many instances they'll stretch you beyond what you know. If you can't do an exercise, that is perfectly okay and to be expected. Simply try your best to solve each one, and if you *really* don't succeed, refer to Appendix B for the solution, along with an explanation of how to solve the problem where applicable. As stated before, this should only be done as a last resort or when verifying your final solutions. Also, please refrain from skipping any exercises, as each exercise will build upon the knowledge gained by the previous one. The content of the chapter can only be grasped fully by working through each exercise, this being the recurring theme throughout the book.

# EXERCISE 3.9

Our implementation of `foldRight` is not tail-recursive and will result in a `StackOverflowError` for large lists (we say it's not stack-safe). Convince yourself that this is the case, and then write another general list-recursion function, `foldLeft`, that is tail-recursive, using the techniques we discussed in the previous chapter. Here is its signature:

```
tailrec fun <A, B> foldLeft(xs: List<A>, z: B, f: (B, A) -> B): B = TODO()
```

# EXERCISE 3.10

Write `sum`, `product`, and a function to compute the length of a list using `foldLeft`.