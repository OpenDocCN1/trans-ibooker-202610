
List<Charge>。最后一步是将数据重构为所需的输出。这是通过构造一个 Pair，将 List<Coffee> 映射到列表中所有咖啡的合并 Charges 来完成的。reduce 是高阶函数的示例，将在第 2 章中正式介绍。

### SIDEBAR 通过解构提取值

Kotlin 允许我们解构对象（也称为分解或提取）。当赋值（左侧）中的值从表达式（右侧）中提取时就会发生这种情况。当我们想要将 Pair 解构为其左右组件时，我们只需将包含的值赋给它们，用逗号分隔并用一对括号 ( 和 ) 包围：

```
val (left, right) = Pair(1, 2)
assert left == 1
assert right == 2
```

后续代码中，我们现在可以像在 Kotlin 中使用任何值一样使用这些解构后的值。也可以通过将其替换为下划线 _ 来忽略不需要的解构值：

```
val (_, right) = Pair(1, 2)
```

解构并不局限于 Pair 类型，也可用于其他许多类型，例如 List 甚至数据类。

总体而言，这一解决方案有了显著改进——我们现在可以直接重用 buyCoffee 来定义 buyCoffees 函数，并且这两个函数都可以轻松测试，而无需为某些 Payments 接口定义复杂的模拟实现！事实上，Cafe 现在完全不知道 Charge 值将如何被处理。当然，我们仍然可以拥有一个 Payments 类来实际处理费用，但 Cafe 不需要知道它。将 Charge 设为一等值还有其他我们可能没有预料到的好处：我们可以更轻松地组装用于处理这些费用的业务逻辑。例如，Alice 可能会带笔记本电脑到咖啡店工作几个小时，偶尔购买一些东西。如果咖啡店能够将 Alice 做的这些购买合并为一次收费，那就很好了，这样又可以节省信用卡处理费。由于 Charge 是第一等值，我们现在可以向 List<Charge> 添加以下扩展方法，以便合并任何同一卡的费用：

### Listing 1.6 合并费用

```
fun List<Charge>.coalesce() : List<Charge> =
    this.groupBy { it.cc }.values
        .map { it.reduce { a, b -> a.combine(b) } }
```

目前我们需要知道的是，我们正在通过使用扩展方法添加一些行为，在本例中是为 List<Charge> 添加一个 coalesce 函数。让我们关注此方法的正文。你会注意到我们正在将函数作为值传递给 `groupBy`、`map` 和 `reduce` 函数。如果你还不能，现在在接下来的几章中你将学会阅读和编写这种一行代码。语句 { `it.cc` } 和 { `a, b -> a.combine(b)` } 是匿名函数的语法，我们将在下一章介绍。你可能会发现这种代码难以阅读，因为记号非常紧凑。但随着你阅读本书，阅读和编写这种 Kotlin 代码会很快变得得心应手。该函数接收一个费用列表，按使用的信用卡分组，然后将它们合并为每个卡片的一笔费用。它完全可重用且可测试，无需任何额外的模拟对象或接口。试想用我们第一个 `buyCoffee` 实现来实现同样的逻辑！

这只是函数式编程具有所宣称的好处的一个小尝，而本示例是有意保持简单的。如果这里使用的重构系列看起来自然、显而易见、不引人注意或符合标准做法，那很好。FP 只是一种纪律，它将许多人认为的好主意推向其逻辑终点，即使在适用性不太明显的情况下也应用这种纪律。正如你在本书中将学到的那样，始终如一地遵循 FP 纪律的后果是深远的，其好处是巨大的。FP 是对程序在各个层面上组织方式的真正根本性转变——从最简单的循环到高级程序架构。由此产生的风格相当不同，但它是一种优美且连贯的编程方法，我们希望你能学会欣赏。

### SIDEBAR 现实世界的情况如何？

我们在 `buyCoffee` 的例子中看到，我们如何将 `Charge` 的创建与该 `Charge` 的解释或处理分离。一般来说，我们将学习如何将这种转换应用于具有副作用的 *any* 函数，以将这些副作用推到程序的外层。函数式程序员经常谈论用纯核心和外部处理副作用的薄层来实现程序。

但即便如此，我们肯定在某个时刻必须对世界产生实际影响，并将 `Charge` 提交给某个外部系统进行处理。难道没有其他有用的程序需要副作用或可变性吗？我们如何编写这样的程序？在阅读本书的过程中，我们会发现许多看似需要副作用的程序都有某种函数式类比。在其他情况下，我们会找到结构化代码的方法，使副作用发生但不可 *observable*。（例如，如果我们确保它不能在该函数体之外被引用，我们可以修改在某个函数体中声明为本地的数据，或者只要封闭函数无法观察到这种情况发生，我们就可以写入文件。）

