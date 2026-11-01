```
// other combinators

internal abstract fun char(c: Char): Parser<Char>

internal abstract fun <A> many(p: Parser<A>): Parser<List<A>>

internal abstract fun <A> many1(p: Parser<A>): Parser<List<A>>

internal abstract fun <A> listOfN(
    n: Int,
    p: Parser<A>
): Parser<List<A>>

internal abstract fun <A, B> product(
    pa: Parser<A>,
    pb: () -> Parser<B>
): Parser<Pair<A, B>>

internal abstract fun <A, B, C> map2(
    pa: Parser<A>,
    pb: () -> Parser<B>,
    f: (A, B) -> C
): Parser<C>

internal abstract fun <A, B> map(pa: Parser<A>, f: (A) -> B): Parser<B>

internal abstract fun <A> defer(pa: Parser<A>): () -> Parser<A>

internal abstract fun <A> skipR(
    pa: Parser<A>,
    ps: Parser<String>
): Parser<A>

internal abstract fun <B> skipL(
    ps: Parser<String>,
    pb: Parser<B>
): Parser<B>

internal abstract fun <A> sep(
    p1: Parser<A>,
    p2: Parser<String>
): Parser<List<A>>

internal abstract fun <A> surround(
    start: Parser<String>,
    stop: Parser<String>,
    p: Parser<A>
): Parser<A>
}

abstract class ParsersDsl<PE> : Parsers<PE>() {

    fun <A> Parser<A>.defer(): () -> Parser<A> = defer(this)

    fun <A, B> Parser<A>.map(f: (A) -> B): Parser<B> =
        this@ParsersDsl.map(this, f)

    fun <A> Parser<A>.many(): Parser<List<A>> =
        this@ParsersDsl.many(this)

    infix fun <A> Parser<out A>.or(p: Parser<out A>): Parser<A> =
        this@ParsersDsl.or(this, p.defer())

    infix fun <A, B> Parser<A>.product(p: Parser<B>): Parser<Pair<A, B>> =
        this@ParsersDsl.product(this, p.defer())

    infix fun <A> Parser<A>.sep(p: Parser<String>): Parser<List<A>> =
        this@ParsersDsl.sep(this, p)
}
```