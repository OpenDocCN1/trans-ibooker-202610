# 基于属性的测试

### 本章涵盖：

- 理解基于属性测试的概念
- 通过发现适当的数据类型和函数来建模属性
- 使用生成器伪造测试数据
- 最小化测试用例结果以提供有意义的反馈
- 使用属性来确认定律
- 应用语法糖以改善用户体验

在第 7 章中，我们逐步完成了用于表达并行计算的函数式库的设计。我们介绍了 API 应该形成一个 *algebra* 的想法——即一组数据类型、这些数据类型上的函数，以及重要的是表达这些函数之间关系的 *laws* 或 *properties*。我们还暗示了可能以某种方式 *validate* 这些定律的想法。验证是一个重要步骤，因为我们需要知道我们编写的代码是否符合我们对程序施加的定律。如果能以某种方式自动化此验证过程将大有裨益。

本章将带我们进入一个简单但功能强大的自动化 *property-based testing* 库。这样的库的总体思想是将程序行为的规范与测试用例的创建解耦。程序员专注于指定程序的行为并给出测试用例的高级约束。然后框架自动生成满足这些约束的测试用例并运行测试以验证程序的行为是否如预期。

虽然测试库的目的与并行计算库大不相同，但令人惊讶的是，我们会发现它们都有非常相似的组合子。这种相似性将在第 3 部分再次讨论。

## 8.1 基于属性测试简览

基于属性的测试框架在许多不同的语言（如 Haskell、Scala，甚至 Kotlin）中的函数式程序员中已被广泛接受和使用。作为示例，让我们来看一下 KotlinTest，这是一个流行的 Kotlin 开发测试框架。它内置了基于属性的测试支持，其中一个属性看起来像这样。

### 列表 8.1 使用 KotlinTest 演示基于属性的测试

```
val intList = Gen.list(Gen.choose(0, 100)) ①

forAll(intList) { ②
    (it.reversed().reversed() == it) and ③
    (it.firstOption() == it.reversed().lastOrNone()) ④
}

forAll(intList) { ⑤
    it.reversed() == it
}
```

- ① 一个生成包含 0 到 100 之间整数的列表的生成器。
- ② 一个有效的属性，指定 List.reversed 方法的行为。
- ③ 检查将列表反转两次是否能得到原始列表。
- ④ 检查第一个元素在反转后是否变为最后一个元素。
- ⑤ 第二个属性在大多数情况下会失败

这里，intList 并不是你可能期望的 List<Int>，而是一个 Gen<List<Int>>，它知道如何生成 List<Int> 类型的测试数据。我们可以从此生成器采样以生成不同长度的列表，每个列表都填充了 0 到 100 之间的随机数。基于属性测试库中的生成器具有丰富的 API。我们可以以不同的方式组合和组合生成器，重用它们，等等。

函数 forAll 通过将类型为 Gen<A> 的生成器与类型为 (A) -> Boolean 的谓词组合来创建一个属性。该属性断言生成器产生的所有值都应满足此谓词。与生成器一样，属性也应具有丰富的 API。

尽管 KotlinTest 目前不支持此功能，但我们应该能够使用像 and 和 or 这样的运算符来组合多个属性。 resulting property 只有在没有任何生成的测试用例能够证伪这些属性中的任何一个时才成立。一起，这些组合的属性将形成要验证的正确行为的完整规范。

值得注意的是，这种测试的目标不一定是完全指定程序行为，而是为代码提供更大的信心。基于属性的测试不能替代单元测试，后者的目的更多在于表达意图和驱动设计，而非验证我们对代码的信心。

当我们表达这些属性时，KotlinTest 将随机生成 `List<Int>` 值以尝试找到证伪我们提供的谓词的情况。它生成 100 个测试用例（类型为 `List<Int>`），每个列表都会被检查是否满足谓词。属性当然可能会失败——第二个属性应该表明谓词对某些输入测试为假，然后将其打印到标准输出以便于进一步测试或调试。

## 练习 8.1

为了习惯这种方式思考测试，请提出指定 sum 实现的属性：`(List<Int>) -> Int` 函数。你不必将属性写成可执行的 KotlinTest 代码——非正式描述即可。以下是一些入门思路：

- 反转列表然后求和应与对原始未反转列表求和得到相同的结果。
- 如果列表的所有元素都是相同的值，求和应该是多少？
- 还有其他属性浮现出来吗？

## 练习 8.2

指定一个查找 `List<Int>` 最大值的函数的属性是什么？

基于属性的测试库通常配备其他有用的功能。我们稍后会谈论其中一些功能，但只是为了给你一个可能性的概念：

- *Test case minimization* — 在测试失败的情况下，框架尝试越来越小的 数据集大小，直到找到仍然失败的 *smallest* 数据集，这对于诊断失败更有启发性。例如，如果一个属性对大小为 10 的列表失败，框架会尝试更小的列表并报告导致测试失败的最小列表。
- *Exhaustive test case generation* — 我们称由某个 `Gen<A>` 可能产生的值的集合为 *domain*。³⁸ 当域足够小时（例如，如果它是小于 100 的所有偶数），我们可能会穷举测试其所有值而不是生成样本值。如果属性对域中的所有值都成立，我们就有了真正的 *proof* 而不仅仅是缺乏相反证据。

