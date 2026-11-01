# 纯函数式并行

![](img/255e7d4fd2ecc6516db2405b06be1018_118_0.png)

### 本章涵盖：

- 通过探索性设计设计纯函数式库
- 选择适当的数据类型和函数来建模领域
- 从代数的角度推理 API 以发现类型
- 定义规范 API 行为的定律
- 泛化组合子使其广泛适用
- 将纯函数式并行作为库设计的实践示例

因为现代计算机每个 CPU 都有多个核心，而且通常有多个 CPU，因此以能够利用这种并行处理能力的方式设计程序比以往任何时候都更重要。但并行进程的交互很复杂，而执行线程之间通信的传统机制——共享可变内存—— notoriously 难以推理。这很容易导致程序出现竞争条件和死锁，很难测试，并且扩展性不佳。

在本章中，我们将构建一个用于创建并行和异步计算的纯函数式库。我们将通过仅使用纯函数来描述它们，从而控制并行程序固有的复杂性。这将使我们能够使用替换模型来简化我们的推理，并希望使处理并发计算既简单又愉快。

从本章中你应该学到的不仅是如何为纯函数式并行编写库，还应学习如何处理设计纯函数式库的问题。我们的主要关注点是使我们的库高度可组合和模块化。为此，我们将继续坚持我们分离描述计算的关注点与实际运行它的主题。我们希望允许我们的库的用户在非常高的层面上编写程序，使他们免受程序将如何执行的细节困扰。例如，在本章末尾我们将开发一个组合子，parMap，它将让我们轻松地将函数 f 同时应用于集合中的每个元素，看起来像这样：

```
val outputList = parMap(inputList, f)
```

为了达到这个目标，我们将迭代工作。我们将从一个我们希望库处理的简单用例开始，然后开发一个促进该用例的接口。只有在那之后，我们才会考虑该接口的实现应该是什么。随着我们不断完善设计，我们将在接口和实现之间来回切换，因为我们通过逐步更复杂的用例更好地理解领域和设计空间。我们将引入代数推理并证明 API 可以由遵循特定定律的代数来描述。

为什么要设计自己的库？为什么不直接使用 Kotlin 标准库通过协程提供的并发？我们设计自己的库有两个原因：第一个是为了教学目的，演示设计自己的库是多么容易。第二个原因是我们希望鼓励这样一种观点：没有任何现有库是权威的或超出重新审视的，即使是由专家设计并标记为“标准”。跟随大家做某事有一定安全感，但常规不一定是最实用的。大多数库包含大量任意的设计选择，其中许多是无意中做出的。从零开始，你可以重新审视进入库设计的所有基本假设，采取不同的路径，并发现他人可能从未考虑过的问题空间中的事物。因此，你可能会得到更适合自己目的的设计。在这种特殊情况下，我们的基本假设是我们的库绝对不允许副作用。

在本章中我们将编写大量代码，部分作为给读者的练习。如往常一样，你可以在本书末尾的附录 B 和 C 中找到提示和答案。

## 7.1 选择数据类型和函数

当你开始设计一个函数式库时，你通常有一些关于你想要能够做什么的一般想法，而设计过程中的困难在于细化这些想法并找到能够实现你想要的功能的数据类型。在我们的例子中，我们希望能够“创建并行计算”，但这到底意味着什么？让我们通过检查一个简单的可并行化计算——对整数列表求和，来尝试将其细化为可以实现的东西。这种情况的通常左折叠如下：

```
fun sum(ints: List<Int>): Int =
    ints.foldLeft(0) { a, b -> a + b }
```

> **注意** 为了易用性，我们没有使用第 3 章中自己的 `List` 实现，而是选择了 Kotlin 标准库提供的 `List`。此列表实现公开只读接口，尽管底层实现是可变的。我们愿意做出这种妥协，因为我们不会使用任何高级列表功能，并且作为库作者希望保持依赖图尽可能小。

而不是顺序折叠，我们可以使用像列表 7.1 中类似分而治之的算法。

### 列表 7.1 使用分而治之方法对列表求和

```
```kotlin
fun sum(ints: List<Int>): Int =
    if (ints.size <= 1)
        ints.firstOption().getOrElse { 0 } ①
    else {
        val (l, r) = ints.splitAt(ints.size / 2) ②
        sum(l) + sum(r) ③
    }
```
```

- 1. 处理 1 个或 0 个整数的情况，使用 Arrow 扩展方法 `firstOption`，类似于第 3 章中的 `headOption`。
- 2. 使用辅助扩展方法 `splitAt` 将列表拆分为两个并解构
- 3. 对 `l` 和 `r` 递归调用 `sum` 并将它们求和

我们使用 `splitAt` 函数将序列分成两半，递归求和两半，然后合并它们的结果。与基于 `foldLeft` 的实现不同，此实现可以并行化，两半可以并行求和。

> **侧边栏 简单示例的重要性**
> 
> 实际上，对整数求和非常快，以至于并行化带来的开销比节省的更多。但像这样简单的示例正是设计函数式库时最有帮助的考虑对象。
> 
> 复杂的示例包含各种偶然细节，可能使初始设计过程感到困惑。我们试图解释问题域的本质，一种好的做法是从平凡的示例开始，提取这些示例中的共同关注点，并逐步增加复杂性。
> 
> 在函数式设计中，我们的目标是实现表达力而没有众多特殊情况，而是构建一个简单且可组合的核心数据类型和函数集合。

当我们思考什么样的类型和函数可以实现此计算的并行化时，我们开始转变视角。 rather than focusing on how this parallelism will be implemented and forcing ourselves to work with the underlying APIs directly, we turn the tables. Instead we’ll design our own shiny new API as illuminated by our examples, working backward from there to our own implementation that uses underlying libraries such as java.concurrent to do the heavy lifting.

### 7.1.1 并行计算的数据类型

让我们更仔细地看看列表 7.1 中的行 sum(l) + sum(r)，它递归地在两半上调用 sum。我们立即看到，我们可能选择来表示并行计算的任何数据类型都需要能够包含一个结果。此结果将具有某种有意义的类型（在本例中为 Int）。我们还需要某种提取此结果的方法。让我们将这一新发现的知识应用到我们的设计中。现在，我们可以只为结果发明一个容器类型，称为 Par<A>（代表 parallel），并规定我们需要的函数存在：

### 列表 7.2 定义新的并行数据类型

```
class Par<A>(val get: A)

fun <A> unit(a: () -> A): Par<A> = Par(a())

fun <A> get(a: Par<A>): A = a.get
```

