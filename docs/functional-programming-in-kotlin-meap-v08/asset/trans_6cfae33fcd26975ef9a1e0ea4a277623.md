
现在试着想象一下，如果我们能够访问 ExecutorService 的实例，如何修改 Par 数据类型中的 run。

```
fun <A> run(es: ExecutorService, a: Par<A>): A = TODO()
```

最简单的表达 Par<A> 的方式可能是将其转换为一个函数类型的别名，例如 (ExecutorService) -> A。如果我们用 ExecutorService 的实例调用此函数，它会产生类型为 A 的值，使得实现变得简单。我们可以进一步改进，通过让 run 的调用者能够推迟决定等待计算多长时间，甚至完全取消它。考虑到这一点，Par<A> 变为 (ExecutorService) -> Future<A>，而 run 只需返回 Future<A>。

```
typealias Par<A> = (ExecutorService) -> Future<A>

fun <A> run(es: ExecutorService, a: Par<A>): Future<A> = a(es)
```

请注意，由于 Par 现在由需要 ExecutorService 的函数表示，Future 的创建实际上要等到提供此 ExecutorService 时才会发生。

真的就这么简单吗？暂时假设是的，如果我们发现它将来无法满足需求，我们再修改模型。

## 7.3 以最终用户为中心完善 API

我们迄今为止的工作方式有点 artificial。在实践中，设计 API 和选择表示之间并没有如此清晰的界限，前者先于后者。表示的想法可以驱动 API 设计，但反过来也可能发生——API 的选择可以驱动表示。自然地在两种视角之间流畅地切换，在问题出现时运行实验，构建原型，等等都是很自然的。

我们将把本节用于探索和完善我们的 API。虽然我们已经通过演进这个简单示例获得了大量收益，但让我们尝试了解在添加任何新操作之前，使用我们已经构建的原始操作能表达什么。凭借我们的原语及其选定的含义，我们为自己开辟了一个小宇宙。我们现在可以发现在这个宇宙中可以表达哪些想法。这可以，也应该是一个流畅的过程——我们可以在任何时候改变我们宇宙的规则，对表示进行根本性更改或引入新的原语，同时观察我们的创作随之如何表现。

我们将通过实现到目前为止开发的 API 的函数来开始。现在我们有了 Par 的表示，第一次尝试应该很直接。接下来是使用此初始表示的朴素实现。

## 列表 7.8 Par 的原始操作可以在 Pars 对象中找到

```
object Pars {
    fun <A> unit(a: A): Par<A> =
        { es: ExecutorService -> UnitFuture(a) }

    data class UnitFuture<A>(val a: A) : Future<A> {
        override fun get(): A = a
        override fun get(timeout: Long, timeUnit: TimeUnit): A = a
        override fun cancel(evenIfRunning: Boolean): Boolean = false
        override fun isDone(): Boolean = true
        override fun isCancelled(): Boolean = false
    }

    fun <A, B, C> map2(
        a: Par<A>,
        b: Par<B>,
        f: (A, B) -> C
    ): Par<C> =
        { es: ExecutorService ->
            val af: Future<A> = a(es)
            val bf: Future<B> = b(es)
            UnitFuture(f(af.get(), bf.get()))
        }

    fun <A> fork(
        a: () -> Par<A>
    ): Par<A> =
        { es: ExecutorService ->
            es.submit(Callable<A> { a()(es).get() })
        }
}
```

- 1. `unit` 表示为返回 `UnitFuture` 的函数
- 2. `map2` 只负责组合逻辑，没有隐式线程
- 3. 由于调用了 `get()`，超时未被尊重
- 4. 由于阻塞调用 `a()`，`fork` 并非真正并行运行

如前所述，这是对该问题的朴素解决方案。我们现在将识别问题并逐一解决它们。`unit` 运算符表示为返回 `UnitFuture` 的函数，`UnitFuture` 是 `Future` 的简单实现，它只是包装一个常量值并且从不使用 `ExecutorService`。它始终被执行且无法取消。它的 `get` 方法只是返回我们给它的值。

接下来是 `map2` 运算符。它不会在单独的逻辑线程中评估对 `f` 的调用，这符合我们将 `fork` 设计为 API 中控制并行的唯一函数的设计选择。如果我们希望 `f` 的求值发生在单独的线程中，我们始终可以将 `map2` 与 `fork` 的调用一起包装。

