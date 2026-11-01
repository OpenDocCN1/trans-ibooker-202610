## 第11章. 单子和函子

### 练习 11.1

为 Par、Option 和 List 编写单子实例。此外，为 arrow.core.ListK 和 arrow.core.SequenceK 提供单子实例。

注意 Arrow 提供的 ListK 和 SequenceK 类型是包装类，它们将平台等价物 List 和 Sequence 转换为完全配备的类型构造子。

提示 对于这些类型，unit 和 flatMap 组合子已经以各种方式实现。只需从你的 Monad 实现中调用它们即可。

### 练习 11.2 (HARD)

State 看起来也可能是一个单子，但它接受两个类型参数，即 S 和 A。你需要一个只有一个参数的类型构造子来实现 Monad。尝试实现一个 State 单子，看看会遇到什么问题，并思考是否以及如何解决这个问题。我们将在本章后面讨论解决方案。

提示 由于 State 是一个二元类型构造子，我们需要用 s 类型参数对其进行部分应用，就像对部分应用的函数所做的那样。因此，它不仅仅是一个单子，而是一整个单子族，每个类型 s 一个。考虑设计一种方法来在类型级作用域中捕获类型 s，并在该作用域中提供部分应用的 State 类型。这应该可以使用 Arrow 的 Kind2 接口实现。

### 练习 11.3

sequence 和 traverse 组合子现在应该对你很熟悉了，你在前几章中对它们的实现可能都非常相似。在 Monad<F> 上一次性实现它们。

提示 这些实现应与前几章中的实现非常相似，只是类型更通用。考虑将折叠操作与在 Monad 上使用 unit 和 map2 结合起来作为你的解决方案。

### 练习 11.4

实现 replicateM 以生成 Kind<F, List<A>>，其中列表长度为 n。

提示 编写此函数有多种方法。例如，尝试填充长度为 n 的 List<Kind<F, A>> 并结合 Monad 接口上的另一个组合子。或者，使用简单递归构建封闭的列表。

### 练习 11.6 (HARD)

这是一个我们之前未见过的函数的例子。实现函数 filterM。它有点像 filter，只是函数从 (A) -> Boolean 变成了 (A) -> Kind<F, Boolean>。用单子等价物替换各种普通函数如 filter 通常会产生有趣的结果。实现此函数，然后思考它对 Par、Option 和 Gen 等各种数据类型意味着什么。

提示 首先对参数进行模式匹配。如果列表为空，我们唯一的选项是返回 unit(Nil)。

### 练习 11.7

在 Monad 中实现以下 Kleisli 组合函数。



```
fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C> = TODO()
```

提示
遵循类型推导出唯一可能的实现。

## 练习 11.8 (困难)

实现 flatMap 基于 compose 的抽象定义。通过这种方式，似乎我们找到了另一组最小的 Monad 组合子：compose 和 unit。

提示
考虑如果我们假设 A 为 Unit 会产生什么影响。

## 练习 11.9

使用以下值，证明用 compose 表示的恒等律等价于用 flatMap 表示的恒等律：

```
val f: (A) -> Kind<F, A>
val x: Kind<F, A>
val v: A
```

提示
将 compose 的每次出现替换为 flatMap，然后将类型为 A 的值 v 应用于每个等式两边。

## 练习 11.10

证明恒等律对 Option Monad 成立。

提示
我们应再次考虑 None 和 Some 两种情况，并对每个情况展开等式的左右两边。Monadic unit 可以表示为 { a: A Some(a) }，或者如果你更喜欢，也可以简写为 { Some(it) }。

## 练习 11.13 (困难/可选)

使用 join、map 和 unit 用 flatMap 重述 Monad 的结合律。

> 提示
> 重写定律时，考虑使用以下类型声明来表达你的解决方案：
> 
> 在重写解决方案时，尽可能使用恒等函数。

```
val f: (A) -> Kind<F, A>
val g: (A) -> Kind<F, A>
val x: Kind<F, A>
val y: Kind<F, Kind<F, Kind<F, A>>>
val z: (Kind<F, Kind<F, A>>) -> Kind<F, Kind<F, A>>
```

## 练习 11.16

在此类上实现 map、flatMap 和 unit 作为方法，并为 Monad<Id> 提供实现。

> 提示
> 实现 ForId、IdOf 和 fix() 扩展函数以提供更高阶类型，以便你可以表达 Monad<ForId>。

## 练习 11.18

表达你期望 getState、setState、unit 和 flatMap 共同成立的定律？

> 提示
> 你期望在调用 setState 之后 getState 返回什么？反过来呢？

## 练习 11.19 (困难)

为了巩固你对 monads 的理解，给出 Reader 数据类型的 monad 实例并解释其含义。它的原始操作是什么？flatMap 的作用是什么？它对像 sequence、join 和 replicateM 这样的 monadic 函数赋予了什么含义？它对 monadic 定律赋予了什么含义？

> 提示
> 这个 monad 与 State monad 非常相似，只是它是“只读”的。你可以从中“询问”，但不能“设置” flatMap 携带的 R 值。

## 附录 B. 练习解答

![](img/255e7d4fd2ecc6516db2405b06be1018_279_0.png)

### B.1 在查看解答之前

本节包含书中所有练习的解答。请在跳过本节获取答案之前，尽力完成练习。本书的编写方式使练习成为你学习体验的重要组成部分。每个练习都建立在之前练习所获得的知识之上。**请仅使用本节来验证你的答案，或在你完全卡住时寻求帮助。**

## 第 2 章. 开始使用函数式编程

### 2.1

编写一个递归函数来获取第 n 个斐波那契数 (en.wikipedia.org/wiki/Fibonacci_number)。前两个斐波那契数是 0 和 1。第 n 个数始终是前两个数的和——序列开始为 0, 1, 1, 2, 3, 5, 8, 13, 21。你的定义应使用局部尾递归函数。

```
fun fib(i: Int): Int {
    tailrec fun go(cnt: Int, curr: Int, nxt: Int): Int =
        if (cnt == 0)
            curr
        else go(cnt - 1, nxt, curr + nxt)
    return go(i, 0, 1)
}
```

### 2.2

实现 isSorted，用于检查 List<A> 是否根据给定的比较函数排序。该函数前面有两个扩展属性，可为任何 List 值添加 .head 和 .tail。

```
val <T> List<T>.tail: List<T>
    get() = drop(1)

val <T> List<T>.head: T
    get() = first()
```

```
fun <A> isSorted(aa: List<A>, order: (A, A) -> Boolean): Boolean {
    tailrec fun go(x: A, xs: List<A>): Boolean =
        if (xs.isEmpty()) true
        else if (!order(x, xs.head)) false
        else go(xs.head, xs.tail)

    return aa.isEmpty() || go(aa.head, aa.tail)
}
```

### 2.3

让我们看另一个例子，currying，它将两个参数的函数 f 转换为一个参数的函数，该函数对 f 进行部分应用。

```
fun <A, B, C> curry(f: (A, B) -> C): (A) -> (B) -> C =
    { a: A -> { b: B -> f(a, b) } }
```

### 2.4

实现 uncurry，它逆转 curry 的转换。请注意，由于 -> 右结合，(A) -> ((B) -> C) 可以写成 (A) -> (B) -> C。

```
fun <A, B, C> uncurry(f: (A) -> (B) -> C): (A, B) -> C =
    { a: A, b: B -> f(a)(b) }
```

### 2.5

实现组合两个函数的高阶函数。

```
fun <A, B, C> compose(f: (B) -> C, g: (A) -> B): (A) -> C =
    { a: A -> f(g(a)) }
```

## 第 3 章. 函数式数据结构

### 练习 3.1

实现 tail 函数以移除 List 的第一个元素。请注意，该函数耗时恒定。如果 List 为 Nil，你在实现中可以做出哪些不同的选择？我们将在下一章回到这个问题。

```
fun <A> tail(xs: List<A>): List<A> =
    when (xs) {
        is Cons -> xs.tail
        is Nil ->
            throw IllegalStateException("Nil cannot have a `tail`")
    }
```

### 练习 3.2

使用相同的思路，实现 setHead 函数以用不同值替换 List 的第一个元素。

```
fun <A> setHead(xs: List<A>, x: A): List<A> =
    when (xs) {
        is Nil ->
            throw IllegalStateException(
                "Cannot replace `head` of a Nil list"
            )
        is Cons -> Cons(x, xs.tail)
    }
```

### 练习 3.3

将 `tail` 推广为 `drop` 函数，它从列表中移除前 n 个元素。请注意，该函数耗时仅与被丢弃的元素数量成比例——我们不需要复制整个 `List`。

```
tailrec fun <A> drop(l: List<A>, n: Int): List<A> =
    if (n == 0) l
    else when (l) {
        is Cons -> drop(l.tail, n - 1)
        is Nil -> throw IllegalStateException(
            "Cannot drop more elements than in list"
        )
    }
```

### 练习 3.4

实现 `dropWhile`，它只要元素匹配谓词就从 List 前缀中移除元素。

```
tailrec fun <A> dropWhile(l: List<A>, f: (A) -> Boolean): List<A> =
    when (l) {
        is Cons ->
            if (f(l.head)) dropWhile(l.tail, f) else l
        is Nil -> l
    }
```

### 练习 3.5

并非所有事情都如此顺利。实现一个函数 `init`，它返回一个由 `List` 中除最后一个元素外的所有元素组成的 `List`。因此，给定 `List(1,2,3,4)`，`init` 将返回 `List(1,2,3)`。为什么这个函数不能像 `tail` 一样在恒定时间内实现？

```
fun <A> init(l: List<A>): List<A> =
    when (l) {
        is Cons ->
            if (l.tail == Nil) Nil
            else Cons(l.head, init(l.tail))
        is Nil ->
            throw IllegalStateException("Cannot init Nil list")
    }
```

### 练习 3.6

使用 `foldRight` 实现的 `product`，如果遇到 `0.0` 能否立即停止递归并返回 `0.0`？为什么或为什么不？考虑如果使用大型列表调用 `foldRight`，任何短路如何工作。这个问题有更深的含义，我们将在第 5 章中再次讨论。

No, this is not possible! The reason is because *before* we ever call our function, *f*, we evaluate its argument, which in the case of `foldRight` means traversing the list all the way to the end.

我们需要 *非严格* 求值来支持提前终止——我们在第 5 章中讨论这个问题。

### 练习 3.7

看看当你像这样将 Nil 和 Cons 本身传递给 foldRight 会发生什么：

```
fun <A, B> foldRight(xs: List<A>, z: B, f: (A, B) -> B): B =
    when (xs) {
        is Nil -> z
        is Cons -> f(xs.head, foldRight(xs.tail, z, f))
    }

val f = { x: Int, y: List<Int> -> Cons(x, y) }
val z = Nil as List<Int>

val trace = {
    foldRight(List.of(1, 2, 3), z, f)
    Cons(1, foldRight(List.of(2, 3), z, f))
    Cons(1, Cons(2, foldRight(List.of(3), z, f)))
    Cons(1, Cons(2, Cons(3, foldRight(List.empty(), z, f))))
    Cons(1, Cons(2, Cons(3, Nil)))
}
```

你认为这说明了 foldRight 与 List 的数据构造器之间有什么关系？

*Replacing z and f with Nil and Cons respectively when invoking foldRight results in xs being copied.*

### 练习 3.8

使用 foldRight 计算列表的长度。

```
fun <A> length(xs: List<A>): Int =
    foldRight(xs, 0, { _, acc -> 1 + acc })
```

### 练习 3.9

我们的 foldRight 实现不是尾递归的，对于大型列表会导致 StackOverflowError（我们说它不是栈安全的）。说服自己这是事实，然后使用我们在上一章讨论的技术编写另一个通用的列表递归函数 foldLeft，使其为尾递归。

```
tailrec fun <A, B> foldLeft(xs: List<A>, z: B, f: (B, A) -> B): B =
    when (xs) {
        is Nil -> z
        is Cons -> foldLeft(xs.tail, f(z, xs.head), f)
    }
```