- ① 一个新的数据类型来包含结果
- ② 从未评估的 A 创建并行单元
- ③ 提取 A 的已评估结果

我们真的能做到这样的事情吗？当然可以！现在，我们不需要担心我们还需要什么其他函数，Par 的内部表示可能是什么，或者这些函数是如何实现的。我们只是凭空创造出满足我们简单示例需求所需的数据类型及其相关函数。让我们现在重新审视列表 7.1 中的示例。

### 列表 7.3 使用我们新的数据类型同化并行性

```
fun sum(ints: List<Int>): Int =
    if (ints.size <= 1)
        ints.firstOption().getOrElse { 0 }
    else {
        val (l, r) = ints.splitAt(ints.size / 2)
        val sumL: Par<Int> = unit { sum(l) }
        val sumR: Par<Int> = unit { sum(r) }
        sumL.get + sumR.get
    }
```

- ① 在 Par 上下文中计算列表的左侧
- ② 在 Par 上下文中计算列表的右侧
- ③ 从任一 Par 中提取 Int 结果并将它们求和

我们现在已将新的 Par 数据类型添加到混合中。我们使用 unit 工厂方法将对 sum 的两次递归调用都包装在 Par 中，而 unit 反过来负责评估对 sum 的所有调用。



接下来我们从它们的 Par 中提取两个结果以便求和。

我们现在对 unit 和 get 的含义有一个选择——unit 可能立即在单独的逻辑线程中开始评估其参数，34 或者它可以简单地将参数的评估推迟到 get 被调用时。但请注意，在列表 7.3 中，如果我们想要获得任何程度的并行性，就要求 unit 并发地开始评估其参数并立即返回而不阻塞。你能看出原因吗？Kotlin 中的函数参数是严格从左到右求值的，因此如果 unit 将执行推迟到 get 被调用时，我们会启动并行计算并等待其完成，然后再启动第二个并行计算。这意味着计算实际上是顺序的！

但是如果 unit 开始并发地评估其参数，那么调用 get 就负责破坏引用透明性。我们可以通过用它们的定义替换 sumL 和 sumR 来看到这一点——如果这样做，我们仍然得到相同的结果，但我们的程序不再是并行的，如下所示：

```
unit { l }.get + unit { r }.get
```

如果 unit 立即开始评估其参数，接下来的事情是 get 将等待该评估完成。因此，如果我们简单地内联 sumL 和 sumR 变量，+ 号的两侧将不会并行运行。我们可以看到 unit 有一个确定的副作用，但只有在与 get 结合时。我们之所以这么说是因为 unit 仅仅表示一个异步计算 Par<Int>，当我们调用 get 时它会阻塞执行，这反过来暴露了副作用。因此我们应该避免调用 get，或者至少推迟到最后再调用。我们希望在不等待它们完成的情况下组合异步计算。

在继续之前，让我们反思一下我们做了什么。首先，我们构想了一个简单、几乎微不足道的例子。接下来，我们探索这个例子以揭示一个设计选择。然后，通过一些实验，我们发现了一个选项的有趣后果，并在过程中学到了关于我们问题域本质的一些基本知识！整体设计过程是一系列的小冒险。你不需要特殊许可来进行这样的探索，当然你也不需要是函数式编程专家。只要深入进去看看能发现什么！

### 7.1.2 组合并行计算以确保并发

让我们看看是否可以避免前面提到的组合 unit 和 get 的陷阱。如果我们不调用 get，这意味着我们的 sum 函数必须返回一个 Par<Int>。这种改变揭示了什么后果？再次，让我们随意发明一个函数，比如 map2，具有所需的签名：

```
fun sum(ints: List<Int>): Par<Int> =
    if (ints.size <= 1)
        unit { ints.firstOption().getOrElse { 0 } }
    else {
        val (l, r) = ints.splitAt(ints.size / 2)
        map2(sum(l), sum(r)) { lx: Int, rx: Int -> lx + rx }
    }
```

### 练习 7.1

高阶函数 `map2` 是一个用于组合两个并行计算结果的新函数。它的签名是什么？给出可能的最通用签名（不要假设它仅适用于 `Int`）。

注意，我们在递归情况下不再调用 `unit`，而且 `unit` 是否应该再懒惰地接受其参数也不清楚。在这个例子中，懒惰地接受参数似乎没有任何好处，但也许并非总是如此。让我们稍后再回头讨论这个问题。

那 `map2` 呢——它应该懒惰地接受其参数吗？让 `map2` 并行运行计算的两侧，给每侧平等的运行机会是否有意义？`map2` 参数的顺序似乎是任意的，因为我们只是希望它表明正在组合的两个计算是独立的并且可以并行运行。什么选择能让我们实现这种含义？作为一个简单示例，考虑在评估 `sum(listOf(1, 2, 3, 4))` 时如果 `map2` 在两个参数上都是严格的会发生什么。花点时间研究并理解以下（有些程式化）的程序跟踪。

### 列表 7.4 两个参数的严格求值导致左侧先被求值

```
sum(listOf(1, 2, 3, 4)) ①

map2(
    sum(listOf(1, 2)),
    sum(listOf(3, 4))
) { i: Int, j: Int -> i + j } ①

map2(
    map2(
        sum(listOf(1)),
        sum(listOf(2))
    ) { i: Int, j: Int -> i + j }, ③
    sum(listOf(3, 4))
) { i: Int, j: Int -> i + j }

map2(
    map2(
        unit { 1 },
        unit { 2 }
    ) { i: Int, j: Int -> i + j }, ④
    sum(listOf(3, 4))
) { i: Int, j: Int -> i + j }

map2(
    map2(
        unit { 1 },
        unit { 2 }
    ) { i: Int, j: Int -> i + j },
    map2(
        sum(listOf(3)),
        sum(listOf(4))
    ) { i: Int, j: Int -> i + j } ⑤
) { i: Int, j: Int -> i + j }
```

- ① 未求值的表达式
- ② 用 map2 的定义替换 sum
- ③ 用 map2 的定义替换左参数
- ④ 用其结果替换左侧 sum 表达式
- ⑤ 用其定义替换右侧

要在列表 7.4 中求值 sum(x)，我们将 x 代入 sum 的定义，就像我们在前几章中所做的那样。因为 map2 是严格的，并且 Kotlin 从左到右求值参数，每当我们遇到 map2(sum(x), sum(y)) { i, j -> i + j } 时，我们就必须求值 sum(x) 依此类推。这导致了一个不幸的后果：我们会先严格构造求和树的整个左半部分，然后再（严格地）构造右半部分。在这里，sum(listOf(1,2)) 在我们考虑 sum(listOf(3,4)) 之前就完全展开了。而且如果 map2 并行求值其参数（使用实现并行性的任何资源，如线程池），这意味着我们的计算的左半部分将在我们甚至开始构造计算的右半部分之前就开始执行。

如果我们保持 map2 严格，但不让它立即开始执行呢？这有帮助吗？如果

map2 不立即开始求值，这意味着 Par 值仅仅是在构造并行需要计算的内容的描述。直到我们求值该描述（也许通过使用类似 get 的函数）之前，实际上什么也不会发生。问题是，如果我们严格构造描述，它们将是相当重量级的对象。查看列表 7.5 中的跟踪，我们的描述必须包含要执行的完整操作树。

### 列表 7.5 描述的严格构造导致产生完整的操作树

```
map2(
    map2(
        unit { 1 },
        unit { 2 }) { i: Int, j: Int -> i + j },
    map2(
        unit { 3 },
        unit { 4 }) { i: Int, j: Int -> i + j }
) { i: Int, j: Int -> i + j }
```

无论我们使用什么数据结构来存储此描述，它很可能占用的空间比原始列表本身还要多！如果我们的描述更轻量就好了。看起来我们还应该使 map2 懒惰并立即并行开始执行两边。这也解决了给任一侧优先权的问题。

### 7.1.3 显式标记要分叉的计算

我们最新的选择仍然感觉有些不对劲。是否总是希望并行求值 map2 的两个参数？可能不是。考虑这个简单的假设示例：

```
map2(
    unit { 1 },
    unit { 2 }
) { i: Int, j: Int -> i + j }
```

在这种情况下，我们恰好知道我们正在组合的两个计算执行的速度会如此之快，以至于为它们生成新的逻辑线程来求值毫无意义。但我们的 API 没有提供提供此类信息的方法。也就是说，我们当前的 API 关于计算何时从主线程分叉到新线程进程非常不明确——程序员无法指定分叉应该发生的位置。如果我们使分叉更明确会怎样？我们可以通过发明另一个函数来实现这一点，我们可以将其理解为给定的 Par 应该在单独的逻辑线程中运行：

```
fun <A> fork(a: () -> Par<A>) : Par<A> = TODO()
```

将此函数应用于我们正在运行的 sum 示例，我们可以如下表达：

```
fun sum(ints: List<Int>): Par<Int> =
    if (ints.size <= 1)
        unit { ints.firstOption().getOrElse { 0 } }
    else {
        val (l, r) = ints.splitAt(ints.size / 2)
        map2(

```
fork { sum(l) },
    fork { sum(r) }
) { lx: Int, rx: Int -> lx + rx }
}
```

借助 fork，我们现在可以让 map2 变得严格，将是否包装参数的决定权留给程序员。像 fork 这样的函数解决了并行计算实例化过于严格的问题，但更根本的是它将并行性明确置于程序员的控制之下。我们在这里处理两个问题。首先，我们需要某种方式来表示两个并行任务的结果应该被合并。除此之外，我们可以选择某个特定任务是否应异步执行。通过将这些关注点分开，我们避免了在 map2 和我们编写的其他组合子中附加任何全局并行策略，这意味着要做出艰难且最终武断的选择，关于哪种全局策略最好。

让我们现在回到 unit 应该是严格还是惰性的问题。有了 fork，我们现在可以在不损失表达能力的情况下让 unit 变得严格。其非严格版本，我们称之为 lazyUnit，可以使用 unit 和 fork 实现。

## Listing 7.6 严格的 unit 和 fork 可以组合形成 unit 的惰性变体。

```
fun <A> unit(a: A): Par<A> = Par(a)

fun <A> lazyUnit(a: () -> A): Par<A> =
    fork { unit(a()) }
```

函数 lazyUnit 是派生组合子的一个简单示例，而 unit 之类的则是原始组合子。我们只用其他操作定义了 lazyUnit。稍后，当我们为 Par 选择表示时，lazyUnit 不需要知道有关该表示的任何信息——它对 Par 的唯一了解是通过定义在 Par 上的 fork 和 unit 操作。

我们知道我们希望 fork 表明其参数在单独的逻辑线程中被求值。但我们仍然有问题，即它是否应在被调用时立即开始这样做，还是保留其参数以便在稍后使用类似 get 的东西强制计算时在逻辑线程中求值。换句话说，求值应该是 fork 的责任还是 get 的责任？求值应该是急切还是惰性？当你不确定要为 API 中的某个函数指定什么含义时，你总可以继续设计过程——在某个后续阶段，不同含义选择的权衡可能会变得清晰。在这里我们使用一个有用的技巧——我们将思考实现 fork 和 get 在各种含义下所需的信息类型。

如果 fork 开始立即并行求值其参数，实现必须明确知道一些东西，无论是直接还是间接，关于如何创建线程或将任务提交到某种线程池。这意味着用于实现并行的线程池或任何资源必须在我们想要调用 fork 的任何地方全局可访问并正确初始化。这意味着我们失去了控制程序不同部分使用的并行策略的能力。尽管拥有用于执行并行任务的全局资源并没有什么问题，但我们可以想象在更细粒度的控制下使用什么实现以及在什么上下文中使用会很有用。例如，我们可能希望大型应用程序的每个子系统都有自己的线程池并具有不同的参数。在考虑这一点时，将创建线程和提交执行任务的责任交给 get 似乎更合适。

请注意，得出这些结论不需要确切知道 fork 和 get 将如何实现，甚至不需要知道 Par 的表示是什么。我们只是非正式地推理了实际生成并行任务所需的类型信息，然后检查 Par 值知道这些信息的影响。

如果 fork 只是保留其未求值的参数直到以后，它不需要访问实现并行的机制。它只是接受一个未求值的 Par 并“标记”它以进行并发求值。在此模型下，Par 本身不需要知道如何实际实现并行。它更像是对并行计算的描述，该描述在稍后由类似 get 的函数解释。这是从之前的一个转变，在那里我们将 Par 视为一个值容器，我们可以在它可用时简单地获取。现在它更像是一个可以运行的一等程序。记住这一新发现，让我们将 get 函数重命名为 run，并规定这就是并行实际实现的地方。

```
fun <A> run(a: Par<A>): A = TODO()
```

因为 Par 现在只是一个纯数据结构，run 必须有某种手段来实现并行，无论是生成新线程、将任务委托给线程池，还是使用其他机制。

## 7.2 选择表示

仅通过探索这个简单示例并思考不同选择的后果，我们已经得出了以下 API。

```
fun <A> unit(a: A): Par<A> ①

