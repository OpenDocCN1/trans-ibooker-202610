possible representations. We should be able to come up with a simple, purely functional representation of Parser that can be used to implement the Parsers interface. But first, let’s express the top level constructs that will be used as starting point for our implementation.

# Listing 9.10 Top level representation of the parser combinator library

```
interface Parser<A>

data class ParseError(val stack: List<Pair<Location, String>>)

abstract class Parsers<PE> {
    internal abstract fun <A> or(p1: Parser<A>, p2: Parser<A>): Parser<A>
}

open class ParsersImpl<PE>() : Parsers<PE>() {
    override fun <A> or(p1: Parser<A>, p2: Parser<A>): Parser<A> = TODO()
}

abstract class ParserDsl<PE> : ParsersImpl<PE>() {
    infix fun <A> Parser<A>.or(p: Parser<A>): Parser<A> =
        this@ParserDsl.or(this, p)
}

object Example : ParserDsl<ParseError>() {
    init {
        val p1: Parser<String> = TODO()
        val p2: Parser<String> = TODO()
        val p3 = p1 or p2
    }
}
```

+   1. Whatever representation we will discover for Parser, this is merely an example
+   2. The Parsers class holds all the unimplemented primitive and combinator algebra definitions
+   3. The ParsersImpl is the concrete implementation for all Parsers
+   4. The ParserDsl adds syntactic sugar to make working with combinators easier
+   5. Access the or function in ParserImpl through this@ParserDsl, workaround to prevent a circular reference.
+   6. An object that makes use of our combinator library

# 9.6.1 Building up the algebra implementation gradually

We are finally going to discuss a concrete implementation of Parsers that fulfills all the accumulated features so far. Rather than jumping straight to the end with a final representation of Parser, we’ll build it up gradually. We will do so by inspecting the primitives of the algebra, then reasoning about the information that will be required to support each one.

Let’s begin with the string combinator:

```
fun string(s: String): Parser<String>
```

We also know we need to support the function run: