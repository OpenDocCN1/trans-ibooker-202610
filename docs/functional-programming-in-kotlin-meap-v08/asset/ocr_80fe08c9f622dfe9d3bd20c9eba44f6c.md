+   ① The `Monad` type constructor takes partially applied type parameter `StatePartialOf` for any `S`
+   ② Monadic combinators no longer restricted to deal in single type parameter currency

The main difference comes in the declaration of the `StateMonad` interface itself. The monad interface has a type parameter `S` for the family member that it represents, and it will extend from a new type alias `StatePartialOf<S>`, that is an alias for `Kind<ForState, S>`.

The types such as `StatePartialOf` and `StateOf` are merely boilerplate code that we can write ourselves, although Arrow already conveniently generates all this for us. Appendix D of the book describes exactly what boilerplate is required, as well as how to let Arrow do all the hard work on our behalf.

**NOTE** Please revise appendix D section 2 that describes the boilerplate code required for declaring a partially applied type constructor. **Please ensure that you have read and thoroughly understood this content.**

We can now declare a new member of the state monad family using the `StateMonad` interface. Let’s stick with our `intStateMonad` example from before using this interface and boilerplate code:

# Listing 11.18 A partially applied `State` monad that brings flexibility to the type family members it represents

```
val intStateMonad: StateMonad<Int> = object : StateMonad<Int> {
    override fun <A> unit(a: A): StateOf<Int, A> =
        State { s -> Pair(a, s) }

    override fun <A, B> flatMap(
        fa: StateOf<Int, A>,
        f: (A) -> StateOf<Int, B>
    ): StateOf<Int, B> =
        fa.fix().flatMap { a -> f(a).fix() }
}
```

We can see that we have evolved from the hard-coded `ForIntState` monad in listing 11.16 to a more flexible partially applied variant in listing 11.17. Once more, just by giving implementations of `flatMap` and `unit`, we get implementations of all the other monadic combinators for free.