fun <A, B, C> map2(
    a: Par<A>,
    b: Par<B>,
    f: (A, B) -> C
): Par<C> ①

fun <A> fork(a: () -> Par<A>): Par<A> ②

fun <A> lazyUnit(a: () -> A): Par<A> ③

fun <A> run(a: Par<A>): A ⑤
```

- 1. 创建一个立即产生值 a 的计算
- 2. 使用二元函数合并两个并行计算的结果。
- 3. 通过 `run` 标记计算以进行并发求值。
- 4. 将表达式 a 包装以供 `run` 并发求值。
- 5. 完全求值给定的 `Par`，生成计算并提取值。

## EXERCISE 7.2

在演进 API 的任何时候，你都可以开始思考出现的抽象类型的可能 *表示*。尝试为 `Par` 想出一种表示，使其能够实现我们 API 的函数。

让我们一起看看是否能想出一个表示。我们知道 `run` 以某种方式需要执行异步任务。我们可以编写自己的低级 API，但 Java 标准库中已经有一个类可以使用，`java.util.concurrent.ExecutorService`。以下是该 API 的大致样子，用 Kotlin 重新表述。

## Listing 7.7 用 Kotlin 表示的 Executor API。

```
```kotlin
interface Callable<A> {
    fun call(): A
}

interface Future<A> {
    fun get(): A
    fun get(timeout: Long, timeUnit: TimeUnit): A
    fun cancel(evenIfRunning: Boolean): Boolean
    fun isDone(): Boolean
    fun isCancelled(): Boolean
}

interface ExecutorService {
    fun <A> submit(c: Callable<A>): Future<A>
}
```
```

`ExecutorService` 允许我们提交一个 `Callable`，这相当于 Kotlin 中的惰性参数或 thunk。调用 `submit` 的结果将是一个 `Future`，它是可能在新线程中运行的计算的句柄。我们可以通过调用其阻塞的 `get` 方法之一来查询 `Future` 的结果。它还提供了一些用于取消和查询当前状态的额外方法。

## 侧边栏

## 直接使用并发原语的问题

那么 `java.lang.Thread` 和 `Runnable` 呢？让我们看看这些类。以下是它们 API 的部分摘录，用 Kotlin 重新表述：

```
```kotlin
interface Runnable {
    fun run(): Unit
}

class Thread(r: Runnable) {
    fun start(): Unit = TODO()
    fun join(): Unit = TODO()
}
```
```

- ① 在单独的线程中开始运行 r。
- ② 阻塞调用线程，直到 r 完成运行。

我们已经可以看到这两个类型的共同问题——没有任何方法返回有意义的值。因此，如果我们想从 `Runnable` 中获取任何信息，它必须产生副作用，比如修改我们可以检查的状态。这对组合性很糟——我们无法通用地操作 `Runnable` 对象，因为我们总是需要知道它们内部行为的某些东西。`Thread` 也有缺点，它直接映射到操作系统线程，而操作系统线程是稀缺资源。最好创建尽可能多的“逻辑线程”以符合我们的问题的自然情况，然后再处理将其映射到实际 OS 线程的问题。

你可以通过使用类似 `java.util.concurrent.Future`、`ExecutorService` 和朋友的东西来处理这类事情。为什么我们不直接使用它们呢？以下是它们 API 的重新表述部分：

```
```kotlin
class ExecutorService {
    fun <A> submit(a: Callable<A>): Future<A> = TODO()
}

interface Future<A> {
    fun get(): A
}
```
```

尽管 `java.util.concurrent` 在抽象物理线程方面提供了巨大帮助，但这些原语仍然处于比本章我们想要创建的库低得多的抽象级别。例如，对 `Future.get` 的调用会阻塞调用线程，直到 `ExecutorService` 完成执行，而其 API 提供了 no means of *组合* futures。当然，我们可以在这些工具之上构建我们库的实现（这也是我们在本章后面所做的），但它们并不提供我们希望直接从函数式程序中使用的模块化和组合式 API。



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

我们所定义的各种操作。理解哪些组合子才是真正原生的将在第 3 部分变得更加重要，在那里我们将展示如何在库之间抽象出常见模式。

不将 parMap 实现为新的原语还有另一个很好的原因——正确实现它具有挑战性，特别是如果我们希望正确地尊重超时的话。通常情况下，原语组合子封装了一些相当棘手的逻辑，而重用它们意味着我们不必重复此逻辑。

让我们看看在现有组合子的基础上实现 parMap 能做到什么程度：

```kotlin
fun <A, B> parMap(
    ps: List<A>,
    f: (A) -> B
): Par<List<B>> {
    val fbs: List<Par<B>> = ps.map(asyncF(f))
    TODO()
}
```

记住，asyncF 通过 fork 一个并行计算来生成结果，将 (A) -> B 转换为 (A) -> Par<B>。因此我们可以相当轻松地 fork 出我们的 N 个并行计算，但我们需要某种方式来收集它们的结果。我们被困住了吗？好吧，仅从检查类型就可以看出，我们需要某种方式将 List<Par<B>> 转换为 parMap 返回类型所需的 Par<List<B>>。

## 练习 7.5（困难）

编写这个名为 sequence 的函数。不需要额外的原语。不要调用 run。

```kotlin
fun <A> sequence(ps: List<Par<A>>): Par<List<A>> = TODO()
```

一旦我们有了 sequence，我们就可以完成 parMap 的实现：

```kotlin
fun <A, B> parMap(
    ps: List<A>,
    f: (A) -> B
): Par<List<B>> = fork {
    val fbs: List<Par<B>> = ps.map(asyncF(f))
    sequence(fbs)
}
```

请注意，我们已将实现包装在对 fork 的调用中。有了这个实现，parMap 将立即返回，即使对于巨大的输入列表也是如此。当我们稍后调用 run 时，它将 fork 一个单一的异步计算，该计算本身会生成 N 个并行计算，然后等待这些计算完成，并将它们的结果收集到一个列表中。

## 练习 7.6

实现 parFilter，它并行过滤列表中的元素。

```kotlin
fun <A> parFilter(
    sa: List<A>,
    f: (A) -> Boolean
): Par<List<A>> = TODO()
```

你能想到其他有用的函数来编写吗？尝试编写一些自己的并行计算，看看哪些可以在没有额外原语的情况下表达。这里有一些可以尝试的想法：

- 我们在本章开头编写的并行求和函数是否有更通用的版本？尝试使用它并行找到 `List` 的最大值。
- 编写一个函数，接收段落列表（一个 `List<String>`）并返回所有段落中单词的总数，并行执行。将此函数尽可能泛化。
- 基于 `map2` 实现 `map3`、`map4` 和 `map5`。

## 7.4 用代数方程推理 API

正如上一节所示，我们通常只需写下我们想要的操作的类型签名，然后“跟随类型”到实现，就能取得很大进展。以这种方式工作时，我们几乎可以忘记具体的领域（例如，当我们用 map2 和 unit 实现 map 时），只需专注于对齐类型。这不是作弊；这是一种自然的推理风格，类似于我们在简化代数方程时所做的事情。我们将 API 视为代数³⁵，或一组抽象操作以及我们假设为真的定律或属性的集合，并简单地按照该代数指定的游戏规则进行形式符号操作。

到目前为止，我们一直以非正式的方式推理我们的 API。这没什么问题，但让我们退一步，形式化一些我们希望 API 遵守的定律。在不知不觉中，我们在脑海中建立了一个我们期望的属性或定律的模型。通过明确表达它们，我们可以突出在非正式推理时不会显现的设计选择。两个想到的定律是映射定律和 fork 定律，我们将在本节后面讨论。

### 7.4.1 映射定律

像任何设计选择一样，选择定律具有深远的后果——它限制了操作的含义，决定了可能的实现选择，并影响了哪些其他属性可以为真。让我们看一个我们会编造一些看似可行的定律的例子。如果我们在为库编写测试，这可能被用作测试用例。

```
map(unit(1)) { it + 1 } == unit(2)
```

我们说的是，使用 { it + 1 } 函数对 unit(1) 进行映射在某种意义上等价于 unit(2)。定律通常以这种方式开始，作为我们期望成立的恒等式的具体例子。这里我们指的是数学意义上的恒等，即两个表达式相同或等价的陈述。它们在什么意义上是等价的？鉴于 Par 是一个简单的 (ExecutorService) -> Future 函数，这是一个有趣的问题。现在，让我们说如果对于任何有效的 ExecutorService 参数，它们的 Future 结果具有相同的值，则两个 Par 对象是等价的。

定律和函数有很多共同点。正如我们可以泛化函数一样，我们也可以泛化定律。例如，前面的表达式可以这样泛化：

```
map(unit(x), f) == unit(f(x))
```

这里我们说这对于 x 和 f 的任何选择都应成立，而不仅仅是 1 和 { it + 1 } 函数。这对我们的实现施加了一些约束。我们的 unit 实现不能检查接收到的值并决定在输入为 1 时返回结果为 42 的并行计算——它只能传递接收到的内容。同样适用于我们的 ExecutorService——当我们向其提交 Callable 对象以供执行时，它不能对接收到的值做出任何假设或改变行为。更具体地说，该定律禁止在 map 和 unit 的实现中使用 downcasting 或 is 检查（通常归类为类型转换）。

正如我们努力用只做一件事的更简单函数来定义函数一样，我们也可以用各自断言一件事的更简单定律来定义定律。让我们看看是否可以进一步简化这个定律。我们说我们希望这个定律对 x 和 f 的任何选择都成立。如果我们用恒等函数替换 f，就会发生一些有趣的事情。恒等函数只是传递其值，可以定义为 fun <A> id(a: A): A = a。现在我们可以简化等式的两边，得到一个相当简单的新定律，就像在求解代数方程时可能做的那种同样的替换和简化。

### 列表 7.9 使用替换模型简化等式的两边

```
val x = 1
val y = unit(x)
val f = { a: Int -> a + 1 }
val id = { a: Int -> a }

