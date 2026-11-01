
## 2.1 Higher-order functions: passing functions to functions

Let’s get right into it by covering some of the basics of writing functional programs. The first new idea is this: functions are values. And just like values of other types—such as integers, strings, and lists—functions can be assigned to variables, stored in data structures, and passed as arguments to functions.

When writing purely functional programs, we’ll often find it useful to write a function that accepts other functions as arguments. This is called a higher-order function (HOF), and we’ll look next at some simple examples to illustrate this. In later chapters, we’ll see how useful this capability really is, and how it permeates the functional programming style. But to start, suppose we wanted to adapt our program to print out both the absolute value of a number and the factorial of another number. Here’s a sample run of such a program:

```
The absolute value of -42 is 42
The factorial of 7 is 5040
```

### 2.1.1 A short detour: writing loops functionally

In order to adapt our existing program to demonstrate HOFs, we should introduce some new behaviour. We will do so by adding a new function that calculates the nth factorial. To write this simple function, we will need to take a short detour by showing how loops are written in a purely functional way. We do this by introducing recursion.

First, let’s write factorial:

### Listing 2.1 A factorial function

```
fun factorial(i: Int): Int {
    fun go(n: Int, acc: Int): Int =
        if (n <= 0) acc
        else go(n - 1, n * acc)
    return go(i, 1)
}
```

- 1. An inner or local function definition.
- 2. Calling the local function.

NOTE It is common to write functions that are local to the body of another function. In functional programming we shouldn’t consider this any stranger than a local integer or string.

The way we write loops functionally, without mutating a loop variable, is with a recursive function. Here we’re defining a recursive helper function inside the body of the factorial function. This function will typically handle recursive calls that require an accumulator parameter, or some other signature change that the enclosing function does not have. Such a helper function is often called `go` or `loop` by convention. In Kotlin, we can define functions inside any block, including within another function definition. Since it’s local, the `go` function can only be referred to from within the scope of the body of the `factorial` function, just like a local variable would. The definition of `factorial` finally just consists of a call to `go` with the initial conditions for the loop. The arguments to `go` are the state for the loop. In this case, they’re the remaining value `n`, and the current accumulated factorial `acc`. To advance to the next iteration, we simply call `go` recursively with the new loop state (here, `go(n-1, n*acc)`), and to exit from the loop, we return a value without a recursive call (here, we return `acc` in the case that `n <= 0`).

Kotlin *does not* manually detect this sort of self-recursion, but requires the function to declare the `tailrec` modifier. This in turn will instruct the compiler to emit the same kind of bytecode as would be found for a while loop⁴, provided that the recursive call is in tail position. See the *Tail calls in Kotlin* sidebar for the technical details on this, but the basic idea is that this optimization⁵ (or tail call elimination) can be applied when there’s no additional work left to do after the recursive call returns.

## SIDEBAR

### Tail calls in Kotlin

A call is said to be in tail position if the caller does nothing other than return the value of the recursive call. For example, the recursive call to `go(n-1,n*acc)` we discussed earlier is in tail position, since the method returns the value of this recursive call directly and does nothing else with it. On the other hand, if we said `1 + go(n-1,n*acc)`, `go` would no longer be in tail position, since the method would still have work to do when `go` returned its result (namely, adding 1 to it).

If all recursive calls made by a function are in tail position, and the function declares the `tailrec` modifier, Kotlin compiles the recursion to iterative loops that don’t consume call stack frames for each iteration.

```
fun factorial(i: Int): Int {
    tailrec fun go(n: Int, acc: Int): Int = ①
        if (n <= 0) acc
        else go(n - 1, n * acc) ②
    return go(i, 1)
}
```

- ① The `tailrec` modifier instructs the compiler to eliminate tail calls.
- ② The function’s final declaration is in tail position

If a recursive function has a call in tail position, but does not declare itself as `tailrec`, the compiler won’t eliminate tail calls, which in turn could result in a StackOverflowError being thrown.

In the case where we apply the `tailrec` modifier to a function without its final declaration being in tail position, the compiler will issue a warning:

```
Warning: (19, 9) Kotlin: A function is marked as tail-recursive but no tail calls are found
```

Even though a warning is better than nothing, a compilation error would have been far more helpful and much safer in this instance.

## EXERCISE

### 2.1

Write a recursive function to get the nth Fibonacci number (en.wikipedia.org/wiki/Fibonacci_number). The first two Fibonacci numbers are 0 and 1. The nth number is always the sum of the previous two—the sequence begins 0, 1, 1, 2, 3, 5, 8, 13, 21. Your definition should use a local tail-recursive function.

```
fun fib(i: Int): Int = TODO()
```

> **NOTE** Kotlin provides us with a convenient way to mark something as *TODO*. A builtin function called `TODO()` is provided that will result in a `NotImplementedError` being thrown when evaluated. The result is that such unimplemented code will always compile, but will result in the exception being thrown as soon as it is evaluated in a program. This gives us a useful way of putting reminder placeholders in our code without affecting the compilation of our code or breaking the build.

### 2.1.2 Writing our first higher-order function

The code that we have written so far has only one specific purpose. How can we adapt it to handle several scenarios? In this section, we follow an iterative approach where we will crudely introduce a new requirement, then gradually improve the design until we are left with a functional solution using a *higher-order function*.

![](img/255e7d4fd2ecc6516db2405b06be1018_22_0.png)

Figure 2.1 Introducing new behaviour to our program adding functions related to factorials.

Now that we have a function that calculates the *nth* factorial called `factorial`, let’s introduce it to the code from before. In addition we’ll do some naive duplication by introducing `formatFactorial`, just as we had `formatAbs` for the `abs` function. The new `formatFactorial` function will then in turn be called from `main` as we did for `formatAbs`.

### Listing 2.2 A simple program including the factorial function

```
object Example {
    private fun abs(n: Int): Int =
        if (n < 0) -n
        else n

    private fun factorial(i: Int): Int {
        fun go(n: Int, acc: Int): Int =
            if (n <= 0) acc
            else go(n - 1, n * acc)
        return go(i, 1)
    }

    fun formatAbs(x: Int): String {
        val msg = "The absolute value of %d is %d"
        return msg.format(x, abs(x))
    }

    fun formatFactorial(x: Int): String {
        val msg = "The factorial of %d is %d"
        return msg.format(x, factorial(x))
    }
}

fun main() {
    println(Example.formatAbs(-42))
    println(Example.formatFactorial(7))
}
```

① Add the `factorial` function, making it private.

② Add the `formatFactorial` function, public by default.

③ Call `formatFactorial` from the `main` method.

The two functions, `formatAbs` and `formatFactorial`, are almost identical. If we like, we can generalize these to a single function, `formatResult`, which accepts as an argument the function to apply to its argument:

```
fun formatResult(name: String, n: Int, f: (Int) -> Int): String {
    val msg = "The %s of %d is %d."
    return msg.format(name, n, f(n))
}
```

Our `formatResult` function is a higher-order function (HOF) that takes another function, called `f` (see *Variable-naming conventions* sidebar). We give a type to `f`, as we would for any other parameter. Its type is `(Int) -> Int` (pronounced “int to int” or “int arrow int”), which indicates that `f` expects an integer argument and will also return an integer.
