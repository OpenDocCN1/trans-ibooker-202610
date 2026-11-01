# 11.6 Summary

+   - The type constructor F representing types like `List` or `Option` is a functor, and the `Functor<F>` instance proves that this assumption holds true.
+   - The functor interface has a `map` method, which is a higher order function that applies a transformation to each element of the enclosing kind.
+   - Laws have importance because they establish the semantics of an interface. This results in an algebra that may be reasoned about *independently* from its instances.
+   - The functor law stipulates the relationship between `map` and identity functions. It preserves the structure of the enclosing kind and is only concerned with transforming its elements.
+   - The monad interface *is a functor* that typically has `flatMap` and `unit` primitives. These primitive functions can be used to derive many other useful combinators, including those of the functor.
+   - The monadic laws constrain the behavior of a monad by enforcing principles of *associativity* and *identity* on its instances.
+   - The *associative law* deals with ordering, and it guarantees that outcomes will remain the same no matter how `flatMap` operations are nested.
+   - The identity laws are comprised of *left identity* and *right identity*, each dealing with a situation where the result of `unit` is the subject or object of a `flatMap` expression.
+   - There are three minimal sets of combinators that can define a monad, namely `unit` combined with either `flatMap`, `compose` or `map` and `join`.
+   - Each monad has a set of basic primitives along with its own set of additional combinators, the interaction of all these combined making the behavior of each monad unique.
+   - The monad contract doesn’t specify what is happening *between the lines* of a for-comprehension, only that whatever is happening satisfies the monadic laws.