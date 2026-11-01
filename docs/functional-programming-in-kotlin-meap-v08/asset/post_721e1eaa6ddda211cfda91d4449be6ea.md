
### Listing 9.17 Extension function to tag `ParseError` on `Result` failure

```
fun ParseError.tag(msg: String): ParseError {
    val latest = this.stack.lastOrNone() ①
    val latestLocation = latest.map { it.first } ②
    return ParseError(latestLocation.map { Pair(it, msg) }.toList()) ③
}
```

- ① Gets the last element of the stack or None if the stack is empty.
- ② Use its location if present
- ③ Assemble a new `ParseError` with *only* this location and the tag message.

### EXERCISE 9.12

Revise your implementation of `string` to provide a meaningful error message in the event of an error.

## 9.6.4 Recovering from error conditions and backtracking over them

Next, let’s consider `or` and `attempt`. If we consider what we’ve already learned about `or`, we can summarize its behavior as follows: it should run the first parser, and if that fails *in an uncommitted state*, it should run the second parser on the same input. We also said that consuming at least one character should result in a *committed* parse, and that `attempt(p)` converts committed failures of p to *uncommitted* failures.

We can support the behavior we want by simply adding a field to the `Failure` case of `Result`. All we need is a `Boolean` value indicating whether the parser failed in a committed state, let’s call it `isCommitted`:

```
data class Failure(
    val get: ParseError,
    val isCommitted: Boolean
) : Result<Nothing>()
```

The implementation of `attempt` now draws upon this new information and simply cancels the commitment of any failures that occur. It does so by using a helper function called `uncommit`, which we can define on `Result`:

### Listing 9.18 An implementation of attempt that cancels commitment of any failures

```
fun <A> attempt(p: Parser<A>): Parser<A> = { s -> p(s).uncommit() }

fun <A> Result<A>.uncommit(): Result<A> =
    when (this) {
        is Failure ->
            if (this.isCommitted)
                Failure(this.get, false)
            else this
        is Success -> this
    }
```

Now the implementation of or can simply check the isCommitted flag before running the second parser. Consider the parser x or y: if x succeeds, then the whole expression succeeds. If x fails in a committed state, we fail early and skip running y. Otherwise, if x fails in an uncommitted state, we run y and ignore the result of x:

### Listing 9.19 An implementation of or that honors committed state

```
fun <A> or(pa: Parser<A>, pb: () -> Parser<A>): Parser<A> =
    { state ->
        when (val r: Result<A> = pa(state)) {
            is Failure ->
                if (!r.isCommitted) pb() (state) ①
                else r ②
            is Success -> r ③
        }
    }
```

- ① An uncommitted failure will invoke lazy pb and run it with original state passed to or
- ② A committed failure will pass through
- ③ Success will pass through

## 9.6.5 Propagating state through context-sensitive parsers

Now for the final primitive in our list, flatMap. Recall that flatMap enables context-sensitive parsers by allowing the selection of a second parser to depend on the result of the first parser. The implementation is simple as we advance the location before calling the second parser. Again we will use a helper function, this time called advanceBy on Location. Despite this being simple, there is one caveat to be dealt with: if the first parser consumes any characters, we ensure that the second parser is committed using a helper function called addCommit on ParseError.

### Listing 9.20 Ensuring that the parser is committed using addCommit

```
fun <A, B> flatMap(pa: Parser<A>, f: (A) -> Parser<B>): Parser<B> =
    { state ->
        when (val result = pa(state)) {
            is Success ->
                f(result.a)(state.advanceBy(result.consumed))
                    .addCommit(result.consumed != 0)
                    .advanceSuccess(result.consumed)
            is Failure -> result
        }
    }
```

- ① Advance the source location before calling second parser.
- ② Commit if the first parser has consumed any characters.
- ③ Increment number of characters consumed to account for characters already consumed by pa.

In advanceBy on Location, we simply increment the offset:

```
fun Location.advanceBy(n: Int): Location =
    this.copy(offset = this.offset + n)
```

The addCommit function on ParseError is equally straightforward, ensuring that the committed state is updated if it was not already committed:

```
fun <A> Result<A>.addCommit(commit: Boolean): Result<A> =
    when (this) {
        is Failure ->
            Failure(this.get, this.isCommitted || commit)
        is Success -> this
    }
```

The final piece of the puzzle is the advanceSuccess function on Result, which is responsible for incrementing the number of consumed characters of a successful result. We want the total number of characters consumed by flatMap to be the sum of the consumed characters of the parser pa and the parser produced by f. We use advanceSuccess on the result of f to ensure that this adjustment is made:

```
fun <A> Result<A>.advanceSuccess(n: Int): Result<A> =
    when (this) {
        is Success ->
            Success(this.a, this.consumed + n)
        is Failure -> this
    }
```

- ① Advance the number of consumed characters by n on success
- ② Pass through the result on failure.

### EXERCISE 9.13
Implement `run`, as well as any of the remaining primitives not yet implemented using our current representation of `Parser`. Try running your JSON parser on various inputs.

### NOTE
We should now have working code, although unfortunately you'll find that this causes a stack overflow for large inputs. Even though it is not required for this exercise, one simple solution to this problem is to provide a specialized implementation of `many` that avoids using a stack frame for each element of the list being built up. Ensuring that any combinators that do repetition are defined in terms of `many` (which they all can be), this solves the problem.

### EXERCISE 9.14
Come up with a nice way of formatting a `ParseError` for human consumption. There are a lot of choices to make, but a key insight is that we typically want to combine or group tags attached to the same location when presenting the error as a `String` for display.

We could spend a lot more time improving and developing the example in this chapter, but we’ll leave it at what it is for now. We do encourage you to keep playing and enhancing what you have on your own, though the parser combinator library itself really isn’t the most important point that we’re trying to bring home in this chapter—it was really all about demonstrating the approach of algebra-first library design.

This chapter concludes part 2 of the book. We hope that you’ve come away with an understanding of how to go about designing your own functional library. More importantly, we also hope that this section will inspire you to begin designing and building your own libraries based on domains that are of personal interest to you. Functional design isn’t something reserved only for experts. It should be part of the day-to-day work done by functional programmers at all levels of experience.

Before you start on part 3, we implore you to venture out on your own by designing some libraries while writing functional code as you’ve been learning up to this point. Have lots of fun while you wrestle with, and conquer design problems that emerge as you go along. When you are done playing and come back, a universe of patterns and abstractions awaits you in part 3.

## 9.7 Summary

- Algebraic library design establishes the interface with associated laws up front, then drives implementation as a result.
- A parser combinator library provides a motivating use case for functional library design and is well suited for an algebraic design approach.
- Primitives are simple combinators that don’t depend on others and provide building blocks for more complex higher order combinators.
- Algebraic design encourages invention of primitives first which allows discovery of more complex combinators to follow.
- A combinator is said to be context sensitive when it passes on state, so allowing the ability to sequence combinators.
- A parser combinator may accumulate errors, which allows for surfacing an error report in case of failure.
- A parser may fail with an uncommitted state which allows for backtracking and recovery from errors.
- Starting design with the algebra lets combinators specify information to implementation
