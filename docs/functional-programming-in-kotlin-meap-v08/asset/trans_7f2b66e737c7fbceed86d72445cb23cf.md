
我们一直以面向对象的方式来看待它们。也就是说，一个有用的接口可能仅由一组受定律关联的操作定义。

本章将继续这种思考模式，并将其应用于从第 1 部分和第 2 部分编写的一些库中提取代码重复的问题。我们将发现两个新的抽象接口 Monad 和 Functor，并获得在代码中识别此类抽象结构的更通用经验。

## 11.1 函子：泛化 map 函数

本章的焦点是 Monad，但要完全理解它，我们需要先弄清楚它所依赖的 Functor。在第 10 章中我们了解到 Monoid 与 Semigroup 之间存在关系。事实上，我们发现 Monoid 是一个具有一些额外功能⁵¹ 的 Semigroup。虽然 Monad 与 Functor 之间的关系没有这么明确，但我们仍然可以说 Monad 通常也是一个 Functor。因此，本节将帮助我们首先理解 Functor 是什么以及如何应用它。一旦我们奠定了这一基础，就能自信地进入 Monad 的领域。

在第 1 部分和第 2 部分中，我们实现了几种不同的组合子库。在每种情况下，我们都是先编写一组小的原语，然后再编写许多纯粹以这些原语定义的组合子。我们注意到我们在编写的库之间派生的组合子存在一些相似之处。例如，我们为每种数据类型实现了一个 map 函数，以提升在某种数据类型的“上下文中”转换一个参数的函数。对于 Option、Gen 和 Parser，其类型签名如下：

```kotlin
fun <A, B> map(ga: Option<A>, f: (A) -> B): Option<B>
fun <A, B> map(ga: Gen<A>, f: (A) -> B): Gen<B>
fun <A, B> map(ga: Parser<A>, f: (A) -> B): Parser<B>
```

这些类型签名仅在具体的 数据类型（Option、Gen 或 Parser）上有所不同。我们可以用一个名为 Functor 的 Kotlin 接口来捕捉这一思想，作为实现 map 的数据类型：

### 列表 11.1 为类型 F 定义 map 功能的函子接口

```kotlin
interface Functor<F> {
    fun <A, B> map(fa: Kind<F, A>, f: (A) -> B): Kind<F, B>
}
```

这里我们将 map 参数化为类型构造子 Kind<F, A>，就像我们在第 10 章中对 Foldable 所做的那样。回想一下，类型构造子应用于一个类型以生成另一个类型。例如，List 是一个类型构造子，而不是一个类型。List 类型没有值，但我们可以将其应用于类型 Int 以生成类型 List<Int>。同样，Parser 可以应用于 String 得到 Parser<String>。而不是选择特定的 Kind<F, A>，比如 Gen<A> 或 Parser<A>，Functor 接口在 F 的选择上是参数化的。以下是 List 的一个实例：

```kotlin
val listFunctor = object : Functor<ForList> {
    override fun <A, B> map(fa: ListOf<A>, f: (A) -> B): ListOf<B> =
        fa.fix().map(f)
}
```

**NOTE** 与第 10 章一样，我们借助 Kind 类型及其相关的样板代码来在 Kotlin 中表达高阶类型。请参阅附录 D 以了解其含义。

我们说像 List（或 Option，或 F）这样的类型构造子是一个函子，而 Functor<F> 实例构成了 F 确实是函子的证明。

我们可以用这个抽象做什么？我们可以通过纯粹代数的方式对接口的操作进行尝试，从而发现有用的函数。让我们看看仅用 map 能定义哪些（如果有的话）有用操作。让我们看一个这样的例子。如果我们有 F<Pair<A, B>>，其中 F 是一个函子，我们可以将 F “分配”到 pair 上得到 Pair<F<A>, F<B>>：

```kotlin
fun <A, B> distribute(
    fab: Kind<F, Pair<A, B>>
): Pair<Kind<F, A>, Kind<F, B>> =
    Pair(map(fab) { it.first }, map(fab) { it.second })
```

我们只是通过遵循类型写出了它，但让我们思考一下它对 List、Gen、Option 等具体数据类型意味着什么。例如，如果我们对 List<Pair<A, B>> 进行 distribute，我们会得到两个长度相同的列表，一个包含所有的 A，另一个包含所有的 B。该操作有时称为 unzip。因此，我们刚刚编写了一个通用的 unzip 函数，它不仅适用于列表，还适用于任何函子！

当我们有这样一个在乘积上的操作时，我们应该看看是否可以在和或余积上构造相反的操作。余积是范畴论中给不相交并集的术语，也就是我们迄今所知的 Either。在我们的例子中，给定一个高阶类型的余积，我们应该得到一个余积的类型。我们将称之为 codistribute。