### 练习 3.10

使用 foldLeft 编写 sum、product 以及计算列表长度的函数。

```
fun sumL(xs: List<Int>): Int =
    foldLeft(xs, 0, { x, y -> x + y })
```


```
fun productL(xs: List<Double>): Double =
    foldLeft(xs, 1.0, { x, y -> x * y })

fun <A> lengthL(xs: List<A>): Int =
    foldLeft(xs, 0, { acc, _ -> acc + 1 })
```

## 练习 3.11

编写一个返回列表反转的函数（给定 List(1,2,3) 它返回 List(3,2,1)）。看看你能否使用折叠来实现它。

```
fun <A> reverse(xs: List<A>): List<A> =
    foldLeft(xs, List.empty(), { t: List<A>, h: A -> Cons(h, t) })
```

## 练习 3.12（困难）

你能用 foldRight 来表达 foldLeft 吗？反过来呢？通过 foldLeft 实现 foldRight 很有用，因为它让我们可以尾递归地实现 foldRight，这意味着即使对于大型列表也能工作而不会导致栈溢出。

```
fun <A, B> foldLeftR(xs: List<A>, z: B, f: (B, A) -> B): B =
    foldRight(
        xs,
        { b: B -> b },
        { a, g ->
            { b ->
                g(f(b, a))
            }
        }
    )(z)

fun <A, B> foldRightL(xs: List<A>, z: B, f: (A, B) -> B): B =
    foldLeft(xs,
        { b: B -> b },
        { g, a ->
            { b ->
                g(f(a, b))
            }
        }
    )(z)

//expanded example
typealias Identity<B> = (B) -> B

fun <A, B> foldLeftRDemystified(
    ls: List<A>,
    acc: B,
    combiner: (B, A) -> B
): B {
    val identity: Identity<B> = { b: B -> b }
    val combinerDelayer: (A, Identity<B>) -> Identity<B> =
        { a: A, delayedExec: Identity<B> ->
            { b: B ->
                delayedExec(combiner(b, a))
            }
        }
    val chain: Identity<B> = foldRight(ls, identity, combinerDelayer)
    return chain(acc)
}
```

foldLeft 处理元素的顺序与 foldRight 相反。在这里使用 reverse 是作弊，因为它本身是基于 foldLeft 实现的！相反，将每个操作包装在简单的恒等函数中以延迟求值，直到稍后，再将函数堆叠（嵌套）起来，以便可以反转应用的顺序。我们将为这种特定的恒等/延迟函数 Identity<B> 创建别名，这样就不用到处写 (B) -> B 了。

接下来，我们声明 Identity 的一个简单值 innerIdentity，它将简单地作为其值的直通。这将是内部 foldRight 的恒等值。

对于 ls 列表中的每个元素（a 参数），创建一个新的延迟函数，该函数将在稍后求值时使用 combiner 函数（作为参数传递给 foldLeftR_2 函数）。每个新函数都成为前一个 delayExec 函数的输入。

然后我们通过 go 函数将原始列表 ls，加上简单的恒等函数和新的 combinerDelayer 传递给 foldRight。这将为延迟求值创建带有组合器的函数，但不会调用这些函数中的任何一个。

最后，调用 go 函数，这将导致每个元素被延迟求值。

## 练习 3.13

使用 foldLeft 或 foldRight 实现 append。

```
fun <A> append(a1: List<A>, a2: List<A>): List<A> =
    foldRight(a1, a2, { x, y -> Cons(x, y) })
```

## 练习 3.14（困难）

你能用 foldRight 来表达 foldLeft 吗？反过来呢？通过 foldLeft 实现 foldRight 很有用，因为它让我们可以尾递归地实现 foldRight，这意味着即使对于大型列表也能工作而不会导致栈溢出。

```
fun <A> concat(xxs: List<List<A>>): List<A> =
    foldRight(
        xxs,
        List.empty(),
        { xs1: List<A>, xs2: List<A> ->
            foldRight(xs1, xs2, { a, ls -> Cons(a, ls) })
        })

fun <A> concat2(xxs: List<List<A>>): List<A> =
    foldRight(
        xxs,
        List.empty(),
        { xs1, xs2 ->
            append(xs1, xs2)
        })
```

## 练习 3.15

编写一个函数，通过给每个元素加 1 来转换整数列表。这应该是一个纯函数，返回一个新的 List。

```
fun increment(xs: List<Int>): List<Int> =
    foldRight(
        xs,
        List.empty(),
        { i: Int, ls ->
            Cons(i + 1, ls)
        })
```

## 练习 3.16

编写一个函数，将 List<Double> 中的每个值转换为 String。你可以使用表达式 d.toString() 将某个 d: Double 转换为 String。

```
fun doubleToString(xs: List<Double>): List<String> =
    foldRight(
        xs,
        List.empty(),
        { d, ds ->
            Cons(d.toString(), ds)
        })
```

## 练习 3.17

编写一个 map 函数，将修改列表中每个元素进行泛化，同时保持列表的结构。使用使用 foldLeft 的 foldRight 变体，以防止大型列表导致栈溢出。

```
fun <A, B> map(xs: List<A>, f: (A) -> B): List<B> =
    foldRightL(
        xs,
        List.empty(),
        { a, xa -> Cons(f(a), xa) })
```

## 练习 3.18

编写一个 filter 函数，除非元素满足给定谓词，否则从列表中移除元素。用它来从 List<Int> 中移除所有奇数。

```
fun <A> filter(xs: List<A>, f: (A) -> Boolean): List<A> =
    foldRight(
        xs,
        List.empty(),
        { a, ls ->
            if (f(a)) Cons(a, ls)
            else ls
        })
```

## 练习 3.19

编写一个 flatMap 函数，它的工作方式类似于 map，只是给定的函数将返回一个列表而不是单个结果，并且该列表应插入到最终结果列表中。以下是其签名：

```
fun <A, B> flatMap(xa: List<A>, f: (A) -> List<B>): List<B> =
    foldRight(
        xa,
        List.empty(),
        { a, lb ->
            append(f(a), lb)
        })
```

```
fun <A, B> flatMap2(xa: List<A>, f: (A) -> List<B>): List<B> =
    foldRight(
        xa,
        List.empty(),
        { a, xb ->
            foldRight(f(a), xb, { b, lb -> Cons(b, lb) })
        }
    )
```

## 练习 3.20

使用 flatMap 实现 filter。

```
fun <A> filter2(xa: List<A>, f: (A) -> Boolean): List<A> =
    flatMap(xa) { a ->
        if (f(a)) List.of(a) else List.empty()
    }
```

## 练习 3.21

编写一个函数，接受两个列表并通过相加对应元素构造一个新列表。例如，List(1,2,3) 和 List(4,5,6) 变为 List(5,7,9)。

```
fun add(xa: List<Int>, xb: List<Int>): List<Int> =
    when (xa) {
        is Nil -> Nil
        is Cons -> when (xb) {
            is Nil -> Nil
            is Cons ->
                Cons(xa.head + xb.head, add(xa.tail, xb.tail))
        }
    }
```

## 练习 3.22

将你刚刚编写的函数泛化，使其不特定于整数或加法。将你的泛化函数命名为 zipWith。

```
fun <A> zipWith(xa: List<A>, xb: List<A>, f: (A, A) -> A): List<A> =
    when (xa) {
        is Nil -> Nil
        is Cons -> when (xb) {
            is Nil -> Nil
            is Cons -> Cons(
                f(xa.head, xb.head),
                zipWith(xa.tail, xb.tail, f)
            )
        }
    }
```

## 练习 3.23

作为示例，实现 hasSubsequence 用于检查一个 List 是否包含另一个 List 作为子序列。例如，List(1,2,3,4) 将具有 List(1,2)、List(2,3) 和 List(4) 作为子序列，等等。你可能很难找到一个简洁且高效的纯函数实现。这没关系。无论如何最自然地实现该函数。我们将在第 5 章重新审视此实现并希望改进它。

```
tailrec fun <A> startsWith(l1: List<A>, l2: List<A>): Boolean =
    when (l1) {
        is Nil -> l2 == Nil
        is Cons -> when (l2) {
            is Nil -> true
            is Cons ->
                if (l1.head == l2.head)
                    startsWith(l1.tail, l2.tail)
                else false
        }
    }

tailrec fun <A> hasSubsequence(xs: List<A>, sub: List<A>): Boolean =
    when (xs) {
        is Nil -> false
        is Cons ->
            if (startsWith(xs, sub))
                true
            else hasSubsequence(xs.tail, sub)
    }
```

## 练习 3.24

编写一个 size 函数，计算树中节点的数量（叶子和分支）。

```
fun <A> size(tree: Tree<A>): Int =
    when (tree) {
        is Leaf -> 1
        is Branch -> 1 + size(tree.left) + size(tree.right)
    }
```

## 练习 3.25

编写一个 maximum 函数，返回 Tree<Int> 中的最大元素。

```
fun maximum(tree: Tree<Int>): Int =
    when (tree) {
        is Leaf -> tree.value
        is Branch -> maxOf(maximum(tree.left), maximum(tree.right))
    }
```

## 练习 3.26

编写一个 depth 函数，返回从树的根到任意叶子的最大路径长度。

```
fun depth(tree: Tree<Int>): Int =
    when (tree) {
        is Leaf -> 0
        is Branch -> 1 + maxOf(depth(tree.left), depth(tree.right))
    }
```

## 练习 3.27

编写一个 map 函数，类似于 List 上同名的方法，用给定函数修改树中的每个元素。

```
fun <A, B> map(tree: Tree<A>, f: (A) -> B): Tree<B> =
    when (tree) {
        is Leaf -> Leaf(f(tree.value))
        is Branch -> Branch(
            map(tree.left, f),
```



```
map(tree.right, f)
}
}
```

### 练习 3.28

概括 Tree 的 size、maximum、depth 和 map，编写一个新的函数 fold 来抽象它们的相似之处。用这个更通用的函数重新实现它们。你能在这个 fold 函数与 List 的左折叠和右折叠之间画出类比吗？

```
fun <A, B> fold(ta: Tree<A>, l: (A) -> B, b: (B, B) -> B): B =
    when (ta) {
        is Leaf -> l(ta.value)
        is Branch -> b(fold(ta.left, l, b), fold(ta.right, l, b))
    }

fun <A> sizeF(ta: Tree<A>): Int =
    fold(ta, { 1 }, { b1, b2 -> 1 + b1 + b2 })

fun maximumF(ta: Tree<Int>): Int =
    fold(ta, { a -> a }, { b1, b2 -> maxOf(b1, b2) })

fun <A> depthF(ta: Tree<A>): Int =
    fold(ta, { 0 }, { b1, b2 -> 1 + maxOf(b1, b2) })

fun <A, B> mapF(ta: Tree<A>, f: (A) -> B): Tree<B> =
    fold(ta, { a: A -> Leaf(f(a)) },
        { b1: Tree<B>, b2: Tree<B> -> Branch(b1, b2) })
```

## 第 4 章. 不使用异常处理错误

### 练习 4.1

实现 Option 上的以下所有函数。在实现每个函数时，尝试思考它的含义以及在什么情况下会使用它。

```
fun <A, B> Option<A>.map(f: (A) -> B): Option<B> =
    when (this) {
        is None -> None
        is Some -> Some(f(this.get))
    }

fun <A> Option<A>.getOrElse(default: () -> A): A =
    when (this) {
        is None -> default()
        is Some -> this.get
    }

fun <A, B> Option<A>.flatMap(f: (A) -> Option<B>): Option<B> =
    this.map(f).getOrElse { None }

fun <A> Option<A>.orElse(ob: () -> Option<A>): Option<A> =
    this.map { Some(it) }.getOrElse { ob() }

fun <A> Option<A>.filter(f: (A) -> Boolean): Option<A> =
    this.flatMap { a -> if (f(a)) Some(a) else None }
```

### 替代方法：

```
fun <A, B> Option<A>.flatMap_2(f: (A) -> Option<B>): Option<B> =
    when (this) {
        is None -> None
        is Some -> f(this.get)
    }
fun <A> Option<A>.orElse_2(ob: () -> Option<A>): Option<A> =
    when (this) {
        is None -> ob()
        is Some -> this
    }

fun <A> Option<A>.filter_2(f: (A) -> Boolean): Option<A> =
    when (this) {
        is None -> None
        is Some ->
            if (f(this.get)) this
            else None
    }
```

### 练习 4.2

用 flatMap 实现方差函数。如果序列的均值为 m，则方差是序列中每个元素 x 减去 m 的平方的均值。在代码中，这将是 (x - m).pow(2)。第 4.2 节中开发的 mean 方法可用于实现此功能。

//using `mean` method from listing 4.2
fun mean(xs: List<Double>): Option<Double> =
    if (xs.isEmpty()) None
    else Some(xs.sum() / xs.size())

fun variance(xs: List<Double>): Option<Double> =
    mean(xs).flatMap { m ->
        mean(xs.map { x ->
            (x - m).pow(2)
        })
    }

### 练习 4.3

编写一个通用函数 map2，使用二元函数组合两个 Option 值。如果任一 Option 值为 None，则返回值也为 None。

```
fun <A, B, C> map2(
    oa: Option<A>,
    ob: Option<B>,
    f: (A, B) -> C
): Option<C> =
    oa.flatMap { a ->
        ob.map { b ->
            f(a, b)
        }
    }
```

### 练习 4.4

编写一个函数 sequence，将 Option 的列表组合成一个包含原始列表中所有 Some 值的列表的 Option。如果原始列表甚至包含一次 None，则函数的结果应为 None；否则结果应为包含所有值的 Some。

```
fun <A> sequence(
    xs: List<Option<A>>
): Option<List<A>> =
    xs.foldRight(Some(Nil),
        { oal: Option<A>, oa2: Option<List<A>> ->
            map2(oal, oa2) { a1: A, a2: List<A> ->
                Cons(a1, a2)
            }
        }
    )
```

### 练习 4.5

实现 traverse 函数。使用 map 和 sequence 来实现很简单，但请尝试实现一个更高效的实现，只需查看列表一次。完成后，使用 traverse 实现 sequence。

```
fun <A, B> traverse(
    xa: List<A>,
    f: (A) -> Option<B>
): Option<List<B>> =
    when (xa) {
        is Nil -> Some(Nil)
        is Cons ->
            map2(f(xa.head), traverse(xa.tail, f)) { b, xb ->
                Cons(b, xb)
            }
    }

fun <A> sequence(xs: List<Option<A>>): Option<List<A>> =
    traverse(xs) { it }
```

### 练习 4.6

在 Either 上实现 map、flatMap、orElse 和 map2 的版本，使其在 Right 值上操作。

```
fun <E, A, B> Either<E, A>.map(f: (A) -> B): Either<E, B> =
    when (this) {
        is Left -> this
        is Right -> Right(f(this.value))
    }

fun <E, A> Either<E, A>.orElse(f: () -> Either<E, A>): Either<E, A> =
    when (this) {
        is Left -> f()
        is Right -> this
    }

fun <E, A, B> Either<E, A>.flatMap(f: (A) -> Either<E, B>): Either<E, B> =
    when (this) {
        is Left -> this
        is Right -> f(this.value)
    }

fun <E, A, B, C> map2(
    ae: Either<E, A>,
    be: Either<E, B>,
    f: (A, B) -> C
): Either<E, C> =
    ae.flatMap { a -> be.map { b -> f(a, b) } }
```

### 练习 4.7

为 Either 实现 `sequence` 和 `traverse`。如果有错误，这些应该返回遇到的第一个错误。

```
fun <E, A, B> traverse(
    xs: List<A>,
    f: (A) -> Either<E, B>
): Either<E, List<B>> =
    when (xs) {
        is Nil -> Right(Nil)
        is Cons ->
            map2(f(xs.head), traverse(xs.tail, f)) { b, xb ->
                Cons(b, xb)
            }
    }

fun <E, A> sequence(es: List<Either<E, A>>): Either<E, List<A>> =
    traverse(es) { it }
```

### 练习 4.8

在第 4.8 节中找到的实现中，即使姓名和年龄都无效，`map2` 也只能报告一个错误。要报告两个错误，你需要做出什么改变？你会改变 `map2` 还是 `mkPerson` 的签名？或者你能否创建一个新的数据类型，比 `Either` 更能捕捉这一需求，并具有一些附加结构？对于该数据类型，`orElse`、`traverse` 和 `sequence` 的行为会有什么不同？

`Option` 和 `Either` 有许多变体。如果我们想累积多个错误，一个简单的方法是创建一个新的数据类型，让我们可以在表示失败的数据构造函数中保留错误列表：

```
sealed class Partial<out A, out B>

data class Failures<out A>(val get: List<A>) : Partial<A, Nothing>()
data class Success<out B>(val get: B) : Partial<Nothing, B>()
```

Arrow 库中有一个非常相似的类型叫做 `Validated`。你可以为此类型实现 `map`、`map2`、`sequence` 等，使得在可能的情况下累积错误（`flatMap` 无法累积错误——你能看出为什么吗？）。这个想法甚至可以进一步推广——我们不需要将失败值累积到列表中；我们可以使用任何用户提供的二元函数来累积值。也可以直接使用 `Either<List<E>, _>` 来累积错误，使用像 `map2` 和 `sequence` 这样的辅助函数的不同实现。

## 第 5 章. 严格性与惰性

### 练习 5.1

编写一个函数将 Stream 转换为 List，这将强制其求值，让你可以在 REPL 中查看它，并在源代码仓库提供的单元测试中执行断言。你可以将其转换为我们在本书第 3 章中开发的单链表 List 类型。你可以使用扩展方法实现此函数以及操作 Stream 的其他函数。

//Unsafe! Naive solution could cause a stack overflow.
fun <A> Stream<A>.toListUnsafe(): List<A> = when (this) {
    is Empty -> NilL
    is Cons -> ConsL(this.head(), this.tail().toListUnsafe())
}

//Use tailrec in combination with reverse for a safer implementation
fun <A> Stream<A>.toList(): List<A> {
    tailrec fun go(xs: Stream<A>, acc: List<A>): List<A> = when (xs) {
        is Empty -> acc
        is Cons -> go(xs.tail(), ConsL(xs.head(), acc))
    }
    return reverse(go(this, NilL))
}

### 练习 5.2

编写函数 take(n) 用于返回 Stream 的前 n 个元素，以及 drop(n) 用于跳过 Stream 的前 n 个元素。

```
fun <A> Stream<A>.take(n: Int): Stream<A> {
    fun go(xs: Stream<A>, n: Int): Stream<A> = when (xs) {
        is Empty -> empty()
        is Cons ->
            if (n == 0) empty()
            else cons(xs.head, { go(xs.tail(), n - 1) })
    }
    return go(this, n)
}

fun <A> Stream<A>.drop(n: Int): Stream<A> {
    tailrec fun go(xs: Stream<A>, n: Int): Stream<A> = when (xs) {
        is Empty -> empty()
        is Cons ->
            if (n == 0) xs
            else go(xs.tail(), n - 1)
    }
    return go(this, n)
}
```

### 练习 5.3

编写函数 takeWhile 用于返回与给定谓词匹配的 Stream 的所有起始元素。

```
fun <A> Stream<A>.takeWhile(p: (A) -> Boolean): Stream<A> =
    when (this) {
        is Empty -> empty()
        is Cons ->
            if (p(this.head()))


```
cons(this.head, { this.tail().takeWhile(p) })
    else empty()
}
```

## 练习 5.4

实现 forAll，用于检查 Stream 中的所有元素是否都匹配给定的谓词。你的实现应在遇到不匹配的值时立即终止遍历。

```
fun <A> Stream<A>.forAll(p: (A) -> Boolean): Boolean =
    foldRight({ true }, { a, b -> p(a) && b() })
```

## 练习 5.5

使用 foldRight 实现 takeWhile。

```
fun <A> Stream<A>.takeWhile(p: (A) -> Boolean): Stream<A> =
    foldRight({ empty() },
        { h, t -> if (p(h)) cons({ h }, t) else t() })
```

## 练习 5.6 (困难)

使用 foldRight 实现 headOption。

```
fun <A> Stream<A>.headOption(): Option<A> =
    this.foldRight(
        { Option.empty() },
        { a, _ -> Some(a) }
    )
```

## 练习 5.7

使用 foldRight 实现 map、filter、append 和 flatMap。append 方法对其参数应是非严格的。

```
fun <A, B> Stream<A>.map(f: (A) -> B): Stream<B> =
    this.foldRight(
        { empty<B>() },
        { h, t -> cons({ f(h) }, t) })

fun <A> Stream<A>.filter(f: (A) -> Boolean): Stream<A> =
    this.foldRight(
        { empty<A>() },
        { h, t -> if (f(h)) cons({ h }, t) else t() })

fun <A> Stream<A>.append(sa: () -> Stream<A>): Stream<A> =
    foldRight(
        sa,
        { h, t -> cons({ h }, t) })

fun <A, B> Stream<A>.flatMap(f: (A) -> Stream<B>): Stream<B> =
    foldRight(
        { empty<B>() },
        { h, t -> f(h).append(t) })
```

## 练习 5.8

将 ones 稍微推广为函数 constant，它返回一个给定值的无限 Stream。

```
fun <A> constant(a: A): Stream<A> =
    Stream.cons({ a }, { constant(a) })
```

## 练习 5.9

编写一个函数，生成从 n 开始的无限整数流，然后是 n + 1、n + 2，依此类推。

```
fun from(n: Int): Stream<Int> =
    cons({ n }, { from(n + 1) })
```

## 练习 5.10

编写一个函数 fibs，生成无限的斐波那契数流：0、1、1、2、3、5、8，依此类推。

```
fun fibs(): Stream<Int> {
    fun go(curr: Int, nxt: Int): Stream<Int> =
        cons({ curr }, { go(nxt, curr + nxt) })
    return go(0, 1)
}
```

## 练习 5.11

编写一个更通用的流构建函数，名为 unfold。它接收一个初始状态，以及一个用于生成下一个状态和生成流中下一个值的函数。

```
fun <A, S> unfold(z: S, f: (S) -> Option<Pair<A, S>>): Stream<A> =
    f(z).map { pair ->
        cons({ pair.first }, {
            unfold(pair.second, f) })
    }.getOrElse {
        empty()
    }
```

## 练习 5.12

用 unfold 表示 fibs、from、constant 和 ones。

```
fun fibs(): Stream<Int> =
    unfold(Pair(0, 1), { (curr, next) ->
        Some(Pair(curr, Pair(next, curr + next)))
    })

fun from(n: Int): Stream<Int> =
    unfold(n, { a -> Some(Pair(a, a + 1)) })

fun <A> constant(n: A): Stream<A> =
    unfold(n, { a -> Some(Pair(a, a)) })

fun ones(): Stream<Int> =
    unfold(1, { Some(Pair(1, 1)) })
```

## 练习 5.13

使用 unfold 实现 map、take、takeWhile、zipWith（见第 3 章）和 zipAll。zipAll 函数应只要任一流仍有更多元素就继续遍历——它使用 Option 来指示每个流是否已耗尽。

```
fun <A, B> Stream<A>.map(f: (A) -> B): Stream<B> =
    unfold(this,
        { s: Stream<A> ->
            when (s) {
                is Cons -> Some(Pair(f(s.head()), s.tail()))
                else -> None
            }
        }
    )

fun <A> Stream<A>.take(n: Int): Stream<A> =
    unfold(this,
        { s: Stream<A> ->
            when (s) {
                is Cons ->
                    if (n > 0)
                        Some(Pair(s.head(), s.tail().take(n - 1)))
                    else None
                else -> None
            }
        }
    )

