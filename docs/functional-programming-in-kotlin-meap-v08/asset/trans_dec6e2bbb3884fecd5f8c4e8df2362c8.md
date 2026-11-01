
- 2. 创建一个解析器，通过函数 f 结合解析器 fa 和 fb 的结果来生成 C。
- 3. 如果两个 Option 都有值，则使用函数 f 将它们结合，否则返回 None。

这些函数不仅仅在名称上相似。尽管它们操作的数据类型看似毫无关联，实现却完全相同！唯一不同的是所操作的具体数据类型。这证实了我们一直暗示的观点——这些是某种更一般模式的特定实例。我们应该能够利用这种模式来避免重复。例如，我们应该能够只编写一次 map2，使其可用于所有这些数据类型。

我们在这里通过为函数和参数选择统一的名称、按相同顺序传递参数等方式，有意让代码重复变得非常明显。在日常工作中可能更难察觉。但你编写的库越多，就越擅长识别可以提取为通用抽象的模式。

## 11.2.1 介绍 Monad 接口

单子无处不在！事实上，这正是将 Parser、Gen、Par、Option 以及我们迄今为止看到的许多其他数据类型联系在一起的东西。就像我们对 Foldable 和 Functor 所做的那样，我们可以为 Monad 设计一个 Kotlin 接口，一次性定义 map2 和许多其他函数，而不必为每种具体数据类型重复其定义。

在本书记第 2 部分中，我们关注的是单个数据类型，寻找一组最小的原始操作，从中可以派生出大量有用的组合子。我们在这里也会做类似的事情，将抽象接口精炼为一组小的原始操作。

让我们先引入一个新的接口，暂时称之为 Mon。既然我们最终想要定义 map2，那就按列表 11.4 的方式先定义它。

### 列表 11.4 将 Mon 接口定义为 map2 的归属地

```
interface Mon<F> {
    fun <A, B, C> map2(
        fa: Kind<F, A>,
        fb: Kind<F, B>,
        f: (A, B) -> C
    ): Kind<F, C> =
        flatMap(fa) { a -> map(fb) { b -> f(a, b) } }
}
```

- 1. Mon 接口使用高阶类型 F 进行参数化
- 2. 使用 Kind<F, A> 表示 F<A>
- 3. 由于在 F 的上下文中未定义 map 和 flatMap，因此无法编译

在这个示例中，我们只是取了 map2 的实现，并在签名中将 Parser、Gen 和 Option 改为 Mon<F> 接口中的多态 F。52 我们使用 Kind 接口来引用 F 的种类。但在这种多态上下文中，实现无法编译！我们在这里对 F 一无所知，因此当然不知道如何对 Kind<F, A> 进行 flatMap 或 map！

我们可以做的就是简单地将 map 和 flatMap 添加到 Mon 接口并保持它们抽象。这样做可以保持 map2 与之前一致。

### 列表 11.5 向 Mon 接口引入 flatMap 和 map 声明

```
fun <A, B> map(fa: Kind<F, A>, f: (A) -> B): Kind<F, B>

fun <A, B> flatMap(fa: Kind<F, A>, f: (A) -> Kind<F, B>): Kind<F, B>
```

这种转换相当机械。我们只是检查了 map2 的实现，并将它调用的所有函数 map 和 flatMap 作为合适的抽象方法添加到我们的接口中。这个接口现在可以编译了，但在宣布胜利并继续定义 Mon<List>、Mon<Parser>、Mon<Option> 等实例之前，让我们看看是否可以精炼我们的原始集合。我们的当前原始集合是 map 和 flatMap，可以从中派生出 map2。flatMap 和 map 是否是最小的原始集合？嗯，实现 map2 的数据类型都有一个 unit，而且我们知道 map 可以用 flatMap 和 unit 来实现。例如，在 Gen 上：

```
fun <A, B> map(fa: Gen<A>, f: (A) -> B): Gen<B> =
    flatMap(fa) { a -> unit(f(a)) }
```

所以让我们选择 flatMap 和 unit 作为我们的最小原始集合。我们将把所有具有这些函数定义的数据类型统一在单一概念下。该接口将命名为 Monad，具有 flatMap 和 unit 的抽象声明，同时提供 map 和 map2 的默认实现，基于我们的抽象原始操作。

### 列表 11.6 声明具有 flatMap 和 unit 原始定义的 Monad

```
interface Monad<F> : Functor<F> {
    fun <A> unit(a: A): Kind<F, A>
    fun <A, B> flatMap(fa: Kind<F, A>, f: (A) -> Kind<F, B>): Kind<F, B>
    override fun <A, B> map(
        fa: Kind<F, A>,
        f: (A) -> B
    ): Kind<F, B> =
        flatMap(fa) { a -> unit(f(a)) }
    fun <A, B, C> map2(
        fa: Kind<F, A>,
        fb: Kind<F, B>,
        f: (A, B) -> C
    ): Kind<F, C> =
        flatMap(fa) { a -> map(fb) { b -> f(a, b) } }
}
```

