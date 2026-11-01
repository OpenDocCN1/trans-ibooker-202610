And do we always want to report both errors, or do we want to give the programmer a way to specify which of the two errors is reported?

# Combinators specify information to implementation

In a typical library design scenario where we have at least some idea of a concrete representation, we often think of functions in terms of how they will affect the final representation of our program.

By starting with the algebra first, we’re forced to think differently—we must think of functions in terms of what information they specify to a possible implementation. The signatures determine what information is given to the implementation, and the implementation is free to use this information however it wants as long as it respects any specified laws.

# 9.5.1 First attempt at representing errors

Now that we have considered these ideas about error handling, we will start defining the algebra by progressively introducing our error-reporting combinators. Let’s begin with an obvious one. None of the primitives so far let us assign an error message to a parser. We can introduce a primitive combinator for this called tag:

```
fun <A> tag(msg: String, p: Parser<A>) : Parser<A>
```

The intended meaning of tag is that if p fails, its ParseError will somehow incorporate msg. What does this mean exactly? Well, we could do the simplest thing possible and assume that ParseError is simply a type alias for String, and that the returned ParseError will equal the tag. But we’d like our parse error to also tell us where the problem occurred. Let’s tentatively add this concept to our algebra, call it Location.

# Listing 9.7 Representing ParseError in terms of message and location

```
data class Location(val input: String, val offset: Int = 0) {

    private val slice by lazy { input.slice(0..offset + 1) }

    val line by lazy { slice.count { it == '\n' } + 1 }

    val column by lazy {
        when (val n = slice.lastIndexOf('\n')) {
            -1 -> offset + 1
            else -> offset - n
        }
    }
}

fun errorLocation(e: ParseError): Location

fun errorMessage(e: ParseError): String
```

+   - prepare a substring of the input up to where the error occurred
+   - calculate the number of lines to the error location