fun <A> Stream<A>.takeWhile(p: (A) -> Boolean): Stream<A> =
    unfold(this,
        { s: Stream<A> ->
            when (s) {
                is Cons ->
                    if (p(s.head()))
                        Some(Pair(s.head(), s.tail()))
                    else None
                else -> None
            }
        }
    )

fun <A, B, C> Stream<A>.zipWith(
    that: Stream<B>,
    f: (A, B) -> C
): Stream<C> =
    unfold(Pair(this, that)) { (ths: Stream<A>, tht: Stream<B>) ->
        when (ths) {
            is Cons ->
                when (tht) {
                    is Cons ->
                        Some(
                            Pair(
                                f(ths.head(), tht.head()),
                                Pair(ths.tail(), tht.tail())
                            )
                        )
                    else -> None
                }
            else -> None
        }
    }

fun <A, B> Stream<A>.zipAll(
    that: Stream<B>
): Stream<Pair<Option<A>, Option<B>>> =
    unfold(Pair(this, that)) { (ths, tht) ->
        when (ths) {
            is Cons -> when (tht) {
```
```
is Cons ->
    Some(
        Pair(
            Pair(Some(ths.head()), Some(tht.head())),
            Pair(ths.tail(), tht.tail())
        )
    )
else ->
    Some(
        Pair(
            Pair(Some(ths.head()), None),
            Pair(ths.tail(), empty<B>())
        )
    )
}
else -> when (tht) {
    is Cons ->
        Some(
            Pair(
                Pair(None, Some(tht.head())),
                Pair(empty<A>(), tht.tail())
            )
        )
    else -> None
}
}
```

## 练习 5.14 (困难)

使用你已编写的函数实现 startsWith。它应检查一个 Stream 是否是另一个 Stream 的前缀。例如，Stream(1,2,3) startsWith Stream(1,2) 将为 true。

```
fun <A> Stream<A>.startsWith(that: Stream<A>): Boolean =
    this.zipAll(that)
        .takeWhile { !it.second.isEmpty() }
        .forAll { it.first == it.second }
```

## 练习 5.15

使用 unfold 实现 tails。对于给定的 Stream，tails 返回输入序列的后缀流，从原始 Stream 开始。例如，给定 Stream.of(1,2,3)，它将返回 Stream.of(Stream.of(1,2,3), Stream.of(2,3), Stream.of(3), Stream.empty())。

```
fun <A> Stream<A>.tails(): Stream<Stream<A>> =
    unfold(this) { s: Stream<A> ->
        when (s) {
            is Cons ->
                Some(Pair(s, s.tail()))
            else -> None
        }
    }
```

## 练习 5.16 (困难)

将 tails 推广为函数 scanRight，它类似于返回中间结果流的 foldRight。例如：

```
>>> Stream.of(1, 2, 3).scanRight(0, { a, b -> a + b }).toList()
res1: chapter3.List<kotlin.Int> = Cons(head=6, tail=Cons(head=5, tail=Cons(head=3,
```
```
tail=Cons(head=0, tail=Nil)))
```

此示例应等价于表达式 List.of(1+2+3+0, 2+3+0, 3+0, 0)。你的函数应重用中间结果，以便遍历包含 n 个元素的 Stream 时时间始终为 n 的线性。能否使用 unfold 实现？如何实现，或为何不能？能否使用我们已编写的另一个函数实现？

```
fun <A, B> Stream<A>.scanRight(z: B, f: (A, () -> B) -> B): Stream<B> =
    foldRight({ Pair(z, Stream.of(z)) },
        { a: A, p0: () -> Pair<B, Stream<B>> ->
            val p1: Pair<B, Stream<B>> by lazy { p0() }
            val b2: B = f(a) { p1.first }
            Pair<B, Stream<B>>(b2, cons({ b2 }, { p1.second }))
        }).second
```

## 第 6 章 纯函数式状态

## 练习 6.1

编写一个函数，使用 RNG.nextInt 生成介于 0 和 Int.MAX_VALUE（包含）之间的随机整数。确保处理 nextInt 返回 Int.MIN_VALUE 的边界情况，该值没有非负对应值。

```
fun nonNegativeInt(rng: RNG): Pair<Int, RNG> {
    val (i1, rng2) = rng.nextInt()
    return Pair(if (i1 < 0) -(i1 + 1) else i1, rng2)
}
```

## 练习 6.2

编写一个函数生成介于 0 和 1 之间（不包括 1）的 Double。除了你已开发的函数外，你可以使用 Int.MAX_VALUE 获取最大正整数值，并可以使用 x.toDouble() 将 x: Int 转换为 Double。

```
fun double(rng: RNG): Pair<Double, RNG> {
    val (i, rng2) = nonNegativeInt(rng)
    return Pair(i / (Int.MAX_VALUE.toDouble() + 1), rng2)
}
```

## 练习 6.3

编写函数生成 Pair<Int, Double>、Pair<Double, Int> 和 Triple<Double, Double, Double>。你应该能够重用已编写的函数。

```
fun intDouble(rng: RNG): Pair<Pair<Int, Double>, RNG> {
    val (i, rng2) = rng.nextInt()
    val (d, rng3) = double(rng2)
    return Pair(Pair(i, d), rng3)
}

fun doubleInt(rng: RNG): Pair<Pair<Double, Int>, RNG> {
    val (id, rng2) = intDouble(rng)
    val (i, d) = id
    return Pair(Pair(d, i), rng2)
}
```



```
fun double3(rng: RNG): Pair<Triple<Double, Double, Double>, RNG> {
    val (d1, rng2) = doubleR(rng)
    val (d2, rng3) = doubleR(rng2)
    val (d3, rng4) = doubleR(rng3)
    return Pair(Triple(d1, d2, d3), rng4)
}
```

### 练习 6.4

编写一个函数来生成随机整数列表。

```
fun ints(count: Int, rng: RNG): Pair<List<Int>, RNG> =
    if (count > 0) {
        val (i, r1) = rng.nextInt()
        val (xs, r2) = ints(count - 1, r1)
        Pair(Cons(i, xs), r2)
    } else Pair(Nil, rng)
```

### 练习 6.5

使用 map 以更优雅的方式重新实现 doubleR。参见练习 6.2。

```
val doubleR: Rand<Double> =
    map(::nonNegativeInt) { i ->
        i / (Int.MAX_VALUE.toDouble() + 1)
    }
```

### 练习 6.6

基于以下签名编写 map2 的实现。此函数接收两个动作 ra 和 rb 以及一个用于组合其结果的函数 f，并返回一个组合它们的新的动作：

```
fun <A, B, C> map2(ra: Rand<A>, rb: Rand<B>, f: (A, B) -> C): Rand<C> =
    { r1: RNG ->
        val (a, r2) = ra(r1)
        val (b, r3) = rb(r2)
        Pair(f(a, b), r3)
    }
```

### 练习 6.7

如果你可以组合两个 RNG 转换，那么你应该能够组合整个列表。实现 sequence 以将转换的 List 组合成单个转换。利用它重新实现你之前编写的 ints 函数。为了本练习的简单性，允许使用递归来构建一个重复 n 次的 x 列表的方式编写 ints。

```
//using a simpler recursive strategy, could blow the stack
fun <A> sequence(fs: List<Rand<A>>): Rand<List<A>> = { rng ->
    when (fs) {
        is Nil -> unit(List.empty<A>())(rng)
        is Cons -> {
            val (a, nrng) = fs.head(rng)
            val (xa, frng) = sequence(fs.tail)(nrng)
            Pair(Cons(a, xa), frng)
        }
    }
}

//a better approach using foldRight
fun <A> sequence2(fs: List<Rand<A>>): Rand<List<A>> =
    foldRight(fs, unit(List.empty()), { f, acc ->
        map2(f, acc, { h, t -> Cons(h, t) })
    })

fun ints2(count: Int, rng: RNG): Pair<List<Int>, RNG> {
    fun go(c: Int): List<Rand<Int>> =
        if (c == 0) Nil
        else Cons({ r -> Pair(1, r) }, go(c - 1))
    return sequence2(go(count))(rng)
}
```

### 练习 6.8

实现 flatMap，然后使用它来实现 nonNegativeLessThan。

```
fun <A, B> flatMap(f: Rand<A>, g: (A) -> Rand<B>): Rand<B> =
{ rng ->
    val (a, rng2) = f(rng)
    g(a)(rng2)
}

fun nonNegativeIntLessThan(n: Int): Rand<Int> =
    flatMap(::nonNegativeInt) { i ->
        val mod = i % n
        if (i + (n - 1) - mod >= 0) unit(mod)
        else nonNegativeIntLessThan(n)
    }
```

### 练习 6.9

用 flatMap 的形式重新实现 map 和 map2。我们之所以说 flatMap 比 map 和 map2 更强大，正是因为这种实现是可能的。

```
fun <A, B> mapF(ra: Rand<A>, f: (A) -> B): Rand<B> =
    flatMap(ra) { a -> unit(f(a)) }

fun <A, B, C> map2F(
    ra: Rand<A>,
    rb: Rand<B>,
    f: (A, B) -> C
): Rand<C> =
    flatMap(ra) { a ->
        map(rb) { b ->
            f(a, b)
        }
    }
```

### 练习 6.10

将函数 unit、map、map2、flatMap 和 sequence 泛化。在可能的情况下将它们作为 State 数据类的方法添加。否则应将它们放在 State 伴生对象中。

```
data class State<S, out A>(val run: (S) -> Pair<A, S>) {

    companion object {
        fun <S, A> unit(a: A): State<S, A> =
            State { s: S -> Pair(a, s) }

        fun <S, A, B, C> map2(
            ra: State<S, A>,
            rb: State<S, B>,
            f: (A, B) -> C
        ): State<S, C> =
            ra.flatMap { a ->
                rb.map { b ->
                    f(a, b)
                }
            }

        fun <S, A> sequence(fs: List<State<S, A>>): State<S, List<A>> =
            foldRight(fs, unit(List.empty<A>()), { f, acc ->
                map2(f, acc) { h, t -> Cons(h, t) }
            })
    }

    fun <B> map(f: (A) -> B): State<S, B> =
        flatMap { a -> unit<S, B>(f(a)) }

    fun <B> flatMap(f: (A) -> State<S, B>): State<S, B> =
        State { s: S ->
            val (a: A, s2: S) = this.run(s)
            f(a).run(s2)
        }
}
```

### 练习 6.11（困难/可选）

为了获得使用 State 的经验，实现一个有限状态自动机来模拟一个简单的糖果分配器。该机器有两种类型的输入：你可以插入硬币，或转动旋钮来分配糖果。它可以处于两种状态之一：锁定或解锁。它还跟踪剩余的糖果数量和包含的硬币数量。

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
- 将硬币插入锁定的机器中，如果还有糖果，则会使其解锁。
- 在未锁定的机器上转动旋钮会使其分配糖果并变为锁定状态。
- 在锁定的机器上转动旋钮或向未锁定的机器中插入硬币不会产生任何效果。
- 糖果用完的机器会忽略所有输入。

方法 simulateMachine 应根据输入列表操作机器，并在结束时返回机器中剩余的硬币数和糖果数。例如，如果输入的 Machine 有 10 个硬币和 5 个糖果，并且总共成功购买了 4 个糖果，则输出应为 (14, 1)。

```
val update: (Input) -> (Machine) -> Machine =
  { i: Input ->
    { s: Machine ->
      when (i) {
        is Coin ->
          if (!s.locked || s.candies == 0) s
          else Machine(false, s.candies, s.coins + 1)
        is Turn ->
          if (s.locked || s.candies == 0) s
          else Machine(true, s.candies - 1, s.coins)
      }
    }
  }

fun simulateMachine(
  inputs: List<Input>
): State<Machine, Tuple2<Int, Int>> =
  StateFX(Id.monad()) {
    val (x) = inputs.map(update).map(StateApi::modify)
      .stateSequential()
    val (s: Machine) = StateApi.get<Machine>()
    Tuple2(s.candies, s.coins)
  }
```

## 第 7 章. 纯函数式并行

### 练习 7.1

Par.map2 是一个用于组合两个并行计算结果的新高阶函数。它的签名是什么？给出最一般的签名（不要假设它仅适用于 Int）。

```
fun <A, B, C> map2(
  sum: Par<A>,
  sum1: Par<B>,
  function: (A, B) -> C
): Par<C> = Par(function(sum.get, sum1.get))
```

### 练习 7.2

在演进 API 的任何阶段，你都可以开始思考出现的抽象类型的可能表示。尝试为 Par 提出一种表示，使其能够实现我们的 API 的函数。

```
class Par<A>(val get: A) {
  companion object {
    fun <A> unit(a: A): Par<A> = Par(a)
    fun <A, B, C> map2(
      a1: Par<A>,
      a2: Par<B>,
      f: (A, B) -> C
    ): Par<C> = Par(f(a1.get, a2.get))
    fun <A> fork(f: () -> Par<A>): Par<A> = f()
    fun <A> lazyUnit(a: () -> A): Par<A> = Par(a())
    fun <A> run(a: Par<A>): A = a.get
  }
}
```

### 练习 7.3

修复 map2 的实现，使其遵守 Future 上超时契约。

```
fun <A, B, C> map2(a: Par<A>, b: Par<B>, f: (A, B) -> C): Par<C> =
  { es: ExecutorService ->
    val fa = a(es)
    val fb = b(es)
    TimedMap2Future(fa, fb, f)
  }

data class TimedMap2Future<A, B, C>(
  val pa: Future<A>,
  val pb: Future<B>,
  val f: (A, B) -> C
) : Future<C> {

  override fun isDone(): Boolean = TODO()

  override fun get(): C = TODO()

  override fun get(to: Long, tu: TimeUnit): C {
    val timeoutMillis = TimeUnit.MILLISECONDS.convert(to, tu)

    val start = System.currentTimeMillis()
    val a = pa.get(to, tu)
    val duration = System.currentTimeMillis() - start

    val remainder = timeoutMillis - duration
    val b = pb.get(remainder, TimeUnit.MILLISECONDS)
    return f(a, b)
  }

  override fun cancel(b: Boolean): Boolean = TODO()

  override fun isCancelled(): Boolean = TODO()
}
```

### 练习 7.4

此 API 已启用丰富的操作集。作为示例，使用 lazyUnit 编写一个函数，将任意函数 (A) -> B 转换为异步求值其结果的函数。

```
fun <A, B> asyncF(f: (A) -> B): (A) -> Par<B> =
  { a: A -> lazyUnit { f(a) } }
```

### 练习 7.5

编写此函数，名为 sequence。不需要额外的原语。不要调用 run。

提供了两种实现。第一种是更天真的方法，使用简单递归来实现其目标。

```
val <T> List<T>.head: T
  get() = first()

val <T> List<T>.tail: List<T>
  get() = this.drop(1)

val Nil = listOf<Nothing>()
```



```
fun <A> sequence1(ps: List<Par<A>>): Par<List<A>> =
    when (ps) {
        Nil -> unit(Nil)
        else -> map2(
            ps.head,
            sequence1(ps.tail)
        ) { a: A, b: List<A> ->
            listOf(a) + b
        }
    }
```

第二种、也可能是更好的方法同样使用递归，但结合了拆分技术和 map2 来并行化处理。

```
fun <A> sequence(ps: List<Par<A>>): Par<List<A>> =
    when {
        ps.isEmpty() -> unit(Nil)
        ps.size == 1 -> map(ps.head) { listOf(it) }
        else -> {
            val (l, r) = ps.splitAt(ps.size / 2)
            map2(sequence(l), sequence(r)) { la, lb ->
                la + lb
            }
        }
    }
```

### 练习 7.6

实现 parFilter，用于并行过滤列表中的元素。

```
fun <A> parFilter(sa: List<A>, f: (A) -> Boolean): Par<List<A>> {
    val pars: List<Par<A>> = sa.map { lazyUnit { it } }
    return map(sequence(pars)) { la: List<A> ->
        la.flatMap { a ->
            if (f(a)) listOf(a) else emptyList()
        }
    }
}
```

### 练习 7.7 (困难)

浏览 Executors 中的各种静态方法，以了解存在的不同 ExecutorService 实现。然后，在继续之前，回到你的 fork 实现并重新审视它，尝试找到一个反例，或说服自己该定律对你的实现成立。

继续阅读。问题在下一段中解释。

### 练习 7.8 (困难)

证明对于此 fork 实现，任何固定大小的线程池都可能导致死锁。

对于大小为 2 的线程池，fork(fork(fork(x))) 将导致死锁，等等。另一个可能更有趣的例子是 fork(map2(fork(x), fork(y)))。在这种情况下，外部任务首先被提交并占用一个线程等待 fork(x) 和 fork(y)。fork(x) 和 fork(y) 任务被提交并并行运行，除了只有一个线程可用，导致死锁。

### 练习 7.9 (困难/可选)

我们当前的非阻塞表示根本不处理错误。如果在任何时候我们的计算抛出异常，run 实现的 latch 永远不会递减，并且异常会被简单地吞掉。你能修复这个问题吗？

我们在第 13 章的代码中 Task 数据类型里给出了一个完整的解决方案。

### 练习 7.10

实现 choiceN，然后用 choiceN 实现 choice。

```
fun <A> choiceN(n: Par<Int>, choices: List<Par<A>>): Par<A> =
    { es: ExecutorService ->
        choices[n(es).get()].invoke(es)
    }

fun <A> choice(cond: Par<Boolean>, t: Par<A>, f: Par<A>): Par<A> =
    { es: ExecutorService ->
        choiceN(
            map(cond, { if (it) 1 else 0 }),
            listOf(f, t)
        )(es)
    }
```

### 练习 7.11

实现一个名为 choiceMap 的组合子，它接受 Map<K, Par<V>> 作为容器。

```
fun <K, V> choiceMap(key: Par<K>, choices: Map<K, Par<V>>): Par<V> =
    { es: ExecutorService ->
        choices[key(es).get()]!!.invoke(es)
    }
```

### 练习 7.12

实现这个新的基本原语 chooser，然后用它来实现 choice、choiceN 和 choiceMap。

```
fun <A, B> chooser(pa: Par<A>, choices: (A) -> Par<B>): Par<B> =
    { es: ExecutorService ->
        choices(pa(es).get())(es)
    }
```

### 练习 7.13

实现 join。你能看出如何使用 join 实现 flatMap 吗？并且你能使用 flatMap 实现 join 吗？

```
fun <A> join(a: Par<Par<A>>): Par<A> =
    { es: ExecutorService -> a(es).get()(es) }

fun <A, B> flatMapViaJoin(pa: Par<A>, f: (A) -> Par<B>): Par<B> =
    join(map(pa, f))
fun <A> joinViaFlatMap(a: Par<Par<A>>): Par<A> =
    flatMap(a, { it })
```

## 第 8 章. 基于属性的测试

### 练习 8.1

为了习惯这种测试思维方式，请提出指定 sum: (List<Int>) -> Int 函数实现的性质。你不必将这些性质写成可执行的 Kotest 代码——非正式描述即可。

- 空列表的和为 0。
- 元素全部等于 x 的列表的和只是列表长度乘以 x。我们可以将其表达为 `sum(List(n){x}) == n * x`
- 对于任意列表 l，`sum(l) == sum(l.reverse())`，因为加法是可交换的。
- 给定一个列表 `List(x,y,z,p,q)`，`sum(List(x,y,z,p,q)) == sum(List(x,y)) + sum(List(z,p,q))`，因为加法是结合的。更一般地，我们可以将一个列表划分为两个子序列，其和等于整体列表的和。
- 1, 2, 3...n 的和为 `n*(n+1)/2`。

### 练习 8.2

哪些性质指定了查找 List<Int> 最大值的函数？

- 单个元素列表的最大值等于该元素。
- 列表的最大值大于或等于列表中的所有元素。
- 列表的最大值是该列表的一个元素。
- 空列表的最大值未指定，应抛出错误或返回 None。

### 练习 8.3

假设以下表示，使用 check 实现 and 作为 Prop 的方法。

```
interface Prop {
    fun check(): Boolean
    fun and(p: Prop): Prop {
        val checked = this.check() && p.check()
        return object : Prop {
            override fun check() = checked
        }
    }
}
```

返回一个匿名 Prop 实例，它基于 this 和传入的属性 p。

### 练习 8.4

使用此 Gen 表示实现 Gen.choose。它应生成 start 到 stopExclusive 范围内的整数。可随意使用你已编写的函数。

```
fun choose(start: Int, stopExclusive: Int): Gen<Int> =
    Gen(State { rng: RNG -> nonNegativeInt(rng) }
        .map { start + (it % (stopExclusive - start)) })
```

### 练习 8.5

让我们看看使用此 Gen 表示还能实现什么。尝试使用以下签名实现 unit、boolean 和 listOfN，再次利用之前编写的函数：

```
fun <A> unit(a: A): Gen<A> = Gen(State.unit(a))

fun boolean(): Gen<Boolean> =
    Gen(State { rng -> nextBoolean(rng) })

fun <A> listOfN(n: Int, ga: Gen<A>): Gen<List<A>> =
    Gen(State.sequence(List(n) { ga.sample }))
```

此解决方案大量依赖于第 6 章中开发的 State API。我们将解决方案建立在 State.sequence() 函数之上，该函数能够将 List<State<S, A>> 转换为 State<A, List<A>>。当将包含 n 的列表和包装后的 sample 应用于此状态转换时，我们会得到一个新的 State，随后可以再次将其包装为新的 Gen。

### 练习 8.6

实现 flatMap，然后用它来实现这个更动态的 listOfN 版本。按所示将 flatMap 和 listOfN 放在 Gen 数据类中。

```
data class Gen<A>(val sample: State<RNG, A>) {

    companion object {
        fun <A> listOfN(gn: Gen<Int>, ga: Gen<A>): Gen<List<A>> =
            gn.flatMap { n -> listOfN(n, ga) }
    }

    fun <B> flatMap(f: (A) -> Gen<B>): Gen<B> =
        Gen(sample.flatMap { a -> f(a).sample })
}
```

### 练习 8.7

实现 union，用于通过以相等概率从每个生成器中提取值，将两个相同类型的生成器合并为一个。

```
fun <A> union(ga: Gen<A>, gb: Gen<A>): Gen<A> =
    boolean().flatMap { if (it) ga else gb }
```

### 练习 8.8

实现 weighted，即 union 的一个版本，它接受每个 Gen 的权重，并以与权重成比例的概率从每个 Gen 生成值。

```
fun <A> weighted(
    pga: Pair<Gen<A>, Double>,
    pgb: Pair<Gen<A>, Double>
): Gen<A> {
    val (ga, p1) = pga
    val (gb, p2) = pgb
    val prob =
        p1.absoluteValue /
            (p1.absoluteValue + p2.absoluteValue)
    return Gen(State { rng: RNG -> double(rng) })
        .flatMap { d ->
            if (d < prob) ga else gb
        }
}
```

### 练习 8.9

现在我们有了 Prop 的表示，实现用于组合 Prop 值的 and 和 or。请注意，在 or 失败的情况下，我们不知道是哪一个属性负责，左边还是右边。你能设计一种处理方法吗？

```
data class Prop(val run: (TestCases, RNG) -> Result) {
    fun and(other: Prop) = Prop { n, rng ->
        when (val prop = run(n, rng)) {
            is Passed -> other.run(n, rng)
            is Falsified -> prop
        }
    }

    fun or(other: Prop) = Prop { n, rng ->
        when (val prop = run(n, rng)) {
            is Falsified ->
                other.tag(prop.failure).run(n, rng)
            is Passed -> prop
        }
    }

    private fun tag(msg: String) = Prop { n, rng ->
        when (val prop = run(n, rng)) {
            is Falsified ->
                Falsified(
                    "$msg: ${prop.failure}",
                    prop.successes
                )
            is Passed -> prop
        }
    }
}
```

我们引入了一个 tag 方法，以便在遇到 or 条件且计算必须继续时添加有关左侧失败的元数据。如果在进行到 or 条件的右侧之前为 Falsified，我们会标记或为属性添加失败消息。这非常简单，但目前足以解决问题。

### 练习 8.10

实现一个名为 unsized 的辅助函数，用于将 Gen 转换为 SGen。你可以将它作为 Gen 的方法添加。

```
data class Gen<A>(val sample: State<RNG, A>) {
    fun unsized(): SGen<A> = SGen { _ -> this }
}
```

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

// other combinators

internal abstract fun char(c: Char): Parser<Char>

internal abstract fun <A> many(p: Parser<A>): Parser<List<A>>

internal abstract fun <A> many1(p: Parser<A>): Parser<List<A>>

internal abstract fun <A> listOfN(
    n: Int,
    p: Parser<A>
): Parser<List<A>>

internal abstract fun <A, B> product(
    pa: Parser<A>,
    pb: () -> Parser<B>
): Parser<Pair<A, B>>

internal abstract fun <A, B, C> map2(
    pa: Parser<A>,
    pb: () -> Parser<B>,
    f: (A, B) -> C
): Parser<C>

internal abstract fun <A, B> map(pa: Parser<A>, f: (A) -> B): Parser<B>

internal abstract fun <A> defer(pa: Parser<A>): () -> Parser<A>

internal abstract fun <A> skipR(
    pa: Parser<A>,
    ps: Parser<String>
): Parser<A>

internal abstract fun <B> skipL(
    ps: Parser<String>,
    pb: Parser<B>
): Parser<B>

internal abstract fun <A> sep(
    p1: Parser<A>,
    p2: Parser<String>
): Parser<List<A>>

internal abstract fun <A> surround(
    start: Parser<String>,
    stop: Parser<String>,
    p: Parser<A>
): Parser<A>
}

abstract class ParsersDsl<PE> : Parsers<PE>() {

    fun <A> Parser<A>.defer(): () -> Parser<A> = defer(this)

    fun <A, B> Parser<A>.map(f: (A) -> B): Parser<B> =
        this@ParsersDsl.map(this, f)

    fun <A> Parser<A>.many(): Parser<List<A>> =
        this@ParsersDsl.many(this)

    infix fun <A> Parser<out A>.or(p: Parser<out A>): Parser<A> =
        this@ParsersDsl.or(this, p.defer())

    infix fun <A, B> Parser<A>.product(p: Parser<B>): Parser<Pair<A, B>> =
        this@ParsersDsl.product(this, p.defer())

    infix fun <A> Parser<A>.sep(p: Parser<String>): Parser<List<A>> =
        this@ParsersDsl.sep(this, p)

infix fun <A> Parser<A>.skipR(p: Parser<String>): Parser<A> =
    this@ParsersDsl.skipR(this, p)

infix fun <B> Parser<String>.skipL(p: Parser<B>): Parser<B> =
    this@ParsersDsl.skipL(this, p)

infix fun <T> T.cons(la: List<T>): List<T> = listOf(this) + la
}

abstract class JSONParsers : ParsersDsl<ParseError>() {

    // {
    //   "Company name" : "Microsoft Corporation",
    //   "Ticker": "MSFT",
    //   "Active": true,
    //   "Price": 30.66,
    //   "Shares outstanding": 8.38e9,
    //   "Related companies": [ "HPQ", "IBM", "YHOO", "DELL", "GOOG" ]
    // }

    val JSON.parser: Parser<JSON>
        get() = succeed(this)

    val String.rp: Parser<String>
        get() = regex(this)

    val String.sp: Parser<String>
        get() = string(this)

    fun thru(s: String): Parser<String> =
        ".*?(Pattern.quote(s))\".rp

    val quoted: Parser<String> =
        "\"\".sp skipL thru("\"\").map { it.dropLast(1) }

    val doubleString: Parser<String> =
        "[-+]?([0-9]*\\.)?[0-9]+([eE][-+][0-9]+)?".rp

    val double: Parser<Double> = doubleString.map { it.toDouble() }

    val lit: Parser<JSON> =
        JNull.parser or
            double.map { JNumber(it) } or
            JBoolean(true).parser or
            JBoolean(false).parser or
            quoted.map { JString(it) }

    val value: Parser<JSON> = lit or obj() or array()

    val keyval: Parser<Pair<String, JSON>> =
        quoted product (":".sp skipL value)

    val whitespace: Parser<String> = """\s*""".rp

    val eof: Parser<String> = """\z""".rp

    fun array(): Parser<JArray> =
        surround("[\".sp, \"]\".sp,
            (value sep \",\".sp).map { vs -> JArray(vs) })

    fun obj(): Parser<JObject> =
        surround("{\".sp, "}\".sp,
            (keyval sep \",\".sp).map { kvs -> JObject(kvs.toMap()) })

    fun <A> root(p: Parser<A>): Parser<A> = p skipR eof

    val jsonParser: Parser<JSON> =
        root(whitespace skipL (obj() or array()))
}

## 练习 9.10

你能想到在 or 链中指定报告哪些错误时可能有用的其他原语吗？

```
fun <A> furthest(pa: Parser<A>): Parser<A>
```

在发生错误时，返回消耗字符数最多的那个错误。

```
fun <A> latest(pa: Parser<A>): Parser<A>
```

在发生错误时，返回最近发生的错误。

## 练习 9.11（困难）

为此 Parser 表示实现 string、regex、succeed 和 slice。已包含一些私有辅助函数存根来引导你走向正确的方向。

```
override fun string(s: String): Parser<String> =
    { state: State ->
        when (val idx =
            firstNonMatchingIndex(state.input, s, state.offset)) {
            is None ->
                Success(s, s.length)
            is Some ->
                Failure(
                    state.advanceBy(idx.t).toError("'$s'"),
                    idx.t != 0
                )
        }
    }

private fun firstNonMatchingIndex(
    s1: String,
    s2: String,
    offset: Int
): Option<Int> {
    var i = 0
    while (i < s1.length && i < s2.length) {
        if (s1[i + offset] != s2[i])
            return Some(i)
        else i += 1
    }
    return if (s1.length - offset >= s2.length) None
    else Some(s1.length - offset)
}

private fun State.advanceBy(i: Int) =
    this.copy(offset = this.offset + i)

override fun regex(r: String): Parser<String> =
    { state: State ->
        when (val prefix = state.input.findPrefixOf(r.toRegex())) {
            is Some ->
                Success(prefix.t.value, prefix.t.value.length)
            is None ->
                Failure(state.toError("regex ${r.toRegex()}"))
        }
    }

private fun String.findPrefixOf(r: Regex): Option<MatchResult> =
    r.find(this).toOption().filter { it.range.first == 0 }
```

```
override fun <A> succeed(a: A): Parser<A> = { Success(a, 0) }

override fun <A> slice(p: Parser<A>): Parser<String> =
    { state: State ->
        when (val result = p(state)) {
            is Success ->
                Success(state.slice(result.consumed), result.consumed)
            is Failure -> result
        }
    }

private fun State.slice(n: Int) =
    this.input.substring(this.offset..this.offset + n)
```

## 练习 9.12

修改你对 string 的实现，以便在发生错误时提供有意义的错误消息。

```
override fun string(s: String): Parser<String> =
    { state: State ->
        when (val idx =
            firstNonMatchingIndex(state.input, s, state.offset)) {
            is None ->
                Success(s, s.length)
            is Some ->
                Failure(
                    state.advanceBy(idx.t).toError("'$s'"),
                    idx.t != 0
                )
        }
    }
```

## 练习 9.13

实现 run，以及使用我们当前 Parser 表示尚未实现的任何剩余原语。尝试在各种输入上运行你的 JSON 解析器。

```
override fun <A> run(p: Parser<A>, input: String): Result<A> =
    p(Location(input))
```

## 练习 9.14

想出一种将 ParseError 格式化为人可读形式的好方法。有很多选择，但一个关键的见解是，在将错误作为 String 显示时，我们通常希望合并或分组附加到同一位置的标签。

```
data class ParseError(val stack: List<Pair<Location, String>> = emptyList()) {

    fun push(loc: Location, msg: String): ParseError =
        this.copy(stack = listOf(Pair(loc, msg)) + stack)

    fun label(s: String): ParseError =
        ParseError(latestLoc()
            .map { Pair(it, s) }
            .toList())

    private fun latest(): Option<Pair<Location, String>> = stack.lastOrNone()

    private fun latestLoc(): Option<Location> = latest().map { it.first }
}
```

```
/**
 * Display collapsed error stack - any adjacent stack elements with the
 * same location are combined on one line. For the bottommost error, we
 * display the full line, with a caret pointing to the column of the
 * error.
 * Example:
 * 1.1 file 'companies.json'; array
 * 5.1 object
 * 5.2 key-value
 * 5.10 ':'
 * { "MSFT" ; 24,
 * ^
 */
override fun toString(): String =
    if (stack.isEmpty()) "no error message"
    else {
        val collapsed = collapseStack(stack)
        val context =
            collapsed.lastOrNone()
                .map { "\n\n" + it.first.line }
                .getOrElse { "" } +
            collapsed.lastOrNone()
                .map { "\n" + it.first.col }
                .getOrElse { "" }

        collapsed.joinToString { (loc, msg) ->
            "${loc.line}.${loc.col} $msg"
        } + context
    }

/* Builds a collapsed version of the given error stack -
 * messages at the same location have their messages merged,
 * separated by semicolons.
 */
private fun collapseStack(
    stk: List<Pair<Location, String>>
): List<Pair<Location, String>> =
    stk.groupBy { it.first }
        .mapValues { it.value.joinToString() }
        .toList()
        .sortedBy { it.first.offset }
}
```

## 第10章. 幺半群

## 练习 10.1

为整数加法和乘法以及布尔运算符提供 Monoid 实例。

```
val intAddition: Monoid<Int> = object : Monoid<Int> {

    override fun combine(a1: Int, a2: Int): Int = a1 + a2

    override val nil: Int = 0
}

val intMultiplication: Monoid<Int> = object : Monoid<Int> {

    override fun combine(a1: Int, a2: Int): Int = a1 * a2

    override val nil: Int = 1
}

val booleanOr: Monoid<Boolean> = object : Monoid<Boolean> {

    override fun combine(a1: Boolean, a2: Boolean): Boolean = a1 || a2
}
```



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



```
fa: ListOf<A>,
    z: B,
    f: (A, B) -> B
): B =
    fa.fix().foldRight(z, f)

