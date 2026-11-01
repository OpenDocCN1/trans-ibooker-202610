
- 对于 Option，它过滤一个列表，但允许过滤函数失败并中止过滤计算。
- 对于 Gen，它为输入列表的子集生成一个生成器，其中函数 f 以 Gen<Boolean> 的形式为每个元素选择一个“权重”。

## 练习 11.7

在 Monad 中实现以下 Kleisli 组合函数。

```
fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C> =
    { a: A -> flatMap(f(a)) { b: B -> g(b) } }
```

## 练习 11.8 (困难)

根据 compose 的抽象定义实现 flatMap。由此看来，我们似乎找到了另一组最小的 Monad 组合子：compose 和 unit。

```
fun <A, B> flatMap(fa: Kind<F, A>, f: (A) -> Kind<F, B>): Kind<F, B> =
    compose<Unit, A, B>({ _ -> fa }, f)(Unit)
```

## 练习 11.9

使用以下值，证明用 compose 表示的左右恒等律等价于用 flatMap 所述的恒等律：

```
val f: (A) -> Kind<F, A>
val x: Kind<F, A>
val v: A
```

右恒等律可以化简如下：

```
compose(f, { a: A -> unit(a) })(v) == f(v)
{ b: A -> flatMap(f(b), { a: A -> unit(a) }) }(v) == f(v)
flatMap(f(v)) { a: A -> unit(a) } == f(v)
flatMap(x) { a: A -> unit(a) } == x
```

左恒等律可以化简如下：

```
compose({ a: A -> unit(a) }, f)(v) == f(v)
{ b: A -> flatMap({ a: A -> unit(a) }(b), f) }(v) == f(v)
{ b: A -> flatMap(unit(b), f) }(v) == f(v)
flatMap(unit(v), f) == f(v)
```

因此，最终证明可以表示为：

```
flatMap(x) { a -> unit(a) } == x
flatMap(unit(v), f) == f(v)
```

## 练习 11.10

证明恒等律对 Option Monad 成立。

```
flatMap(None) { a: A -> Some(a) } == None
None == None

flatMap(Some(v)) { a: A -> Some(a) } == Some(v)
Some(v) == Some(v)

flatMap(Some(None)) { a -> Some(a) } == Some(None)
Some(None) == Some(None)

flatMap(Some(Some(v))) { a -> Some(a) } == Some(Some(v))
Some(Some(v)) == Some(Some(v))
```

## 练习 11.11

Monadic 组合子可以用另一组最小集合表示，即 map、unit 和 join。根据 flatMap 实现 join 组合子。

```
fun <A> join(mma: Kind<F, Kind<F, A>>): Kind<F, A> =
    flatMap(mma) { ma -> ma }
```

## 练习 11.12

现在 flatMap 或 compose 都可以用 join 实现。为了本练习的目的，请同时实现两者。

```
fun <A, B> flatMap(fa: Kind<F, A>, f: (A) -> Kind<F, B>): Kind<F, B> =
    join(map(fa, f))

fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C> =
    { a -> join(map(f(a), g)) }
```

## 练习 11.13 (困难/可选)

使用 join、map 和 unit 用 flatMap 重述 Monad 的结合律。

我们首先根据之前建立的前提，用 flatMap 表示的结合律进行观察：

```
flatMap(flatMap(x, f), g) ==
    flatMap(x) { a -> flatMap(f(a), g) }
```

我们可以用恒等函数替换 f 和 g，并用更高阶的 y 替换 x，以不同方式表达：

```
flatMap(flatMap(y, z)) { b -> b } ==
    flatMap(y) { a -> flatMap(z(a)) { b -> b } }

flatMap(flatMap(y, z)) { it } ==
    flatMap(y) { a -> flatMap(a) { it } }
```

我们也知道从练习 11.12 中，join 是 flatMap 与恒等函数组合而成：

```
flatMap(join(y)) { it } ==
    flatMap(y) { join(it) }
```

```
join(join(y)) ==
    flatMap(y) { join(it) }
```

我们也在练习 11.3 中了解到 flatMap 可以表示为 map 和 join，这消除了最后的 flatMap。

```
join(join(y)) ==
    join(map(y) { join(it) })
```

最后，我们现在用 unit(x) 替换 join(y) 的出现，在这两种情况下都相当于 Kind<F, A>

```
join(unit(x)) ==
    join(map(x) { unit(it) })
```

## 练习 11.14 (困难/可选)

用你自己的话写下结合律对 Par 和 Parser 的含义。

对于 Par，join 组合子意味着类似“使外部线程等待内部线程完成”。该定律所说的是，如果你有线程启动线程三层深，那么先连接内部线程再连接外部线程，与先连接外部线程再连接内部线程是相同的。

对于 Parser，join 组合子是运行外部解析器以生成一个 Parser，然后在剩余输入上运行内部 Parser。结合律大致说的是，只有嵌套顺序才重要，因为这会影响解析器的运行顺序。