map(unit(x), f) == unit(f(x)) ①
map(unit(x), id) == unit(id(x)) ②
map(unit(x), id) == unit(x) ③
map(y, id) == y ④
```

- ① 初始定律声明
- ② 用恒等函数 id 替换 f
- ③ 将 id(x) 简化为 x
- ④ 用等价的 y 替换 `unit(x)`

这很迷人！我们的简化定律只谈论 `map`，将 `unit` 的提及作为多余的细节留下。为了深入了解这个新定律的含义，让我们思考 `map` *不能* 做什么。它不能在将函数应用于结果之前抛出异常并使计算崩溃。你能看出为什么这违反了定律吗？它所能做的全部是*将函数 `f` 应用于 `y` 的结果*，进而当该函数为 `id` 时使 `y` 不受影响。我们说 `map` 必须*保持结构*，即它不会改变并行计算的结构，只改变计算“内部”的值。

### 7.4.2 fork 定律

这个特定定律对我们 `Par` 的实现并没有多大约束。你可能在不知不觉中就假设了这些属性。在 `map`、`unit`、`ExecutorService.submit` 的实现中存在任何特殊情况，或 `map` 随机抛出异常是奇怪的。让我们考虑一个更强的属性——`fork` 不应影响并行计算的结果：

```
fork { x } == x
```

这个声明似乎显而易见，并且*应该*对我们的实现为真。这显然是一个期望的属性，与我们对 `fork` 应如何工作的期望一致。`fork(x)` 应该与 `x` 做同样的事情，尽管是在与主线程分离的逻辑线程中异步执行。如果这个定律并不总是成立，我们就必须知道在何时调用是安全的，而不会改变其含义，并且没有类型系统的任何帮助。

令人惊讶的是，这个简单的属性对我们 `fork` 的实现施加了强约束。在你写下这样的定律后，摘下你的实现者帽子，戴上测试帽子，尝试打破你的定律。思考任何可能的边界情况，尝试提出反例，甚至构建一个非正式的证明来证明定律成立——至少足以说服一个怀疑的同行程序员。

## 违反定律：一个微妙的错误

让我们尝试这种思考模式：我们期望 `fork(x) == x` 对 *所有* `x` 的选择以及任何 `ExecutorService` 的选择都成立。我们对 `x` 可能是什么有很好的认识——它是使用 `fork`、`unit`、`map2` 或从这些派生的任何其他可能组合子的表达式。那么 `ExecutorService` 呢？有哪些实现可用？查看 `java.util.concurrent.Executors` 36 的 API 文档让我们对所有可能性有了很好的了解。



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

originate from here, it will release the latch and make the result available immediately.

### Listing 7.11 实现 `run` 方法以适应非阻塞 `Par`

```kotlin
fun <A> run(es: ExecutorService, pa: Par<A>): A {
    val ref = AtomicReference<A>()
    val latch = CountDownLatch(1)
    pa(es).invoke { a: A ->
        ref.set(a)
        latch.countDown()
    }
    latch.await()
    return ref.get()
}
```

- 1. 创建一个可变、线程安全的引用来存储结果。
- 2. `CountDownLatch` 会阻塞线程，直到倒计时达到 0。
- 3. 在接收到结果时设置结果并释放闩锁。
- 4. 等待结果可用且闩锁被释放。
- 5. 一旦我们通过了闩锁，就知道 `ref` 已被设置并返回其值。

在我们当前的实现中，`run` 会在等待 `latch` 被释放时阻塞调用线程。事实上，不可能编写一个不阻塞的 `run` 实现。我们的 方法必须等待 A 的值 materialize 之后才能返回任何内容。出于这个原因，我们希望 API 的使用者在确实想要等待结果时才调用 `run`。我们甚至可以进一步将 `run` 完全从 API 中移除，并公开 `Par` 上的 `invoke` 方法，以便使用者注册异步回调。那当然是一个有效的设计选择，但我们目前仍保持 API 不变。

如果 `latch` 只需要释放一次，上面的函数可以使用 `CompletableFuture` 简化。`CompletableFuture` 类是自 Java 8 起 JDK 中 `Future` 接口的非抽象实现。它使开发人员能够完全控制结果的可用性，同时包含 `ExecutorService` 方法返回的任何 `Future` 实现提供的所有线程阻塞管理。

### Listing 7.12 使用 `CompletableFuture` 实现 `run` 方法

```kotlin
fun <A> run2(es: ExecutorService, pa: Par<A>): A {
    val ref = CompletableFuture<A>()
    pa(es).invoke { a: A ->
        ref.complete(a)
    }
    return ref.get()
}
```

- 1. 创建一个 `CompletableFuture` 来管理当前线程的阻塞并存储结果。
- 2. 设置结果。这会解锁 `CompletableFuture` 并使其结果可用。

等待结果可用，然后返回其值。

从此片段可以看出，`CountDownLatch` 不再必要，因为线程阻塞由 `CompletableFuture` 管理。

我们已经在列表 7.10 中看到过 unit，但 fork 呢？这就是我们引入实际并行的地方：

### Listing 7.13 fork 函数分叉出一个任务来评估惰性参数

```kotlin
fun <A> fork(a: () -> Par<A>): Par<A> =
  { es: ExecutorService ->
      object : Future<A>() {
        override fun invoke(cb: (A) -> Unit) =
          eval(es) { a()(es).invoke(cb) }
      }
  }

