# EXERCISE 11.15 (Hard/Optional)

Explain in your own words what the identity laws are stating in concrete terms for Gen and List.

Recall the identity laws for left and right identity respectively:

```
flatMap(x) { a -> unit(a) } == x
flatMap(unit(a), f) == f(a)
```

# 11.5 Just what is a monad?

Up to now, we’ve been examining monads at the micro-level by identifying various combinators and proving associated laws. Even though this is useful, it doesn’t really tell us much about what a monad is. In order to further our understanding, we will zoom out to a broader perspective on this subject. In doing so, we see something unusual about the Monad interface. The data types for which we’ve given monad instances don’t seem to have much to do with each other. Yes, Monad factors out code duplication among them, but what is a monad exactly? What does "monad" mean?

You may be used to thinking of interfaces as providing a relatively complete API for an abstract data type, merely abstracting over the specific representation. After all, a singly linked list and an array-based list may be implemented differently behind the scenes, but they’ll probably share a common List interface in terms of which a lot of useful and concrete application code can be written. Monad, like Monoid, is a more abstract and purely algebraic interface. The Monad combinators are often just a small fragment of the full API for a given data type that happens to be a monad. So Monad doesn’t generalize one type or another; rather, many vastly different data types can satisfy the Monad interface and laws.

We’ve seen three minimal sets of primitive monadic combinators, and instances of Monad will have to provide implementations of one of these sets:

+   - flatMap and unit
+   - compose and unit
+   - map, join and unit

We also know that there are two monad laws to be satisfied, namely associativity and identity, that can be formulated in various ways. So we can state plainly what a monad is:

> A monad is an implementation of one of the minimal sets of monadic combinators, satisfying the laws of associativity and identity.

That’s a perfectly respectable, precise and terse definition. And if we’re being precise, this is the