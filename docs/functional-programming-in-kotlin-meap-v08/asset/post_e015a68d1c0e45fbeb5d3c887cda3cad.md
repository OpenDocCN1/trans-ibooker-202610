
③ calculate the number of columns to the error location

We’ve picked a concrete representation for `Location` here that includes the full input, an offset into this input where the error occurred, and the line and column numbers which are computed lazily from the full input and offset. We can now say more precisely what we expect from `tag`. In the event of failure with `Left(e)`, `errorMessage(e)` will equal the message set by `tag`. What about the `Location`? We’d like for this to be provided by the `Parsers` implementation with the location of where the error occurred. This notion still seems a bit fuzzy at the moment—if we have `a or b`, and both parsers fail on the input, which location will be reported? In addition to that, which tag(s) will we see? We’ll discuss this in greater depth in the following section on error nesting.

## 9.5.2 Accumulating errors through error nesting

Is the `tag` combinator sufficient for all our error-reporting needs? Not quite. Let’s take a closer look with an example:

```
tag("first magic word", string("abra")) product
    string(" ").many() product
    tag("second magic word", string("cadabra"))
```

- ① Tag the first parser
- ② Skip any whitespace
- ③ Tag the next parser

What sort of `ParseError` would we like to get back from `run(p, "abra cAdabra")`? Note the capital A in cAdabra. The immediate cause for error is this capital 'A' instead of the expected lowercase 'a'. That error has an exact location, and it will be helpful to report this somehow when debugging the issue. But reporting only that low-level error wouldn’t be very informative, especially if this were part of a large grammar and we were running the parser on an even larger input.

When using `tag`, we should have some more contextual information—the immediate error occurred in the `Parser` tagged "second magic word". This is certainly very helpful in pinpointing where things went wrong. Ideally, the error message should tell us that while parsing "cAdabra" using "second magic word", there was an unexpected capital 'A'. That highlights the error and gives us the context needed to understand it. Perhaps the top-level parser (p in this case) might be able to provide an even higher-level description of what the parser was doing when it failed, for instance "parsing magic spell", which could also be informative.

So it seems wrong to assume that one level of error reporting will always be sufficient. Let’s therefore provide a way to nest tags:

### Listing 9.8 The scope combinator is used for nesting tags

```
fun <A> scope(msg: String, p: Parser<A>): Parser<A>
```

Despite scope having the same method declaration as tag, the implementation of scope doesn’t throw away the tag(s) attached to p—it merely adds additional information in the event that p fails. Let’s specify what this means exactly. First, we modify the functions that pull information out of a ParseError. Rather than containing just a single Location and String message, we should rather get a List<Pair<Location, String>>:

### Listing 9.9 Representing stacked errors using the ParseError data type

```
data class ParseError(val stack: List<Pair<Location, String>>)
```

This is a stack of error messages indicating what the Parser was doing when it failed. We can now specify what scope does when it encounters multiple errors—if run(p, s) is Left(e1), then run(scope(msg, p), s) is Left(e2), where e2.stack.head will contain msg and e2.stack.tail will contain e1.

We can write helper functions later to make constructing and manipulating ParseError values more convenient, and also to format them nicely for human consumption. For now we just want to make sure it contains all the relevant information for error reporting. For now, it does seem like ParseError will be sufficient for most purposes. Let’s pick this as our concrete representation for use in the return type of run in the Parsers interface:

```
fun <A> run(p: Parser<A>, input: String): Either<ParseError, A>
```

## 9.5.3 Controlling branching and backtracking

There’s one last concern regarding error reporting that we need to address. As we just discussed, when we have an error that occurs inside an or combinator, we need some way of determining which error(s) to report. We don’t want to only have a global convention for this; we sometimes want to allow the programmer to control this choice. Let’s look at a more concrete motivating example:

```
val spaces = string(" ").many()

val p1 = scope("magic spell") {
    string("abra") product spaces product string("cadabra")
}
val p2 = scope("gibberish") {
    string("abba") product spaces product string("babba")
}

val p = p1 or p2
```