fun eval(es: ExecutorService, r: () -> Unit) {
  es.submit(Callable { r() })
}
```

调用 eval 会分叉出 a 的求值并立即返回。

一个辅助函数，用于使用某个 ExecutorService 异步求值一个动作。

当 fork 返回的 Future 接收到其 continuation cb 时，它会分叉出一个任务来评估惰性参数 a。一旦参数被求值并调用以产生一个 Future<A>，我们就会注册 cb，以便在该 Future 具有其结果 A 时调用它。

让我们考虑 map2。回想一下此组合子的签名。

```
fun <A, B, C> map2(pa: Par<A>, pb: Par<B>, f: (A, B) -> C): Par<C>
```

这里，非阻塞实现要复杂得多。概念上，我们希望 map2 并行运行两个 Par 参数。当两个结果都到达时，我们希望调用 f，然后将结果 c 传递给 continuation。但这里有几个竞态条件需要担心，并且仅使用 java.util.concurrent 提供的低级原语就很难实现一个正确的非阻塞实现。

## 简要插曲演示 ACTORS 的使用

为了实现 map2，我们将使用一种称为 actors 的非阻塞并发原语。Actor 本质上是一个并发进程，它不会 constantly 占用一个线程。相反，它只有在接收到消息时才会占用一个线程。重要的是，虽然多个线程可能并发地向 actor 发送消息，但 actor 一次只处理一条消息，将其他消息排队以供后续处理。这使得它们在编写必须被多个线程访问且否则容易出现竞态条件或死锁的棘手代码时作为并发原语很有用。

最好用一个例子来说明这一点。许多 actor 的实现都足以满足我们的目的，但为了简单起见，我们将使用随章节代码一起提供的我们自己的最小 actor 实现，文件为 actor.kt。我们将通过一些客户端代码与之交互，以了解其工作原理。我们首先启动 actor。

### Listing 7.14 设置一个 actor 来处理客户端请求

```kotlin
val es: ExecutorService = Executors.newFixedThreadPool(4)
val s = Strategy.from(es)
val echoer = Actor<String>(s) {
    println("got message: $it")
}
```

- 创建一个 `ExecutorService` 实例 `es` 来支持我们的 actor
- 将 `es` 包装在名为 `s` 的 `Strategy` 中
- 使用 `Strategy` 启动一个 actor，并向其传递一个匿名处理函数。

现在我们有了由 echoer 引用的 actor 实例，我们可以发送一些消息。

```kotlin
echoer.send("hello")
//got message: hello

echoer.send("goodbye")
//got message: goodbye

echoer.send("You're just repeating everything I say, aren't you?")
//got message: You're just repeating everything I say, aren't you?
```

- 向 actor 发送 "hello" 消息。
- 生成的进程会调用处理程序，立即释放当前线程以处理下一个消息。
- 在未等待 "hello" 处理程序完成的情况下，向 Actor 发送新的 "goodbye" 消息。

理解 Actor 实现并非必需。正确且高效的实现相当微妙，但如果你好奇，请参阅章节代码中的 actor.kt 文件。实现只有不到 100 行普通的 Kotlin 代码。理解 actor 实现最困难的部分在于多个线程可能同时向 actor 发送消息。实现需要确保消息一次处理一条，并且发送给 actor 的所有消息最终都会被处理，而不是无限期地排队。即使如此，代码仍然简洁。

## 通过 ACTORS 实现 MAP2

我们现在可以使用 Actor 来收集两个参数的结果来实现 map2。代码相当直接，并且无需担心竞态条件，因为我们知道 Actor 将一次只处理一条消息。

```kotlin
fun <A, B, C> map2(pa: Par<A>, pb: Par<B>, f: (A, B) -> C): Par<C> =
  { es: ExecutorService ->
    object : Future<C>() {
      override fun invoke(cb: (C) -> Unit) {
        val ar = AtomicReference<Option<A>>(None)
        val br = AtomicReference<Option<B>>(None)
        val combiner =
          Actor<Either<A, B>>(Strategy.from(es)) { eab ->
            when (eab) {
              is Left<A> ->
                br.get().fold(
                  { ar.set(Some(eab.a)) },
                  { b -> eval(es) { cb(f(eab.a, b)) } }
                )
              is Right<B> ->
                ar.get().fold(
                  { br.set(Some(eab.b)) },
                  { a -> eval(es) { cb(f(a, eab.b)) } }
                )
            }
          }
        pa(es).invoke { a: A -> combiner.send(Left(a)) }
        pb(es).invoke { b: B -> combiner.send(Right(b)) }
      }
    }
  }
