# EXERCISE 3.16

Write a function that turns each value in a `List<Double>` into a `String`. You can use the expression `d.toString()` to convert some `d: Double` to a `String`.

**TIP**      Again, use `foldRight` without resorting to recursion.

# EXERCISE 3.17

Write a function `map` that generalizes modifying each element in a list while maintaining the structure of the list. Use the `foldRight` variant that uses `foldLeft` in order to prevent large lists from blowing the stack.

**TIP**      Once more, use `foldRight` without resorting to recursion.

# EXERCISE 3.18

Write a function `filter` that removes elements from a list unless they satisfy a given predicate. Use it to remove all odd numbers from a `List<Int>`.

**TIP**      One more time, `foldRight` is your friend!

# EXERCISE 3.19

Write a function `flatMap` that works like `map` except that the function given will return a list instead of a single result, and that list should be inserted into the final resulting list. Here is its signature:

```
fun <A, B> flatMap(xa: List<A>, f: (A) -> List<B>): List<B> = TODO()
```

**TIP**      Use a combination of existing functions that we have already defined.

# EXERCISE 3.23

As an example, implement `hasSubsequence` for checking whether a `List` contains another `List` as a subsequence. For instance, `List(1,2,3,4)` would have `List(1,2)`, `List(2,3)`, and `List(4)` as subsequences, among others. You may have some difficulty finding a concise purely functional implementation that is also efficient. That’s okay. Implement the function however comes most naturally. We’ll return to this implementation in chapter 5 and hopefully improve on it.

As an extra hint, the exercise file suggests starting by implementing the following function: