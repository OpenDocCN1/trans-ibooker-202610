
### 练习 8.11

不出所料，SGen 至少支持与 Gen 相同的许多操作，并且实现相当机械化。在 SGen 上定义一些便捷函数，它们只是委派给 Gen 上的对应函数。还提供一种便捷的方式来调用 SGen。

```
data class SGen<A>(val forSize: (Int) -> Gen<A>) {
    operator fun invoke(i: Int): Gen<A> = forSize(i)
    fun <B> map(f: (A) -> B): SGen<B> =
        SGen<B> { i -> forSize(i).map(f) }
    fun <B> flatMap(f: (A) -> Gen<B>): SGen<B> =
        SGen<B> { i -> forSize(i).flatMap(f) }
}
```

### 练习 8.12

实现一个在 Gen 上的 listOf 组合子，它不接受显式大小并且应该返回 SGen 而不是 Gen。实现应生成由 SGen 提供的大小的列表。

```
fun listOf(): SGen<List<A>> =
    SGen { i -> Gen.listOfN(i, this) }
```

### 练习 8.13

定义 nonEmptyListOf 用于生成非空列表，然后更新 max 的规范以使用此生成器。

```
fun <A> nonEmptyListOf(ga: Gen<A>): SGen<List<A>> =
    SGen { i -> Gen.listOfN(max(1, i), ga) }
val maxProp =
    Prop.forAll(nonEmptyListOf(smallInt)) { ns: List<Int> ->
        val mx = ns.max()
        ?: throw IllegalStateException("max on empty list")
        !ns.exists { it > mx }
    }
```

### 练习 8.14

编写一个名为 maxProp 的属性来验证 List.sorted 的行为，你可以用它来对（其中包括）List<Int> 进行排序。

```
val maxProp = forAll(SGen.listOf(smallInt)) { ns ->
    val nss = ns.sorted()
    nss.isEmpty() or ①
    (nss.size == 1) or ②
    nss.zip(nss.prepend(Int.MIN_VALUE))
        .foldRight(true, { p, b ->
            val (pa, pb) = p
            b && (pa >= pb)
        }) and ③
    nss.containsAll(ns) and ④
    !nss.exists { !ns.contains(it) } ⑤
}
```

- 1. 列表可能为空
- 2. 列表可能只有一个元素
- 3. 列表必须按升序排列
- 4. 列表必须包含未排序列表的所有元素
- 5. 列表可能*不*包含任何*不*在未排序列表中的元素

### 练习 8.15

编写一个更丰富的 Par<Int> 生成器，它构建比我们迄今提供的简单变体更深层的嵌套并行计算。

```
val pint2: Gen<Par<Int>> =
    Gen.choose(0, 20).flatMap { n ->
        Gen.listOfN(n, Gen.choose(-100, 100)).map { ls ->
            ls.foldLeft(unit(0)) { pint, i ->
                fork {
                    map2(pint, unit(i)) { a, b ->
                        a + b
                    }
                }
            }
        }
    }
```

### 练习 8.16

表达第 7 章中关于 fork 的属性 fork(x) == x。

```
forAllPar(pint) { x ->
    equal(fork { x }, x)
}
```

### 练习 8.17

想出 takeWhile 应满足的其他一些属性。你能想到一个表达 takeWhile 和 dropWhile 之间关系的良好属性吗？

```
l.takeWhile(f) + l.dropWhile(f) == l
```

```
val l = listOf(1, 2, 3, 4, 5)
val f = { i: Int -> i < 3 }
val res0 = l.takeWhile(f) + l.dropWhile(f)

assert(res0 == l)
```

我们希望强制 takeWhile 返回满足谓词的最长前缀。有各种方法来表述这一点，但总体思路是，剩余列表如果非空，应该以一个不满足谓词的元素开头。

### 第 9 章. 解析器组合子

### 练习 9.1

使用 product 实现现在熟悉的组合子 map2。进而，使用它来实现 many1 以 many 为基础。

```
override fun <A, B, C> map2(
    pa: Parser<A>,
    pb: () -> Parser<B>,
    f: (A, B) -> C
): Parser<C> =
    (pa product pb).map { (a, b) -> f(a, b) }

override fun <A> many1(p: Parser<A>): Parser<List<A>> =
    map2(p, { p.many() }) { a, b -> listOf(a) + b }
```

### 练习 9.2 (HARD)

尝试提出一些规律来指定 product 的行为。

```
(a product b) product c
a product (b product c)
```

product 组合子是结合的，所以两个表达式或多或少相等。这里唯一的差别是配对的嵌套方式。(a product b) product c 解析器返回 Pair<Pair<A, B>, C> 而 a product (b product c) 组合子返回 Pair<A, Pair<B, C>>。我们可以很容易地引入一些名为 unbiasL 和 unbiasR 的新函数，将这些结构展平为 Triples。

```
fun <A, B, C> unbiasL(p: Pair<Pair<A, B>, C>): Triple<A, B, C> =
    Triple(p.first.first, p.first.second, p.second)

fun <A, B, C> unbiasR(p: Pair<A, Pair<B, C>>): Triple<A, B, C> =
    Triple(p.first, p.second.first, p.second.second)
```

这现在允许我们如下表达结合律：

```
((a product b) product c).map(::unbiasL) ==
    (a product (b product c)).map(::unbiasR)
```

我们经常将这种两边的双射写成 ~=，如下表达式所示：

```
(a product b) product c ~= a product (b product c)
```

另一个有趣的观察是 map 和 product 之间的关系。可以先对两个解析器取 product 之前或之后进行 map，而不影响行为。

```
a.map(::f) product b.map(::g) ==
    (a product b).map { (a1, b1) -> Pair(f(a1), g(b1)) }
```

例如，如果 a 和 b 都是 Parser<String>，并且 f 和 g 都计算字符串的长度，那么在应用 product 之前先对 a 和 b 的结果进行 map 以计算各自的长度，或者在 product 之后进行，都无关紧要。

### 练习 9.3

在继续之前，看看你是否可以用 or、map2 和 succeed 来定义 many。

```
fun <A> many(pa: Parser<A>): Parser<List<A>> =
    map2(pa, many(pa)) { a, la ->
        listOf(a) + la
    } or succeed(emptyList())
```

### 练习 9.4

使用 map2 和 succeed 实现前面介绍的 listOfN 组合子。

```
fun <A> listOfN(n: Int, pa: Parser<A>): Parser<List<A>> =
    if (n > 0)
        map2(pa, listOfN(n - 1, pa)) { a, la ->
            listOf(a) + la
        }
    else succeed(emptyList())
```

### 练习 9.5

我们也可以像第 7 章那样使用单独的组合子来处理非严格性。提供一个名为 defer 的新组合子并对现有组合子进行必要的更改。你认为在这种情况下这种方法如何？

```
fun <A> defer(pa: () -> Parser<A>): Parser<A> = TODO()

fun <A> many(pa: Parser<A>): Parser<List<A>> =
    map2(pa, defer { many(pa) }) { a, la ->
        listOf(a) + la
    } or succeed(emptyList())
```

这种方法可能可行，但可以说造成的混淆比它值得的还要多。因此，我们将不引入它并保持我们的组合子免于延迟初始化的解析器。

### 练习 9.6

使用 flatMap 和任何其他组合子，编写我们之前无法表达的上下文相关解析器。结果应为返回读取字符数的 Parser<Int>。你可以使用名为 regex 的新原语来解析数字，它将正则表达式 String 提升为 Parser<String>。

```
val parser: Parser<Int> = regex("[0-9]+")
    .flatMap { digit: String ->
        val reps = digit.toInt()
        listOfN(reps, char('a')).map { _ -> reps }
    }
```

### 练习 9.7

用 flatMap 和 map 实现 product 和 map2。

```
fun <A, B> product(
    pa: Parser<A>,
    pb: Parser<B>
): Parser<Pair<A, B>> =
    pa.flatMap { a -> pb.map { b -> Pair(a, b) } }

fun <A, B, C> map2(
    pa: Parser<A>,
    pb: Parser<B>,
    f: (A, B) -> C
): Parser<C> =
    pa.flatMap { a -> pb.map { b -> f(a, b) } }
```

### 练习 9.8

map 不再是原语。用 flatMap 和/或其他组合子来表达它。

```
fun <A, B> map(pa: Parser<A>, f: (A) -> B): Parser<B> =
    pa.flatMap { a -> succeed(f(a)) }
```

### 练习 9.9 (HARD)

在这一点上，你将接管设计过程。你将从头开始使用我们定义的原语创建一个 Parser<JSON>。你暂时不需要担心 Parser 的表示。随着你的进展，你无疑会发现额外的组合子和惯用法，注意并提取常见模式，等等。利用你在整本书中培养的技能，并享受乐趣！

注意 本练习是关于仅定义由原语和组合子声明组成的代数。最终解决方案中不应出现实现。

```
abstract class Parsers<PE> {
    // primitives
    internal abstract fun string(s: String): Parser<String>
    internal abstract fun regex(r: String): Parser<String>
    internal abstract fun <A> slice(p: Parser<A>): Parser<String>
    internal abstract fun <A> succeed(a: A): Parser<A>
    internal abstract fun <A, B> flatMap(
        p1: Parser<A>,
        f: (A) -> Parser<B>
    ): Parser<B>
    internal abstract fun <A> or(
        p1: Parser<out A>,
        p2: () -> Parser<out A>
    ): Parser<A>
```
