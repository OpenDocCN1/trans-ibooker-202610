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