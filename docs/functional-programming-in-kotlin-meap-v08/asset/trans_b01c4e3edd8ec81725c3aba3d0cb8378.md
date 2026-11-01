
## Listing 11.8 使用 map2 组合子实现 product 的通用实现。

```
fun <A, B> product(
    ma: Kind<F, A>,
    mb: Kind<F, B>
): Kind<F, Pair<A, B>> =
    map2(ma, mb) { a, b -> Pair(a, b) }
```

我们不必将自己限制在已经看到的组合子上。我们也应该有探索新解决方案的自由。

## 练习 11.6（困难）

这里有一个我们之前没有见过的函数的例子。实现函数 `filterM`。它有点像 `filter`，只是我们不是有一个从 `(A) -> Boolean` 的函数，而是有 `(A) -> Kind<F, Boolean>`。用单子等价物替换各种普通函数如 `filter` 通常会产生有趣的结果。实现此函数，然后思考它对 `Par`、`Option` 和 `Gen` 等各种数据类型意味着什么。

```
fun <A> filterM(
    ms: List<A>,
    f: (A) -> Kind<F, Boolean>
): Kind<F, List<A>> = TODO()
```

我们在这里看到的组合子只是 `Monad` 让我们一次又一次实现的全库中的一小部分。我们将在第 13 章看到更多示例。

## 11.4 Monad 定律

像 monad 和 functor 这样的代数概念是定义和规范它们的定律的体现。在本节中，我们将介绍规范我们 `Monad` 接口的定律。当然，我们会期望 functor 定律也适用于 `Monad`，因为 `Monad<F>` 是一个 `Functor<F>`，但我们还期望什么？什么定律应该约束 `flatMap` 和 `unit`？简而言之，我们可以引用几个满足这些约束的定律：

- 结合律
- *左* 恒等律
- *右* 恒等律

本节将逐一查看每一个，同时证明它们对 monad 成立。

### 11.4.1 结合律

我们要研究的第一个单子定律是结合律。这个定律完全关乎操作的顺序。让我们通过示例来看这个问题。如果我们想将三个单子值合并成一个，应该先合并哪两个？这重要吗？为了解答这个问题，让我们暂时离开抽象层面，看看使用 Gen monad 的一个简单具体示例。

假设我们在测试一个产品订单系统，需要为测试生成一些假订单作为 fixture。我们可能有一个 Order 数据类和该类的生成器。

## Listing 11.9 Item 和 Order 文本 fixture 生成器的声明

```
data class Order(val item: Item, val quantity: Int)
data class Item(val name: String, val price: Double)

val genOrder: Gen<Order> =
    Gen.string().flatMap { name: String ->
        Gen.double(0..10).flatMap { price: Double ->
            Gen.choose(1, 100).map { quantity: Int ->
                Order(Item(name, price), quantity)
            }
        }
    }
```

- ① 生成一个随机字符串名称
- ② 生成 0 到 10 之间的双精度价格
- ③ 生成 1 到 100 之间的整数数量

这里我们在内联生成 Item（来自 name 和 price），但可能有地方我们想单独生成一个 Item。我们可以将其提取到自己的生成器中：

```
val genItem: Gen<Item> =
    Gen.string().flatMap { name: String ->
        Gen.double(0..10).map { price: Double ->
            Item(name, price)
        }
    }
```

这现在可以用来生成订单：

```
val genOrder2: Gen<Order> =
    Gen.choose(1, 100).flatMap { quantity: Int ->
        genItem.map { item: Item ->
            Order(item, quantity)
        }
    }
```

这应该做完全相同的事情，对吧？似乎可以安全地假设。但别急！我们怎么能确定？它并不完全是相同的代码！

让我们将 genOrder 的实现展开为对 map 和 flatMap 的调用，以便更好地了解正在发生的事情。

```
val genOrder3: Gen<Order> =
    Gen.choose(1, 100).flatMap { quantity: Int ->
        Gen.string().flatMap { name: String ->
            Gen.double(0..10).map { price: Double ->
                Order(Item(name, price), quantity)
            }
        }
    }
```

当我们将此与 listing 11.8 进行比较时，我们可以清楚地看到它们并不相同，但似乎完全合理地假设这两个实现做完全相同的事情。事实上，即使顺序已经改变，如果它们不这样做也会令人惊讶和奇怪。这是因为我们假设 flatMap 遵守结合律。

## Listing 11.10 用 flatMap 表示结合律

```
x.flatMap(f).flatMap(g) ==
    x.flatMap { a -> f(a).flatMap(g) }
```

