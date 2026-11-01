# Listing 10.4 Determine the mean of a list by calculating the length and sum simultaneously

```
>>> val m = productMonoid<Int, Int>(intAdditionMonoid, intAdditionMonoid)
>>> val p = ListFoldable.foldMap(List.of(1, 2, 3, 4), m, { a -> Pair(1, a) })

res0: kotlin.Pair<kotlin.Int, kotlin.Int> = (4, 10)

>>> val mean = p.first / p.second.toDouble()
>>> mean

res1: kotlin.Double = 0.4
```

It can be tedious to assemble monoids by hand using productMonoid and foldMap. Part of the problem is that we’re building up the Monoid separately from the mapping function of foldMap, and we must manually keep these "aligned" as we did here. A better way would be to create a combinator library that makes it more convenient to assemble these composed monoids and define complex computations that may be parallelized and run in a single pass. Such a library is beyond the scope of this chapter, but worth exploring as a fun project if this fascinates you.

Our goal in part 3 is to get you accustomed to working with more abstract structures, and to develop the ability to recognize them. In this chapter, we introduced one of the simplest purely algebraic abstractions, the monoid. When you start looking for it, you’ll find ample opportunity to exploit the monoidal structure in your own libraries. The associative property enables folding any Foldable data type and gives the flexibility of doing so in parallel. Monoids are also compositional, and you can use them to assemble folds in a declarative and reusable way.

Monoid has been our first purely abstract algebra, defined only in terms of its abstract operations and the laws that govern them. We saw how we can still write useful functions that know nothing about their arguments except that their type forms a monoid. This more abstract mode of thinking is something we’ll develop further in the rest of part 3. We’ll consider other purely algebraic interfaces and show how they encapsulate common patterns that we’ve repeated throughout this book.