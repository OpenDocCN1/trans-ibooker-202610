code into a while loop by hand, but ideally we’d like to have this done automatically while retaining the same high-level compositional style. We want to compose our programs using higher-order functions like map and filter instead of writing monolithic loops.

It turns out that we can accomplish this kind of automatic loop fusion through the use of non-strictness (or, less formally, laziness). In this chapter, we’ll explain exactly what this means, and we’ll work through the implementation of a lazy list type that fuses sequences of transformations. Although building a “better” list is the motivation for this chapter, we’ll see that non-strictness is a fundamental technique for improving on the efficiency and modularity of functional programs in general.

# 5.1 Strict and non-strict functions

Before we get to our example of lazy lists, we need to cover some basics. What are strictness and non-strictness, and how can we express these concepts in Kotlin?

Non-strictness is a property of a function. To say a function is non-strict just means that the function may choose not to evaluate one or more of its arguments. In contrast, a strict function always evaluates its arguments. Strict functions are the norm in most programming languages, and indeed most languages only support functions that expect their arguments fully evaluated. Unless we tell it otherwise, any function definition in Kotlin will be strict (and all the functions we’ve defined so far have been strict). As an example, consider the following function:

```
fun square(x: Double): Double = x * x
```

When you invoke square(41.0 + 1.0), the function square will receive the evaluated value of 42.0 because it’s strict. If you invoke square(exitProcess(-1)), the program will be terminated before square has a chance to do anything, since the exitProcess(-1) expression will be evaluated before entering the body of square.

Although we haven’t yet learned the syntax for indicating non-strictness in Kotlin, you’re almost certainly familiar with the concept. For example, the short-circuiting Boolean functions && and ||, found in many programming languages including Kotlin, are non-strict. You may be used to thinking of && and || as built-in syntax, part of the language, but you can also think of them as functions that may or may not choose not to evaluate their arguments. The function && takes two Boolean arguments, but only evaluates the second argument if the first is true:

```
>>> false && { println("!!"); true }.invoke() //does not print anything
res0: kotlin.Boolean = false
```

And || only evaluates its second argument if the first is false:

```
>>> true || { println("!!"); false }.invoke() //does not print anything either
res1: kotlin.Boolean = true
```