override fun <A, B> foldLeft(
    fa: ListOf<A>,
    z: B,
    f: (B, A) -> B
): B =
    fa.fix().foldLeft(z, f)
}
```

## 练习 10.14

请回忆我们在第 3 章中实现了一个二叉 Tree。接下来，实现 Foldable<ForTree>。你只需要重写 Foldable 的 foldMap 即可使其工作，让提供的 foldLeft 和 foldRight 方法使用你的新实现。

注意    第 10 章练习样板代码中已提供了 Tree 的可折叠版本。

```
object TreeFoldable : Foldable<ForTree> {
    override fun <A, B> foldMap(
        fa: TreeOf<A>,
        m: Monoid<B>,
        f: (A) -> B
    ): B =
        when (val t = fa.fix()) {
            is Leaf ->
                f(t.value)
            is Branch ->
                m.combine(foldMap(t.left, m, f), foldMap(t.right, m, f))
        }
}
```

## 练习 10.15

编写 Foldable<ForOption> 的一个实例。

```
object OptionFoldable : Foldable<ForOption> {
    override fun <A, B> foldMap(
        fa: OptionOf<A>,
        m: Monoid<B>,
        f: (A) -> B
    ): B =
        when (val o = fa.fix()) {
            is None -> m.nil
            is Some -> f(o.get)
        }
}
```

## 练习 10.16

任何 Foldable 结构都可以转换为 List。使用接口上已有的一个方法为 Foldable<F> 编写这个便捷方法。

```
fun <A> toList(fa: Kind<F, A>): List<A> =
    foldLeft(fa, List.empty(), { la, a -> Cons(a, la) })
