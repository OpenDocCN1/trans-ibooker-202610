
```
override val nil: Boolean = false
}

val booleanAnd: Monoid<Boolean> = object : Monoid<Boolean> {
    override fun combine(a1: Boolean, a2: Boolean): Boolean = a1 && a2
    override val nil: Boolean = true
}
```

## 练习 10.2

给出一个用于合并 Option 值的 Monoid 实例。

```
fun <A> optionMonoid(): Monoid<Option<A>> = object : Monoid<Option<A>> {
    override fun combine(a1: Option<A>, a2: Option<A>): Option<A> =
        a1.orElse { a2 }
    override val nil: Option<A> = None
}

fun <A> dual(m: Monoid<A>): Monoid<A> = object : Monoid<A> {
    override fun combine(a1: A, a2: A): A = m.combine(a2, a1)
    override val nil: A = m.nil
}

fun <A> firstOptionMonoid() = optionMonoid<A>()
fun <A> lastOptionMonoid() = dual(firstOptionMonoid<A>())
```

请注意，我们在实现 op 时有选择。我们可以按任一顺序组合选项。两种实现都满足幺半群定律，但它们并不等价。这在一般情况下是成立的——也就是说，每个幺半群都有一个对偶，其中 op 以相反的顺序组合事物。像 booleanOr 和 intAddition 这样的幺半群与其对偶等价，因为它们的 op 既是可交换的也是可结合的。

## 练习 10.3

具有相同参数和返回类型的函数有时被称为自同态函数。54 编写自同态函数的幺半群。

```
fun <A> endoMonoid(): Monoid<(A) -> A> =
    object : Monoid<(A) -> A> {
        override fun combine(a1: (A) -> A, a2: (A) -> A): (A) -> A =
            { a -> a1(a2(a)) }
        override val nil: (A) -> A
            get() = { a -> a }
    }

fun <A> endoMonoidComposed(): Monoid<(A) -> A> =
    object : Monoid<(A) -> A> {
        override fun combine(a1: (A) -> A, a2: (A) -> A): (A) -> A =
            a1 compose a2
        override val nil: (A) -> A
            get() = { it }
    }
```

## 练习 10.4

使用我们在第 8 章中开发的属性测试框架来实现结合律和幺元律的幺半群定律属性。使用你的属性来测试我们迄今为止编写的一些幺半群。

```
fun <A> monoidLaws(m: Monoid<A>, gen: Gen<A>) =
    forAll(
        gen.flatMap { a ->
            gen.flatMap { b ->
                gen.map { c ->
                    Triple(a, b, c)
                }
            }
        }
    ) { (a, b, c) ->
        m.combine(a, m.combine(b, c)) == m.combine(m.combine(a, b), c) &&
            m.combine(m.nil, a) == m.combine(a, m.nil) &&
            m.combine(m.nil, a) == a
    }

class Exercise4 : WordSpec({
    val max = 100
    val count = 100
    val rng = SimpleRNG(42)
    val intGen = Gen.choose(-10000, 10000)

    "law of associativity" should {
        "be upheld using existing monoids" {
            monoidLaws(intAdditionMonoid, intGen)
                .check(max, count, rng) shouldBe Passed

            monoidLaws(intMultiplicationMonoid, intGen)
                .check(max, count, rng) shouldBe Passed
        }
    }
})
```

## 练习 10.5

foldMap 函数用于对齐列表元素的类型，以便可以将 Monoid 实例应用于列表。实现此函数。

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B =
    la.foldLeft(m.nil, { b, a -> m.combine(b, f(a)) })
```

## 练习 10.6 (HARD)

foldMap 函数可以使用 foldLeft 或 foldRight 实现。但你也可以使用 foldMap 来编写 foldLeft 和 foldRight。试试看，挺有趣的！

```
fun <A, B> foldRight(la: Sequence<A>, z: B, f: (A, B) -> B): B =
    foldMap(la, endoMonoid()) { a: A -> { b: B -> f(a, b) } }(z)
```

函数类型 (A, B) B，柯里化后为 (A) (B) B。当然，对于任意 B，(B) B 都是一个幺半群（通过函数组合）。

```
fun <A, B> foldLeft(la: Sequence<A>, z: B, f: (B, A) -> B): B =
    foldMap(la, dual(endoMonoid())) { a: A -> { b: B -> f(b, a) } }(z)
```

向左折叠与此相同，只是我们翻转函数 f 的参数以将 B 放在正确的一侧。然后我们还必须“翻转”幺半群，使其从左到右操作。

## 练习 10.7

基于平衡折叠技术实现 foldMap。你的实现应使用将序列分成两半、递归处理每一半，然后使用提供的幺半群将答案加在一起的策略。

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B =
    when {
        la.size >= 2 -> {
            val (lal, la2) = la.splitAt(la.size / 2)
            m.combine(foldMap(lal, m, f), foldMap(la2, m, f))
        }
        la.size == 1 ->
            f(la.first())
        else -> m.nil
    }
```

## 练习 10.8 (HARD/OPTIONAL)

还使用我们在第 7 章中开发的库实现一个名为 parFoldMap 的并行版本的 foldMap。

