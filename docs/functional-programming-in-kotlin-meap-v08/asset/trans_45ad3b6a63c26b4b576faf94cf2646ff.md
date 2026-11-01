
monad 实例，而是一整个族，每个 S 的选择对应一个。我们真正想要做的是对 State 进行部分应用，将 S 类型参数固定为某个具体类型，从而只剩下唯一的类型变量 A。

这与我们对函数进行部分应用的方式非常相似，只是现在我们在类型层面进行。例如，我们可以创建一个 IntState 类型构造器，它是 State 的别名，其第一个类型参数固定为 Int：

```
typealias IntState<A> = State<Int, A>
```

而 IntState 正是我们能够为其构建 Monad 的那种东西：

## Listing 11.16 用于 Int 类型的部分应用的状态 monad 实例

```
val intState = object : Monad<ForIntState> {
    override fun <A> unit(a: A): IntStateOf<A> =
        IntState { s: Int -> Pair(a, s) }

    override fun <A, B> flatMap(
        fa: IntStateOf<A>,
        f: (A) -> IntStateOf<B>
    ): IntStateOf<B> =
        fa.fix().flatMap { a: A -> f(a).fix() }
}
```

- ① 用来替代 Kind<Int, A> 的代理类型，以安抚编译器
- ② Kind<ForIntState, A> 的类型别名

当然，如果我们必须为每一种特定的状态类型编写显式的 Monad 实例，这将非常重复。考虑 IntState、DoubleState、StringState，列表还在继续。除了这种方式扩展性不佳之外，这还意味着我们的 State 数据类型需要继承自 IntState，以及单子族中每一个其他的部分应用类型。这在 Kotlin 中根本不可能！

抛开这种硬编码 monad 实例的方法不谈，让我们看看如何以更少的代码重复来解决这个问题。幸运的是，通过引入 StateMonad 接口可以做到这一点，该接口可以用 Int 等类型进行部分应用，从而得到 StateMonad<Int>。

## Listing 11.17 状态 monad 接口去除了硬编码的部分应用类型

```
interface StateMonad<S> : Monad<StatePartialOf<S>> {
    override fun <A> unit(a: A): StateOf<S, A>

    override fun <A, B> flatMap(
        fa: StateOf<S, A>,
        f: (A) -> StateOf<S, B>
    ): StateOf<S, B>
}
```

- ① `Monad` 类型构造器对任意 `S` 接受部分应用的类型参数 `StatePartialOf`
- ② 单子组合子不再受限于处理单一类型参数货币

主要区别在于 `StateMonad` 接口本身的声明。monad 接口有一个类型参数 `S`，用于表示它所代表的族成员，并且它将扩展自一个新的类型别名 `StatePartialOf<S>`，即 `Kind<ForState, S>` 的别名。

诸如 `StatePartialOf` 和 `StateOf` 之类的类型只是我们自己可以编写的样板代码，尽管 Arrow 已经很方便地为我们生成了所有这些内容。书的附录 D 描述了确切需要哪些样板代码，以及如何让 Arrow 代替我们完成所有繁重的工作。

**NOTE** 请重新审阅附录 D 第 2 节，其中描述了声明部分应用类型构造器所需的样板代码。**请确保您已阅读并彻底理解了此内容。**

我们现在可以使用 `StateMonad` 接口声明状态 monad 族的新成员。让我们继续使用之前的 `intStateMonad` 示例，使用此接口和样板代码：

## Listing 11.18 一个部分应用的 `State` monad，为其所代表的类型族成员带来灵活性

```
val intStateMonad: StateMonad<Int> = object : StateMonad<Int> {
    override fun <A> unit(a: A): StateOf<Int, A> =
        State { s -> Pair(a, s) }

    override fun <A, B> flatMap(
        fa: StateOf<Int, A>,
        f: (A) -> StateOf<Int, B>
    ): StateOf<Int, B> =
        fa.fix().flatMap { a -> f(a).fix() }
}
```

