15. The naming is not coincidental. There’s a deep connection, beyond the scope of this book, between the “addition” and “multiplication” of types to form an ADT and addition and multiplication of numbers.

16. Arity is the number of arguments or operands that a function or operation in logic, mathematics, and computer science takes.

17. A function may also be partial if it doesn’t terminate for some inputs. We won’t discuss this form of partiality here, since it’s not a recoverable error and there’s no question of how best to handle it.

18. In general, we’ll use this object-oriented style of syntax where possible for functions that have a single, clear operand (like `List.map`), and the standalone function style otherwise.

19. Perhaps it should have been called `mapFlat`, but that doesn’t sound quite as appealing!

20. This is a clear instance where it’s not appropriate to define the function in the OO style. This shouldn’t be a method on `List` (which shouldn’t need to know anything about `Option`), and it can’t be a method on `Option`, so it goes in the `Option` companion object.

21. `Either` is also often used more generally to encode one of two possibilities in cases where it isn’t worth defining a fresh data type. We’ll see some examples of this throughout the book.

22. With program traces like these, it’s often more illustrative to not fully trace the evaluation of every subexpression. In this case, we’ve omitted the full expansion of `List.of(1,2,3,4).map { it + 10 }`. We could “enter” the definition of map and trace its execution step by step, but we chose to omit this level of detail for the sake of simplicity.

23. In fact, the type `() -> A` is a syntactic alias for the type `Function<A>`.

24. kotlinlang.org/docs/reference/delegated-properties.html

25. Recall that Kotlin uses subtyping to represent data constructors, but we almost always want to infer `Stream` as the type, not `Cons` or `Empty`. Making smart constructors that return the base type is a common trick.

26. This definition of `exists`, though illustrative, isn’t stack-safe if the stream is large and all elements test `false`.

27. It’s possible to define a stack-safe version of `forAll` using an ordinary recursive loop.

28. In Kotlin, the `Int` type is a 32-bit signed integer, so this stream will switch from positive to negative values at some point, and will repeat itself after about four billion elements.