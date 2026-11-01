EXERCISE 9.6
Using `flatMap` and any other combinators, write the context-sensitive parser we couldn't express earlier. The result should be a `Parser<Int>` that returns the number of characters read. You can make use of a new primitive called `regex` to parse digits, which promotes a regular expression `String` to a `Parser<String>`.

EXERCISE 9.7
Implement `product` and `map2` in terms of `flatMap` and `map`.

EXERCISE 9.8
`map` is no longer primitive. Express it in terms of `flatMap` and/or other combinators.

We have now introduced a new primitive called `flatMap` which enables context-sensitive parsing and allows us to implement `map` and `map2`. This is not the first time `flatMap` has come to the rescue.

Our list of primitives has now shrunk down to six: `string`, `regex`, `slice`, `succeed`, `or`, and `flatMap`. Even though we have less primitives, we have more capability than before by adopting the more general `flatMap` in favor of `map` and `product`. This new power tool enables us to parse arbitrary context-free grammars like JSON, as well as context-sensitive grammars including extremely complex ones like C++ and PERL!

NOTE
Up to now, we have spent very little time implementing any of these primitives, and have rather worked on fleshing out the *algebra* by defining abstract definitions in our `Parsers` interface. Let's persist with this approach and defer implementation of these primitives as much as possible.

# 9.4 Writing a JSON parser

Up to this point we have been building up a set of primitives that give us the basic building blocks to construct more complex parsers. We have managed to parse characters, strings, recognize repetitions and pass context. In this section we will build upon the list of primitives that have been derived so far by developing something of real use, a JSON parser. This is the fun part, so let's jump right into it!