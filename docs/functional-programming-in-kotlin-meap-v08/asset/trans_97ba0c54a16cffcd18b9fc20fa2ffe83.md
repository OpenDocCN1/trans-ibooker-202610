
## 列表 6.16 执行状态传播所需的状态声明和组合子

```
val int: State<RNG, Int> = TODO()

fun ints(x: Int): State<RNG, List<Int>> = TODO()

fun <A, B> flatMap(
    s: State<RNG, A>,
    f: (A) -> State<RNG, B>
): State<RNG, B> = TODO()

fun <A, B> map(
    s: State<RNG, A>,
    f: (A) -> B
): State<RNG, B> = TODO()
```

- 1. 是一个具有生成单个随机整数能力的 `State<RNG, Int>`
- 2. 返回一个可以生成长度为 x 的随机整数列表的 `State<RNG, List<Int>>`
- 3. 一个 `flatMap` 函数，它对 `State<RNG, A>` 操作，并带有一个从 A 到 `State<RNG, B>` 的函数
- 4. 一个 `map` 函数，它对 `State<RNG, A>` 操作，并带有一个将 A 转换为 B 的函数

现在我们编写一些使用这些声明的代码，这些声明随后可从列表 6.16 中获得。

## 列表 6.17 使用一系列 flatMap 和 map 的状态传播

```
val ns: State<RNG, List<Int>> =
    flatMap(int) { x ->
        flatMap(int) { y ->
            map(ints(x)) { xs ->
                xs.map { it % y }
            }
        }
    }
```

- 1. int 将生成一个随机整数。
- 2. ints(x) 生成一个长度为 x 的随机整数列表
- 3. 用列表中每个元素除以 y 的余数替换该元素

由于所有嵌套的 flatMap 和 map 调用，这里发生的事情并不清楚。让我们通过关注 for-comprehension 来寻找更简单的方法。这种构造会展开一系列 flatMap 调用，使我们能够将此代码重写为一系列看似命令式声明的形式。此代码的技巧在于每一步发生的解构。每次看到某物被解构时，都意味着一次 flatMap 调用。

例如，写成 flatMap(int) { x -> ... } 的一行可以在 for-comprehension 的范围内重写为 val (x: Int) = int。考虑到这一点，让我们尝试使用此技术重写列表 6.17。

## 列表 6.18 使用 for-comprehension 的状态传播

```
val ns2: State<RNG, List<Int>> =
    State.fx(Id.monad()) {
        val (x: Int) = int
        val (y: Int) = int
        val (xs: List<Int>) = ints(x)
        xs.map { it % y }
    }
```

- - ① 通过将代码块传递给 `State.fx(Id.monad())` 来打开 for-comprehension
- - ② `int` 被解构为名为 x 的 `Int`
- - ③ `int` 再次被解构为名为 y 的 `Int`
- - ④ `ints(x)` 被解构为长度为 x 的 `List<Int>`
- - ⑤ 用 xs 中每个元素除以 y 的余数替换每个元素，返回结果

我们首先通过调用 `State.fx` 并传入 `Id.monad()` 的一个实例来打开 for-comprehension。我们不会过多关注此细节，但可以说，这将允许我们传入一个充当命令式代码块的匿名函数，然后将该块的最终结果作为值返回。

构成 for-comprehension 的代码块更容易阅读（和编写），并且它看起来就是它所代表的东西——一个维护某种状态的命令式程序。但它与前一个示例中的代码相同。我们获取下一个 `Int` 并将其赋给 x，获取其后的下一个 `Int` 并将其赋给 y，然后生成长度为 x 的列表，最后返回所有元素对 y 取模的列表。

我们几乎拥有以命令式风格编写完整函数式程序所需的一切。为了便于使用 for-comprehensions（或 `flatMaps`）进行这种命令式编程，我们只需要再有两个原始的 `State` 组合子——一个用于读取状态，一个用于写入状态。如果我们想象有一个组合子 `get` 用于获取当前状态，以及一个组合子 `set` 用于设置新状态，我们就可以实现一个可以以任意方式修改状态的组合子：