KotlinTest 只是提供基于属性测试能力的众多框架之一。虽然它没什么问题，但我们将在本章中从头开始推导自己的库。就像第 7 章一样，这主要是出于教学目的，但也部分是因为我们应该认为没有任何库是任何主题的最终答案。使用像 KotlinTest 这样的现有库绝对没有问题，现有库可以成为很好的灵感来源。但即使你决定喜欢现有库的解决方案，花一两个小时玩弄设计并写下一些类型签名也是了解更多领域知识并理解设计权衡的好方法。



## 8.2 选择数据类型和函数

本节将是又一个 somewhat messy 且迭代的过程，用于发现我们库的数据类型和函数。这次我们正在设计一个基于属性的测试库，以便验证我们程序的定律或属性。正如之前一样，这是一个机会，让我们窥探某人处理可能场景和设计时的思路。

我们所走的特定路径以及最终得到的库，不一定与你自己想出来的相同。如果你对基于属性的测试还不熟悉，那就更好了；这是一个机会，让你在探索新领域及其设计空间的同时，自己发现其中的奥妙。如果你在任何时候感到有灵感，或者对如何设计这样的库有自己的想法，不要等到练习提示你——放下书本去探索你的想法。你可以在用尽想法或遇到困难时随时回到本章。

### 8.2.1 收集可能 API 的初始片段

话虽如此，让我们开始吧。每当我们开始库设计时，我们需要定义一些体现我们库概念的数据类型。基于这个起点，我们的测试库应该使用什么数据类型？我们应该定义哪些原语，它们可能意味着什么？我们的函数应该满足什么定律？正如之前一样，我们可以看一个简单示例并“读出”所需的数据类型和函数，看看能发现什么。为了获得灵感，让我们看一下之前展示的 KotlinTest 示例：

```
val intList = Gen.list(Gen.choose(0, 100))

forAll(intList) {
    (it.reversed().reversed() == it) and
    (it.firstOption() == it.reversed().lastOrNone())
}
```

在不知道 Gen.choose 或 Gen.list 的实现的情况下，我们可以猜测它们返回的任何数据类型（我们称之为 Gen，generator 的缩写）必须在某种类型上是参数化的。也就是说，Gen.choose(0,100) 可能返回一个 Gen<Int>，而 Gen.list 然后是一个具有签名 (Gen<Int>) -> Gen<List<Int>> 的函数。但由于 Gen.list 似乎不应该关心它作为输入接收的 Gen 的类型。为 Int、Double、String 等分别创建列表而要求单独的组合子似乎很奇怪，所以让我们继续使其多态。我们将方法命名为 listOf 以明确其意图。

```
fun <A> listOf(a: Gen<A>): Gen<List<A>> = TODO()
```

通过查看这个签名我们可以学到很多东西。请注意，我们没有指定要生成的列表的大小。因此，要使其可实现，我们的生成器必须做出假设，或者以其他方式被明确告知大小。假设一个大小似乎有点不灵活，任何假设在所有上下文中都不太可能合适。因此，看起来生成器必须被告知要生成测试用例的大小。我们可以想象一个 API，其中这一点被明确化：

```
fun <A> listOfN(n: Int, a: Gen<A>): Gen<List<A>> = TODO()
```

这当然会是一个有用的组合子，但也不必显式指定大小也很强大。这意味着运行测试的任何函数都有自由选择测试用例大小的自由，这为我们之前提到的测试用例最小化提供了可能性。如果大小始终固定并由程序员指定，测试运行器将不会有这种灵活性。在我们进一步推进设计时，请记住这一点。

这个示例的其余部分呢？forAll 函数看起来很有趣。我们可以看到它接受一个 Gen<List<Int>> 以及看起来像是对应的谓词，(List<Int>) -> Boolean。但同样，forAll 似乎不应该关心生成器和谓词的类型，只要它们匹配即可。我们可以用以下类型来表达这一点：

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

在这里，我们简单地发明了一个新类型 Prop（property 的缩写），用于将 Gen 绑定到谓词的结果。我们可能不知道 Prop 的内部表示或它支持的其他函数，但根据我们在第 8.1 节之前的讨论，它应该通过 and 方法与其他 Prop 实例组合。让我们将其作为新的接口引入：

```
interface Prop {
    fun and(p: Prop): Prop
}
```

### 8.2.2 探索属性的含义和 API

现在我们有了一些 API 片段，让我们讨论一下我们希望我们的类型和函数包含什么。首先，让我们考虑 Prop。我们知道有函数 forAll（用于创建属性）、and（用于组合属性），现在将学习关于 check。在这里我们进一步偏离了 KotlinTest 的属性设计，因为其 API 中没有这样的方法。我们将想象这是一个运行我们属性并具有将输出打印到控制台的副作用的方法。我们将将其作为 Prop 上的便捷函数公开，目前将其返回类型设为 Unit。

```
interface Prop {
    fun check(): Unit
    fun and(p: Prop): Prop
}
```

这种返回类型确实带来了一个问题，即我们不能使用 and 运算符将多个已检查的 Prop 链接在一起。这可能会让你想起我们在第 7 章中研究使用 Thread 和 Runnable 实现并行时遇到的类似问题。

