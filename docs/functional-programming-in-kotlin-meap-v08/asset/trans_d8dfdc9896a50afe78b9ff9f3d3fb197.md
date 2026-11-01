
## 2.1 高阶函数：向函数传递函数

立即开始，我们先介绍编写函数式程序的一些基础知识。第一个新概念是：函数是值。就像其他类型的值——例如整数、字符串和列表——函数可以被赋给变量、存储在数据结构中，并作为参数传递给函数。

在编写纯函数式程序时，我们经常会发现编写接受其他函数作为参数的函数很有用。这被称为高阶函数（HOF），接下来我们将通过一些简单示例来说明这一点。在后面的章节中，我们将看到这种能力到底有多有用，以及它如何渗透到函数式编程风格中。但首先，假设我们想修改程序以同时打印一个数的绝对值和另一个数的阶乘。以下是此类程序的示例运行：

```
The absolute value of -42 is 42
The factorial of 7 is 5040
```

### 2.1.1 一个简短的插曲：以函数式的方式编写循环

为了使现有程序能够演示 HOF，我们应该引入一些新的行为。我们将通过添加一个计算第 n 个阶乘的新函数来实现这一点。为了编写这个简单的函数，我们需要做一个简短的插曲，展示循环如何在纯函数式的方式下编写。我们通过引入递归来实现这一点。

首先，让我们编写阶乘：

### 列表 2.1 一个阶乘函数

```
fun factorial(i: Int): Int {
    fun go(n: Int, acc: Int): Int =
        if (n <= 0) acc
        else go(n - 1, n * acc)
    return go(i, 1)
}
```

- 1. 一个内部或局部函数定义。
- 2. 调用局部函数。

注意 在另一个函数的函数体中编写局部函数是很常见的。在函数式编程中，我们不应该认为这比局部整数或字符串更奇怪。

我们以函数式的方式编写循环，而不改变循环变量，是使用递归函数。这里我们在阶乘函数的函数体内部定义了一个递归辅助函数。该函数通常会处理需要累加器参数的递归调用，或对封闭函数不具有的其他签名更改。这种辅助函数通常按惯例称为 `go` 或 `loop`。在 Kotlin 中，我们可以在任何块中定义函数，包括在另一个函数定义内部。由于它是局部的，`go` 函数只能从 `factorial` 函数体内部的范围内引用，就像局部变量一样。`factorial` 的定义最终只由对 `go` 的调用组成，并带有循环的初始条件。`go` 的参数是循环的状态。在这种情况下，它们是剩余值 `n`，以及当前累积的阶乘 `acc`。要推进到下一迭代，我们只需递归调用 `go` 并传入新的循环状态（这里是 `go(n-1, n*acc)`），要退出循环，我们在没有递归调用的情况下返回一个值（这里在 `n <= 0` 的情况下返回 `acc`）。

Kotlin *不* 手动检测这种自递归，但要求函数声明 `tailrec` 修饰符。这反过来会指示编译器发出与 while 循环⁴ 找到的同类字节码，前提是递归调用处于尾位置。有关此技术的技术细节，请参阅 *Kotlin 中的尾调用* 侧边栏，但基本思想是这种优化⁵（或尾调用消除）可以在递归调用返回后没有额外工作需要完成时应用。

## 侧边栏

### Kotlin 中的尾调用

如果调用者除了返回递归调用的值之外什么都不做，则称该调用处于尾位置。例如，我们之前讨论的对 `go(n-1,n*acc)` 的递归调用就处于尾位置，因为该方法直接返回此递归调用的值并且不对它做任何其他处理。另一方面，如果我们说 `1 + go(n-1,n*acc)`，`go` 就不再处于尾位置，因为当 `go` 返回其结果时，该方法仍有工作要做（即向其添加 1）。

如果函数所做的所有递归调用都处于尾位置，并且函数声明了 `tailrec` 修饰符，Kotlin 会将递归编译为不为每次迭代消耗调用栈帧的迭代循环。

```
fun factorial(i: Int): Int {
    tailrec fun go(n: Int, acc: Int): Int = ①
        if (n <= 0) acc
        else go(n - 1, n * acc) ②
    return go(i, 1)
}
```

- ① `tailrec` 修饰符指示编译器消除尾调用。
- ② 函数的最终声明处于尾位置

如果递归函数有处于尾位置的调用，但未声明为 `tailrec`，编译器不会消除尾调用，这反过来可能导致抛出 StackOverflowError。

在我们将 `tailrec` 修饰符应用于其最终声明不处于尾位置的函数的情况下，编译器会发出警告：

```
Warning: (19, 9) Kotlin: A function is marked as tail-recursive but no tail calls are found
```

即使警告总比没有好，在这种情况下，编译错误会更有帮助也更安全。

## 练习

### 2.1

编写一个递归函数来获取第 n 个斐波那契数（en.wikipedia.org/wiki/Fibonacci_number）。前两个斐波那契数是 0 和 1。第 n 个数始终是前两个之和——序列开始为 0, 1, 1, 2, 3, 5, 8, 13, 21。你的定义应使用局部尾递归函数。

```
fun fib(i: Int): Int = TODO()
```

> **NOTE** Kotlin 为我们提供了一种方便的方式来标记某物为 *待办*。提供了一个内置函数 `TODO()`，当求值时会抛出 `NotImplementedError`。结果是，这种未实现的代码始终可以编译，但只要在程序中求值就会抛出异常。这为我们在代码中放置提醒占位符提供了一种有用的方法，而不会影响代码的编译或破坏构建。

### 2.1.2 编写我们的第一个高阶函数

我们到目前为止编写的代码只有一个特定目的。我们如何将其改编以处理多种场景？在本节中，我们采用迭代方法，我们将粗略地引入一个新需求，然后逐步改进设计，直到我们得到一个使用 *高阶函数* 的函数式解决方案。

![](img/255e7d4fd2ecc6516db2405b06be1018_22_0.png)

图 2.1 向我们的程序引入新行为，添加与阶乘相关的函数。

现在我们有一个计算 *第 n 个* 阶乘的函数 `factorial`，让我们将其引入之前的代码中。此外，我们将通过引入 `formatFactorial` 来进行一些天真的重复，就像我们对 `abs` 函数有 `formatAbs` 一样。然后新的 `formatFactorial` 函数将像我们对 `formatAbs` 所做的那样从 `main` 调用。

### 列表 2.2 包含阶乘函数的简单程序

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

① 添加 `factorial` 函数，使其为私有。
② 添加 `formatFactorial` 函数，默认为公开。
③ 从 `main` 方法调用 `formatFactorial`。

两个函数 `formatAbs` 和 `formatFactorial` 几乎相同。如果我们愿意，可以将它们概括为一个单一函数 `formatResult`，该函数接受要对其参数应用的函数作为参数：

```
fun formatResult(name: String, n: Int, f: (Int) -> Int): String {
    val msg = "The %s of %d is %d."
    return msg.format(name, n, f(n))
}
```

我们的 `formatResult` 函数是一个高阶函数（HOF），它接受另一个函数，称为 `f`（参见 *变量命名约定* 侧边栏）。我们为 `f` 提供类型，就像为任何其他参数一样。它的类型是 `(Int) -> Int`（读作“int to int”或“int arrow int”），这表明 `f` 预期一个整数参数并且也将返回一个整数。
