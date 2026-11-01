2 + 3 is an expression that applies the pure function `plus` on 2 to 3 (which is also an expression). This has no side effect. The evaluation of this expression results in the same value 5 every time. In fact, if we saw 2 + 3 in a program we could simply replace it with the value 5 and it wouldn’t change a thing about the meaning of our program.

This is all it means for an expression to be referentially transparent—in any program, the expression can be replaced by its result without changing the meaning of the program. And we say that a function is pure if calling it with RT arguments is also RT. We’ll look at some examples next.

> **SIDEBAR  Referential transparency and purity**
> 
> An expression `e` is referentially transparent if, for all programs `p`, all occurrences of `e` in `p` can be replaced by the result of evaluating `e` without affecting the meaning of `p`. A function `f` is pure if the expression `f(x)` is referentially transparent for all referentially transparent `x`.

# 1.3 Referential transparency, purity, and the substitution model

Let’s see how the definition of RT applies to our original `buyCoffee` example:

```
fun buyCoffee(cc: CreditCard): Coffee {
  val cup = Coffee()
  cc.charge(cup.price)
  return cup
}
```

Whatever the return type of `cc.charge(cup.price)`, even if it’s `Unit`, is discarded by `buyCoffee`. Thus, the result of evaluating `buyCoffee(aliceCreditCard)` will be merely `cup`, which is equivalent to a new `Coffee()`. For `buyCoffee` to be pure, by our definition of RT, it must be the case that `p(buyCoffee(aliceCreditCard))` behaves the same as `p(Coffee())`, for any `p`. This clearly doesn’t hold—the program `Coffee()` doesn’t do anything, whereas `buyCoffee(aliceCreditCard)` will contact the credit card provider and authorize a charge. Already we have an observable difference between the two programs.

Referential transparency enforces the rule that everything a function does should be represented by the value that it returns, according to the result type of the function. This constraint enables a simple and natural mode of reasoning about program evaluation called the substitution model. When expressions are referentially transparent, we can imagine that computation proceeds much like we’d solve an algebraic equation. We fully expand every part of an expression, replacing all variables with their referents, and then reduce it to its simplest form. At each step we replace a term with an equivalent one; computation proceeds by substituting equals for equals. In other words, RT enables equational reasoning about programs.

Let’s look at two more examples—one where all expressions are RT and can be reasoned about