① Monad 提供了 map 的默认实现，并且也可以实现 Functor

② 为了成功编译，Functor 中 map 的重写需要显式声明

### 侧边栏 单子名称的含义

我们本可以把 Monad 命名为任何东西，比如 FlatMappable、Unicorn 或 Bicycle。但 monad 已经是一个在常用中非常合适的名称。这个名称来自范畴论，一个启发了大量函数式编程概念的数学分支。monad 这个名称是有意与 monoid 相似，两者在深层次上相关。

为了将其与具体数据类型联系起来，我们可以为 Gen 实现 Monad 实例。

### 列表 11.7 使用具体类型声明 Gen 的 Monad 实例

```
object Monads {
    val genMonad = object : Monad<ForGen> {
        override fun <A> unit(a: A): GenOf<A> = Gen.unit(a)
        override fun <A, B> flatMap(
            fa: GenOf<A>,
            f: (A) -> GenOf<B>
        ): GenOf<B> =
            fa.fix().flatMap { a: A -> f(a).fix() }
    }
}
```

① 类型 ForGen 是我们提供的替代类型，用来绕过 Kotlin 在表达高阶类型方面的限制

② 类型别名 GenOf<A> 是 Kind<ForGen, A> 的语法糖

使用提供的扩展方法 fix() 将所有 GenOf<A> 向下转型为 Gen<A>，以便与 Gen.flatMap 兼容

我们只需要实现 flatMap 和 unit，就能免费获得 map 和 map2。这是由于 Monad 从 Functor 继承了这两个函数。我们一次性实现了它们，适用于任何可以创建 Monad 实例的数据类型！但我们才刚刚开始。以这种方式可以实现的函数还有很多。

### 练习 11.1

为 Par、Option 和 List 编写单子实例。此外，为 arrow.core.ListK 和 arrow.core.SequenceK 提供单子实例。

### 注意

Arrow 提供的 ListK 和 SequenceK 类型是包装类，它们将平台等价物 List 和 Sequence 转换为功能齐全的类型构造子。

### 练习

```
object Monads {
    val parMonad: Monad<ForPar> = TODO()
    val optionMonad: Monad<ForOption> = TODO()
    val listMonad: Monad<ForList> = TODO()
    val listKMonad: Monad<ForListK> = TODO()
    val sequenceKMonad: Monad<ForSequenceK> = TODO()
}
```

### 练习 11.2（困难）

State 看起来也可能是一个单子，但它接受两个类型参数，即 S 和 A。要实现 Monad，你需要一个只有一个参数的类型构造子。尝试实现一个 State 单子，看看会遇到什么问题，并思考如何解决。我们将在本章后面讨论解决方案。

```
data class State<S, out A>(val run: (S) -> Pair<A, S>) : StateOf<S, A>
```

## 11.3 单子组合子

我们已经到了为单子定义原始操作的地步。有了这些，我们现在可以继续前进，发现额外的组合子。事实上，现在我们可以回顾之前的章节，看看是否还有为每个单子数据类型实现的其它函数。这些类型中的许多都可以作为一次到位的单子来实现，所以现在就让我们这样做。

### 练习 11.3
sequence 和 traverse 组合子现在应该相当熟悉了，你在前几章中对它们的实现可能都非常相似。在 Monad<F> 上一次性实现它们。

```
fun <A> sequence(lfa: List<Kind<F, A>>): Kind<F, List<A>> = TODO()

fun <A, B> traverse(
    la: List<A>,
    f: (A) -> Kind<F, B>
): Kind<F, List<B>> = TODO()
```

我们曾在 Gen 和 Parser 上看到过 listOfN 组合子，它允许我们重复生成器或解析器 n 次，以获得该长度的列表的解析器或生成器。我们可以通过将其添加到 Monad 接口，为所有单子 F 实现这个组合子。我们也可以给它一个更通用的名称，比如 replicateM，意思是“在单子中复制”。

### 练习 11.4
实现 replicateM 以生成 Kind<F, List<A>>，列表长度为 n。

```
fun <A> replicateM(n: Int, ma: Kind<F, A>): Kind<F, List<A>> = TODO()

fun <A> _replicateM(n: Int, ma: Kind<F, A>): Kind<F, List<A>> = TODO()
```

### 练习 11.5
思考 replicateM 在各种 F 选择下的行为。例如，它在 List 单子中如何表现？而 Option 呢？用你自己的话描述 replicateM 的一般含义。

我们的数据类型 Parser 还有一个名为 product 的组合子，它接受两个解析器并将其转换为成对的解析器。我们用 map2 实现这个 product 组合子。我们也可以为任意单子 F 通用地编写它：