由于 check 有副作用，在这种情况下实现 and 的唯一选择就是在两个 Prop 实例上运行 check。因此如果 check 打印出测试报告，我们将得到两个，
每个都独立地打印失败和成功。这可能不是正确的结果。问题不在于 check 有副作用，而更一般地说，它通过返回 Unit 丢弃了信息。

为了使用像 and 这样的组合子组合 Prop 值，我们需要 check（或任何“运行”属性的函数）返回一些有意义的值。这种值应该是什么类型？好吧，让我们考虑一下我们期望从已检查的属性中获得什么样信息。至少，我们需要知道属性是否成功或失败，因此布尔返回值为第一步已经足够好。我们现在有足够的信息来继续实现 and 方法。

### 练习 8.3

假设以下表示，使用 check 实现 Prop 的 and 方法。

```
interface Prop {
    fun check(): Boolean
    fun and(p: Prop): Prop = TODO()
}
```

在这种表示中，Prop 不过是非严格的布尔值。Prop 可以很容易地定义通常的布尔函数（AND、OR、NOT、XOR 等）。但仅靠布尔值可能是不够的。如果属性失败，我们可能想知道首先有多少个测试成功。我们也可能对导致失败的参数感兴趣。如果属性成功，知道它运行了多少个测试会很有用。接下来，让我们通过返回 Either 来编码此信息以指示成功或失败：

```
typealias SuccessCount = Int

interface Prop {
    fun check(): Either<String, SuccessCount>
    fun and(p: Prop): Prop
}
```

目前，我们已将失败情况指定为 String，但左侧应该返回什么类型？我们不知道正在生成的测试用例的类型。我们是否应该为 Prop 添加类型参数并使其成为 Prop<A>，以便 check 可以返回 Either<A, SuccessCount>。在走得太远之前，让我们问问自己，我们是否真的关心导致属性失败的值的类型。我们并不真正关心。只有当我们要对失败进行进一步计算时，我们才会关心类型。

### 注意

我们更喜欢使用类型别名而不是像 String、Int 或 Double 这样的简单类型，因为我们可以为它们分配有意义的名称。这使得我们的代码对将与之交互的其他人来说更容易理解。

最有可能的是，我们最终只会将其打印到屏幕上，供运行测试的人检查。毕竟，这里的目标是找到错误并指示触发它们的测试用例，以便它们可以被修复。作为一般规则，我们不应该使用 String 来表示我们想要计算的数据。但对于我们只打算展示给人类的值，String 绝对是合适的。这表明我们可以采用以下表示形式来表示 Prop：

```
typealias SuccessCount = Int
typealias FailedCase = String

interface Prop {
    fun check(): Either<Pair<FailedCase, SuccessCount>, SuccessCount>
    fun and(p: Prop): Prop
}
```

在失败的情况下，check 返回 Left(Pair(s,n))，其中 s 是表示导致属性失败的值的某个 String，n 是在失败发生前成功的情况数。相反，成功将是 Right(n)，其中 n 表示成功的情况总数。

目前，这解决了 check 的返回值，但它的参数呢？现在，check 方法不接受任何参数。这足够吗？由于 check 是 Prop 的方法，我们可以思考在创建时有哪些信息可供它使用。特别是，让我们再看一下 forAll：

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

在不了解 Gen 的表示的情况下，很难说这里是否有足够的信息来生成类型 A 的值。为什么这很重要？我们需要这些信息来实现 check。因此，目前我们将退一步，关注 Gen，以更好地了解它的含义及其可能的依赖关系。

### 8.2.3 发现生成器的含义和 API

我们之前确定 Gen<A> 是知道如何生成类型 A 的值的东西。它如何做到这一点？好吧，它可以随机生成这些值。考虑到我们将整章 6 都用于这个主题，如果我们不使用在该章中学到的知识，似乎我们错过了一个技巧！回顾我们的示例，我们为纯函数式随机数生成器 RNG 提供了接口。然后我们展示了如何使其方便地组合使用它的计算。如果我们回忆 State 的定义，我们可以简单地将 Gen 设为一种包装随机数生成器上的 State 转换的类型。



## Listing 8.2 通过在随机数生成器上包装状态转换来定义 Gen

```
interface RNG {
    fun nextInt(): Pair<Int, RNG>
}

data class State<S, out A>(val run: (S) -> Pair<A, S>)

data class Gen<A>(val sample: State<RNG, A>)
```

## 练习 8.4

使用此 Gen 表示法实现 Gen.choose。它应生成从 start 到 stopExclusive 范围内的整数。可随意使用已编写的函数。

```
fun choose(start: Int, stopExclusive: Int): Gen<Int> = TODO()
```

## 练习 8.5

让我们看看使用此 Gen 表示法还能实现什么。尝试使用以下签名实现 unit、boolean 和 listOfN，再次利用之前编写的函数：

```
fun <A> unit(a: A): Gen<A> = TODO()

fun boolean(): Gen<Boolean> = TODO()

fun <A> listOfN(n: Int, ga: Gen<A>): Gen<List<A>> = TODO()
```