```
fun <A> par(m: Monoid<A>): Monoid<Par<A>> = object : Monoid<Par<A>> {
    override fun combine(pa1: Par<A>, pa2: Par<A>): Par<A> =
        map2(pa1, pa2) { a1: A, a2: A ->
            m.combine(a1, a2)
        }
    override val nil: Par<A>
        get() = unit(m.nil)
}

fun <A, B> parFoldMap(
    la: List<A>,
    pm: Monoid<Par<B>>,
    f: (A) -> B
): Par<B> =
    when {
        la.size >= 2 -> {
            val (lal, la2) = la.splitAt(la.size / 2)
            pm.combine(parFoldMap(lal, pm, f), parFoldMap(la2, pm, f))
        }
        la.size == 1 ->
            unit(f(la.first()))
        else -> pm.nil
    }

parFoldMap(
    listOf("lorem", "ipsum", "dolor", "sit"),
    par(stringMonoid),
    { it.toUpperCase() }
)(es).invoke { cb -> result.set(cb) }
```

- 使用第 7 章中的 map2 来组合两个 Par 实例
- 使用第 7 章中的 unit 将零包装在 Par 中
- 使用 par 将 Monoid<A> 提升为 Monoid<Par<A>>
- 应用执行器服务并在 Future 上调用回调函数

## 练习 10.9 (HARD/OPTIONAL)

使用练习 10.7 中开发的 foldMap 来检测 List<Int> 的升序。这在推导适当的 Monoid 实例时需要一些创造力。

```
typealias TrackingState = Triple<Int, Int, Boolean>

val m = object : Monoid<Option<TrackingState>> {
    override fun combine(
        a1: Option<TrackingState>,
        a2: Option<TrackingState>
    ): Option<TrackingState> =
        when (a1) {
            is None -> a2
            is Some ->
                when (a2) {
                    is None -> a1
                    is Some -> Some(
                        Triple(
                            min(a1.t.first, a2.t.first),
                            max(a1.t.second, a2.t.second),
                            a1.t.third &&
                            a2.t.third &&
                            a1.t.second <= a2.t.first
                        )
                    )
                }
        }

    override val nil: Option<TrackingState> = None
}

fun ordered(ints: Sequence<Int>): Boolean =
    foldMap(ints, m) { i: Int -> Some(TrackingState(i, i, true)) }
        .map { it.third }
        .getOrElse { true }
```

## 练习 10.10

为 WC 编写一个幺半群实例，并确保它满足两个幺半群定律。

```
val wcMonoid: Monoid<WC> = object : Monoid<WC> {
    override fun combine(a1: WC, a2: WC): WC =
        when (a1) {
            is Stub -> when (a2) {
                is Stub ->
                    Stub(a1.chars + a2.chars)
                is Part ->
                    Part(a1.chars + a2.ls, a2.words, a2.rs)
            }
            is Part -> when (a2) {
                is Stub ->
                    Part(a1.ls, a1.words, a1.rs + a2.chars)
                is Part ->
                    Part(
                        a1.ls,
                        a1.words + a2.words +
                        (if ((a1.rs + a2.ls).isEmpty()) 0 else 1),
                        a2.rs
                    )
            }
        }

    override val nil: WC
        get() = Stub("")
}
```

## 练习 10.11

使用 wc 幺半群实现一个函数，通过递归地将 String 分成子字符串并统计这些子字符串中的单词来计算单词数。

```
fun wordCount(s: String): Int {

    fun wc(c: Char): WC =
        if (c.isWhitespace()) Part("", 0, "")
        else Stub("$c")

    fun unstub(s: String): Int = min(s.length, 1)

    return when (val wc = foldMap(s.asSequence(), wcMonoid) { wc(it) }) {
        is Stub -> unstub(wc.chars)
        is Part -> unstub(wc.ls) + wc.words + unstub(wc.rs)
    }
}
```

## 练习 10.12

以相互定义的方式在 Foldable<F> 接口上实现 foldLeft、foldRight 和 foldMap。

**注意**
以相互定义的方式使用所有这些函数可能会导致不期望的效果，如循环引用。我们将在练习 10.13 中解决这个问题。

```
interface Foldable<F> {

    fun <A, B> foldRight(fa: Kind<F, A>, z: B, f: (A, B) -> B): B =
        foldMap(fa, endoMonoid()) { a: A -> { b: B -> f(a, b) } }(z)

    fun <A, B> foldLeft(fa: Kind<F, A>, z: B, f: (B, A) -> B): B =
        foldMap(fa, dual(endoMonoid())) { a: A -> { b: B -> f(b, a) } }(z)

    fun <A, B> foldMap(fa: Kind<F, A>, m: Monoid<B>, f: (A) -> B): B =
        foldRight(fa, m.nil, { a, b -> m.combine(f(a), b) })
}
```

## 练习 10.13

使用上一个练习中的 Foldable<F> 接口实现 Foldable<ForList>。

**注意**
我们在第 3 章中开发的列表实现的可折叠版本已在第 10 章练习样板代码中提供。

```
object ListFoldable : Foldable<ForList> {

    override fun <A, B> foldRight(
```