```

## 练习 10.17

通过组合两个幺半群来实现 productMonoid。只要 A.op 和 B.op 都是结合的，你的 op 实现就应该是结合的。

```
fun <A, B> productMonoid(
    ma: Monoid<A>,
    mb: Monoid<B>
): Monoid<Pair<A, B>> =
    object : Monoid<Pair<A, B>> {
        override fun combine(a1: Pair<A, B>, a2: Pair<A, B>): Pair<A, B> =
            Pair(
                ma.combine(a1.first, a2.first),
                mb.combine(a1.second, a2.second)
            )

        override val nil: Pair<A, B>
            get() = Pair(ma.nil, mb.nil)
    }
```

## 练习 10.18

为结果本身是幺半群的函数编写一个幺半群实例。

```
fun <A, B> functionMonoid(b: Monoid<B>): Monoid<(A) -> B> =
    object : Monoid<(A) -> B> {
        override fun combine(f: (A) -> B, g: (A) -> B): (A) -> B =
            { a: A -> b.combine(f(a), g(a)) }

        override val nil: (A) -> B =
            { a -> b.nil }
    }
```

## 练习 10.19

bag 类似于 set，只是它由一个映射表示，该映射为每个元素包含一个条目，以该元素作为键，而该键下的值是该元素在 bag 中出现的次数。例如：

```
>>> bag(listOf("a", "rose", "is", "a", "rose"))