正如第 7 章所讨论的，我们感兴趣的是了解哪些操作是原始的，哪些是派生的，以及找到一个小而表达力强的原始集合。使用一组原始集合可以实现什么的一种好方法是选择一些你想要表达的具体示例，看看是否可以组装出你想要的功能。在这样做的过程中，寻找模式，尝试将这些模式分解为组合子，并完善你的原始集合。我们鼓励你在此停止阅读，简单地使用我们迄今为止编写的原始集合和组合子进行操作。如果你想要一些具体的示例来激发灵感，这里有一些想法：

- 如果我们能生成某个范围内的单个 Int，是否还需要一个新的原始操作来生成某个范围内的 Pair<Int, Int>？
- 我们能否从 Gen<A> 生成 Gen<Option<A>>？那么从 Gen<Option<A>> 生成 Gen<A> 呢？
- 我们能否使用现有的原始操作生成字符串？

## 侧边栏

### 玩耍的重要性

你不必等待具体的示例来探索你的库的问题域。事实上，如果你完全依赖于此类有用或重要的示例来设计你的 API，你经常会错过关键的设计方面，最终编写出过于特定的 API。

我们不想将设计 *overfit* 到我们现在偶然想到的特定示例上。我们希望将问题简化到其本质，有时最好的方法就是 *playing*。不要试图解决重要问题或产生有用的功能。至少，不是一开始。只需尝试不同的表示、原始操作和操作。让问题自然出现并探索任何引起你兴趣的东西。

诸如“这两个函数看起来很相似，我想知道里面是否隐藏着更一般的运算”，或“让这个数据类型多态是否有意义？”，或“将表示的这一方面从单个值改为 `List` 的值会意味着什么？”之类的观察将会浮现。

这样做没有对错之分，但设计选择如此之多，以至于不可能不一头撞进令人着迷的问题中去玩耍。无关紧要你从哪里开始——如果你继续玩耍，领域将不可避免地引导你做出所需的所有设计选择。

## 8.2.4 依赖于生成值的生成器

假设我们想要一个 `Gen<Pair<String, String>>`，它生成第二字符串只包含第一个字符串中字符的对。或者我们有一个 `Gen<Int>`，它在 0 到 11 之间选择一个整数，我们想要制作一个 `Gen<List<Double>>`，然后生成所选长度的列表。在这两种情况下都存在依赖关系——我们生成一个值，然后使用该值来确定接下来使用什么生成器。为此我们需要 `flatMap`，它允许一个生成器依赖于另一个生成器。

## 练习 8.6

实现 flatMap，然后使用它来实现这个更动态的 listOfN 版本。按所示将 flatMap 和 listOfN 放在 Gen 数据类中。

```
data class Gen<A>(val sample: State<RNG, A>) {
    companion object {
        fun <A> listOfN(gn: Gen<Int>, ga: Gen<A>): Gen<List<A>> = TODO()
    }
    fun <B> flatMap(f: (A) -> Gen<B>): Gen<B> = TODO()
}
```

## 练习 8.7

实现 union，通过从每个生成器中以相等可能性提取值，将两个相同类型的生成器组合成一个。

```
fun <A> union(ga: Gen<A>, gb: Gen<A>): Gen<A> = TODO()
```

## 练习 8.8

实现 weighted，这是一个接受每个 Gen 的权重的 union 版本，并以与权重成比例的概率从每个 Gen 生成值。

```
fun <A> weighted(
    pga: Pair<Gen<A>, Double>,
    pgb: Pair<Gen<A>, Double>
): Gen<A> = TODO()
```

## 8.2.5 细化属性数据类型

现在我们已经探索了生成器的表示法，让我们回到 Prop 的定义。我们的 Gen 表示法揭示了 Prop 的要求信息。我们当前的 Prop 定义如下，暂时忽略 and 操作符：

```
interface Prop {
    fun check(): Either<Pair<FailedCase, SuccessCount>, SuccessCount>
}
```

此时 Prop 不过是一个 Either，尽管它缺少一些重要信息。我们在 SuccessCount 中有成功测试用例的数量，但我们没有指定在我们认为属性通过测试之前需要检查多少个测试用例。我们当然可以硬编码某个值，但抽象出这个细节会更好。我们将通过注入一个别名为 TestCases 的整数来实现。我们还将把 Prop 变成一个数据类，并使 check 成为一个值而不是方法。

```
typealias TestCases = Int

typealias Result = Either<Pair<FailedCase, SuccessCount>, SuccessCount>

data class Prop(val check: (TestCases) -> Result)
```

此外，我们在 Either 的两侧都记录了成功测试的数量。但当属性通过时，隐含的是通过的测试数量将等于 check 的参数。因此，check 的调用者通过被告知成功计数而学不到新信息。由于我们目前在该 Either 的 Right 情况下不需要任何信息，我们可以将其变成一个 Option：

```
typealias Result = Option<Pair<FailedCase, SuccessCount>>

data class Prop(val check: (TestCases) -> Result)
```

这现在看起来有点奇怪，因为 None 将意味着所有测试都成功，而 Some 将表示失败。直到现在，我们只使用过 Option 的 None 情况来表示失败，但在这种情况下我们用它来表示失败的缺失。这是对 Option 的完全合法使用，但其意图并不非常清晰。所以让我们创建一个等价于 Option<Pair<FailedCase, SuccessCount>> 的新数据类型，使我们的意图更明确。