并且此定律应适用于所有适当类型的值 x、f 和 g —— 不仅适用于 Gen，还适用于 Parser、Option 或任何其他 monad。

### 11.4.2 对特定 monad 证明结合律

到目前为止，我们一直严格在抽象层面处理。但这对现实情况有什么影响？它如何适用于我们在过去章节中处理的数据类型？为了找出答案，让我们证明此定律对 Option 成立。我们所要做的就是用 None 或 Some(v) 替换前一等式中的 x 并展开其两侧。我们将从 x 为 None 的情况开始：

```
None.flatMap(f).flatMap(g) ==
    None.flatMap { a -> f(a).flatMap(g) }
```

由于 None.flatMap(f) 对所有 f 都是 None，这可以简化为：

```
None == None
```

换句话说，该定律对 None 成立。让我们确认当 x 为任意值 v 的 Some(v) 时也是如此。

## Listing 11.11 通过将 x 替换为 Some(v) 验证结合律

```
x.flatMap(f).flatMap(g) == x.flatMap { a -> f(a).flatMap(g) } ①

Some(v).flatMap(f).flatMap(g) ==
    Some(v).flatMap { a -> f(a).flatMap(g) } ②

f(v).flatMap(g) == { a: Int -> f(a).flatMap(g) }(v) ③

f(v).flatMap(g) == f(v).flatMap(g) ④
```

- 1. `flatMap` 的原始结合律
- 2. 在两侧将 x 替换为 `Some(v)`
- 3. 通过直接对 f 应用 v 在两侧折叠 `Some(v).flatMap`
- 4. 在右侧直接对 g 应用 v，证明相等

因此我们可以得出结论，此定律在 x 为任意值 v 的 Some(v) 时也成立。我们可以因此得出结论，该定律对 Option 的两种情况都成立。

## KLEISLI 组合：对结合律更清晰的视角

在前面的示例中很难识别结合律。相比之下，还记得 monoid 的结合律有多清晰吗？

```
combine(combine(x,y), z) == combine(x, combine(y,z))
```

我们的 monad 结合律看起来一点也不像那样！幸运的是，有一种方法可以通过考虑单子函数而不是我们到目前为止一直处理的单子值，使此定律更清晰。

我们到底是什么意思单子函数，它与我们迄今为止看到的单子值有何不同？如果单子值是 F<A> 的实例，单子函数是形式为 (A) F<B> 的函数。这种函数被称为 Kleisli 箭头，以瑞士数学家 Heinrich Kleisli 的名字命名。Kleisli 箭头特别之处在于它们可以相互组合：

```
fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C>
```

## 练习 11.7

在 Monad 中实现以下 Kleisli 组合函数。

```
fun <A, B, C> compose(
    f: (A) -> Kind<F, B>,
    g: (B) -> Kind<F, C>
): (A) -> Kind<F, C> = TODO()
```

考虑到 flatMap 将 Kleisli 箭头作为参数，我们现在可以使用此新函数以更对称的方式陈述 monad 的结合律。

## Listing 11.12 用 compose 表示结合律

```
compose(compose(f, g), h) == compose(f, compose(g, h))
```

## 练习 11.8（困难）

用 compose 的抽象定义实现 flatMap。通过这种方式，似乎我们找到了另一组最小的 monad 组合子：compose 和 unit。

```
fun <A, B> flatMap(
  fa: Kind<F, A>,
  f: (A) -> Kind<F, B>
): Kind<F, B> = TODO()
```

## 用 flatMap 和 compose 验证结合性

在 listing 11.10 中，我们用 flatMap 表示了 monad 的结合律。然后我们在 listing 11.12 中使用 compose 选择了此定律的更清晰表示。在本节中，我们将通过使用练习 11.8 中得出的 flatMap 实现，对用 compose 表示的定律应用替换模型，证明这两个证明是等价的。为了简单起见，我们一次看一侧。让我们先关注等式的左侧。

## Listing 11.13 将替换模型应用于用 compose 表示的结合律的左侧

```
compose(compose(f, g), h) ①

{ a -> flatMap(compose(f, g)(a), h) } ②

{ a -> flatMap({ b: A -> flatMap(f(b), g) }(a), h) } ③

{ a -> flatMap(flatMap(f(a), g), h) } ④

flatMap(flatMap(x, g), h) ⑤
```

- 1. 用 compose 表示的结合律的左侧
- 2. 用 flatMap 替换外部 compose，传播 a
- 3. 用 flatMap 替换内部 compose，传播 b
- 4. 将 a 通过 b 应用到 f
- 5. 通过为应用了 a 的任意 f 引入别名 x 来简化

接下来，我们将注意力转向右侧。