res0: kotlin.collections.Map<kotlin.String, kotlin.Int> = {a=2, rose=2, is=1}
```

使用幺半群从 List<A> 计算这样的 bag。

```
object ListFoldable : Foldable<ForList> {

    override fun <A, B> foldRight(
        fa: ListOf<A>,
        z: B,
        f: (A, B) -> B
    ): B =
        fa.fix().foldRight(z, f)

    fun <A> bag(la: List<A>): Map<A, Int> =
        foldMap(la, mapMergeMonoid<A, Int>(intAdditionMonoid)) { a: A ->
            mapOf(a to 1)
        }
}
```

## 第 11 章. 单子与函子

## 练习 11.1

为 Par、Option 和 List 编写单子实例。此外，为 arrow.core.ListK 和 arrow.core.SequenceK 提供单子实例。

> 注意 Arrow 提供的 ListK 和 SequenceK 类型是包装类，它们将平台等价物 List 和 Sequence 转换为完全配备的类型构造子。

```
object Monads {

    val parMonad = object : Monad<ForPar> {

        override fun <A> unit(a: A): ParOf<A> = Par.unit(a)

        override fun <A, B> flatMap(
            fa: ParOf<A>,
            f: (A) -> ParOf<B>
        ): ParOf<B> =
            fa.fix().flatMap { f(it).fix() }
    }

    val optionMonad = object : Monad<ForOption> {

        override fun <A> unit(a: A): OptionOf<A> = Some(a)

        override fun <A, B> flatMap(
            fa: OptionOf<A>,
            f: (A) -> OptionOf<B>
        ): OptionOf<B> =
            fa.fix().flatMap { f(it).fix() }
    }

    val listMonad = object : Monad<ForList> {

        override fun <A> unit(a: A): ListOf<A> = List.of(a)

        override fun <A, B> flatMap(
            fa: ListOf<A>,
            f: (A) -> ListOf<B>
        ): ListOf<B> =
            fa.fix().flatMap { f(it).fix() }
    }

    val listKMonad = object : Monad<ForListK> {

        override fun <A> unit(a: A): ListKOf<A> = ListK.just(a)

        override fun <A, B> flatMap(
            fa: ListKOf<A>,
            f: (A) -> ListKOf<B>
        ): ListKOf<B> =
            fa.fix().flatMap(f)
    }

    val sequenceKMonad = object : Monad<ForSequenceK> {

        override fun <A> unit(a: A): Kind<ForSequenceK, A> =
            SequenceK.just(a)

        override fun <A, B> flatMap(
            fa: Kind<ForSequenceK, A>,
            f: (A) -> Kind<ForSequenceK, B>
        ): Kind<ForSequenceK, B> {
            return fa.fix().flatMap(f)
        }
    }
}
```

## 练习 11.2（困难）

State 看起来也可能是一个单子，但它接受两个类型参数，即 S 和 A。要实现 Monad，你需要一个只有一个参数的类型构造子。尝试实现一个 State 单子，看看会遇到什么问题，并思考是否以及如何解决这个问题。我们将在本章后面讨论解决方案。

```
data class State<S, out A>(val run: (S) -> Pair<A, S>) : StateOf<S, A>

sealed class ForState private constructor() {
    companion object
}

typealias StateOf<S, A> = Kind2<ForState, S, A>

fun <S, A> StateOf<S, A>.fix() = this as State<S, A>

typealias StatePartialOf<S> = Kind<ForState, S>

interface StateMonad<S> : Monad<StatePartialOf<S>> {

    override fun <A> unit(a: A): StateOf<S, A>

    override fun <A, B> flatMap(
        fa: StateOf<S, A>,
        f: (A) -> StateOf<S, B>
    ): StateOf<S, B>
}
```

## 练习 11.3

sequence 和 traverse 组合子到现在应该对你很熟悉了，你在前几章中对它们的实现可能都非常相似。在 Monad<F> 上一次性实现它们。

```
fun <A> sequence(lfa: List<Kind<F, A>>): Kind<F, List<A>> =
    lfa.foldRight(
        unit(List.empty<A>()),
        { fa: Kind<F, A>, fla: Kind<F, List<A>> ->
            map2(fa, fla) { a: A, la: List<A> -> Cons(a, la) }
        }
    )

fun <A, B> traverse(
    la: List<A>,
    f: (A) -> Kind<F, B>
): Kind<F, List<B>> =
    la.foldRight(
        unit(List.empty<B>()),
        { a: A, acc: Kind<F, List<B>> ->
            map2(f(a), acc) { b: B, lb: List<B> -> Cons(b, lb) }
        }
    )
```

## 练习 11.4

实现 replicateM 以生成 Kind<F, List<A>>，其中列表长度为 n。

```
fun <A> replicateM(n: Int, ma: Kind<F, A>): Kind<F, List<A>> =
    when (n) {
        0 -> unit(List.empty())
        else ->
            map2(ma, replicateM(n - 1, ma)) { m: A, ml: List<A> ->
                Cons(m, ml)
            }
    }

fun <A> _replicateM(n: Int, ma: Kind<F, A>): Kind<F, List<A>> =
    sequence(List.fill(n, ma))
```

## 练习 11.5

思考 replicateM 在各种 F 选择下的行为。例如，它在 List 单子中如何表现？Option 呢？用你自己的话描述 replicateM 的一般含义。

对于 List，replicateM 函数会生成一个列表的列表。它将包含所有长度为 n 且元素从输入列表中选取的列表。

对于 Option，它会根据输入是 Some 还是 None 生成 Some 或 None。Some 情况将包含一个长度为 n 且重复输入 Option 中元素的列表。

它重复 ma 单子值 n 次，并将结果收集到一个单一值中，其中单子 F 决定值实际如何组合。

## 练习 11.6（困难）

这里有一个我们之前没见过的函数示例。实现 filterM 函数。它有点像 filter，只是函数不是 (A) -> Boolean，而是 (A) -> Kind<F, Boolean>。用单子等价物替换各种普通函数如 filter 通常会产生有趣的结果。实现这个函数，然后思考它对于 Par、Option 和 Gen 等各种数据类型意味着什么。

```
fun <A> filterM(
    ms: List<A>,
    f: (A) -> Kind<F, Boolean>
): Kind<F, List<A>> =
    when (ms) {
        is Nil -> unit(Nil)
        is Cons ->
            flatMap(f(ms.head)) { succeed ->
                if (succeed) map(filterM(ms.tail, f)) { tail ->
                    Cons(ms.head, tail)
                } else filterM(ms.tail, f)
            }
    }
