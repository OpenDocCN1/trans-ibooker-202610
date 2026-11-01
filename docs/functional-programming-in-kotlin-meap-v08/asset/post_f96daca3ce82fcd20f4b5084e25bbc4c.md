
```
fun <A, B> product(
    pa: Parser<A>,
    pb: () -> Parser<B>
): Parser<Pair<A, B>> = TODO()

fun <A, B, C> map2(
    pa: Parser<A>,
    pb: () -> Parser<B>,
    f: (A, B) -> C
): Parser<C> =
    product(pa, pb).map { (a, b) -> f(a, b) }
```

## EXERCISE 9.5

We could also deal with non-strictness using a separate combinator like we did in chapter 7. Provide a new combinator called defer and make the necessary changes to your existing combinators. What do you think of that approach in this instance?

## NOTE

The purpose of this exercise is merely to try the approach of introducing a defer function, and the impact that it would have on our existing combinators. We won’t be introducing it beyond this exercise because of the complexity it adds, along with limited benefit that it gives our library. That said, it was worth having a play and trying it out!

By updating our implementation of many to take advantage of the second lazy parameter of map2 by using defer, our problem now goes away:

### Listing 9.3 Implementation of many that relies on lazy evaluation

```
fun <A> many(pa: Parser<A>): Parser<List<A>> =
    map2(pa, many(pa).defer()) { a, la ->
        a cons la
    } or succeed(emptyList())
```

① Second parameter to map2 becomes a chunk.

Because map2 draws on the functionality of product, it should be non-strict in its second argument too since if the first Parser fails, the second won’t even be consulted.

We now have good combinators for parsing one thing followed by another, or multiple things of the same kind in succession. But since we’re considering whether combinators should be non-strict, let’s revisit the or combinator from earlier:

```
fun <A> or(pa: Parser<A>, pb: Parser<A>): Parser<A>
```

We’ll assume that or is left-biased, meaning it tries p1 on the input and then tries p2 only if p1 fails. This is purely a design choice, and you may prefer to have a version of or that always runs both p1 and p2. In our case we opt for the non-strict version with the second argument which may never even be consulted. This is what such an or combinator would look like:

```
fun <A> or(pa: Parser<A>, pb: () -> Parser<A>): Parser<A>
```

## 9.3 Handling context sensitivity

In this section we will explore a combinator that allows us to pass context on to the next combinator. We call this ability for a combinator to pass state context sensitivity.

Let’s pause now for a few seconds and reflect on what we’ve covered so far in this chapter. We’ve already come a long way in defining a set of useful primitives that we can use in subsequent sections. Here’s a review of the most useful ones that we’ve defined.

### Table 9.1 A list of useful primitives derived so far

| Primitive | Description |
|---|---|
| `string(s)` | Recognizes and returns a single `String` |
| `slice(p)` | Returns the portion of input inspected by `p` if successful |
| `succeed(a)` | Always succeeds with the value `a` |
| `map(p, f)` | Applies the function `f` to the result of `p` if successful |
| `product(p1, p2)` | Sequences two parsers, running `p1` and then `p2`, then returns the pair of their results if both succeed |
| `or(p1, p2)` | Chooses between two parsers, first attempting `p1`, then passing `p2` any uncommitted input in case `p1` failed |

Using these primitives we can express various forms of repetition (many, listOfN, and many1) as well as combinators like char and map2. Would it surprise you if these primitives were sufficient for parsing any context-free grammar, including JSON? Well, they are! We’ll get to writing that JSON parser soon, but we need a few more building blocks to get there.

Suppose we want to parse a single digit like '4', followed by as many 'a' characters as that digit. Examples of this kind of input are "0", "1a", "2aa", "4aaaa" and so on. This is an example of a context-sensitive grammar, and can’t be expressed with the product primitive that we’ve defined already. The reason being that the choice of the second parser depends on the result of the first. In other words, the second parser depends on the context of the first. Back to our example, we want to run the first parser to extract the digit, then do a listOfN using the number from the first parser’s result. The product combinator simply can’t express something like this.

