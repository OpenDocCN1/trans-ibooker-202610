
## 练习 7.7（困难）

浏览 `Executors` 中的各种静态方法，以了解存在的不同 `ExecutorService` 实现。然后，在继续之前，回顾一下你的 `fork` 实现，尝试找到一个反例，或说服自己该定律对你的实现成立。

## 侧边栏 为什么代码和证明的定律很重要

陈述和证明 API 的属性似乎很不寻常。这肯定不是普通编程中通常做的事情。为什么在函数式编程中很重要？

在 FP 中，将通用功能提取到可通用、可重用的组件中是容易的并且是预期的，这些组件可以被 *组合*。副作用会损害组合性，但更一般地，任何阻止我们将组件视为黑盒的隐藏非确定性行为都会使组合变得困难或不可能。

`fork` 定律的描述是一个很好的例子。我们可以看到，如果我们提出的定律不成立，许多依赖于 `fork` 的通用组合子，如 `parMap` 将不再是可靠的。因此，它们的使用可能会很危险，因为在更广泛的并行计算中使用时可能会导致死锁。

为我们的 API 提供一个具有有助于推理的有意义定律的代数，使其对客户端更易于使用。这也意味着我们可以确信地将 API 的所有对象视为黑盒。正如我们在第 3 部分将看到的，这对于我们提取我们在不同库中编写的共同模式的能力至关重要。

戴上测试的帽子，我们编写一个断言函数来验证给定 `ExecutorService` 的两个 `Par` 实例的相等性。它被添加为 `Par` 上的中缀 `shouldBe` 扩展方法：

```
infix fun <A> Par<A>.shouldBe(other: Par<A>) = { es: ExecutorService ->
    if (this(es).get() != other(es).get())
        throw AssertionError("Par instances not equal")
}
```

使用这个方便的新断言方法，我们会发现 `fork` 的大多数实现中会出现一个相当微妙的问题。当使用由有界大小的线程池支持的 `ExecutorService` 时（参见 `Executors.newFixedThreadPool`），很容易遇到死锁。假设我们有一个由线程池支持的 `ExecutorService`，其中最大线程数为 1。如果我们尝试使用当前实现运行以下示例，将会发生死锁：

```
val es = Executors.newFixedThreadPool(1)

val a: Par<Int> = lazyUnit { 42 + 1 }
val b: Par<Int> = fork { a }
(a shouldBe b) (es)
```

你能看出为什么会这样吗？让我们更仔细地看看 `fork` 的实现：

```
fun <A> fork(a: () -> Par<A>) : Par<A> =
  { es ->
    es.submit(Callable<A> {
      a() (es).get()
    })
  }
```

① 在另一个 Callable 内部等待一个 Callable 的结果

我们首先提交 Callable，并在该 Callable 内部向 ExecutorService 提交另一个 Callable 并阻塞等待其结果。请回想一下，a() (es) 会向 ExecutorService 提交一个 Callable 并返回一个 Future。如果我们的线程池大小为 1，这就是一个问题。外部 Callable 被提交并由唯一的线程拾取。在该线程内，在它完成之前，我们提交并阻塞等待另一个 Callable 的结果。但没有可用的线程来运行这个 Callable。它们相互等待，因此我们的代码死锁了。

## 练习 7.8（困难）
证明对于 `fork` 的这种实现，任何固定大小的线程池都可以导致死锁。

当你找到这样的反例时，你有两个选择——你可以尝试修复你的实现以使定律成立，或者你可以细化你的定律，更明确地说明其成立的条件。例如，你可以简单地规定你需要可以无界增长的线程池。即使这样也是一个很好的练习。它迫使你记录以前隐含的不变量或假设。

我们能否修复 fork 使其在固定大小的线程池上工作？现在让我们看一个不同的实现：

```
fun <A> fork(pa: () -> Par<A>) : Par<A> =
  { es -> pa() (es) }
```

这肯定避免了死锁。唯一的问题是我们根本没有为评估 pa 分叉一个单独的逻辑线程。因此，对于某个 ExecutorService，fork(hugeComputation) (es) 会在主线程中运行 hugeComputation，这正是我们通过调用 fork 想要避免的。尽管这不是 fork 的意图，但这仍然是一个有用的组合子，因为它让我们能够延迟计算的实例化，直到实际需要时。让我们给它一个新名字，delay：