我们可以看到，我们已从 listing 11.16 中的硬编码 `ForIntState` monad 演进到 listing 11.17 中的更灵活的部分应用变体。再一次，只需提供 `flatMap` 和 `unit` 的实现，我们就能免费获得所有其他单子组合子的实现。

## EXERCISE 11.17

现在我们有了 State monad，试着使用它看看其行为如何。声明 replicateM、map2 和 sequence 的一些值，使用上述 intStateMonad 的类型声明。描述每个在幕后如何行为？

```
val replicateIntState: StateOf<Int, List<Int>> = TODO()

val map2IntState: StateOf<Int, Int> = TODO()

val sequenceIntState: StateOf<Int, List<Int>> = TODO()
```

既然我们已经检查了 Id 和 State，我们可以再次退一步问 monad 的含义是什么。让我们看看两者之间的区别。还记得第 6 章中，State 上的原始操作（除了 monadic 操作 flatMap 和 unit）是，我们可以通过某种形式的 get 和 set 组合子来修改当前状态：

```
fun <S> getState(): State<S, S> = State { s -> Pair(s, s) }

fun <S> setState(s: S): State<S, Unit> = State { Pair(Unit, s) }
```

还记得我们也发现这些组合子构成了 State 的最小原始操作集合。因此，与 monadic 原语 flatMap 和 unit 结合起来，它们完全指定了我们可以用 State 数据类型做的一切。这对于 monad 来说通常是正确的——它们都有 flatMap 和 unit，并且每个 monad 都带来自己特有的额外原始操作集合。

## EXERCISE 11.18

表达你期望 getState、setState、flatMap 和 unit 共同遵守的定律？

这告诉我们关于 State monad 的含义是什么？为了充分理解我们想要传达的内容，让我们再次将注意力转向 listing 11.18 中的 intStateMonad，并通过一个真实示例使用它。

## Listing 11.19 使用 flatMap 和 map 操作序列进行获取和设置状态

```
val F = intStateMonad

fun <A> zipWithIndex(la: List<A>): List<Pair<Int, A>> =
    la.foldLeft(F.unit(emptyList<Pair<Int, A>>())) { acc, a ->
        acc.fix().flatMap { xs ->
            acc.fix().getState<Int>().flatMap { n ->
                acc.fix().setState(n + 1).map { _ ->
                    listOf(Pair(n, a)) + xs
                }
            }
        }
    }
    }.fix().run(0).first.reversed()
```

此函数使用 State action 为列表中的所有元素编号。它保持一个 Int 状态，在每一步都会递增。我们从 0 开始运行复合 State action。最后我们反转顺序，因为我们使用 foldLeft 以相反顺序运行了计算。

为了更清晰地表达这一点，我们可以想象使用 Arrow 风格的 for-comprehension 传递给 foldLeft 的主体，在这段伪代码片段中：

## Listing 11.20 使用 for-comprehension 进行获取和设置状态

```
...
{ acc: StateOf<Int, List<Pair<Int, A>>>, a: A ->
    acc.fx {
        val (xs) = acc
        val (n) = acc.getState()
        val (_) = acc.setState(n + 1)
        listOf(Pair(n, a)) + xs
    }
}
...
```

for-comprehension 去除了 flatMap 和 map 引入的所有杂乱，让我们专注于似乎使用 State 传播递增计数器的命令式指令序列。

注意 for-comprehension 中 getState 和 setState 的情况。我们显然获得了变量绑定，就像在 Id monad 中一样——我们将每个连续的状态 action（acc、getState，然后是 setState）的值绑定到变量。但在行与行之间还有更多事情发生。在 for-comprehension 的每一行中，flatMap 的实现确保当前状态可供 getState 使用，并且新状态会传播到所有跟随 setState 的 action。

Id 的 action 与 State 的 action 之间的差异告诉我们关于 monad 的一般情况是什么？我们可以看出，一连串的 flatMap 调用（或等价的 for-comprehension）就像一个具有向变量赋值的语句的命令式程序，而 monad 指定了在语句边界发生什么。例如，对于 Id，除了解包之外什么都没有发生
