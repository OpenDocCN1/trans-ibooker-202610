
```
fun <A, B> foldRight(z: B, f: (A, B) -> B): B

fun <A, B> foldLeft(z: B, f: (B, A) -> B): B
```

我们看到初始值 z、合并函数 (A, B) -> B 以及从初始值传递而来的结果类型 B。

如果我们把 A 和 B 合并成一个单一类型并称之为 A 会怎样？

```
fun <A> foldRight(z: A, f: (A, A) -> A): A

fun <A> foldLeft(z: A, f: (A, A) -> A): A
```

幺半群的各个组成部分就像手套一样完美契合这些参数类型。因此，如果我们有一个 List<String>，words，我们就可以简单地传递 stringMonoid 的 combine 和 nil 来用幺半群归约列表并连接所有字符串。让我们在 REPL 中尝试一下：

```
>>> val words = listOf("Hic", "Est", "Index")
res0: kotlin.collections.List<kotlin.String> = [Hic, Est, Index]

>>> words.foldRight(stringMonoid.nil, stringMonoid::combine)
res1: kotlin.String = HicEstIndex

>>> words.foldLeft(stringMonoid.nil, stringMonoid::combine)
res2: kotlin.String = HicEstIndex
```

请注意，使用幺半群进行折叠时，选择 foldLeft 还是 foldRight 都无关紧要。我们在两种情况下都应得到相同的结果。这正是由于结合律和恒等律成立。左折叠将操作关联到左侧，而右折叠将操作关联到右侧，恒等元分别位于最左和最右：

```
>>> words.foldLeft("") { a, b -> a + b } == (("" + "Hic") + "Est") + "Index"
res3: kotlin.Boolean = true

>>> words.foldRight("") { a, b -> a + b } == "Hic" + ("Est" + ("Index" + ""))
res4: kotlin.Boolean = true
```

有了这些知识，我们现在可以编写一个名为 concatenate 的函数，用它来用幺半群折叠列表。

```
fun <A> concatenate(la: List<A>, m: Monoid<A>): A =
  la.foldLeft(m.nil, m::combine)
```

在某些情况下，元素类型可能没有 Monoid 实例。在这种情况下，我们总可以对列表进行映射，将其转换为具有关联实例的类型。

## 练习 10.5

函数 foldMap 用于对齐列表元素的类型，以便可以对列表应用 Monoid 实例。实现此函数。

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B = TODO()
```

## 练习 10.6（困难）

foldMap 函数可以使用 foldLeft 或 foldRight 实现。但你也可以使用 foldMap 来编写 foldLeft 和 foldRight。试试看，挺有意思的！

```
fun <A, B> foldRight(la: Sequence<A>, z: B, f: (A, B) -> B): B = TODO()

fun <A, B> foldLeft(la: Sequence<A>, z: B, f: (B, A) -> B): B = TODO()
```

## 10.3 结合性与并行

当我们想并行化这样一个过程时，从左到右顺序处理列表效率不高。随着列表大小的增长和计算变得更复杂，这一点变得越来越重要。我们可以利用幺半群的结合性来设计更高效的折叠列表的方法。本节将探讨如何使用一种称为平衡折叠的技术来实现这一点。它利用幺半群实现更高效的折叠，进而可用于并行计算。

平衡折叠到底是什么？让我们通过示例来看。

假设我们有一个序列 a、b、c、d，想用某个幺半群对其进行归约。向右折叠时，a、b、c、d 的组合看起来像这样：

```
combine(a, combine(b, combine(c, d)))
```

向左折叠看起来像这样：

```
combine(combine(combine(a, b), c), d)
```

而平衡折叠看起来像这样：

```
combine(combine(a, b), combine(c, d))
```

![](img/255e7d4fd2ecc6516db240b06be1018_219_0.png)

图 10.3 平衡折叠将工作负载拆分为相等组以实现高效处理

请注意，平衡折叠允许并行，因为两个内部的 combine 调用是独立的，可以同时运行。除此之外，更平衡的树结构在每次 combine 的成本与其参数大小成比例的情况下可能更高效。例如，考虑以下表达式的运行时性能：

```
listOf("lorem", "ipsum", "dolor", "sit")
.foldLeft("") { a, b -> a + b }
```

在折叠的每一步，我们都在分配完整的中间 String，然后丢弃它并在下一步分配更大的字符串。请记住，String 值是不可变的，评估字符串 a 和 b 的 a + b 需要分配一个新的字符数组并将 a 和 b 都复制到这个新数组中。它花费的时间与 a.length + b.length 成正比。

我们可以通过追踪前面表达式的求值来确认这一点：

```
listOf("lorem", "ipsum", "dolor", "sit")
    .foldLeft("") { a, b -> a + b }

listOf("ipsum", "dolor", "sit")
    .foldLeft("lorem") { a, b -> a + b }

listOf("dolor", "sit")
    .foldLeft("loremipsum") { a, b -> a + b }

listOf("sit")
    .foldLeft("loremipsumdolor") { a, b -> a + b }

listOf<String>()
    .foldLeft("loremipsumdolorsit") { a, b -> a + b }

"loremipsumdolorsit"
```

请注意中间字符串在每一步都被创建然后立即被丢弃。更高效的策略是前面描述的平衡折叠。这里我们将序列按一半组合，首先构造 "loremipsum" 和 "dolorsit"，然后最终将它们加在一起形成 "loremipsumdolorsit"。

## 练习 10.7

基于 *平衡折叠* 技术实现 `foldMap`。你的实现应使用将序列拆分为两半、递归处理每一半，然后使用提供的幺半群将答案加在一起的策略。

```
fun <A, B> foldMap(la: List<A>, m: Monoid<B>, f: (A) -> B): B = TODO()
```

## 练习 10.8

还使用我们在第 7 章开发的库实现一个名为 `parFoldMap` 的 *并行* 版本的 `foldMap`。

### 提示

实现 `par`，一个组合子，将 `Monoid<A>` 提升为 `Monoid<Par<A>>`，然后用它来实现 `parFoldMap`。

## 练习

```
fun <A> par(m: Monoid<A>): Monoid<Par<A>> = TODO()

fun <A, B> parFoldMap(
    la: List<A>,
    pm: Monoid<Par<B>>,
    f: (A) -> B
): Par<B> = TODO()
```

## 练习 10.9（困难/可选）

使用练习 10.7 中开发的 `foldMap` 来检测 `List<Int>` 是否升序。这需要在推导合适的 `Monoid` 实例时发挥一些创造力。

```
fun ordered(ints: Sequence<Int>): Boolean = TODO()
```

## 10.4 示例：并行解析

到目前为止，我们一直在看一些在日常工作中几乎没有用处的平凡示例。即使断言列表顺序可能有点用处，我们也将把它应用到像现实世界中会遇到的非平凡用例。词频统计程序就是这样一个很好的例子。

对于我们的示例，假设我们想统计一个 `String` 中的单词数。这是一个相当简单的解析问题。我们可以逐字符扫描字符串，查找空白并统计连续非空白字符的运行次数。

像这样顺序解析时，解析器状态可能简单到只需跟踪上一个看到的字符是否是空白。

对于短字符串来说这很好，但想象一下对一个可能太大而无法放入单台机器内存的巨大文本文件执行此操作。如果我们能并行处理文件的块就太好了。策略是将文件拆分为可管理的块，并行处理多个块，然后合并结果。在这种情况下，解析器状态需要稍微复杂一些，并且我们需要能够合并中间结果，而不管我们查看的部分是在文件的开头、中间还是结尾。换句话说，我们希望合并操作是结合性的。

为了保持简单和具体，让我们考虑一个短字符串并假装它是大型文件：

```
"lorem ipsum dolor sit amet, "
```

如果我们大致将这个字符串分成两半，可能会在单词中间拆分。对于我们的字符串，这将产生 "lorem ipsum do" 和 "lor sit amet, "。当我们把这些字符串中单词计数的结果加起来时，我们希望避免重复计算单词 dolor。显然，仅将单词计数为 Int 是不够的。我们需要找到一种数据结构，可以处理像 do 和 lor 这样的半个单词的部分结果，并能跟踪到目前为止看到的完整单词，如 ipsum、sit 和 amet。我们可以用以下代数数据类型来表示它。

### 清单 10.2 词数计数的部分结果的代数数据类型表示

```
sealed class WC

data class Stub(val chars: String) : WC()
data class Part(val ls: String, val words: Int, val rs: String) : WC()
```

- ① Stub 是构成部分单词的字符的累积
- ② Part 包含左 stub、单词计数和右 stub。

Stub 是最简单的情况，此时我们还没有看到任何完整单词。Part 将到目前为止看到的完整单词计数保存在整数 words 中。值 ls 保存这些单词左侧看到的任何部分单词，rs 保存右侧的任何部分单词。

例如，对字符串 "lorem ipsum do" 进行计数会得到 Part ("lorem", 1, "do")，因为有一个确定的完整单词 "ipsum"。由于 lorem 左侧没有空白，do 右侧也没有空白，我们无法确定它们是否是完整单词，因此不计数。对 "lor sit amet, " 进行计数会得到 Part("lor", 2, "")，逗号被丢弃。