## Listing 8.3 将测试运行的可能结果建模为 ADT

```
sealed class Result {
    abstract fun isFalsified(): Boolean
}

object Passed : Result() {
    override fun isFalsified(): Boolean = false
}

data class Falsified(
    val failure: FailedCase,
    val successes: SuccessCount
) : Result() {
    override fun isFalsified(): Boolean = true
}
```

- ① Result 的密封类型
- ② 子类型表示所有测试均已通过
- ③ 子类型表示其中一个测试用例证伪了属性

这现在是 Prop 的充分表示吗？让我们再看看 forAll。forAll 能实现吗？如果不能，为什么？

```
fun <A> forAll(a: Gen<A>, f: (A) -> Boolean): Prop = TODO()
```

正如我们所见，forAll 没有足够的信息来返回一个 Prop。除了要尝试的测试用例数量之外，check 必须拥有生成测试用例所需的所有信息。如果它需要使用我们当前的 Gen 表示法生成随机测试用例，它将需要一个 RNG。让我们继续将此依赖项提供给 Prop：

## Listing 8.4 为 Prop 提供 RNG 实例以允许测试用例生成

```
data class Prop(val check: (TestCases, RNG) -> Result)
```

如果我们想到它可能需要的其他依赖项，除了测试用例数量和随机源之外，我们可以稍后将这些作为额外参数添加到 check 中。

通过将 RNG 作为参数提供给 Prop，我们现在有了足够的信息来实现 forAll。这里有一个初步尝试。

## Listing 8.5 使用所有底层构建块实现 forAll

```
fun <A> forAll(ga: Gen<A>, f: (A) -> Boolean): Prop =
    Prop { n: TestCases, rng: RNG ->
        randomSequence(ga, rng).mapIndexed { i, a ->
            try {
                if (f(a)) Passed
                else Falsified(a.toString(), i)
            } catch (e: Exception) {
                Falsified(buildMessage(a, e), i)
            }
        }.take(n)
        .find { it.isFalsified() }
        .toOption()
        .getOrElse { Passed }
    }

private fun <A> randomSequence(
    ga: Gen<A>,
    rng: RNG
): Sequence<A> =
    sequence {
        val (a, rng2: RNG) = ga.sample.run(rng)
        yield(a)
        yieldAll(randomSequence(ga, rng2))
    }

private fun <A> buildMessage(a: A, e: Exception) =
    """
    |test case: $a
    |generated an exception: ${e.message}
    |stacktrace:
    |${e.stackTrace.joinToString("\n")}
    """.trimMargin()
```

- 1. 准备一个将索引 i 映射到生成值 a 的 Sequence
- 2. 测试失败时，记录失败用例和索引，显示失败前成功了多少次测试。
- 3. 出现异常时，以美观的消息记录为结果。
- 4. 递归生成无限的 A 序列，对生成器进行采样。




5 使用字符串插值和边距修剪来构建漂亮的消息。

注意我们捕获异常并将其报告为测试失败，而不是让 `check` 抛出异常。这样做是为了不丢失可能触发失败的参数信息。

**注意** 我们正在使用 Kotlin 标准库的 `Sequence` 类型，它允许我们通过使用 `sequence`、`yield` 和 `yieldAll` 函数来生成惰性值流。其细节并不很重要，我们需要知道的是，我们从 `Sequence` 中取出 *n* 个元素并应用终端操作 `find` 来查找被证伪的情况，否则我们报告通过。

### 练习 8.9
现在我们有了 `Prop` 的表示，请为组合 `Prop` 值实现 `and` 和 `or`。注意在 `or` 失败的情况下，我们不知道是哪一个属性负责，左边还是右边。你能设计一种处理方法吗？

```kotlin
data class Prop(val run: (TestCases, RNG) -> Result) {
    fun and(p: Prop): Prop = TODO()
    fun or(p: Prop): Prop = TODO()
}
```

## 8.3 测试用例最小化

之前，我们提到过测试用例最小化的想法。我们的意思是，我们希望我们的框架能够找到最小或最简单的失败测试用例，以便更好地说明失败并促进调试。让我们看看是否可以调整我们的表示来支持这一结果。我们可以采取两种通用方法：

- *Shrinking* — 在找到失败的测试用例后，我们可以运行一个单独的程序，通过不断减小其“大小”直到它不再失败来最小化测试用例。这称为 *shrinking*，通常需要我们为每种数据类型编写单独的代码来实现此最小化过程。
- *Sized generation* — 而不是缩小测试用例，我们只是按 *increasing* 大小和复杂度的顺序生成测试用例。所以我们从小开始并增加大小直到找到失败。这种想法可以以各种方式扩展，以允许测试运行器在可能的大小空间中进行更大的跳跃，同时仍然能够找到最小的失败。

KotlinTest，除了大多数流行的基于属性的测试框架，如 ScalaCheck³⁹ 和 Haskell 的 QuickCheck⁴⁰，都采用了第一种缩小的方法。由于实现该方法涉及更大的复杂性，我们将选择使用替代方案。Sized generation 更简单，并且在某些方面更模块化，因为我们的生成器只需要知道如何生成给定大小的测试用例。我们很快就会看到这一点是如何展开的。