## 练习 11.15 (困难/可选)

用你自己的话解释恒等律对 Gen 和 List 在具体术语中陈述了什么。

Gen 的左恒等律：该定律指出，如果你取 unit(a) 生成的值（它们始终为 a）并对这些值应用 f，这与 f(a) 返回的生成器完全相同。

Gen 的右恒等律：该定律指出，如果你对生成器 a 内部的值应用 unit，这在任何方面都与 a 本身不同。

List 的左恒等律：该定律说，将列表包装在单例 List 中然后展平结果，与不做任何操作是相同的。

List 的右恒等律：该定律说，如果你取列表中的每个值，将其包装在单例 List 中，然后展平结果，你得到的就是你开始时的列表。

## 练习 11.16

在此类上实现 map、flatMap 和 unit 作为方法，并为 Monad<Id> 提供实现。

```
data class Id<out A>(val a: A) : IdOf<A> {
    companion object {
        fun <A> unit(a: A): Id<A> = Id(a)
    }

    fun <B> flatMap(f: (A) -> Id<B>): Id<B> = f(this.a)
    fun <B> map(f: (A) -> B): Id<B> = unit(f(this.a))
}

class ForId private constructor() {
    companion object
}

typealias IdOf<A> = Kind<ForId, A>

fun <A> IdOf<A>.fix() = this as Id<A>

val idMonad = object : Monad<ForId> {
    override fun <A> unit(a: A): IdOf<A> =
        Id.unit(a)

    override fun <A, B> flatMap(fa: IdOf<A>, f: (A) -> IdOf<B>): IdOf<B> =
        fa.fix().flatMap { a -> f(a).fix() }

    override fun <A, B> map(fa: IdOf<A>, f: (A) -> B): IdOf<B> =
        fa.fix().map(f)
}
```

## 练习 11.17

现在我们有了 State Monad，试着使用它来看看它的行为如何。声明 replicateM、map2 和 sequence 的一些值，使用上面的 intMonad 进行类型声明。描述每个在幕后如何行为？

```
val replicateIntState: StateOf<Int, List<Int>> =
    intMonad.replicateM(5, stateA)

val map2IntState: StateOf<Int, Int> =
    intMonad.map2(stateA, stateB) { a, b -> a * b }

val sequenceIntState: StateOf<Int, List<Int>> =
    intMonad.sequence(List.of(stateA, stateB))
```

对于 State，replicateM 重复相同的状态转换多次并返回结果列表。它不是多次传递相同的起始状态，而是将调用链接在一起，以便一个的输出状态是下一个的输入状态。

map2 的工作方式类似，它取两个状态转换并将一个的输出状态馈送到另一个的输入。输出不会放入列表中，而是与函数 f 组合。

sequence 取整个状态转换列表并做与 replicateM 相同的事情：它将第一个状态转换的输出状态馈送到下一个的输入状态，以此类推。结果累积在列表中。

## 练习 11.18

表达你期望 getState、setState、unit 和 flatMap 共同成立的定律？

```
getState<Int>().flatMap { a -> setState(a) } == unit<Int, Unit>(Unit)

setState<Int>(1).flatMap { _ -> getState<Int>() } == unit<Int, Int>(1)
```

## 练习 11.19 (困难)

为了巩固你对 Monad 的理解，给 Reader 数据类型提供一个 Monad 实例并解释其含义。同时，花一些时间回答以下问题：

- 它的基本操作是什么？
- flatMap 的作用是什么？
- 它对 sequence、join 和 replicateM 等 Monad 函数赋予了什么含义？
- 它对 Monad 定律赋予了什么含义？

```
sealed class ForReader private constructor() {
    companion object
}

typealias ReaderOf<R, A> = Kind2<ForReader, R, A>

typealias ReaderPartialOf<R> = Kind<ForReader, R>

fun <R, A> ReaderOf<R, A>.fix() = this as Reader<R, A>

interface ReaderMonad<R> : Monad<ReaderPartialOf<R>>

data class Reader<R, A>(val run: (R) -> A) : ReaderOf<R, A> {
    companion object {
        fun <R, A> unit(a: A): Reader<R, A> = Reader { a }
    }

    fun <B> map(f: (A) -> B): Reader<R, B> =
        this.flatMap { a: A -> unit<R, B>(f(a)) }

    fun <B> flatMap(f: (A) -> Reader<R, B>): Reader<R, B> =
        Reader { r: R -> f(run(r)).run(r) }

    fun <A> ask(): Reader<R, R> = Reader { r -> r }
}

fun <R> readerMonad() = object : ReaderMonad<R> {
    override fun <A> unit(a: A): ReaderOf<R, A> =
        Reader { a }

    override fun <A, B> flatMap(
        fa: ReaderOf<R, A>,
        f: (A) -> ReaderOf<R, B>
    ): ReaderOf<R, B> =
        fa.fix().flatMap { a -> f(a).fix() }
}
```
