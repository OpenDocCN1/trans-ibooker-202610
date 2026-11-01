
- 1. 将测试用例的最大默认大小设置为 `100`
- 2. 将运行的测试用例默认数量设置为 `100`
- 3. 提供一个可立即使用的简单随机数生成器
- 4. 失败时向标准输出打印错误信息。
- 5. 测试通过时向标准输出打印成功信息。

我们在这里利用了一些默认参数，使方法调用更方便。我们希望默认的测试数量足以获得良好的覆盖率，但又不要太多，否则运行时间会过长。

如果我们尝试运行 `run(maxProp)`，会发现该属性失败！

```
Falsified after 0 passed tests: test case: []
generated and exception: max on empty list
stacktrace:
...
```

基于属性的测试能够揭示我们对代码的隐藏假设，并迫使我们对这些假设更加明确。标准库中 `max` 的实现在处理空列表时会返回 null，我们将其解释为 `IllegalStateException`。我们需要修复我们的属性以考虑这一点。

## 练习 8.13

为生成非空列表定义 `nonEmptyListOf`，然后更新 `max` 的规范以使用此生成器。

```
fun <A> nonEmptyListOf(ga: Gen<A>): SGen<List<A>> = TODO()

val maxProp = TODO()
```

## 练习 8.14

编写一个名为 `maxProp` 的属性来验证 `List.sorted` 的行为，你可以用它来对（其中）一个 `List<Int>` 进行排序。

## 8.4.2 为并行计算编写测试套件

回顾一下，在第 7 章中我们发现了一些应该对我们的并行计算成立的定律。我们能否用我们的库来表达这些定律？我们查看的第一个“定律”实际上是一个特定的测试用例：

```
map(unit(1)) { it + 1 } == unit(2)
```

我们当然可以表达这一点，但假设我们的表示方式如下，结果会有些丑陋

Par<A> 是函数类型 (ExecutorService) -> Future<A> 的别名。

```
val es = Executors.newCachedThreadPool()
val pl = forAll(Gen.unit(Par.unit(1))) { pi ->
  map(pi, { it + 1 })(es).get() == Par.unit(2)(es).get()
}
```

 resulting test is verbose, cluttered, and the idea of the test is obscured by irrelevant detail. 请注意，这并不是 API 是否足够表达力的问题——是的，我们可以表达想要的内容，但缺少辅助函数和糟糕的语法组合掩盖了真正的意图。

## 证明属性

接下来，让我们改进这种冗长和杂乱。我们的第一个观察是，forAll 对于此测试用例来说过于通用。我们并没有改变测试的输入，我们只有一个硬编码的示例，它应该像在任何传统的单元测试框架中一样方便编写。让我们在 Prop 伴生对象上为其引入一个组合子：

```
fun check(p: () -> Boolean): Prop = TODO()
```

我们如何实现它？一种可能的方法是使用 forAll：

```
fun check(p: () -> Boolean): Prop {
  val result by lazy { p() }
  return forAll(Gen.unit(Unit)) {
    result
  }
}
```

- ① 传入一个非严格值
- ② 结果被记忆化以避免重新计算

这似乎不太对。我们提供了一个仅生成单个 Unit 值的单元生成器。然后我们忽略该值，只是为了强制求值给定的布尔值。并不理想。

即使我们记忆化结果使其不被求值多次，测试运行器仍会生成多个测试用例并多次测试该布尔值。例如，如果我们执行 run(check(true))，这将测试该属性 100 次并打印“OK, passed 100 tests.”但检查一个总是为真的属性 100 次是极大的浪费。我们需要一个新的原语。

我们目前拥有的 Prop 的表示只是一个类型为 (MaxSize, TestCases, RNG) -> Result 的函数，其中 Result 要么是 Passed 要么是 Falsified。check 原语的一个简单实现是构造一个忽略测试用例数量的 Prop：

```
fun check(p: () -> Boolean): Prop =
  Prop { _, _, _ ->
    if (p()) Passed
    else Falsified("()", 0)
  }
```

这比使用 forAll 确实更好，但 run(check(true)) 仍会打印“passed 100 tests”，即使它只测试了一次属性。这种属性在经过多次测试后仍未被证伪的意义上“通过”并不是真正正确的。它在一次测试后就被证明了。看起来我们想要一种新的 Result 类型：

### Listing 8.10 引入 Proved 来表示在单次测试后具有证明的结果

```
object Proved : Result()
```

现在我们可以在 check 创建的属性中返回 Proved 而不是 Passed。我们需要修改测试运行器以考虑这种新情况：

### Listing 8.11 更新 run 以处理新的 Proved 结果类型

```
fun run(
    p: Prop,
    maxSize: Int = 100,
    testCases: Int = 100,
    rng: RNG = SimpleRNG(System.currentTimeMillis())
): Unit =
    when (val result = p.run(maxSize, testCases, rng)) {
        is Falsified ->
            println(
                "Falsified after ${result.successes} passed tests: " +
                result.failure
            )
        is Passed ->
            println("OK, passed $testCases tests.")
        is Proved ->
            println("OK, proved property.")
    }
```