## 1.2 究竟什么是（纯）函数？

我们之前说过 FP 意味着使用纯函数编程，而纯函数是没有副作用的函数。在咖啡店示例的讨论中，我们基于副作用和纯度的非正式概念。这里我们将形式化这一概念，以便更精确地指出函数式编程的含义。这也将让我们更深入地了解函数式编程的一个好处：纯函数更容易推理。

具有输入类型 A 和输出类型 B 的函数 f（在 Kotlin 中写为单一类型：`(A) -> B`，读作“A 到 B”）是一种计算，它将类型 A 的每个值 a 关联到恰好一个类型 B 的值 b，使得 b 完全由 a 的值决定。内部或外部过程的任何状态变化与计算结果 `f(a)` 无关。例如，具有类型 `(Int) -> String` 的函数 `intToString` 将每个整数映射到相应的字符串。此外，如果它真的是一个函数，它不会做其他任何事情。

![](img/255e7d4fd2ecc6516db2405b06be1018_12_0.png)

Figure 1.2 A pure function, does only what it states without side effect.

In other words, a function has no observable effect on the execution of the program other than to compute a result given its inputs; we say that it has no side effects. We sometimes qualify such functions as *pure functions* to make this more explicit, but this is somewhat redundant. Unless we state otherwise, we’ll often use *function* to imply no side effects².

换句话说，函数对程序执行的唯一可观察影响就是根据其输入计算结果；我们说它没有副作用。我们有时将此类函数限定为 *pure functions* 以使其更明确，但这有点冗余。除非另有说明，我们通常会使用 *function* 来暗示没有副作用²。

你应该已经熟悉很多纯函数了。考虑加法 (+) 运算符，它解析为所有整数上的 `plus` 函数。它接收一个整数值并返回另一个整数值。对于任意两个给定的整数，它总是返回相同的整数值。另一个例子是 Java、Kotlin 以及许多其他语言中 String 的 length 函数，其中字符串不可修改（是不可变的）。对于任何给定的字符串，总是返回相同的长度且不会发生其他事情。

我们可以利用引用透明性（RT）的概念来形式化纯函数的这一思想。这是对表达式的一般属性，而不仅仅是函数。为了我们的讨论，认为表达式是程序中可以求值得到结果的任何部分——任何你可以在 Kotlin 解释器中输入并得到答案的东西。例如，

2 + 3 is an expression that applies the pure function `plus` on 2 to 3 (which is also an expression). This has no side effect. The evaluation of this expression results in the same value 5 every time. In fact, if we saw 2 + 3 in a program we could simply replace it with the value 5 and it wouldn’t change a thing about the meaning of our program.

这就是表达式具有引用透明性的全部含义——在任何程序中，表达式都可以被其结果替换，而不会改变程序的含义。我们说一个函数是纯的，如果用 RT 参数调用它也是 RT。我们接下来会看一些例子。

> **SIDEBAR 引用透明性和纯度**
> 
> An expression `e` is referentially transparent if, for all programs `p`, all occurrences of `e` in `p` can be replaced by the result of evaluating `e` without affecting the meaning of `p`. A function `f` is pure if the expression `f(x)` is referentially transparent for all referentially transparent `x`.

## 1.3 引用透明性、纯度和替换模型

让我们看看 RT 的定义如何适用于我们原始的 `buyCoffee` 示例：

```
fun buyCoffee(cc: CreditCard): Coffee {
  val cup = Coffee()
  cc.charge(cup.price)
  return cup
}
```

Whatever the return type of `cc.charge(cup.price)`, even if it’s `Unit`, is discarded by `buyCoffee`. Thus, the result of evaluating `buyCoffee(aliceCreditCard)` will be merely `cup`, which is equivalent to a new `Coffee()`. For `buyCoffee` to be pure, by our definition of RT, it must be the case that `p(buyCoffee(aliceCreditCard))` behaves the same as `p(Coffee())`, for any `p`. This clearly doesn’t hold—the program `Coffee()` doesn’t do anything, whereas `buyCoffee(aliceCreditCard)` will contact the credit card provider and authorize a charge. Already we have an observable difference between the two programs.

Referential transparency enforces the rule that everything a function does should be represented by the value that it returns, according to the result type of the function. This constraint enables a simple and natural mode of reasoning about program evaluation called the substitution model. When expressions are referentially transparent, we can imagine that computation proceeds much like we’d solve an algebraic equation. We fully expand every part of an expression, replacing all variables with their referents, and then reduce it to its simplest form. At each step we replace a term with an equivalent one; computation proceeds by substituting equals for equals. In other words, RT enables equational reasoning about programs.

Let’s look at two more examples—one where all expressions are RT and can be reasoned about
