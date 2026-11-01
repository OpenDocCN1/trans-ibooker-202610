

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