```
fun <A> delay(pa: () -> Par<A>): Par<A> =
  { es -> pa() (es) }
```

我们真正想做的是在固定大小的线程池上运行任意计算。为了做到这一点，我们需要选择一个不同的 Par 表示。

## 7.4.3 使用 actors 实现非阻塞实现

在本节中，我们将开发一个完全非阻塞的 Par 实现，该实现适用于固定大小的线程池。由于这对于我们讨论函数式设计的各个方面的整体目标来说不是必不可少的，如果你愿意，可以跳到第 7.5 节。否则，请继续阅读。

当前表示的本质问题是，我们无法在不让当前线程在其 get 方法上阻塞的情况下从 Future 中获取值。一种不会以这种方式泄漏资源 的 Par 表示必须是无阻塞的，意味着 fork 和 map2 的实现绝不能调用像 Future.get 那样阻塞当前线程的方法。正确编写这样的实现可能具有挑战性。幸运的是，我们有定律来测试我们的实现，而且我们只需要做对一次。之后，我们库的用户可以享受一个可组合且抽象的 API，每次都能做正确的事情。

在下面的代码中，你不需要理解每一步发生了什么。我们只想用实际代码来演示，一个正确的遵守定律的 Par 表示可能是什么样子。

## 通过注册回调重新思考 PAR 为非阻塞

那么我们如何实现一个非阻塞的 Par 表示呢？想法很简单。不是将 Par 变成只能通过阻塞调用获取值的 java.util.concurrent.Future，我们将引入我们自己的 Future 版本。我们的版本能够注册一个回调，当结果准备好时将被调用。这是一个轻微的视角转变。

```
abstract class Future<A> {
    internal abstract fun invoke(cb: (A) -> Unit)
}

typealias Par<A> = (ExecutorService) -> Future<A>
```

① invoke 方法被声明为 internal，因此在模块之外不可见

② Par 看起来与之前相同，尽管我们使用的是新的非阻塞 Future 而不是 java.util.concurrent 中的 Future。

我们全新的 Par 类型看起来与我们的初始表示相同，只是我们现在返回的是 Future，其 API 与 java.util.concurrent.Future 中的不同。不是调用 get 来从 Future 中获取结果，我们的 Future 有一个 invoke 方法，它接收一个函数 cb，该函数期望类型为 A 的结果并使用它来执行某些效果。这种函数有时被称为 continuation 或回调。

invoke 方法被标记为 internal，这样我们就不会将其暴露给库的用户。将它标记为 internal 会将方法访问限制在模块范围之外。这样我们的 API 保持纯净，我们可以保证我们的定律得到维护。

## 侧边栏 使用局部副作用实现纯 API

我们在这里定义的 Future 类型相当命令式。定义 (A) -> Unit 立即引起怀疑。这样的函数只能用于使用给定的 A 执行某些副作用，因为我们肯定不会使用返回的 Unit 值。我们在使用像 Future 这样的类型时仍在做函数式编程吗？是的，尽管我们使用了将副作用用作纯函数式 API 实现细节的常见技术。我们可以这样做，因为我们使用的副作用对使用 Par 的代码不可观察。请注意，invoke 方法是 internal 的，甚至在我们的库之外也不可见。

在我们继续实现非阻塞 Par 的其余部分时，你可能希望说服自己所使用的副作用无法被外部代码观察到。局部效应、可观察性和我们纯度和引用透明性定义中的细微差别在第 14 章中有更详细的讨论，但现在非正式的理解就足够了。

让我们从查看 Par 的实际创建示例开始。最简单的方法是通过 unit。

## Listing 7.10 通过 unit 创建新的非阻塞 Par

```
fun <A> unit(a: A): Par<A> =
  { es: ExecutorService ->
    object : Future<A>() {
      override fun invoke(cb: (A) -> Unit) = cb(a)
    }
  }
```

只需将值传递给 continuation。完成！

由于 unit 已经有类型为 A 的值 a 可用，这个函数需要做的就是调用 continuation cb 并传入该值。

使用这种 Par 表示，让我们看看如何实现 run 函数，我们将把它改为只返回一个 A。由于它从 Par<A> 到 A，它必须构造一个 continuation 并将其传递给 Future 值的 invoke 方法。通过使这个 continuation
