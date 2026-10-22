

例如，考虑 *Strategy* 设计模式，它是 GoF 设计模式之一，如图 3-1 所示。无需过多细节，*Strategy* 设计模式以 `Strategy` 基类的形式引入了一种抽象。该基类将 `Strategy` 的使用者（在架构高层中的 `Context` 类）与具体策略的实现细节（在架构低层中的 `ConcreteStrategyA` 和 `ConcreteStrategyB`）解耦。因此，*Strategy* 满足了设计模式的属性。⁵

![](img/7b1b8ee332752e5702cc436a0cba7c1c_135_0.png)

图 3-1. GoF Strategy 设计模式。

一个类似的例子是 Factory Method 设计模式（又一个 GoF 设计模式；参见图 3-2）。Factory Method 的意图是解耦特定产品的创建。为此，它以 `Product` 和 `Creator` 基类的形式引入了两个

抽象，架构上位于高层。实现细节，由 `ConcreteProduct` 和 `ConcreteCreator` 类给出，位于架构的低层。通过这种架构结构，*Factory Method* 也符合设计模式的定义：它有一个名称，解耦的意图，并且引入了抽象。

Creator

virtual factoryMethod() = 0

product = factoryMethod();

Product

High-level
(stable, low
dependencies)

Architectural
Boundary

Low-level
(volatile, malleable,
high dependencies)

ConcreteProduct

ConcreteCreator

virtual factoryMethod()

return new ConcreteProduct();

图 3-2. GoF Factory Method 设计模式。

你应该注意，设计模式引入的抽象不一定是通过基类引入的。正如我在以下各节和章节中将向你展示的那样，这种抽象可以通过许多不同的方式引入，例如通过模板，或简单地通过函数重载。同样，设计模式并不意味着任何特定的实现。

作为一个反例，让我们考虑 std::make_unique() 函数：

```
namespace std {

template< typename T, typename... Args >
unique_ptr<T> make_unique( Args&&... args );

} // namespace std
```

在 C++ 社区中，我们经常将 std::make_unique() 函数称为工厂函数。需要注意的是，虽然工厂函数这个术语给人一种 std::make_unique() 是 Factory Method 设计模式的一个例子的印象，但这种印象是错误的。设计模式通过引入抽象来帮助你解耦，从而允许你自定义和延迟实现细节。特别是，Factory Method 设计模式的意图是为对象实例化引入一个定制点。std::make_unique() 并没有提供这样的定制点：如果使用 std::make_unique()，你就知道你会得到一个指向你所请求类型的 std::unique_ptr，并且实例将通过 new 创建：

```
// This will create a 'Widget' by means of calling 'new'
auto ptr = std::make_unique<Widget>( /* some Widget arguments */ );
```

由于 std::make_unique() 没有提供任何自定义该行为的方法，它就无法帮助减少实体之间的耦合，因此它不能服务于设计模式的目的。⁶ 尽管如此，

`std::make_unique()` 是一种针对特定问题的反复出现的解决方案。换句话说，它是一种模式。然而，它不是 *design pattern*，而是 *implementation pattern*。它是一种将实现细节封装起来的流行解决方案（在本例中是生成 `Widget` 的实例），但它并没有抽象出你得到的是什么以及它将如何被创建。因此它属于 *Implementation Details* 层级，但不属于 *Software Design* 层级（参见图 1-1）。

引入抽象是将软件实体彼此解耦并为变更和扩展而设计的关键。`std::make_unique()` 函数模板中没有抽象，因此你也没有办法扩展其功能（你甚至无法正确重载或特化）。相比之下，*Factory Method* 设计模式 *确实* 提供了对 *创建什么* 以及 *如何创建*（包括实例化前后的操作）的抽象。由于这种抽象，你将能够在以后编写新的工厂，而无需更改现有代码。因此，设计模式帮助你解耦并扩展你的软件，而 `std::make_unique()` 只是一种 *implementation pattern*。

## A Design Pattern Has Been Proven

最后但并非最不重要的一点是，设计模式已经 **经过多年的验证**。四人帮并没有收集所有可能的解决方案，而是只收集了在不同的代码库中被普遍使用来解决相同问题的解决方案（尽管实现可能不同）。因此，一个解决方案必须多次证明其价值后才会成为模式。

总结一下：设计模式是一种经过验证的、命名的解决方案，表达了非常具体的意图。它引入某种抽象，有助于解耦软件实体，从而帮助管理软件实体之间的交互。正如我们应该使用 *Design* 这个术语来表示管理依赖关系和解耦的艺术（参见“Guideline 1: Understand the Importance of Software Design”），我们也应该准确、有目的地使用 *Design Pattern* 这个术语。所以请记住：

## GUIDELINE 11: UNDERSTAND THE PURPOSE OF DESIGN PATTERNS

- - 理解设计模式是经过验证的、命名的解决方案，其意图是解耦；
- - 认识到设计模式引入某种抽象；
- - 记住设计模式针对的是软件设计，即帮助管理依赖关系；
- - 注意设计模式与实现模式之间的区别。

## Guideline 12: Beware of Design Pattern Misconceptions

上一节重点解释了设计模式的目的：名称、意图和某种形式的抽象以解耦软件实体。然而，正如理解设计模式是什么很重要一样，理解设计模式不是什么也很重要。不幸的是，关于设计模式存在几种常见的误解：有些人 ...

- - ... 将设计模式视为目标，并将其视为实现良好软件质量的保证。
- - ... 认为设计模式基于特定的实现，因此是语言特定的惯用法。
- - ... 说设计模式仅限于面向对象编程和动态多态。
- - ... 认为设计模式过时甚至已过时。

这些误解不足为奇，因为我们很少谈论设计，而是专注于功能和语言机制（参见“Guideline 1: Understand the Importance of Software Design”）。因此，我将在本节中驳斥前三种误解，并在下一节中处理第四种误解。

## Design Patterns are not a Goal

一些开发者热爱设计模式。他们痴迷于此，以至于试图用设计模式解决所有问题，无论是否合理。当然，这种思维方式可能会增加代码的复杂性，降低可理解性，因此可能适得其反。因此，设计模式的过度使用可能会导致其他开发者沮丧，对设计模式的整体声誉不佳，甚至拒绝模式的一般思想。

明确地说：设计模式 *不是* 目标。它们是实现目标的手段。它们可能是解决方案的一部分。但它们不是目标。正如 Venkat Subramaniam 会说的那样：如果你在早上起床时想“今天我将使用什么设计模式？”，那么这就是你错过了设计模式目的的明显迹象。⁷ 使用尽可能多的设计模式没有奖励，没有奖牌。设计模式的使用不应创建复杂性，而恰恰相反，应减少复杂性。代码应该变得更简单、更易理解、更容易更改和维护，只是因为设计模式应该有助于解决依赖关系并创建更好的结构。因此，如果使用设计模式导致复杂性更高并为其他开发者带来问题，显然这不是正确的解决方案。

为了明确：我并不是告诉你不要使用设计模式。我只是告诉你不要过度使用它们，就像我会告诉你不要过度使用任何其他工具一样。这始终取决于问题。例如，锤子是一个很棒的工具，只要你的问题是钉子。一旦你的问题变成螺丝，锤子就变得有些笨拙了。⁸ 为了正确使用设计模式，为了知道何时使用它们