与其修改我们已经为其编写了许多有用组合子的 Gen 数据类型，不如将 sized generation 作为我们库中的一个单独层引入。Sized generator 的简单表示只是接受一个大小并生成一个生成器的函数：

### 列表 8.6 将 sized generator 表示为从 Int 到 Gen 的函数

```
data class SGen<A>(val forSize: (Int) -> Gen<A>)
```

### 练习 8.10

实现一个名为 unsized 的辅助函数，用于将 Gen 转换为 SGen。你可以将其作为 Gen 上的方法添加。

```
data class Gen<A>(val sample: State<RNG, A>) {
    fun unsized(): SGen<A> = TODO()
}
```

### 练习 8.11

不出所料，SGen 至少支持与 Gen 相同的许多操作，并且实现相当机械化。在 SGen 上定义一些方便的函数，它们只是委托给 Gen 上的相应函数。还提供一种方便的方式来调用 SGen。

```
data class SGen<A>(val forSize: (Int) -> Gen<A>) {
    operator fun invoke(i: Int): Gen<A> = TODO()
    fun <B> map(f: (A) -> B): SGen<B> = TODO()
    fun <B> flatMap(f: (A) -> Gen<B>): SGen<B> = TODO()
}
```

**注意**

即使这种方法非常重复，我们现在仍将继续这样做。本书的第 3 部分将介绍处理此类重复的更好方法。

### 练习 8.12

在 Gen 上实现一个 listOf 组合子，它不接受显式大小并且应返回 SGen 而不是 Gen。实现应生成 SGen 提供的大小的列表。

```
fun listOf(): SGen<List<A>> = TODO()
```

接下来，让我们看看 SGen 如何影响 Prop 的定义，特别是其 forAll 方法。SGen 版本的 forAll 看起来像这样：

```
fun <A> forAll(g: SGen<A>, f: (A) -> Boolean): Prop = TODO()
```

仔细检查此声明，我们发现无法实现它。这是因为 SGen 期望被告知一个大小，但 Prop 并没有接收此类信息。就像我们对 Prop 的底层 check 函数中的随机源和测试用例数量所做的那样（参见列表 8.4），我们只需将这个新数字作为函数的依赖项添加。因此，由于我们希望 Prop 负责使用各种大小调用底层生成器，我们将让 Prop 接受最大大小。Prop 然后将生成测试用例，直到包括指定的最大大小。额外的好处是，这也将允许它搜索最小的失败测试用例。让我们看看这是如何运作的。

**注意**

这种相当简化的实现为每个生成的大小提供相同数量的测试用例，并从 0 开始每次增加 1 大小。我们可以想象一个更复杂的实现，它做类似对失败测试用例大小进行二分搜索的事情——从大小 0,1,2,4,8,16... 开始，然后在失败的情况下缩小搜索空间。

### 列表 8.7 生成到给定最大大小的测试用例

```
typealias MaxSize = Int

data class Prop(val check: (MaxSize, TestCases, RNG) -> Result) {

    companion object {

        fun <A> forAll(g: SGen<A>, f: (A) -> Boolean): Prop =
            forAll({ i -> g(i) }, f) ①

        fun <A> forAll(g: (Int) -> Gen<A>, f: (A) -> Boolean): Prop =
            Prop { max, n, rng ->

                val casePerSize: Int = (n + (max - 1)) / max  ①

                val props: Sequence<Prop> =
                    generateSequence(0) { it + 1 }  ③
                        .take(min(n, max) + 1)
                        .map { i -> forAll(g(i), f) }  ④

                val prop: Prop = props.map { p ->
                    Prop { max, _, rng ->
                        p.check(max, casePerSize, rng)
                    }
                }.reduce { p1, p2 -> p1.and(p2) }  ⑤

                prop.check(max, n, rng)  ⑥
            }
    }

    fun and(p: Prop): Prop =
        Prop { max, n, rng ->  ⑦
            when (val prop = check(max, n, rng)) {
                is Passed -> p.check(max, n, rng)
                is Falsified -> prop
            }
        }
}
```

- 1. 在测试中使用的入口点
- 2. 为每个大小生成这么多随机用例
- 3. 生成从 0 开始递增的 Sequence<Int>
- 4. 为每个大小创建一个属性，但不超过 n 个属性（使用之前定义的 forAll）
- 5. 使用 Prop.and 将它们全部组合成一个属性。
- 6. 检查组合后的属性
- 7. 改造 and 以处理新的 max 参数

这段代码乍一看可能有点令人生畏，但仔细检查后它相当直接。check 现在有一个新的 MaxSize 参数，它为要运行的测试用例的大小设置了上限。我们的 forAll 入口点接受一个 SGen 和一个谓词，该谓词传递给我们新的 forAll 函数，进而生成一个组合的 Prop。

该属性首先计算每个大小要运行的测试用例数。然后它使用来自列表 8.5 的之前定义的 `forAll` 函数生成一个由每个大小的 `Prop` 组成的 Sequence。最后，它使用我们之前定义的更新版本的 `and` 函数将它们全部组合成一个单一属性。在所有这些之后，剩余的归约属性将被检查。

## 8.4 使用库并改善用户体验