## 列表 6.19 修改当前状态的组合子

```
fun <S> modify(f: (S) -> S): State<S, Unit> =
    State.fx(Id.monad()) {
        val (s: S) = get<S>()
        val (_) = set(f(s))
    }
```

- - ① 为 `State` 设置 for-comprehension
- - ② 获取当前状态并将其赋给 s
- - ③ 设置应用 f 于 s 的新状态

此方法返回一个 `State` 动作，该动作通过函数 f 修改传入的状态。它产生 Unit 以表明除了状态之外它没有返回值。

get 和 set 动作会是什么样子？它们非常简单。get 动作只是传递传入的状态并将其作为值返回：

## 列表 6.20 get 组合子检索然后传递其状态

```kotlin
fun <S> get(): State<S, S> =
    State { s -> Tuple2(s, s) }
```

set 动作是用新的状态 s 构造的。 resulting action 忽略传入的状态，用新状态替换它，并返回 Unit 而不是有意义的值：

## 列表 6.21 set 组合子更新状态，然后返回 Unit

```kotlin
fun <S> set(s: S): State<S, Unit> =
    State { Tuple2(s, Unit) }
```

get、set 和 modify 组合子已经可以在 arrow.mtl.StateApi 类上找到，但我们只是为了演示目的想展示它们包含的内容。这些两个简单的动作，加上我们编写的状态组合子——unit、map、map2 和 flatMap——是我们以纯函数方式实现任何类型状态机或有状态程序所需的所有工具。

**TIP** 我们建议你熟悉 Arrow 关于 State 的文档 (arrow-kt.io/../-state.html)，并查看底层源代码 (github.com/../State.kt) 以便完全掌握其工作原理。如第 1.5 节所述，Arrow 的实现比我们编写的要丰富一些，采用了 monad 转换器 StateT，并为 State 类公开了公共 API StateAPI。不要太担心 monad 转换器是什么，只需知道 StateApi 负责公开诸如 get、set、modify stateSequential 和 stateTraverse 等方法，这些方法在接下来的练习中可能会派上用场。

## 练习 6.11（困难/可选）

为了获得使用 `State` 的经验，实现一个有限状态自动机来模拟一个简单的糖果分配器。机器有两种类型的输入：你可以插入硬币，或转动旋钮来分配糖果。它可以处于两种状态之一：锁定或未锁定。它还跟踪剩余的糖果数量和包含的硬币数量。

```
sealed class Input

object Coin : Input()
object Turn : Input()

data class Machine(
    val locked: Boolean,
    val candies: Int,
    val coins: Int
)
```

机器的规则如下：

- - 将硬币插入锁定机器中，如果还有糖果剩余，则会使其解锁。
- - 在未锁定机器上转动旋钮会使其分配糖果并变为锁定。
- - 在锁定机器上转动旋钮或向未锁定机器中插入硬币不会产生任何效果。
- - 糖果用完的机器会忽略所有输入。

`simulateMachine` 方法应根据输入列表操作机器，并在结束时返回机器中剩余的硬币和糖果数量。例如，如果输入的 `Machine` 有 10 个硬币和 5 个糖果，并且总共成功购买了 4 个糖果，则输出应为 `(14, 1)`。使用以下声明存根来实现你的解决方案：

```
fun simulateMachine(
    inputs: List<Input>
): State<Machine, Tuple2<Int, Int>> = TODO()
```

## 6.7 结论

在本章中，我们涉及了如何编写具有状态的纯函数式程序的主题。我们以随机数生成作为动机示例，但总体模式在许多不同领域都会出现。这个想法很简单：使用一个接受状态作为参数的纯函数，并返回新状态以及其结果。下次你遇到依赖副作用的命令式 API 时，看看是否可以提供其纯函数式版本，并使用我们在此编写的一些函数使使用它更方便。
