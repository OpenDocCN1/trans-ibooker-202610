```
override fun <A, B> flatMap(
    fa: Kind<ForSequenceK, A>,
    f: (A) -> Kind<ForSequenceK, B>
): Kind<ForSequenceK, B> {
    return fa.fix().flatMap(f)
}
}
```

# EXERCISE 11.2 (HARD)

State looks like it could be a monad too, but it takes two type arguments, namely S and A. You need a type constructor of only one argument to implement Monad. Try to implement a State monad, see what issues you run into, and think about if or how you can solve this. We’ll discuss the solution later in this chapter.

```
data class State<S, out A>(val run: (S) -> Pair<A, S>) : StateOf<S, A>

sealed class ForState private constructor() {
    companion object
}

typealias StateOf<S, A> = Kind2<ForState, S, A>

fun <S, A> StateOf<S, A>.fix() = this as State<S, A>

typealias StatePartialOf<S> = Kind<ForState, S>

interface StateMonad<S> : Monad<StatePartialOf<S>> {

    override fun <A> unit(a: A): StateOf<S, A>

    override fun <A, B> flatMap(
        fa: StateOf<S, A>,
        f: (A) -> StateOf<S, B>
    ): StateOf<S, B>
}
```

# EXERCISE 11.3

The sequence and traverse combinators should be pretty familiar to you by now, and your implementations of them from previous chapters are probably all very similar. Implement them once and for all on Monad<F>.

```
fun <A> sequence(lfa: List<Kind<F, A>>): Kind<F, List<A>> =
    lfa.foldRight(
        unit(List.empty<A>()),
        { fa: Kind<F, A>, fla: Kind<F, List<A>> ->
            map2(fa, fla) { a: A, la: List<A> -> Cons(a, la) }
        }
    )

fun <A, B> traverse(
    la: List<A>,
    f: (A) -> Kind<F, B>
): Kind<F, List<B>> =
    la.foldRight(
        unit(List.empty<B>()),
        { a: A, acc: Kind<F, List<B>> ->
            map2(f(a), acc) { b: B, lb: List<B> -> Cons(b, lb) }
        }
    )
```