我们还需要修改 Prop 组合子如 and 的实现。这些更改相当 trivial，因为此类组合子不需要区分 Passed 和 Proved 结果。

### Listing 8.12 更新 Prop 以处理 Passed 和 Proved 两种通过情况

```
fun and(p: Prop) =
    Prop { max, n, rng ->
        when (val prop = run(max, n, rng)) {
            is Falsified -> prop
            else -> p.run(max, n, rng)
        }
    }
```

① else 回退同时处理 Passed 和 Proved 成功类型

如果你想更进一步，请查看附录 C 中的练习 C.1。

## 测试 PAR

回到证明属性 map(unit(1)) { it + 1 } 等于 unit(2)，我们可以使用新的 check 原语以不掩盖意图的方式表达它：

```
val p = check {
  val p1 = map(unit(1)) { it + 1 }
  val p2 = unit(2)
  p1(es).get() == p2(es).get()
}
```

这现在相当清晰了。但我们能对 p1(es).get() 和 p2(es).get() 的噪音做些什么？这种不必要的重复掩盖了测试的意图，与我们试图证明的内容关系不大。我们迫使此代码了解 Par 的内部结构，以便我们可以比较两个 Par 值是否相等。一个改进是使用 map2 将相等性比较提升到 Par 中，这意味着我们只需在最后运行一个 Par 即可获得结果：

```
fun <A> equal(p1: Par<A>, p2: Par<A>): Par<Boolean> =
  map2(p1, p2, { a, b -> a == b })

val p = check {
  val p1 = map(unit(1)) { it + 1 }
  val p2 = unit(2)
  equal(p1, p2)(es).get()
}
```

这已经比分别运行每一侧要好一些。但既然如此，为什么不把 Par 的运行移到一个名为 forAllPar 的单独函数中呢。这也为我们在不同的并行策略之间插入变化提供了一个好地方，而不会使我们指定的属性变得杂乱：

```
val ges: Gen<ExecutorService> = weighted(
  Gen.choose(1, 4).map {
    Executors.newFixedThreadPool(it)
  } to .75,
  Gen.unit(
    Executors.newCachedThreadPool()
  ) to .25
)

fun <A> forAllPar(ga: Gen<A>, f: (A) -> Par<Boolean>): Prop =
  forAll(
    map2(ges, ga) { es, a -> es to a }
  ) { (es, a) -> f(a)(es).get() }
```

- ① 执行器服务的加权生成器
- ② 75% 的时间创建固定线程池
- ③ 25% 的时间创建无界线程池
- ④ 使用 to 关键字创建 Pair<Gen<ExecutorService>, Gen<A>>

值 ges 是一个 Gen<ExecutorService>，它将在 1–4 个线程的固定大小线程池上变化，并考虑无界线程池。

接下来，让我们关注 map2(ges, ga) { es, a -> es to a }。这是一种相当嘈杂的将两个生成器组合以产生其输出对的方式。让我们引入一个组合子来清理这个混乱：

```
fun <A, B> combine(ga: Gen<A>, gb: Gen<B>): Gen<Pair<A, B>> =
    map2(ga, gb) { a, b -> a to b }
```

这已经感觉好多了，不那么笨拙了！

```
fun <A> forAllPar(ga: Gen<A>, f: (A) -> Par<Boolean>): Prop =
    forAll(
        combine(ges, ga)
    ) { esa ->
        val (es, a) = esa
        f(a) (es).get()
    }
```

即使这更好，我们还没有到达目的地。我们的目标是使我们库的用户体验尽可能无摩擦。我们可以通过应用 Kotlin 工具包中的一些特性使其更易于使用和更自然。首先，我们可以将 combine 引入为 Gen 的方法。我们也可以使用 infix 关键字去掉不必要的标点和括号：

```
infix fun <A, B> Gen<A>.combine(gb: Gen<B>): Gen<Pair<A, B>> =
    map2(this, gb) { s, a -> s to a }
```

这反过来给了我们一个更流畅的表达式，例如：

```
fun <A> forAllPar(ga: Gen<A>, f: (A) -> Par<Boolean>): Prop =
    forAll(ges combine ga) { esa ->
        val (es, a) = esa
        f(a) (es).get()
    }
```

我们能做的最后改进是通过内联解构 Pair<ExecutorService, A> 来改进参数向匿名函数的注入，这将我们带到最终迭代。

```
fun <A> forAllPar(ga: Gen<A>, f: (A) -> Par<Boolean>): Prop =
    forAll(ges combine ga) { (es, a) ->
        f(a) (es).get()
    }
```

现在我们可以继续使用我们的新属性来实现 checkPar，它反过来消耗由第 7 章中的 Par.equal 发出的 Par<Boolean>。所有这些结合起来为我们库的用户提供了更好的体验。

```
fun checkPar(p: Par<Boolean>): Prop =
    forAllPar(Gen.unit(Unit)) { p }

val p2 = checkPar(
    equal(
        map(unit(1)) { it + 1 },