我们现在已经收敛到一个似乎合理的 API。我们可以继续对其进行调整，但在这一点上，让我们尝试 *使用* 它。我们将通过构建测试并查看是否注意到任何缺陷来实现这一点，无论是在它能表达的内容方面还是在一般可用性方面。可用性在某种程度上是主观的，但我们通常喜欢方便的语法和针对常见使用模式的适当辅助函数。我们不一定旨在使库更具表达性，但我们确实希望使其愉快地使用。

### 8.4.1 一些简单示例

让我们重新审视本章开头提到的一个示例——指定函数 `max` 的行为，可作为 `List<Int>` 上的方法使用。列表的最大值应大于或等于列表中的每个其他元素。让我们指定这一点：

### 列表 8.8 指定列表中的最大值属性

```
val smallInt = Gen.choose(-10, 10)

val maxProp = forAll(SGen.listOf(smallInt)) { ns ->
    val mx = ns.max()
    ?: throw IllegalStateException("max on empty list")
    !ns.exists { it > mx } ①
}
```

① 不应存在大于 mx 的值在 ns 中

此时，直接在 `Prop` 上调用 `check` 相当麻烦。我们可以引入一个辅助函数来运行属性值并以有用的格式将其结果打印到控制台。让我们简单地称之为 `run`。

### 列表 8.9 使用合理默认值运行属性的便捷方法

```
fun run(
    p: Prop,
    maxSize: Int = 100, ①
    testCases: Int = 100, ②
    rng: RNG = SimpleRNG(System.currentTimeMillis()) ③
): Unit =
    when (val result = p.check(maxSize, testCases, rng)) {
        is Falsified -> ④
            println(
                "Falsified after ${result.successes}" +
                "passed tests: ${result.failure}"
            )
        is Passed -> ⑤
            println("OK, passed $testCases tests.")
    }
```



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


```
unit(2)
)
)
```

经过这些逐步改进，我们的属性变得更容易理解和使用。这些改动看起来似乎微不足道，但这种重构和清理对我们库的可用性有巨大影响。我们编写的辅助函数使属性更易于阅读，也更愉快地使用。

让我们看看第 7 章中的一些其他属性。回想一下，我们将测试用例进行了泛化：

```
map(unit(x), f) == unit(f(x))
```

然后我们将其简化为这样一个定律：对计算应用恒等函数不应产生任何影响：

```
map(y, id) == y
```

我们能表达这一点吗？不完全可以。这个属性隐含地说明等式对所有 y 的选择、所有类型都成立。我们被迫为 y 选择特定的值：

```
val pint: Gen<Par<Int>> = {
    Gen.choose(0, 10).map {
        unit(it)
    }
}

val p = forAllPar(pint) { n ->
    equal(map(n) { it }, n)
}
```

我们当然可以遍历更多 y 的选择，但这里的内容可能已经足够了。map 的实现并不关心我们并行计算的值，因此为 Double、String 等构造相同的测试意义不大。能影响 map 的是并行计算的结构。如果我们想要更大程度地确保属性成立，可以为结构提供更丰富的生成器。在这里，我们只提供了具有一层嵌套的 Par 表达式。

### 练习 8.15

为 `Par<Int>` 编写一个更丰富的生成器，该生成器构建的并行计算嵌套比我们迄今提供的简单变体更深。

### 练习 8.16

表达第 7 章中关于 `fork` 的属性，即 `fork(x) == x`。

## 8.5 生成高阶函数及其他可能性

到目前为止，我们的库似乎相当 expressive，但有一个方面还不够：我们没有很好的方法来测试高阶函数。虽然我们有很多方法可以使用生成器生成 *数据*，但我们并没有很好的方法生成 *函数*。在本节中，我们讨论生成函数以便测试高阶函数。

例如，让我们考虑为 `List` 和 `Sequence` 定义的 `takeWhile` 函数。回想一下，该函数返回输入的最长前缀，其所有元素都满足谓词。例如，`listOf(1,2,3).takeWhile { it < 3 }` 的结果是 `List(1,2)`。我们希望检查的一个简单属性是，对于任意列表 `s: List<A>`，以及任意 `f: (A) -> Boolean`，表达式 `s.takeWhile(f).forAll(f)` 的求值结果为 `true`。也就是说，返回列表中的每个元素都满足谓词。⁴¹

### 练习 8.17

想出 `takeWhile` 应该满足的其他一些属性。你能想到一个表达 `takeWhile` 与 `dropWhile` 之间关系的良好属性吗？

我们当然可以采用在测试高阶函数时只检查 *特定* 参数的方法。例如，这里是 `takeWhile` 的一个更具体的属性：

```
val isEven = { i: Int -> i % 2 == 0 }

val takeWhileProp =
    Prop.forAll(Gen.listOfN(n, ga)) { ns ->
        ns.takeWhile(isEven).forAll(isEven)
    }
```

这可行，但是否有办法让测试框架处理为 `takeWhile` 生成要使用的函数呢？让我们考虑我们的选择。为了具体说明，假设我们有一个 `Gen<Int>`，并且希望生成一个 `Gen<(String) -> Int>`。我们能怎样做呢？嗯，我们可以生成 `(String) -> Int` 函数，这些函数只是忽略其输入并委托给底层的 `Gen<Int>`：