```

- 对于 Par，filterM 在并行应用函数的同时过滤列表。



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

## 侧边栏

`flatMap` 在这里的作用是将 `r` 参数同时传递给外部的 `Reader` 以及 `f` 的结果，即内部的 `Reader`。类似于 `State` 如何传递状态，只是 `Reader` 中的“状态”是只读的。

这里 `sequence` 的含义是，如果你有一个函数列表，你可以将其转换为一个函数，该函数接收一个参数并将其传递给列表中的所有函数，返回结果的列表。

`join` 的含义很简单，就是将同一个值作为两个参数传递给二元函数。

`replicateM` 的含义是将同一个函数对同一个参数应用多次，返回结果的列表。注意，如果这个函数是纯的（它应该是），就可以利用这一点，只调用一次函数并复制结果，而不是多次调用函数。这意味着 `Reader` 单子可以重写 `replicateM` 以提供非常高效的实现。

## 附录 C. 扩展练习

![](img/255e7d4fd2ecc6516db240b06be1018_334_0.png)

### C.1 引言

本附录收录了一些出现在原版《Scala函数式编程》第二部分中的练习，尤其是第8章关于基于属性的测试的内容。我们移至此处的练习难度较大、可选，旨在达到超出各自章节范围的反思和建模水平，因此被归类为*开放式*。

我们在本附录中为其中一些练习提供了提示和答案，但我们鼓励你先自行尝试，在遇到困难时再查阅提示和解答。

### C.2 练习

### 练习 C.1

一个 `check` 属性很容易被确凿地证明，因为测试只是涉及求值 `Boolean` 参数。但某些 `forAll` 属性也可以被证明。例如，如果属性的域是 `Boolean`，那么实际上只有两种情况需要测试。如果属性 `forAll(p)` 对 `p(true)` 和 `p(false)` 都通过，则它已被证明。某些域（如 `Boolean` 和 `Byte`）非常小，可以进行穷举检查。而且使用大小生成器，即使是无限域也可以在最大大小范围内进行穷举检查。自动化测试非常有用，但如果我们能*自动证明我们的代码正确*就更好了。修改我们的库以纳入这种对有限域和大小生成器的穷举检查。这与其说是练习，不如说是一个广泛的、开放式的设计项目。

### 练习 C.2

我们想要生成一个以某种方式*使用其参数*来选择返回哪个 `Int` 的函数。你能想到一个好的表达方式吗？这是一个非常开放且具有挑战性的设计练习。看看你能发现关于这个问题的什么，以及是否有一个漂亮的通用解决方案可以纳入我们迄今为止开发的库中。

### 练习 C.3

强烈建议你走出去尝试使用我们开发的库！看看你还能用它测试什么，并看看你是否发现了其使用的新惯用法，或许还能发现进一步扩展或使其更方便的方法。以下是一些入门想法：

- 编写属性来指定我们为 List 和 Stream 编写的其他一些函数的行为，例如 take、drop、filter 和 unfold。
- 编写一个大小生成器来生成第3章中定义的 Tree 数据类型，然后用它来指定我们为 Tree 定义的 fold 函数的行为。你能想到改进 API 以使其更简单的方法吗？
- 编写属性来指定我们为 Option 和 Either 定义的 sequence 函数的行为。

### C.3 提示

### 练习 C.1

你需要对 Gen 的表示进行补充。例如，Gen<Int> 应该能够生成随机整数，也能够生成从 Int.MIN_VALUE 到 Int.MAX_VALUE 的所有整数的流。你可能希望行为取决于请求的测试用例数量。

### 练习 C.2

如果我们只关注随机情况，一种让生成的 Int 依赖于 String 的方法可能是将新的随机数生成器的种子设置为给定输入 String 的 hashCode。

### C.4 答案

### C.4.1 基于属性的测试

### 练习 C.1

详细答案可在随附录提供的代码中的 Exhaustive.kt 文件中找到。

### 练习 C.2

让我们先看看我们动机示例的签名，从给定的 Gen<Int> 生成一个从 (String) -> Int 的函数：

```
fun genStringInt(g: Gen<Int>): Gen<(String) -> Int> = TODO()
```

让我们稍微将其泛化，使其不专门针对 Int，因为那样我们就可以有点作弊（例如，返回输入 String 的 hashCode，它恰好是一个 Int）。

```
fun <A> genStringFn(g: Gen<A>): Gen<(String) -> A> = TODO()
```

我们已经排除了仅返回忽略输入 String 的函数，因为这没什么有趣！相反，我们希望确保使用输入 String 的信息来影响我们生成的 A。如何做到这一点？好吧，我们能够影响 Gen 产生的值的唯一方式是修改它接收的 RNG 值：

回顾我们对 Gen 的定义：

```
data class Gen<out A>(val sample: State<RNG, A>)
```

仅通过遵循类型，我们就可以开始编写：

```
fun <A> genStringFn(g: Gen<A>): Gen<(String) -> A> = Gen {
  State { (rng: RNG) -> ??? }
}
```

其中 ??? 必须为类型 ((String) -> A, RNG)，而且我们希望 String 以某种方式影响生成的 A。我们通过在将其传递给 Gen<A> sample 函数之前修改 RNG 的种子来实现这一点。一种简单的方法是计算输入字符串的哈希值，并在用于生成 A 之前将其混合到 RNG 状态中：

```
fun <A> genStringFn(g: Gen<A>): Gen<(String) -> A> = Gen {
  State { (rng: RNG) ->
    {
      // we still use `rng` to produce a seed, so we get a new function each time
      val (seed, rng2) = rng.nextInt()
      val f: (String) -> A = s: String -> g.sample.run(
        SimpleRNG(seed.toLong() xor s.hashCode().toLong())).first
      return (f, rng2)
    }
  }
}
```

更一般地，任何接受 String 和 RNG 并产生新 RNG 的函数都可以使用。在这里，我们计算 String 的 hashCode，然后用它与种子值进行 XOR 以产生新的 RNG。我们同样可以取 String 的长度并使用该值来扰动我们的 RNG 状态，或取字符串的前 3 个字符。选择会影响我们生成的是何种函数：

- 如果我们使用 hashCode 来扰动 RNG 状态，我们生成的函数会使用 String 的所有信息来影响生成的 A 值。只有共享相同 hashCode 的输入字符串才保证产生相同的 A。
- 如果我们使用长度，我们生成的函数仅使用 String 的部分信息来影响生成的 A。对于所有具有相同长度的输入字符串，我们保证得到相同的 A。

我们选择的策略取决于我们认为对测试来说现实的函数。我们想要使用所有可用信息来产生结果的函数，还是更感兴趣于仅使用其输入的零碎信息的函数？我们可以将策略封装在一个接口中：

```
interface Cogen<in A> {
    fun sample(a: A, rng: RNG) : RNG
}
```

作为练习，尝试实现 genStringFn 的泛化版本。

```
fun <A, B> fn(cin: Cogen<A>, cout: Gen<B>) : Gen<(A) -> B> = TODO()
```

你可以以 genStringFn 为模式实现它。只要遵循类型即可！

这种方法的一个问题是向用户报告测试用例失败。在失败的情况下，用户看到的全部信息就是对于某个不透明的函数，属性失败了，这并不很有启发性。在 Haskell 库 [QuickCheck](www.cse.chalmers.se/~rjmh/QuickCheck/manual.html) 中已有工作，能够向用户报告回信息，甚至将生成的函数*收缩*到仍能证伪属性的最简单形式。参见[关于收缩和展示函数的演讲](www.youtube.com/watch?v=CH8UQJiv9Q4)。



## 注释
-   1. 可以在 arrow-kt.io/ 网站上了解更多关于 Arrow 的信息。
-   2. *过程* 通常用于指代可能具有副作用的参数化代码块。
-   3. 发音类似“ripple”，但用 e 代替 i
-   4. 我们可以在 Kotlin 中手工编写 while 循环，但这很少必要，且被认为是不好的做法，因为它妨碍了良好的组合风格。
-   5. 这里使用“优化”一词并不完全恰当。优化通常意味着某种非必要的性能改进，但当我们使用尾调用来编写循环时，我们通常依赖于它们被编译为迭代循环，而不会为每次迭代消耗一个调用栈帧（否则对于大输入会导致 StackOverflowError）。
-   6. 从技术上讲，Kotlin 中的所有值都可以使用 == 进行相等性比较，并使用 `toString()` 转换为字符串表示形式，并且可以使用 `hashCode()` 从值的内部生成一个整数。但这是一种从 Java 继承而来的缺陷。
-   7. 尽管这是一个有趣的谜题，但这并不是纯粹的学术练习。实践中的函数式编程涉及大量将构建块以唯一合理的方式组合在一起。本练习的目的是练习使用高阶函数，并利用 Kotlin 的类型系统来指导编程。
-   8. 在这个内部函数的函数体中，外层的 a 仍然在作用域内。我们有时说内部函数闭包捕获了其环境，其中包括 a。
-   9. 这是以发现该原理的数学家 Haskell Curry 命名的。它早先由 Moses Schoenfinkel 独立发现，但 Schoenfinkelization 并未流行起来。
-   10. discuss.kotlinlang.org/t/destructuring-in-when/2391
-   11. 这里需要类型注解 `Nil: List<Int>`，因为否则 Kotlin 会将 `foldRight` 中的 B 类型参数推断为 `List<Nothing>`。
-   12. 在标准库中，`map` 和 `flatMap` 是 `List` 的方法。
-   13. kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/-list/index.html
-   14. 在 Kotlin 中，ADT 的实现通过将基类设为 sealed 来限制。这防止了通过引入用户定义的实现来改变类层次结构
-   15. 这种命名并非巧合。类型形成 ADT 的“加法”和“乘法”与数字的加法和乘法之间存在深层联系，超出了本书的范围。
-   16. 元数是逻辑、数学和计算机科学中函数或操作所接受的参数或操作数的数量。
-   17. 如果函数对某些输入不终止，则它也可能是偏函数。我们在这里不会讨论这种偏性形式，因为它不是可恢复的错误，也没有关于如何最好处理它的疑问。
-   18. 通常，对于具有单个、明确操作数（例如 `List.map`）的函数，我们尽可能使用这种面向对象的语法风格，否则使用独立函数风格。
-   19. 也许它本应被命名为 `mapFlat`，但听起来没有那么吸引人！
-   20. 这是一个明确的例子，不适合以 OO 风格定义该函数。它不应该是 `List` 的方法（`List` 不应需要知道任何关于 `Option` 的信息），也不能是 `Option` 的方法，因此它放在 `Option` 的伴生对象中。
-   21. `Either` 也常被更广泛地用于在不值得定义全新数据类型的情况下编码两种可能性之一。我们将在全书中看到一些例子。
-   22. 对于这类程序跟踪，通常更有启发性的是不完全跟踪每个子表达式的求值。在这种情况下，我们省略了 `List.of(1,2,3,4).map { it + 10 }` 的完整展开。我们可以“进入” map 的定义并逐步跟踪其执行，但我们选择为了简洁而省略这一层细节。
-   23. 事实上，类型 `() -> A` 是类型 `Function<A>` 的语法别名。
-   24. kotlinlang.org/docs/reference/delegated-properties.html
-   25. 回想一下，Kotlin 使用子类型表示数据构造函数，但我们几乎总是希望推断类型为 `Stream`，而不是 `Cons` 或 `Empty`。创建返回基类型的智能构造函数是一种常见技巧。
-   26. 尽管具有说明性，但如果流很大且所有元素测试结果为 `false`，此 `exists` 定义不是栈安全的。
-   27. 可以使用普通的递归循环定义一个栈安全的 `forAll` 版本。
-   28. 在 Kotlin 中，`Int` 类型是 32 位有符号整数，因此此流将在某个点从正值切换到负值，并在大约四十亿个元素后重复自身。
-   29. 使用 `unfold` 定义 `constant` 和 `ones` 意味着我们得不到像递归定义 `fun ones(): Stream<Int> = Stream.cons({ 1 }, { ones() })` 那样的共享。递归定义在遍历时即使保留引用也只消耗常量内存，而基于 `unfold` 的实现则不然。保留共享并不是我们在使用流编程时通常依赖的东西，因为它极其微妙且不被类型跟踪。例如，即使调用 `xs.map { x -> x }` 也会破坏共享。
-   30. 实际上是伪随机，但就我们的目的而言，我们忽略这一区别。
-   31. Kotlin API 链接：bit.ly/35MLFhz
-   32. 回想一下，`Pair<A, B>` 是二元元组的类型，并且给定 `p: Pair<A, B>`，你可以使用 `p.first` 提取 `A`，并使用 `p.second` 提取 `B`。
-   33. 使用纯函数计算下一个状态会带来效率损失，因为这意味着我们实际上无法就地修改数据。在这里，这并不是真正的问题，因为状态只是一个必须复制的单个 `Long`。这种效率损失可以通过使用高效的纯函数数据结构来缓解。在某些情况下，也可以在不破坏引用透明性的前提下就地修改数据，我们将在本书第 4 部分讨论这个问题。
-   34. 我们将 *逻辑线程* 一词 somewhat 非正式地用于表示与程序主执行线程并发运行的计算。逻辑线程与操作系统线程之间不必一一对应。例如，我们可能通过线程池将大量逻辑线程映射到较少数量的操作系统线程上。
-   35. 我们确实指的是代数，在数学意义上，即一个或多个集合，加上在这些集合的对象上操作的一组函数，以及一组 *公理*。公理是我们假定为真的陈述，从中我们可以推导出其他必须为真的 *定理*。在我们的例子中，集合是 `Par<A>` 和 `List<Par<A>>` 等特定类型，函数是 `map2`、`unit` 和 `sequence` 等操作。
-   36. docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Executor.html
-   37. 这种最后定义新定律的方式可能最弱，因为很容易让定律反映实现，即使实现有错误或需要各种不寻常的副作用条件使组合变得困难。
-   38. 这与函数的定义域（en.wikipedia.org/wiki/Domain_of_a_function）中“domain”的用法相同——生成器描述了我们希望测试的函数的可能输入。请注意，我们有时仍会以更口语化的意义使用“domain”，来指代感兴趣的主题或领域，例如，“函数式并行的领域”或“错误处理领域。”
-   39. www.scalacheck.org
-   40. hackage.haskell.org/package/QuickCheck
-   41. Arrow 为 `List` 和 `Sequence` 提供了一个 `forAll` 扩展方法，其签名为 `fun <A> List<A>.forAll(f: (A) -> Boolean): Boolean.`
-   42. en.wikipedia.org/wiki/JSON
-   43. en.wikipedia.org/wiki/Yacc
-   44. www.antlr.org/
-   45. `List.size()` 所需的时间取决于实现。例如，如果项目数量存储在整数中，`size()` 将花费恒定时间。
-   46. en.wikipedia.org/wiki/JSON
-   47. json.org
-   48. `copy` 方法随任何 `data class` 免费提供。它返回对象的副本，但一个或多个属性被修改。如果未为字段指定新值，它将与原始对象中的值相同。这使用了与 Kotlin 中默认参数相同的机制
-   49. 希腊前缀 *endo-* 意为 *内部*，即在自同态函数的陪域在其定义域之内的意义上。
-   50. *同态* 来自希腊语，*homo* 意为“相同”，*morphe* 意为“形状。”
-   51. 半群定义了将同一类型的两个值组合的能力，幺半群在此基础上通过提供一个空或 `nil` 值来扩展。
-   52. 我们在这里将类型构造函数参数命名为 `F` 的决定是任意的。我们本可以将其命名为 `Foo`、`w00t` 或 `Blah2`，不过按惯例，我们通常给类型构造函数参数使用一个字母的大写名称，例如 `F`、`G` 和 `H`，有时也使用 `M` 和 `N`，或 `P` 和 `Q`。
-   53. 希腊前缀 *endo-* 意为 *内部*，即在自同态函数的陪域在其定义域之内的意义上。
-   54. 希腊前缀 *endo-* 意为 *内部*，即在自同态函数的陪域在其定义域之内的意义上。
