```
fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C> = TODO()
```

TIP
Follow the types to the only possible implementation.

# EXERCISE 11.8 (HARD)

Implement flatMap in terms of an abstract definition of compose. By this, it seems as though we’ve found another minimal set of monad combinators: compose and unit.

TIP
Consider what effect it would have if we assumed A to be Unit.

# EXERCISE 11.9

Using the following values, prove that the identity laws expressed in terms of compose are equivalent to that stated in terms of flatMap:

```
val f: (A) -> Kind<F, A>
val x: Kind<F, A>
val v: A
```

TIP
Substitute each occurrence of compose by flatMap, then apply value v of type A to both sides of each equation.

# EXERCISE 11.10

Prove that the identity laws hold for the Option monad.

TIP
We should again consider both None and Some cases, and expand the left and right side of the equation for each. The monadic unit can be expressed as { a: A Some(a) }, or the briefer { Some(it) } if you prefer it.

# EXERCISE 11.13 (HARD/OPTIONAL)

Restate the monad law of associativity in terms of flatMap using join, map and unit.