```
fun genStringIntFn(g: Gen<Int>): Gen<(String) -> Int> =
    g.map { i -> { _: String -> i } }
```

不过这种方法是不够的。我们只是生成忽略输入的常量函数。在 `takeWhile` 的情况下，我们需要一个返回 `Boolean` 的函数，这将是一个始终返回 `true` 或 `false` 的函数，具体取决于底层布尔生成器传递给它的内容——显然对于测试我们函数的行为来说并不很有趣。

```
fun genIntBooleanFn(g: Gen<Boolean>): Gen<(Int) -> Boolean> =
    g.map { b: Boolean -> { _: Int -> b } }
```

现在，让我们考虑以下函数，它将返回一个函数生成器，该生成器将根据传递给它的值执行一些逻辑。在这种情况下，会传递一个阈值 t，并且注入到函数中的任何 Int 都会被测试以查看该值是否超过 t。

```
fun genIntBooleanFn(t: Int): Gen<(Int) -> Boolean> =
    Gen.unit { i: Int -> i > t }
```

现在让我们让新的函数生成器发挥作用。我们首先生成一个 List<Int> 以及一个随机阈值。我们用给定的随机阈值预加载函数生成器，让它生成其函数 of (Int) -> Boolean。最后，我们将此生成的函数应用于我们生成的列表上的 takeWhile，然后将相同的谓词应用于 forAll，这应该始终导致 true。

```
val gen: Gen<Boolean> =
    Gen.listOfN(100, Gen.choose(1, 100)).flatMap { ls: List<Int> ->
        Gen.choose(1, ls.size / 2).flatMap { threshold: Int ->
            genIntBooleanFn(threshold).map { fn: (Int) -> Boolean ->
                ls.takeWhile({fn}).forAll(fn)
            }
        }
    }
```

在我们的测试框架中使用 Prop.forAll 运行此测试时，我们应该始终看到测试通过。

```
run(Prop.forAll(gen) { success -> success })
```

尽管这个例子有些人为且 trivial，但它足以证明随机函数生成器的可能性。我们可以把这些想法推得更远，并建议你通过遵循本书末尾附录 C 中找到的扩展练习来探索选项。

## 8.6 生成器的定律

在我们设计库的过程中，我们看到一些模式正在浮现，这些模式我们在之前的章节中多次遇到过。我们发现的许多组合子甚至具有相同的名称和功能。例如，我们为 Gen 类型实现的大多数函数看起来都与我们在 Par、List、Stream 和 Option 上定义的函数非常相似。回顾我们在第 7 章中对 Par 的实现，我们可以看到我们定义了以下组合子：

```
fun <A, B> map(a: Par<A>, f: (A) -> B): Par<B> = TODO()
```

而在本章中，我们为 Gen 定义了 map（作为 Gen<A> 的方法）：

```
fun <A, B> map(a: Gen<A>, f: (A) -> B): Gen<B> = TODO()
```

我们还为 `Option`、`List`、`Stream` 和 `State` 定义了类似的函数。我们不禁要问，这仅仅是因为我们的函数具有相似的签名，还是它们也满足相同的 *定律*？让我们看看我们在第 7 章中为 `Par` 引入的一个定律：

```
map(y, id) == y
```

这个定律对我们的 `Gen.map` 实现成立吗？对于 `Stream`、`List`、`Option` 和 `State` 呢？是的，它成立！试试看。这表明这些函数不仅具有相似的签名，在某种意义上它们在各自的领域中也有类似的含义。看起来有更深层次的力量在起作用！我们正在揭示贯穿所有这些领域的某些基本模式。在第 3 部分，我们将学习这些模式的名称，发现支配它们的定律，并理解这一切的含义。

重申一下，目标不一定是学习属性测试本身，而是突出函数式设计的一些特定方面。首先，我们看到在抽象代数和具体表示之间来回切换可以让两者相互 informing。这避免了库过度拟合于特定表示，也避免了与最终目标相距甚远的脱节抽象。

其次，我们注意到这个领域让我们发现了许多我们之前已经看到几次的相同组合子：`map`、`flatMap` 等等。这些函数的签名不仅是类似的，实现所满足的 *定律* 也是类似的。软件世界中存在着大量看似不同的 *问题*，但函数式 *解决方案* 的空间要小得多。许多库只是某些基本结构的简单组合，这些结构在各种不同的领域中反复出现。这是一个我们可以利用的代码重用机会，我们将在第 3 部分中利用它。我们将学习这些结构的名字以及如何发现更一般的抽象。



## 8.7 摘要

- 可以使用基于属性的测试来验证将函数相互关联的定律或属性。
- 构建基于属性的测试库是使用迭代方法设计函数式库的一个绝佳示例。
- 可以使用表示属性和生成器的数据类型来建模一个简单的测试库，以验证程序的定律。
- 在验证被测代码时，可以使用生成器与其他生成器结合来表达复杂的定律。
- 通过缩小应用的测试数据或使用增量大小生成，可以最小化测试用例输出。
- 库的用户体验非常重要，可用性始终应是库设计的主要目标。
- 可以通过在抽象代数和具体表示之间振荡来设计函数式库。
- 跨领域的组合子遵循相同的定律并具有相同的语义，从而建立了通用的函数式设计模式。