此 `map2` 实现也不尊重超时。它只是将 ExecutorService 传递给两个 Par 值，等待 Future af 和 bf 的结果，对它们应用 f，最后将它们包装在 UnitFuture 中。为了尊重超时，我们需要一个新的 Future 实现来记录评估 af 所花费的时间，然后从分配给评估 bf 的可用时间内减去该时间。

fork 运算符具有最简单、最自然的实现，但这里也有一些问题——例如，外层的 Callable 会阻塞等待“内部”任务完成。由于这种阻塞占用了 ExecutorService 背后的线程或资源，我们失去了一些潜在的并行性。这是实现中更严重问题的症状，我们将在本章后面讨论。

我们应该注意 Future 没有纯函数式接口。这也是我们不希望库的用户直接处理 Future 的原因之一。一个重要点是，即使 Future 上的方法依赖于副作用，我们整个 Par API 仍然是纯的。只有在用户调用 run 并且实现接收到 ExecutorService 之后，我们才暴露 Future 的机制。因此我们的用户是在编程到一个纯接口，而实现依赖于效应。但由于我们的 API 保持纯净，这些效应不是副作用。在第 4 部分我们将详细讨论这种区别。

练习 7.3（困难）
修复 map2 的实现，使其尊重 Future 上超时的契约。

练习 7.4
此 API 已能实现丰富的操作。例如，使用 lazyUnit 编写一个函数，将任意函数 (A) -> B 转换为异步求值其结果的函数。

```
fun <A, B> asyncF(f: (A) -> B): (A) -> Par<B> = TODO()
```

我们还能用现有组合子表达什么？让我们看一个更具体的例子。

假设我们有一个表示产生 List<Int> 的并行计算的 Par<List<Int>>，并且我们希望将其转换为结果已排序的 Par<List<Int>>：

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> = TODO()
```

我们当然可以运行 Par，对结果列表进行排序，然后用 unit 重新打包到 Par 中。但我们希望避免调用 run。唯一能让我们以任何方式操作 Par 值的其他组合子是 map2。所以如果我们将 parList 传递给 map2 的一侧，我们就能访问内部的 List 并对其进行排序。我们可以向 map2 的另一侧传递任何内容，所以我们就传递一个 Unit：

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> =
    map2(parList, unit(Unit)) { a, _ -> a.sorted() }
```

这很容易。我们现在可以告诉一个 Par<List<Int>> 我们希望该列表被排序。但我们不妨进一步泛化。我们可以将任何类型为 (A) -> B 的函数“提升”为接受 Par<A> 并返回 Par<B> 的函数；我们可以将任意函数映射到 Par 上：

```
fun <A, B> map(pa: Par<A>, f: (A) -> B): Par<B> =
    map2(pa, unit(Unit), { a, _ -> f(a) })
```

因此，sortPar 现在简化为：

```
fun sortPar(parList: Par<List<Int>>): Par<List<Int>> =
    map(parList) { it.sorted() }
```

这简洁明了。我们只是组合操作以使类型对齐。而且，如果你查看 map2 和 unit 的实现，应该很清楚这种 map 实现是有意义的。

传递虚假值 unit(Unit) 作为 map2 的参数然后忽略其值算是作弊吗？完全不是！事实上，我们可以用 map2 实现 map，但反之不行，这表明 map2 比 map 严格更强大。当我们设计库时经常会发生这种情况——通常，看似原语的函数最终会使用更强大的原语表达出来。

使用我们的 API 还能实现什么？能否并行映射一个列表？与合并两个并行计算的 map2 不同，parMap（我们称之为）需要合并 N 个并行计算。看起来这应该可以某种方式表达：

```
fun <A, B> parMap(
    ps: List<A>,
    f: (A) -> B
): Par<List<B>> = TODO()
```

我们始终可以将 parMap 写成一个新的原语。记住 Par<A> 只是一个类型别名 for (ExecutorService) -> Future<A>。

将操作实现为新原语并没有什么问题。在某些情况下，我们甚至可以通过假设我们正在处理的数据类型的底层表示来更高效地实现操作。但现在我们感兴趣的是探索使用现有 API 可以表达哪些操作，以及把握两者之间的关系
