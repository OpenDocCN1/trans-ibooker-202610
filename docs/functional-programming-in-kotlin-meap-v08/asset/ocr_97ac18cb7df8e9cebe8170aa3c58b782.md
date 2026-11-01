# 1.2 Exactly what is a (pure) function?

We said earlier that FP means programming with pure functions, and a pure function is one that has no side effects. In our discussion of the coffee shop example, we worked off an informal notion of side effects and purity. Here we’ll formalize this notion, to pinpoint more precisely what it means to program functionally. This will also give us additional insight into one of the benefits of functional programming: pure functions are easier to reason about.

A function f with input type A and output type B (written in Kotlin as a single type: `(A) -> B`, pronounced "A to B") is a computation that relates every value a of type A to exactly one value b of type B such that b is determined solely by the value of a. Any changing state of an internal or external process is irrelevant to computing the result `f(a)`. For example, a function `intToString` having type `(Int) -> String` will take every integer to a corresponding string. Furthermore, if it really is a function, it will do nothing else.

![](bbox=[0.12, 0.38, 0.82, 0.56])

Figure 1.2 A pure function, does only what it states without side effect.

In other words, a function has no observable effect on the execution of the program other than to compute a result given its inputs; we say that it has no side effects. We sometimes qualify such functions as *pure functions* to make this more explicit, but this is somewhat redundant. Unless we state otherwise, we’ll often use *function* to imply no side effects².

You should be familiar with a lot of pure functions already. Consider the addition (+) operator, which resolves to the `plus` function on all integers. It takes an integer value and returns another integer value. For any two given integers, it will always return the same integer value. Another example is the length function of a String in Java, Kotlin, and many other languages where strings can’t be modified (are immutable). For any given string, the same length is always returned and nothing else occurs.

We can formalize this idea of pure functions using the concept of referential transparency (RT). This is a property of expressions in general and not just functions. For the purposes of our discussion, consider an expression to be any part of a program that can be evaluated to a result—anything that you could type into the Kotlin interpreter and get an answer. For example,