```kotlin
fun <A, B> codistribute(
    e: Either<Kind<F, A>, Kind<F, B>>
): Kind<F, Either<A, B>> =
    when (e) {
        is Left -> map(e.a) { Left(it) }
        is Right -> map(e.b) { Right(it) }
    }
```

codistribute 对 Gen 意味着什么？如果我们有一个生成 A 的生成器或生成 B 的生成器，我们可以构造一个生成器，根据我们实际拥有的生成器生成 A 或 B。

我们刚刚基于 Functor 的抽象接口纯粹地提出了两个通用且可能有用的组合子，并且我们可以将它们重用于任何允许 map 实现的类型。

### 11.1.1 定律的重要性及其与函子的关系

每当我们创建像 Functor 这样的抽象时，我们不仅应考虑它应具有哪些抽象方法，还应考虑我们期望其实现遵守的定律。你为抽象规定的定律完全由你决定，尽管 Kotlin 不会代表你强制执行这些定律中的任何一个。如果你要借用像函子或幺半群这样的现有数学抽象的名称，我们建议使用数学中已指定的定律。定律重要有两个原因：

- 定律有助于接口形成一个新的语义层，其代数可以独立于实例进行推理。例如，当我们取 Monoid<A> 和 Monoid<B> 的乘积形成 Monoid<Pair<A, B>> 时，幺半群定律让我们得出结论，“融合”的幺半群运算也是结合的。我们不需要知道 A 和 B 的任何信息就能得出这一结论。
- 在具体层面，我们在编写从像 Functor 这样的抽象接口的函数派生的各种组合子时经常依赖定律。我们将在本节后面看到这些例子。

对于 Functor，我们将规定我们在第 7 章为我们的 Par 数据类型首次引入的熟悉定律。该定律规定了 map 组合子与恒等函数之间的关系如下：

### 列表 11.2 显示 map 与恒等函数之间关系的函子定律

```kotlin
map(x) { a -> a } == x
```

换句话说，用恒等函数对结构 x 进行映射本身应该是恒等的。这个定律看起来相当自然，随着我们超越 Par，我们注意到像 Gen 和 Parser 这样的其他类型的 map 函数也满足这一定律。该定律捕捉到 map(x) “保持 x 的结构” 的要求。满足该定律的实现被限制做奇怪的事情，比如抛出异常、删除 List 的第一个元素、将 Some 转换为 None 等等。只有结构的元素被 map 修改；形状或结构本身保持不变。请注意，该定律适用于 List、Option、Par、Gen 以及大多数定义 map 的其他数据类型！

为了给出这种结构保持的一个具体例子，我们可以考虑前面定义的 distribute 和 codistribute。以下是它们的签名供参考：

```kotlin
fun <A, B> distribute(fab: Kind<F, Pair<A, B>>): Pair<Kind<F, A>, Kind<F, B>>

fun <A, B> codistribute(e: Either<Kind<F, A>, Kind<F, B>>): Kind<F, Either<A, B>>
```

由于我们对 F 的了解仅限于它是函子，定律保证返回的值将与参数具有相同的形状。如果 distribute 的输入是成对的列表，返回的列表对将与输入长度相同，并且对应元素将以相同的顺序出现。这种代数推理可能为我们节省大量工作，因为依赖于该定律意味着我们不必为这些属性编写单独的测试。

## 11.2 Monad：泛化 flatMap 和 unit 函数

现在我们对 Functor 有了更多的了解以及如何应用它，我们发现与 Monoid 一样，Functor 只是可以从我们的库中提取的众多抽象之一。但 Functor 并不是最引人注目的抽象，因为仅用 map 定义的有用操作并不多。

相反，让我们把注意力集中在名为 Monad 的更有趣的接口上，它在 Functor 的功能上有所增加。使用该接口，我们可以实现比仅使用函子更多的操作，同时提取否则会重复的代码。它还附带定律，使我们能够推理我们的库如何按预期行为。

回想一下，对于本书中的几种数据类型，我们已经实现了 map2 以“提升”一个接受两个参数的函数。对于 Gen、Parser 和 Option，map2 函数可以按如下方式实现。

### 列表 11.3 Gen、Parser 和 Option 的 map2 实现

```kotlin
fun <A, B, C> map2(
    fa: Gen<A>,
    fb: Gen<B>,
    f: (A, B) -> C
): Gen<C> = 
    flatMap(fa) { a -> map(fb) { b -> f(a, b) } }

fun <A, B, C> map2(
    fa: Parser<A>,
    fb: Parser<B>,
    f: (A, B) -> C
): Parser<C> = 
    flatMap(fa) { a -> map(fb) { b -> f(a, b) } }

fun <A, B, C> map2(
    fa: Option<A>,
    fb: Option<B>,
    f: (A, B) -> C
): Option<C> = 
    flatMap(fa) { a -> map(fb) { b -> f(a, b) } }
```

生成一个随机 C 的生成器，运行随机生成器 fa 和 fb，并用函数 f 组合它们的结果。
