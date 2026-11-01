**TIP** Many `Stream` functions can start by matching on the `Stream` and considering what to do in each of the two cases. These particular functions needs to first consider whether it needs to look at the stream at all.

# EXERCISE 5.4

Implement `forAll`, which checks that all elements in the `Stream` match a given predicate. Your implementation should terminate the traversal as soon as it encounters a non-matching value.

**TIP** Use `foldRight` to implement this.

# EXERCISE 5.6 (HARD)

Implement `headOption` using `foldRight`.

**TIP** Let `None: Option<A>` be the first argument to `foldRight`. Follow the types from there.

# EXERCISE 5.9

Write a function that generates an infinite stream of integers, starting from n, then n + 1, n + 2, and so on.

**TIP** The example function `ones` is recursive, how could you define `from` recursively?

# EXERCISE 5.10

Write a function `fibs` that generates the infinite stream of Fibonacci numbers: 0, 1, 1, 2, 3, 5, 8, and so on.

**TIP** Chapter two discussed writing loops functionally, using a recursive helper function. Consider using the same approach here.

# EXERCISE 5.11

Write a more general stream-building function called `unfold`. It takes an initial state, and a function for producing both the next state and the next value in the generated stream.