What ParseError would we like to get back from run(p, "abra cAdabra")? Again, note the offending capital A in cAdabra. Both branches of the or will produce errors on the input. The
"gibberish" parser will report an error due to expecting the first word to be "abba", and the "magic spell" parser will report an error due to the accidental capitalization in "cAdabra". Which of these errors do we want to report back to the user?

In this instance, we happen to want the "magic spell" parse error. After successfully parsing the "abra" word, we’re committed to the "magic spell" branch of the or, which means if we encounter a parse error, we don’t examine the subsequent branch of the or. In other instances, we may want to allow the parser to consider the next branch.

So it appears we need a primitive for letting the programmer indicate when to commit to a particular parsing branch. Recall that we loosely assigned p1 or p2 to mean, try running p1 on the input, and then try running p2 on the same input if p1 fails. We can change its meaning to try running p1 on the input, and if it fails in an uncommitted state, try running p2 on the same input; otherwise, report the failure. This is useful for more than just providing good error messages—it also improves efficiency by letting the implementation avoid examining lots of possible parsing branches.

One common solution to this problem is to have all parsers commit by default if they examine at least one character to produce a result. We now introduce a combinator called attempt, which delays committing to a parse:

```
fun <A> attempt(p: Parser<A>): Parser<A>
```

It should satisfy something like the following situation. This is not exactly an equality; even though we want to run p2 if the attempted parser p1 fails, we may want p2 to somehow incorporate the errors from both branches if it fails.

```
attempt(p1.flatMap { _ -> fail }) or p2 == p2
```

Here fail is a parser that always fails. In fact, we could introduce this as a primitive combinator if we like. What happens next is, even if p1 fails midway through examining the input, attempt reverts the commit to that parse and allows p2 to be run. The attempt combinator can be used whenever dealing with such ambiguous grammar. Multiple tokens may have to be examined before the ambiguity can be resolved and that parsing can commit to a single branch. As an example, we might write this:

```
(attempt(
    string("abra") product spaces product string("abra")
) product string("cadabra")) or
    (string("abra") product spaces product string("cadabra!"))
```

Suppose this parser is run on "abra cadabra!". After parsing the first "abra", we don’t know whether to expect another "abra" (the first branch) or "cadabra!" (the second branch). By
wrapping an attempt around string("abra") product spaces product string("abra"), we allow the second branch to be considered up until we’ve finished parsing the second "abra", at which point we commit to that branch.

EXERCISE 9.10

Can you think of any other primitives that might be useful for specifying what error(s) in an or chain get reported?

Note that we still haven’t written an actual implementation of our algebra! Despite the lack of implementation, this process has been more about making sure that our combinators provide a well defined interface for the users of our library to interact with. More than that, it should provide a way for them to convey the right information to the underlying implementation. It will then be up to the implementation to interpret the information in a way that satisfies the laws we’ve stipulated.

## 9.6 Implementing the algebra

This entire chapter has been focused on building up an algebra of definitions without implementing a single thing! This has culminated in a final definition of Parser<JSON>. At this point it would be prudent to go back and retrofit the parser that you developed in exercise 9.9 with the error-reporting combinators discussed in section 9.5 if you haven’t already done so. Now comes the exciting part where we define an implementation that can be run!

Our list of primitives has once again changed with the addition of our error handling combinators. Let’s review the list one more time.

### Table 9.3 Updated list of primitives to be used as basis for JSON parsing combinators

| Primitive | Description |
|---|---|
| `string(s)` | Recognizes and returns a single `String` |
| `regex(s)` | Recognizes a regular expression of `String` |
| `slice(p)` | Returns the portion of input inspected by `p` if successful |
| `tag(msg, p)` | In the event of failure, replaces the assigned message with `msg` |
| `scope(msg, p)` | In the event of failure, adds `msg` to the error stack returned by `p` |
| `flatMap(p, f)` | Runs a parser, then uses its result to select a second parser to run in sequence |
| `attempt(p)` | Delays committing to `p` until after it succeeds |
| `or(p1, p2)` | Chooses between two parsers, first attempting `p1`, then `p2` if `p1` fails |

The list has changed somewhat with the addition of tag, scope and attempt. We have also dropped succeed from our previous table 9.2.

In the next section, we’ll work through a representation for Parser and implement the Parsers interface using this representation. The algebra we’ve designed places strong constraints on
