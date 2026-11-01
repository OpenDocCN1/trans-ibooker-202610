We typically avoid this problem by defining smart constructors, which is what we call a function for constructing a data type that ensures some additional invariant or provides a slightly different signature than the “real” constructors. By convention, smart constructors live in the companion object of the base class and their names typically lowercase the first letter of the corresponding data constructor. Here, our cons smart constructor takes care of memoizing the by-name arguments for the head and tail of the Cons. This is a common trick, and it ensures that our thunk will only do its work once, when forced for the first time. Subsequent forces will return the cached lazy val:

```
companion object {
    //smart constructors
    fun <A> cons(hd: () -> A, tl: () -> Stream<A>): Stream<A> {
        val head: A by lazy(hd)
        val tail: Stream<A> by lazy(tl)
        return Cons({ head }, { tail })
    }

    fun <A> empty(): Stream<A> = Empty
}
```

The empty smart constructor just returns Empty, but annotates Empty as a Stream<A>, which is better for type inference in some cases.25 We can see how both smart constructors are used in the Stream.of function:

```
companion object {
    //smart constructors

    fun <A> of(vararg xs: A): Stream<A> =
        if (xs.isEmpty()) empty()
        else cons({ xs[0] },
            { of(*xs.sliceArray(1 until xs.size)) })
}
```

Since Kotlin does not take care of wrapping the arguments to cons in thunks, we need to do this explicitly by surrounding xs[0] and of(*xs.sliceArray(1 until xs.size)) in lambdas so that the expressions won’t be evaluated until we force the Stream.

# 5.2.2 Helper functions for inspecting streams

Before continuing, let’s write a few helper functions to make inspecting streams easier.