This progression might seem familiar to you. In past chapters we encountered similar situations and dealt with this by introducing a new primitive called flatMap.

```
fun <A, B> flatMap(pa: Parser<A>, f: (A) -> Parser<B>): Parser<B>
```

Can you see how this combinator solves the problem of context sensitivity as mentioned above? It provides an ability to sequence parsers, where each parser in the chain depends on the output of the previous one.

## EXERCISE 9.6
Using `flatMap` and any other combinators, write the context-sensitive parser we couldn't express earlier. The result should be a `Parser<Int>` that returns the number of characters read. You can make use of a new primitive called `regex` to parse digits, which promotes a regular expression `String` to a `Parser<String>`.

## EXERCISE 9.7
Implement `product` and `map2` in terms of `flatMap` and `map`.

## EXERCISE 9.8
`map` is no longer primitive. Express it in terms of `flatMap` and/or other combinators.

We have now introduced a new primitive called `flatMap` which enables context-sensitive parsing and allows us to implement `map` and `map2`. This is not the first time `flatMap` has come to the rescue.

Our list of primitives has now shrunk down to six: `string`, `regex`, `slice`, `succeed`, `or`, and `flatMap`. Even though we have less primitives, we have more capability than before by adopting the more general `flatMap` in favor of `map` and `product`. This new power tool enables us to parse arbitrary context-free grammars like JSON, as well as context-sensitive grammars including extremely complex ones like C++ and PERL!

## NOTE
Up to now, we have spent very little time implementing any of these primitives, and have rather worked on fleshing out the *algebra* by defining abstract definitions in our `Parsers` interface. Let's persist with this approach and defer implementation of these primitives as much as possible.

## 9.4 Writing a JSON parser

Up to this point we have been building up a set of primitives that give us the basic building blocks to construct more complex parsers. We have managed to parse characters, strings, recognize repetitions and pass context. In this section we will build upon the list of primitives that have been derived so far by developing something of real use, a JSON parser. This is the fun part, so let's jump right into it!

### 9.4.1 Defining expectations of a JSON parser

We don’t have an implementation of our algebra yet, nor do we have combinators for good error reporting. Our JSON parser doesn’t need to know the internal details of how parsers are represented, so we can deal with this later. We can simply write a function that produces a JSON parser using only the set of primitives we’ve defined, as well as any derived combinators we may need along the way.

We will review the JSON format in a minute, but let’s first examine the parse result type that we will expect as an outcome of building our parser. The final outcome will be a structure that looks something like this:

### Listing 9.4 Top level constructs to be developed for JSON parsing

```
object JSONParser : ParsersImpl<ParseError>() {
    val jsonParser: Parser<JSON> = TODO()
}
```

- ① Give access to algebra implementations
- ② Top level declaration for Parser<JSON>, with JSON to be defined shortly.

Defining this top level function at such an early stage might seem like a peculiar thing to do, since we won’t actually be able to run our parser until we have a concrete implementation of the Parsers interface. But we’ll proceed anyway since it’s common FP practice to define an algebra and explore its expressiveness prior to defining an implementation. A concrete implementation can tie us down and makes changes to the API more difficult. This is especially true during the design phase of a library. It is much easier to refine an algebra without having to commit to any particular implementation. This algebra-first design approach is radically different to what has been taken so far in this book, but is probably the most important lesson to be learned in this chapter.

In section 9.5 we’ll return to the question of adding better error reporting to our parsing API. We can do this without disturbing the overall structure of the API or changing the JSON parser very much. We’ll also come up with a concrete, runnable representation of our Parser type. Importantly, the JSON parser we’ll implement in this next section will be completely independent of that representation.

### 9.4.2 Reviewing the JSON format

If you aren’t already familiar with the JSON format, this section will briefly introduce the main concepts of this data representation. You may also want to read Wikipedia’s description 46 and the official grammar specification 47 if you want to know more. Here’s an example of a simple JSON document:
