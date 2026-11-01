actually have.

We just came up with two general and potentially useful combinators based purely on the abstract interface of Functor, and we can reuse them for any type that allows an implementation of map.

# 11.1.1 The importance of laws and their relation to functor

Whenever we create an abstraction like Functor, we should not only consider what abstract methods it should have, but also the laws we expect it to hold for the implementations. The laws you stipulate for an abstraction are entirely up to you, although Kotlin won’t enforce any of these laws on your behalf. If you are going to borrow the name of some existing mathematical abstraction like functor or monoid, we recommend using the laws already specified by mathematics. Laws are important for two reasons:

+   - Laws help an interface form a new semantic level whose algebra may be reasoned about independently of the instances. For example, when we take the product of a Monoid<A> and a Monoid<B> to form a Monoid<Pair<A, B>>, the monoid laws let us conclude that the "fused" monoid operation is also associative. We don’t need to know anything about A and B to conclude this.
+   - On a concrete level, we often rely on laws when writing various combinators derived from the functions of some abstract interface like Functor. We’ll see examples of this later in this section.

For Functor, we’ll stipulate the familiar law we first introduced in chapter 7 for our Par data type. This laws stipulated that relation between the map combinator and an identity function as follows:

# Listing 11.2 The functor law showing the relation between map and identity function

```
map(x) { a -> a } == x
```

In other words, mapping over a structure x with the identity function should itself be an identity. This law seems quite natural, and as we progressed beyond Par, we noticed that this law was satisfied by the map functions of other types like Gen and Parser too. This law captures the requirement that map(x) "preserves the structure" of x. Implementations satisfying this law are restricted from doing strange things like throwing exceptions, removing the first element of a List, converting a Some to None, and so on. Only the elements of the structure are modified by map; the shape or structure itself is left intact. Note that this law holds for List, Option, Par, Gen, and most other data types that define map!

To give a concrete example of this preservation of structure, we can consider distribute and codistribute, defined earlier. Here are their signatures again for reference: