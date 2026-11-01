# SIDEBAR

# Monoid homomorphisms

If you've donned your law-discovering hat while reading this chapter, you may have noticed that a law exists holding for some functions between monoids.

For instance, take the String concatenation monoid and the integer addition monoid. If you take the lengths of two strings and add them up, it's the same as taking the length of the concatenation of those two strings:

```
"foo".length + "bar".length == ("foo" + "bar").length
```

Here, length is a function from String to Int that preserves the monoid structure. Such a function is called a monoid homomorphism.50 A monoid homomorphism f between monoids M and N obeys the following general law for all values x and y:

```
M.combine(f(x), f(y)) == f(N.combine(x, y))
```

The same law should hold for the homomorphism from String to WC in the preceding exercises.

This property can be useful when designing your own libraries. If two types that your library uses are monoids, and some functions exist between them, it's a good idea to think about whether those functions are expected to preserve the monoid structure and to check the monoid homomorphism law with property-based tests.

Sometimes there will be a homomorphism in both directions between two monoids. If they satisfy a monoid isomorphism (iso- meaning equal), we say that the two monoids are isomorphic. A monoid isomorphism between M and N has two homomorphisms f and g, where both f andThen g and g andThen f are an identity function.

For example, the String and Array<Char> monoids are isomorphic in terms of concatenation . The two Boolean monoids (false, ||) and (true, &&) are also isomorphic, via the ! (negation) operation.

# 10.5 Foldable data structures

In chapter 3 we implemented the data structures List and Tree, both of which could be folded. Then in chapter 5 we wrote Stream, a lazy structure that could also be folded much like List. As if that wasn’t enough, we’ve now added fold functionality that operates on Kotlin’s Sequence too.

When we’re writing code that needs to process data contained in one of these structures, we often don’t care about the shape of the structure. It doesn’t matter if it’s a tree, list, lazy, eager, efficiently random access, and so forth.