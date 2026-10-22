

这个 `copy_if()` 算法也遵循 *DIP*。依赖倒置是通过概念 `InputIt`、`OutputIt` 和 `UnaryPredicate` 实现的。这三个概念代表了对传递的迭代器和谓词的要求，需要由调用代码来满足。通过概念来指定这些要求，即通过拥有这些概念，`std::copy_if()` 使其他代码依赖于自身，而自身不依赖于其他代码。这种依赖结构在图 2-8 中有所体现：容器和谓词都依赖于相应算法表达的要求。因此如果我们考虑标准库内部的架构，那么 `std::copy_if()` 是架构的高层部分，容器和谓词（即函数对象、lambda 等）是架构的低层部分。

Algorithms

High-level
(stable,
low dependencies)

Architectural Boundary

Low-level
(malleable, volatile
high dependencies)

Iterator Concepts

Predicate Concepts

Containers

Predicates

Figure 2-8. Dependency structure of the STL algorithms.

## Dependency Inversion via Overload Sets

继承层次结构和概念并不是倒置依赖的唯一手段。任何形式的抽象都能够做到这一点。因此，超载集合也能让你遵循 DIP 并不令人意外。正如你在“准则 8：理解超载集合的语义要求”中所见，超载集合代表一种抽象，因而是一组语义要求和期望。与基类和概念相比，遗憾的是并没有代码明确描述这些要求。但如果这些要求由架构中的更高层拥有，你就可以实现依赖倒置。例如，考虑以下 Widget 类模板：

//---- <Widget.h> ----------------

#include <utility>

template< typename T >
struct Widget
{
    T value;
};

template< typename T >
void swap( Widget<T>& lhs, Widget<T>& rhs )
{
    using std::swap;
    swap( lhs.value, rhs.value );
}
```

Widget 拥有一个未知类型 T 的数据成员。尽管 T 未知，但仍可以通过构建在 swap() 函数的语义期望之上来实现 Widget 的自定义 swap() 函数。只要 T 的 swap() 函数遵循 swap() 的所有期望，从而遵循里氏替换原则 (LSP)，该实现就有效：21

```
#include <Widget.h>
#include <assert>
#include <cstdlib>
#include <string>

int main()
{
    Widget<std::string> w1{ "Hello" };
    Widget<std::string> w2{ "World" };

    swap( w1, w2 );

    assert( w1.value == "World" );
    assert( w2.value == "Hello" );

    return EXIT_SUCCESS;
}
```

因此，Widget swap() 函数本身遵循期望并从而加入超载集合，类似于派生类所做的那样。swap() 超载集合的依赖结构如图 2-9 所示：由于超载集合的要求，即期望，是架构高层的一部分，并且任何 swap() 的实现都依赖于这些期望，因此依赖关系从低层指向高层。依赖关系因此被正确倒置。

std::swap()

High-level
(stable,
low dependencies)

Architectural
Boundary

Low-level
(malleable, volatile
high dependencies)

Widget swap()

Figure 2-9. Dependency structure of the swap () overload set.

## Dependency Inversion Principle vs. Single-Responsibility Principle

正如我们所见，DIP 是通过正确分配所有权并正确分组真正属于一起的事物来实现的。从这个角度来看，将 DIP 视为 SRP 的另一个特例（类似于 ISP）似乎是合理的。然而，希望你能看到 DIP 不止于此。与 SRP 不同，DIP 非常关注架构的观点，我认为它是构建适当全局依赖结构的重要建议。

总之，为了构建具有适当依赖结构的适当架构，关注抽象的所有权至关重要。由于抽象代表对实现的要求，它们应属于高层，以便将所有依赖引导向高层。

## GUIDELINE 9: PAY ATTENTION TO THE OWNERSHIP OF ABSTRACTIONS

- 请记住，在适当的架构中，低层实现细节依赖于高层抽象；
- 遵循依赖倒置原则 (DIP) 并将抽象分配给架构的高层；
- 确保抽象由高层拥有，而不是由低层拥有。

## Guideline 10: Consider Creating an Architectural Document

让我们聊聊你的架构。让我从一个非常简单的问题开始：你有架构文档吗？任何总结你架构主要要点和基本决策的计划或描述，并展示高层、低层以及它们之间的依赖关系？如果你的回答是肯定的，那么你可以跳过本准则继续下一条。如果你的回答是否定的，那么让我问几个后续问题：你有持续集成 (CI) 环境吗？你使用自动化测试吗？你应用静态代码分析工具吗？全部是？好，还有希望……剩下的唯一问题是：为什么你没有架构文档？

“哦，来吧，别把蚊子说成大象。缺少架构文档并不是世界末日！毕竟我们是敏捷的，我们可以快速改变事物！”……想象一下我完全空白的表情，随后是一声长叹。老实说，我担心这会是你的解释。这不幸的是我听到的太多的说法。可能存在误解：能够快速改变事物的能力并不是敏捷方法论的重点。可悲的是，我也必须告诉你，你的回答毫无意义。你完全可以回答“毕竟我们喜欢巧克力！”或“毕竟我们在脖子上戴胡萝卜！”为了解释我的意思，我应该快速总结敏捷方法论的要点，然后随后解释你为什么应该投资于架构文档。

期望敏捷方法有助于快速改变事物的想法相当普遍。然而，正如最近几年的一些作者所阐明的，敏捷方法论的主要，也是可能唯一的要点是获得快速反馈。²² 在敏捷方法中，整个软件开发过程都围绕它构建：由于业务实践（例如规划、小型发布和验收测试）而获得快速反馈，由于团队实践（例如集体所有权、持续集成 (CI) 和站会）而获得快速反馈，以及由于技术实践（例如测试驱动开发、重构和结对编程）而获得快速反馈。然而，与普遍的看法相反，快速反馈并不意味着你可以快速轻松地改变你的软件。虽然当然关键是快速知道需要做些什么，但能够快速改变软件的能力是你仅通过良好的软件设计和架构获得的能力。这两者为你节省了改变事物的赫拉克勒斯式的努力，而不是快速反馈，它只告诉你有些东西坏了。

“好，你是对的，我明白你的观点，关注良好的软件设计和架构很重要。但架构文档的意义是什么？”我很高兴我们达成一致。那是一个绝佳的问题。我认为我们在取得进展。为了解释架构文档的目的，让我给出架构的另一个定义：²³

> 在大多数成功的软件项目中，参与该项目的专家开发人员对系统设计有共同的理解。这种共同的理解被称为‘架构。’
> 
> —Ralph Johnson

Ralph Johnson 将架构描述为对代码库的共同理解，整体愿景。让我们假设没有架构文档，没有任何总结全局图景、代码库全局愿景的东西。让我们也假设你自己相信对代码库的架构有非常清晰的想法。那么这里还有一个问题：你的团队有多少开发人员？你确定所有这些开发人员都熟悉你脑中的架构吗？你确定他们都共享相同的愿景吗？你确定他们都在帮助你朝同一个方向前进吗？

如果你的回答是肯定的，那么你可能还没有明白这一点。相当确定每个开发人员都有不同的经验和略有不同的术语。也很确定每个开发人员对代码的看法不同，因此对当前架构有略有不同的想法。而对现状的这种略有不同的看法可能会导致对未来的略有不同的愿景。虽然这在短期内可能不会立即显现，但从长远来看，出现意外、误解、误读的可能性很大。

