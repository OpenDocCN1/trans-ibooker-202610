

AbstractVisitor，Circle 会判断给定的 visitor 是否支持圆形。它通过执行向 Visitor<Circle> 的 dynamic_cast 来实现 ④。请注意，它执行的是指针转换，这意味着 dynamic_cast 要么返回一个指向 Visitor<Circle> 的有效指针，要么返回一个 nullptr。如果它返回一个指向 Visitor<Circle> 的有效指针，它就会调用相应的 visit() 函数 ⑤。

虽然这种方法肯定有效，并且是打破 Visitor 设计模式循环依赖的一部分，但 dynamic_cast 总是让人感觉不舒服。dynamic_cast 总是应该感觉有点可疑，因为如果使用不当，它会破坏架构。当我们从架构的高层向架构的低层进行转换时就会发生这种情况。¹⁶ 在我们的案例中，使用它实际上是可以的，因为使用发生在我们架构的低层。因此，我们不会通过将低层的知识插入高层来破坏架构。

真正的缺陷在于运行时的开销。当你对无环 Visitor 运行与“Guideline 17: Consider std::variant for Implementing Visitors”中相同的基准测试时，你会发现其运行时间几乎比循环 Visitor 的运行时间高出一个数量级（参见表 4-3）。原因在于 dynamic_cast 很慢。非常慢。对于此应用来说尤其慢。我们在这里做的是交叉转换。我们不是简单地向下转换为某个派生类，而是转换到继承层次结构中的另一个分支。这种交叉转换，后跟一次虚函数调用，比简单的向下转换要昂贵得多。

## 表 4-3. 不同 Visitor 实现的性能结果

| GCC 11.1 | Clang 11.1 |
|---|---|
| Acyclic Visitor | 14.3423s | 7.3445s |
| Cyclic Visitor | 1.6161s | 1.8015s |
| Object-oriented solution | 1.5205s | 1.1480s |
| Enum solution | 1.2179s | 1.1200s |
| std::variant (with std::visit()) | 1.1992s | 1.2279s |
| std::variant (with std::get()) | 1.0252s | 0.6998s |

从架构上讲，无环 Visitor 是一个非常有趣的替代方案，但从实用角度来看，这些性能结果可能会使其不合格。这并不意味着你不应该使用它，但至少要意识到糟糕的性能可能是选择其他解决方案的非常有力的论据。

## GUIDELINE 18: BEWARE THE PERFORMANCE OF ACYCLIC VISITORS

- Understand the architectural advantages of an Acyclic Visitor;
- Be aware of the significant performance disadvantages of that solution.

1 我知道你在翻白眼！“哦，又是那个无聊的例子！”但，请考虑跳过了第 3 章的读者。他们现在很高兴可以在不阅读关于该场景的冗长解释的情况下阅读本节。明白了吗？

2 自 C++11 起，我们可以使用作用域枚举，有时也因其语法 enum class 而被称为类枚举。这例如可以帮助编译器更好地警告不完整的 switch 语句。如果你发现了这个不完美之处，你就获得了一个加分点！

3 Scott Meyers，“More Effective C++”，Item 31，Addison Wesley，1995 年 12 月，ISBN：978-0201633719。

4 请注意，数学上的开集和闭集概念是完全不同的东西。

5 作为静态多态设计的一个例子，请考虑 *Standard Template Library (STL)* 中的算法。你可以轻松添加新的操作，即算法，也可以轻松添加可以被复制、排序等的新类型。

6 做出预测总是很难，尤其是涉及未来的预测。但我们通常对代码库的演变有相当好的了解。如果你在事情如何发展方面没有任何想法，你应该等待第一次更改或扩展，从中学习并做出更明智的决定。这种哲学是广为人知的 *YAGNI* 原则（*You Aren’t Gonna Need It*）的一部分，它警告你不要过度设计；另请参阅“Guideline 2: Design for Change”。

7 我不会对此感到高兴，也许甚至会非常不高兴，但我可能不会生气。但你的其他同事呢？在最坏的情况下，你可能会被排除在下一次团队烧烤之外。

8 `accept()` 是 GoF 书中使用的名称。因此在 *Visitor* 设计模式的上下文中它是传统名称。当然你可以自由使用其他名称，例如 `apply()`。但在重命名之前，请考虑“Guideline 14: Use a Design Pattern’s Name to Communicate Intent”中的建议。

9 真的建议将逻辑提取到单个函数中。原因在于变更：如果以后必须更新实现，你不希望多次执行更改。这就是 *DRY (Don’t Repeat Yourself)* 原则的想法。所以请记住“Guideline 2: Design for Change”。

10 考虑风险：这可能会让你终身被排除在团队烧烤之外！

11 当你使用 `std::make_unique()`，它封装了对 `new` 的调用，而不是某些特殊用途的分配方案时，内存碎片化更有可能发生。

12 我确实在使用随机向量，通过 `std::mt19937` 和 `std::uniform_real_distribution` 创建，但只有在向自己证明性能对 GCC-11.1 没有变化、对 Clang-11.1 只有轻微变化之后才使用。显然，创建随机数本身并不特别昂贵（至少在我机器上如此）。既然你承诺将这些视为定性结果，我们应该没问题。

13 还有其他开源的 `variant` 替代实现。Boost 库提供了两种实现，Abseil 提供了 variant 实现，并且值得一看 Michael Park 的实现。

14 *Proxy* 模式是 GoF 设计模式中的另一个模式，由于篇幅有限，我很遗憾本书中没有涵盖。我将详细介绍 *Bridge* 设计模式；参见“Guideline 28: Build Bridges to Remove Physical Dependencies”。

15 有关该模式发明者对 *Acyclic Visitor* 模式的更多信息，请参阅 Robert C. Martin：“Agile Software Development - Principles, Patterns, and Practices”，Pearson，2002，ISBN：978-0135974445。

16 请参阅“Guideline 9: Pay Attention to the Ownership of Abstractions”以了解 high-level 和 low-level 术语的定义。

## 第 5 章. 策略和命令设计模式

## 早期发布读者的说明

使用 Early Release 电子书，你可以在这些技术正式发布之前很久就获得书籍的最初形式——作者在写作时的原始未编辑内容。

这将是最终书籍的第 5 章。请注意 GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系编辑 sevans@oreilly.com。

本章专门介绍两种最常用的设计模式：*Strategy* 设计模式和 *Command* 设计模式。确实是最常用的：C++ 标准库本身就多次使用它们数十次，你本人很可能也多次使用过它们。因此，这两者可以被视为每个开发人员的基本工具。

在“Guideline 19: Use Strategy to Isolate How Things are Done”中，我将向你介绍 *Strategy* 设计模式。我将演示为什么这是最有用和最重要的设计模式之一，以及为什么你在许多情况下会发现它很有用。

在“Guideline 20: Favor Composition Over Inheritance”中，我们将看看继承以及为什么这么多人抱怨它。你会看到它本身并不坏，但像其他一切一样，它也有其优点和局限性。最重要的是，我将解释，许多人

