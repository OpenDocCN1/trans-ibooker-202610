

> *如果我能回到过去并且有能力改变 C++，与其添加虚函数，我会添加对类型擦除和概念的语言支持。定义一个单类型概念，并为其自动生成一个类型擦除包装器。*
> 
> —Eric Niebler

然而，要确立 *Type Erasure* 作为真正的设计模式，还有更多工作要做。我已将 *Type Erasure* 引入为由 *External Polymorphism*、*Bridge* 和 *Prototype* 构建的复合设计模式。我已将其介绍为一种基于值的技术，用于提供一组类型与其相关操作之间的强解耦。然而，不幸的是，你可能会看到 *Type Erasure* 的其他“形式”：随着时间的推移，*Type Erasure* 一词已被误用和滥用在各种技术和概念上。例如，有时人们将 `void*` 称为 *Type Erasure*。你也很少会听到关于 *Type Erasure* 在继承层次结构上下文中的讨论，或更具体地说是指向基类的指针。最后，你也可能会在 `std::variant` 的上下文中听到关于 *Type Erasure* 的讨论。$^{11}$

尤其是 `std::variant` 示例表明，这种对 *Type Erasure* 一词的过度使用有多么深层次的缺陷。虽然 *External Polymorphism*，即 *Type Erasure* 背后的主要设计模式，是关于让你能够添加新类型的，而 *Visitor* 设计模式及其在 `std::variant` 形式的现代实现则是关于添加新操作（参见“Guideline 15: Design for the Addition of Types or Operations”）。因此从软件设计角度来看，这两种解决方案彼此完全正交：虽然 *Type Erasure* 真正从具体类型解耦并擦除类型信息，但 `std::variant` 的模板参数揭示了所有可能的替代方案，因此使你依赖于这些类型。对两者使用相同的术语会导致在使用 *Type Erasure* 一词时完全没有信息传递：“我建议我们使用类型擦除来解决这个问题。”“你能更具体一点吗？你是想添加类型还是操作？”因此该术语将不具备设计模式的品质，它将不承载任何意图。因此它将毫无用处。

为了让 *Type Erasure* 在设计模式之殿中获得其应有的地位，为了让它具有任何意义，请只将其用于本准则中讨论的意图。

## GUIDELINE 32: CONSIDER REPLACING INHERITANCE HIERARCHIES WITH *TYPE ERASURE*

- 应用 *Type Erasure* 设计模式，其意图是为一组无关联的、可能非多态的具有相同语义行为的可扩展类型提供基于值、非侵入式的抽象；
- 将 *Type Erasure* 视为由 *External Polymorphism*、*Bridge* 和 *Prototype* 设计模式构建的复合设计模式；
- 理解 *Type Erasure* 的优势，但也要牢记其局限性；
- 仅将 *Type Erasure* 一词用于传达其作为设计模式的意图，即允许轻松添加支持固定操作集的类型。

## Guideline 33: Know about the Optimization Potential of Type Erasure

本书的主要关注点是软件设计。因此所有关于如何构建软件、关于设计原则、关于管理依赖和抽象的工具，以及当然所有关于设计模式的信息都是关注的中心。尽管如此，我已多次提到性能很重要。*非常*重要！毕竟，C++ 是一种以性能为中心的编程语言。因此我现在破例：本准则专门讨论性能。是的，我是认真的：不谈依赖关系，（几乎）没有关注点分离的示例，没有 *value semantics*。只是性能。“终于有一些性能内容了，太好了！”，你欢呼。然而，请注意后果：本准则在实现细节上相当深入。而在 C++ 中，提及一个细节就需要你处理另外两个细节，很快你就会被吸入实现细节的领域。为了避免这种情况（并让我的出版商满意），我不会详细阐述每一个实现细节，也不会展示所有替代方案。我将提供额外的参考资料，帮助你深入挖掘。¹²

在“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”中，你已经看到了我们基础的、未优化的 Type Erasure 实现的出色性能数字。然而，由于我们现在拥有一个值类型，一个包装类，而不仅仅是一个指针，我们获得了大量提升性能的机会。这就是为什么我们将研究两种改进性能的选项：Small Buffer Optimization (SBO) 和手动虚分派。

### Small Buffer Optimization (SBO)

让我们开始提升 Type Erasure 实现性能的探索。当谈论性能时，首先想到的事情之一通常是优化内存分配。这是因为获取和释放动态内存可能非常 sloooww 且不确定。说真的：优化内存分配可能是在缓慢和闪电般快速之间产生巨大差异的关键。

然而，查看内存还有第二个原因。在“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”中，我可能无意中给你留下了一种印象，即我们需要动态内存来实现 Type Erasure。的确，我们第一个 Shape 类的初始实现细节之一是在构造函数和 clone() 函数中无条件地进行动态内存分配：无论给定对象的大小如何，对于小对象和大对象，我们都会始终通过 std::make_unique() 执行动态内存分配。这种选择不仅在性能上具有限制性，特别是对于小对象，而且在某些环境中动态内存不可用。因此我应该向你展示，关于内存你可以做很多事情。事实上，你完全控制内存管理！由于你使用的是值类型、包装器，你可以以任何你认为合适的方式处理内存。众多选项之一是完全依赖类内内存，并在对象过大时发出编译时错误。或者你可以根据存储对象的大小在类内和动态内存之间切换。这两种方法都由 Small Buffer Optimization (SBO) 实现。

为了给你一个 SBO 如何工作的概念，让我们看一个 Shape 实现，它永远不动态分配，只使用类内内存：

```
#include <array>
#include <cstdlib>
#include <memory>

template< size_t Capacity = 32U, size_t Alignment = alignof(void*) >
class Shape
{
public:
    // ...
private:
    // ...

    Concept* pimpl()
    {
        return reinterpret_cast<Concept*>( buffer_.data() );
    }

    Concept const* pimpl() const
    {
        return reinterpret_cast<Concept const*>( buffer_.data() );
    }

    alignas(Alignment) std::array<std::byte,Capacity> buffer_;
};
```

这个 Shape 类不再存储 std::unique_ptr，而是拥有一个正确对齐的字节数组（①）。¹³ 为了给 Shape 的用户提供调整数组容量和对齐方式的灵活性，你可以向 Shape 类提供两个非类型模板参数 Capacity 和 Alignment（②）。¹⁴ 虽然这提高了适应不同情况的灵活性，但这种方法的缺点是它将 Shape 类变成了一个类模板。作为结果，使用该抽象的所有函数很可能会变成函数模板。这可能是不可取的，例如因为你可能不得不将代码从源文件移动到头文件。然而，请注意这只是众多可能性之一。如前所述，你完全掌控。

为了方便地使用 std::byte 数组，我们添加了一对 pimpl() 函数（基于此仍然实现了 Bridge 设计模式，只是使用类内内存而命名）（③ 和 ④）。“哦不，reinterpret_cast！”，你开始问道。“这不是超级危险吗？”你是对的，通常 reinterpret_cast 应被视为潜在危险的。然而，在这种特定情况下，我们得到了 C++ 标准的支持，该标准解释了我们在这里所做的事情是完全安全的。

正如你现在所预期的那样，我们还需要基于 External Polymorphism 设计模式引入一个外部继承层次结构。这次我们在 Shape 类的私有部分实现该层次结构。不是因为这更好或更适合此 Shape 实现，而是出于唯一展示另一种替代方案的原因：

```
template< size_t Capacity = 32U, size_t Alignment =
    alignof(void*) >
class Shape
{
    public:
        // ...

    private:
        struct Concept
```