```

- 1. 使用两个 `AtomicReference` 实例来存储可变结果
- 2. 一个 actor 等待两个结果，使用 `f` 将它们组合，并将结果传递给 `cb`
- 3. 接收到 `Left(a)` 时执行的分支，如果 `br` 中之前已设置 `Right(b)` 则进行组合。
- 4. 接收到 `Right(b)` 时执行的分支，如果 `ar` 中之前已设置 `Left(a)` 则进行组合。
- 5. 将 actor 作为 continuation 传递给两侧。

我们在 `combiner` actor 中有四个需要处理的可能场景。让我们逐一查看：

- 如果 A 结果先到达，则将其存储在 ar 中，actor 现在等待 B 到达。
- 如果 A 结果最后到达且 B 已存在，则结果 a 和 b 由 f 组合为类型 C 并传递给回调 cb。
- 如果 B 结果先到达，则将其存储在 br 中，actor 现在等待 A 到达。
- 如果 B 结果最后到达且 A 已存在，则结果 a 和 b 由 f 组合为类型 C 并传递给回调 cb。

然后 actor 被作为 continuation 传递给两侧。它在是 A 的情况下被包装为 `Left`，在是 B 时为 `Right`。我们使用 `Either` 数据类型，为此联合的每一侧调用 `Left(a)` 和 `Right(b)` 构造函数。它们用于指示结果的来源给 actor。

鉴于这些实现，我们现在应该能够运行任意复杂度的 `Par` 值，而无需担心用完线程，即使 actor 只有单个 JVM 线程可用。



现在我们可以编写一些客户端代码来尝试这个花哨的新机制。

```
val p: (ExecutorService) -> Future<List<Double>> =
    parMap((1..10).toList()) { sqrt(it.toDouble()) }

val x: List<Double> =
    run(Executors.newFixedThreadPool(2), p)

