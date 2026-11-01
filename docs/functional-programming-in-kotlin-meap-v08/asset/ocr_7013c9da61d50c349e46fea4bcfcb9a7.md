The partial1 function has three type parameters: A, B, and C. It then takes two arguments. The argument f is itself a function that takes two arguments of types A and B respectively, and returns a value of type C. The value returned by partial1 will also be a function, of type (B) -> C. How would we go about implementing this higher-order function? It turns out that there’s only one implementation that compiles, and it follows logically from the type signature. It’s like a fun little logic puzzle.7 Let’s start by looking at the type of thing that we have to return. The return type of partial1 is (B) -> C, so we know that we have to return a function of that type. We can just begin writing a function literal that takes an argument of type B:

```
fun <A, B, C> partial1(a: A, f: (A, B) -> C): (B) -> C =
  { b: B -> TODO() }
```

This can be weird at first if you’re not used to writing anonymous functions. Where did that B come from? Well, we’ve just written, “Return a function that takes a value b of type B.” On the right-hand-side of the -> arrow (where the TODO() is now) comes the body of that anonymous function. We’re free to refer to the value b in there for the same reason that we’re allowed to refer to the value a in the body of partial1.8 Let’s keep going. Now that we’ve asked for a value of type B, what do we want to return from our anonymous function? The type signature says that it has to be a value of type C. And there’s only one way to get such a value. According to the signature, C is the return type of the function f. So the only way to get that C is to pass an A and a B to f. That’s easy:

```
fun <A, B, C> partial1(a: A, f: (A, B) -> C): (B) -> C =
  { b: B -> f(a, b) }
```

And we’re done! The result is a higher-order function that takes a function of two arguments and partially applies it. That is, if we have an A and a function that needs both A and B to produce C, we can get a function that just needs B to produce C (since we already have the A). It’s like saying, “If I can give you a carrot for an apple and a banana, and you already gave me an apple, you just have to give me a banana and I’ll give you a carrot.” Note that the type annotation on b isn’t needed here. Since we told Kotlin the return type would be (B) -> C, Kotlin knows the type of b from the context and we could just write { b -> f(a,b) } as the implementation. Generally speaking, we’ll omit the type annotation on a function literal if it can be inferred by Kotlin. The final result being:

```
fun <A, B, C> partial1(a: A, f: (A, B) -> C): (B) -> C =
  { b -> f(a, b) }
```