println(x)
```

运行这段代码将产生如下结果：

```
[1.0, 1.4142135623730951, 1.7320508075688772, 2.0, 2.23606797749979,
2.449489742783178, 2.6457513110645907, 2.8284271247461903, 3.0,
3.1622776601683795...]
```

这将大约调用 `fork` 100,000 次，启动相同数量的 actor 来两两组合这些值。得益于我们的非阻塞 `Actor` 实现，我们不需要 100,000 个 JVM 线程来执行此处理，但我们仅用固定线程池大小为 2 就实现了！

因此我们证明了我们的 fork 定律现在对固定大小的线程池也成立。

### 练习 7.9 (Hard/Optional)

我们的非阻塞表示目前完全不处理错误。如果在任何时候我们的计算抛出异常，`run` 实现的 `latch` 从未递减，并且异常被简单地吞没。你能修复它吗？

退一步看，本节的目的不一定是弄清楚 `fork` 的最佳非阻塞实现，而是更多地展示定律的重要性。它们为我们思考库的设计提供了另一个角度。如果我们没有尝试写出 API 的一些定律，我们可能要到很晚才会在第一个实现中发现线程资源泄漏。

总的来说，在为你的 API 选择定律时，你可以考虑多种方法。你可以思考你的概念模型，并从中推理出应该成立的定律。你也可以直接 *发明* 你认为可能有用或具有启发性的定律（如我们对 `fork` 定律所做的），并看看是否可能并且合理地确保它们对你的模型成立。最后，你可以查看你的 *实现* 并根据它推导出你期望成立的定律。³⁷

## 7.5 将组合子细化为最一般的形式

函数式设计是一个迭代过程。在你编写 API 并至少有一个原型实现后，尝试将其用于逐步更复杂或更现实的场景。有时你会发现这些场景需要新的组合子。但在直接实现新组合子之前，最好先看看是否可以将所需的组合子细化为最一般的形式。也许你需要的只是某个更一般组合子的特定情况。

> NOTE 为了简便起见，我们将恢复使用我们原始的、更简单的阻塞表示 Par<A>，而不是更新的基于非阻塞 actor 的解决方案。随意尝试本节的练习使用 Par<A> 的非阻塞变体。

让我们看一个这个泛化的例子。假设我们想要一个函数，根据初始计算的结果在两个 fork 计算之间进行选择：

```
fun <A> choice(cond: Par<Boolean>, t: Par<A>, f: Par<A>): Par<A>
```

这构造了一个计算，如果 cond 的结果为 true 则继续执行 t，否则执行 f。我们当然可以通过阻塞 cond 的结果，然后使用该结果来确定运行 t 还是 f 来实现它。这里是一个简单的阻塞实现。

```
fun <A> choice(cond: Par<Boolean>, t: Par<A>, f: Par<A>): Par<A> =
{ es: ExecutorService ->
    when (run(es, cond).get()) {
        true -> run(es, t)
        false -> run(es, f)
    }
}
```

① 在继续之前阻塞谓词 Par<Boolean>

但在我们满意并继续之前，让我们再进一步思考这个组合子。它在做什么？它运行 cond，然后在结果可用时运行 t 或 f。这似乎合理，但让我们思考一些能够捕捉到这个组合子本质的可能变体。在这个组合子中使用 Boolean 以及我们只在 t 和 f 两个可能的并行计算中进行选择，这有点随意。为什么只有两个？如果根据第一个结果在两个并行计算之间进行选择是有用的，那么在 N 个计算之间进行选择也一定是有用的：

```
fun <A> choiceN(n: Par<Int>, choices: List<Par<A>>): Par<A>
```

假设 choiceN 运行 n，然后使用它从 choices 中选择一个并行计算。这比 choice 更一般一些。

### 练习 7.10
实现 `choiceN`，然后用 `choiceN` 实现 `choice`。

让我们退一步观察我们在本次迭代中所做的工作。我们已将原始组合子 `choice` 泛化为 `choiceN`，现在它能够表达 `choice` 以及 `choice` 不支持的其他用例。让我们继续，看看是否可以将 `choice` 细化为更一般的组合子。

组合子 `choiceN` 仍然有些随意。`List` 的选择似乎过于具体。容器的类型重要吗？例如，如果我们不是有一个 `List`，而是有一个计算的 `Map` 呢？`Map<K, V>` 是一种将类型为 K 的键与类型为 V 的值关联起来的数据结构。K 到 V 的一对一关系允许我们通过关联的键查找值。

### 练习 7.11
实现一个名为 `choiceMap` 的组合子，它接受 `Map<K, Par<V>>` 作为容器。

```
fun <K, V> choiceMap(
    key: Par<K>,
    choices: Map<K, Par<V>>
): Par<V> = TODO()
```

### 提示
不要过于担心 `Map.get` 返回的 null 值处理。为了本练习的目的，请考虑使用 `Map.getValue` 进行值检索。

就像 `List` 一样，可能选择的集合的 `Map` 编码也感觉过于具体。如果我们查看 `choiceMap` 的实现，我们会发现我们实际上并没有使用 `Map` API 的太多部分。实际上，`Map<A, Par<B>>` 用于提供一个函数 `(A) -> Par<B>`。现在我们发现了这一点，回头看 `choice` 和 `choiceN`，我们可以看到对于 `choice`，参数对只是被用作类型为 `(Boolean) -> Par<A>` 的函数（其中 `Boolean` 选择两个 `Par<A>` 参数之一），而对于 `choiceN`，列表只是被用作类型为 `(Int) -> Par<A>` 的函数！

让我们创建一个更一般的签名来统一它们。我们称之为 `chooser`，并允许它通过函数 `(A) -> Par<B>` 进行选择。

### 练习 7.12

实现这个新的原语 `chooser`，然后用它来实现 `choice`、`choiceN` 和 `choiceMap`。

```kotlin
fun <A, B> chooser(pa: Par<A>, choices: (A) -> Par<B>): Par<B> = TODO()
```

每当你像这样泛化函数时，请批判性地审视你的最终产品。尽管函数可能由某些特定用例驱动，但签名和实现可能具有更一般的含义。在这种情况下，`chooser` 也许不再是最合适的名称，因为这个操作实际上相当通用——它是一个并行计算，在被调用时会运行一个初始计算，其结果用于确定第二个计算。没有什么说明第二个计算甚至需要在第一个计算的结果可用之前就存在。它甚至不需要存储在像 `List` 或 `Map` 这样的容器中。也许它正使用第一个计算的结果从头生成。这个函数在函数式库中经常出现，通常称为 `bind` 或 `flatMap`：

```kotlin
fun <A, B> flatMap(pa: Par<A>, f: (A) -> Par<B>): Par<B>
```

`flatMap` 真的是最原始的可能函数，还是我们可以进一步泛化它？让我们再玩弄一下。`flatMap` 这个名字暗示了这个操作可以分解为两个步骤：将 `f: (A) -> Par<B>` 映射到我们的 `Par[A]` 上，生成一个 `Par<Par<B>>`，然后将这个嵌套的 `Par<Par<B>>` 展平为 `Par<B>`。

这里有趣的部分是——它表明我们所需要做的只是添加一个更简单的组合子，我们称之为 `join`，用于将 `Par<Par<X>>` 转换为 `Par<X>`，适用于任何 X 的选择。

我们再次只是遵循类型。我们有一个例子需要具有给定签名的函数，因此我们就把它创造出来。现在它存在了，我们可以思考这个签名意味着什么。我们称之为 `join`，因为概念上它是一个并行计算，在运行时会执行内部计算，等待其完成（类似于 `Thread.join`），然后返回其结果。

### 练习 7.13

实现 `join`。你能看到如何使用 `join` 实现 `flatMap` 吗？并且你能使用 `flatMap` 实现 `join` 吗？

```kotlin
fun <A> join(a: Par<Par<A>>): Par<A> = TODO()
```

我们就到此为止，尽管鼓励你进一步探索这个代数。尝试更复杂的例子，发现新的组合子，看看你能找到什么！如果你愿意的话，

这里有一些需要考虑的问题：

- 你能否使用 `flatMap` 和 `unit` 实现一个与 `map2` 具有相同签名的函数？它的含义与 `map2` 有何不同？
- 你能想到将 `join` 与代数的其他原语关联起来的定律吗？
- 是否存在无法使用此代数表达的并行计算？你能想到任何即使添加新的原语也无法表达的计算吗？

## 侧边栏 识别代数的表达能力与局限性

在你练习更多函数式编程时，你将培养的一项技能是识别 *what functions are expressible from an algebra*，以及该代数的局限性是什么。例如，在前面的示例中，一开始可能并不明显，像 `choice` 这样的函数无法仅用 `map`、`map2` 和 `unit` 纯粹表达。也可能并不明显 `choice` 只是 `flatMap` 的一种特殊情况。随着时间的推移，这样的观察会更快到来，你也会更擅长发现如何修改代数以使所需的组合子变得可表达。这些技能将有助于你进行所有的 API 设计工作。

能够将 API 减少到最小的原语函数集合是一项极其有用的技能。原语组合子经常封装一些棘手的逻辑，重用它们意味着我们不必重复工作。

我们现在已经完成了用于以纯函数方式定义并行和异步计算的库的设计。虽然这个领域很有趣，但本章的主要目标是让你了解函数式设计的过程，对你可能遇到的各种问题有所了解，并提供处理这些问题的思路。

第 4 章到第 6 章不断提到 *separation of concerns* 的原则。具体来说，是将计算的 *description* 与运行它的 *interpreter* 分开的想法。在本章中我们看到了这一原则的实际应用——我们设计了一个库，将并行计算描述为数据类型 `Par` 的值，并使用一个名为 `run` 的独立解释器来生成线程执行它们。

## 7.6 Summary

- Functional API design is an iterative and exploratory process driven by real-world examples.
- A purely functional library that deals with parallelization is a perfect example to demonstrate API design.
- *Data types* and their associated *functions* are born out of exploring domain examples.
- Treating an API as you would an *algebraic equation* leads to following types to a concrete implementation.
- Laws help define constraints on operations, lead to implementation choices and validate properties.
- Combinators can often be generalized to broaden their application across many different applications and scenarios.
- Effective library design separates description of computations from the interpreter that will be responsible for running them.
