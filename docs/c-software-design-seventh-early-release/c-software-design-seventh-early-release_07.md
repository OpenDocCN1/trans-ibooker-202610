## 第 8 章. Type Erasure 设计模式

### 致早期发布读者的说明

使用 Early Release 电子书，您可以在这些书籍正式发布之前很久就获得书籍的最早形式——作者在写作时的原始未编辑内容——从而利用这些技术。

这将是最终书籍的第 8 章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何建议，或者如果您注意到本章中缺少材料，请联系编辑 sevans@oreilly.com。

关注点分离和 *value semantics*：这是本书的两个基本要点，我到目前为止已经多次提到。在本章中，这两者被优美地结合成现代 C++ 设计模式中最有趣的一种：*Type Erasure*。由于这可以被认为是火中最热的铁之一，在本章中我将为您提供关于 *Type Erasure* 所有方面的非常彻底、深入的介绍。这当然包括所有设计相关的方面，但也包括大量关于实现细节的具体内容。

在“准则 32：考虑用 Type Erasure 替换继承层次结构”中，我将向您介绍 *Type Erasure* 并提供一个想法，说明为什么这种设计模式是依赖减少和 *value semantics* 的绝佳结合。我还将带您逐步了解一个基本的、拥有型 *Type Erasure* 实现。

在“准则 33：了解 Type Erasure 的优化潜力”中，我将破例：尽管本书主要关注依赖和设计方面，在这一准则中我将完全专注于与性能相关的实现细节。我将向您展示如何应用 *Small Buffer Optimization (SBO)* 以及如何实现手动虚函数分派来加快您的 *Type Erasure* 实现。

在“准则 34：注意拥有型 Type Erasure 包装器的设置成本”中，我们将研究拥有型 *Type Erasure* 实现的设置成本。我们会发现，与 *value semantics* 相关的是一个成本，有时我们不愿意支付。为此，我们敢于踏入 *reference semantics* 的领域并实现一种非拥有型 *Type Erasure*。

#### 准则 32：考虑用 Type Erasure 替换继承层次结构

本书中反复出现的建议有几条：

- 最小化依赖。
- 分离关注点。
- 优先使用组合而非继承。
- 优先使用非侵入式解决方案。
- 优先使用 *value semantics* 而非 *reference semantics*。

单独使用时，所有这些对代码质量都有非常积极的影响。然而，结合起来，这些准则的效果要好得多。这就是您在“准则 31：对非侵入式运行时多态使用 External Polymorphism”中关于 *External Polymorphism* 设计模式的讨论中所体验到的。提取多态行为被证明是极其强大的，并解锁了前所未有的松耦合程度。尽管如此，可能令人失望的是，展示的 *External Polymorphism* 实现并没有给您留下一种非常现代的解决方案方式。相反，与优先使用 *value semantics* 的建议背道而驰，该实现牢牢建立在 *reference semantics* 上：许多指针，许多手动分配，以及手动生命周期管理。¹ 因此您等待的缺失细节是一个基于 *value semantics* 的 *External Polymorphism* 设计模式实现。而我不会再让您等待了： resulting solution 通常被称为 *Type Erasure.*²

#### Type Erasure 的历史

在详细介绍之前，让我们快速谈谈 *Type Erasure* 的历史。“来吧”，您抱怨道，“这真的有必要吗？我都快要死了，想尽快看看这东西到底怎么工作。” 好吧，我保证会简短。但 是，我觉得这是讨论的必要细节，原因有两个。首先，证明我们作为社区，除了最有经验的 C++ 专家之外，可能太久以来忽视并忽略了这种技术。其次，给出这种技术的发明者应得的认可。

*Type Erasure* 设计模式经常被归因于这种技术的第一个也是因此最著名的展示之一。在 GoingNative 2013 大会上，Sean Parent 谈论了“Inheritance Is The Base Class of Evil”。³ 虽然标题无疑很有趣，但内容相当严肃：Sean 回顾了他从 Photoshop 开发中的经验，并谈论了基于继承的实现所带来的危险和缺点。然而，他也提出了解决继承问题的方案，后来被称为 *Type Erasure*。

尽管 Sean 的演讲是关于 *Type Erasure* 的最早记录来源之一，因此可能是最知名的来源，但该技术在此之前就已被使用。例如，*Type Erasure* 已在 Boost 库的多个地方使用，例如 Douglas Gregor 为 `boost::function` 所使用。尽管如此，据我所知，该技术首次在 Kevlin Henney 于 C++ report 2000 年 7-8 月刊的论文中讨论。⁴ 在这篇论文中，Kevlin 通过一个代码示例展示了 *Type Erasure*，该示例后来演变为我们今天所知的 C++17 的 std::any。最重要的是，他是第一个将多种设计模式优雅地结合起来，形成围绕一组不相关、非多态类型的基于值语义的实现的人。

从那时起，许多通用类型都获得了该技术以提供各种应用的值类型。其中一些类型甚至进入了标准库。例如，我们已经看到 std::function，它代表了可调用对象的基于值的抽象。⁵ 我已经提到过 std::any，它代表了几乎任何东西的抽象容器式值（因此得名），但不暴露任何功能：

```
#include <any>
#include <cstdlib>
#include <string>
using namespace std::string_literals;

int main()
{
    std::any a;                // Creating an empty 'any'
    a = 1;                     // Storing an 'int' inside the 'any';
    a = "some string"s;  // Replacing the 'int' with a
    'std::string'

    // There is nothing we can do with the 'any' except for
    // getting the value back
    std::string s = std::any_cast<std::string>( a );

    return EXIT_SUCCESS;
}
```

然后还有 std::shared_ptr，它使用 Type Erasure 来存储分配的删除器：

```
#include <cstdlib>
#include <memory>

int main()
{
    {
        // Creating a 'std::shared_ptr' with a custom deleter
        // Note that the deleter is not part of the type!
        std::shared_ptr<int> s{ new int{42}, [] (int* ptr){ delete ptr; } };
    }
    // The 'std::shared_ptr' is destroyed at the end of the scope,
    //   deleting the 'int' by means of the custom deleter.

    return EXIT_SUCCESS;
}
```

“看起来只需为删除器提供第二个模板参数，如 std::unique_ptr 所做，就更简单了。为什么 std::shared_ptr 不是以同样的方式实现的？”您询问道。好吧，std::shared_ptr 和 std::unique_ptr 的设计出于非常好的原因而不同。std::unique_ptr 的哲学是代表围绕原始指针的最简单可能的包装器：它应该与原始指针一样快，并且应该与原始指针具有相同的大小。出于这个原因，不希望将删除器与管理的指针一起存储。因此，std::unique_ptr 被设计为对于无状态删除器可以避免任何大小开销。然而，不幸的是，这个第二个模板参数很容易被忽视并导致人为的限制：

```
// This function takes only unique_ptrs that use the default deleter,
//   and thus is artificially restricted
template< typename T >
void func1( std::unique_ptr<T> ptr );
```

```
// This function does not care about the way the resource is cleaned up,
//   and thus is truly generic
template< typename T, typename D >
void func2( std::unique_ptr<T,D> ptr );
```

这种耦合在 std::shared_ptr 的设计中被避免了。由于 std::shared_ptr 必须在其所谓的控制块中存储更多的数据项（其中包括引用计数、弱计数，



etc.), 它有机会使用 *Type Erasure* 来 literally 擦除 deleter 的类型，即移除任何可能的依赖。

### The Type Erasure Design Pattern Explained

“哇，这听起来真的很有趣。这让我更加兴奋地想要了解 *Type Erasure*”。好的，那我们开始吧。然而，请不要期待任何魔法或任何革命性的新想法。*Type Erasure* 只不过是一种复合设计模式，意味着它是将另外三种设计模式进行非常巧妙而优雅的组合。所选的三种设计模式是 *External Polymorphism*（这是实现解耦效果和 *Type Erasure* 非侵入性本质的关键要素；参见“Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”），*Bridge*（它被证明是创建基于值语义的实现的关键；参见“Guideline 28: Build Bridges to Remove Physical Dependencies”），以及（可选的）*Prototype*（它用于处理 resulting values 的拷贝语义；参见“Guideline 30: Apply Prototype for Abstract Copy Operations”）。这三种设计模式构成了 *Type Erasure* 的核心，但当然，请记住存在不同的解释和实现，主要是为了适应特定上下文。结合这三种设计模式的目的是 to to 创建一个包装类型，该类型代表一种松耦合的、非侵入式的抽象：

#### THE *TYPE ERASURE* COMPOUND DESIGN PATTERN

Intent: “Provide a value-based, non-intrusive abstraction for an extendable set of unrelated, potentially non-polymorphic types with the same semantic behavior.”

该表述的目的是尽可能简短，但又尽可能精确。然而，该意图的每一个细节都具有意义。因此进行阐述可能会有所帮助：

- **Value-based**: *Type Erasure* 的意图是创建值类型，即可拷贝、可移动的，并且最重要的是易于推理的类型。然而，这种值类型与常规值类型的质量并不相同，存在一些限制。特别是，*Type Erasure* 对于一元操作效果最佳，但对于二元操作则存在局限。
- **Non-intrusive**: *Type Erasure* 的意图是基于 *External Polymorphism* 设计模式所树立的范例，创建一个外部的、非侵入式的抽象。所有提供抽象所期望行为的类型都会自动得到支持，而无需对其进行任何修改。
- **Extendable, unrelated set of types**: *Type Erasure* 坚定地基于面向对象原则，即它使你能够轻松添加类型。这些类型之间不应有任何关联。这包括它们不必通过某个基类共享通用行为。相反，应该能够在不进行任何侵入性措施的情况下，将任何合适的类型添加到这组类型中。
- **Potentially non-polymorphic**: 如 *External Polymorphism* 设计模式所示，类型不必通过继承加入该集合。它们也不必自行提供虚函数，而是应将其多态行为解耦。然而，当然，具有基类或虚函数的类型也不被排除。
- **Same semantic behavior**: 目标不是为所有可能的类型提供抽象，而是为一组提供相同操作（包括相同语法）并根据 *Liskov Substitution Principle (LSP)*（参见“Guideline 6: Adhere to the Expected Behavior of Abstractions”）遵循某些预期行为的类型提供语义抽象。如果可能，对于任何不提供预期功能的类型，应创建编译时错误。

有了对意图的这种表述，让我们来看一下 *Type Erasure* 的依赖图（参见 Figure 8-1）。该图看起来应该非常熟悉，因为该模式的结构由 *External Polymorphism* 设计模式的固有结构主导（参见 Figure 7-8）。最重要区别和新增之处是架构最高层级的 `Shape` 类。该类作为 *External Polymorphism* 引入的外部层次结构的包装器。主要因为该外部层次结构将不再直接使用，而且为了反映它们存储，即“拥有”一个具体类型的事实，`ShapeModel` 类的名称已被调整为 `OwningShapeModel`。

```
class ShapeConcept
{
    // ...
};

template< typename ShapeT
        , typename DrawStrategy >
class OwningShapeModel
    : public ShapeConcept
{
    // ...
};
```

```
class Shape
{
public:
    template< typename ShapeT, typename DrawStrategy >
    Shape( ShapeT const&, DrawStrategy );
    // ...
private:
    std::unique_ptr<ShapeConcept> pimpl_;
};

void draw( Shape const& shape );
```

High-level

Architectural Boundary

```
class Circle
{
    // ...
};
```

```
class Square
{
    // ...
};
```

Architectural Boundary

```
class OwningShapeModel<Circle,OpenGLDrawStrategy>
{
    // ...
};
```

```
class OwningShapeModel<Square,OpenGLDrawStrategy>
{
    // ...
};
```

```
class OpenGLDrawStrategy
{
public:
    void operator()( Circle const& circle ) const;
    void operator()( Square const& square ) const;
    // ...
};
```

Low-level

Figure 8-1. Dependency graph for the Type Erasure design pattern.

### An Owning Type Erasure Implementation

好的，但现在，在了解了 Type Erasure 的结构之后，让我们来看一下 Type Erasure 的实现细节。尽管你之前已经见过所有组件的实际应用，实现细节仍然不特别适合初学者，也不是胆小者所能承受的。而且这是在我挑选了所知最简单的 Type Erasure 实现的情况下。因此我会尽量保持在合理水平，并且不会过多深入实现细节的领域。在其他方面，这意味着我不会试图榨取每一丁点性能。例如，我不会使用转发引用，也不会避免动态内存分配。此外，我会偏向可读性和代码清晰度。虽然这可能会让你失望，但我相信这会为我们节省很多头痛。然而，如果你想深入研究实现细节和优化选项，我建议查看“Guideline 33: Know about the Optimization Potential of Type Erasure”。

我们再次从 Circle 和 Square 类开始：

```
//---- <Circle.h> ----------------

class Circle
{
public:
    explicit Circle( double radius )
        : radius_( radius )
    {}

    double radius() const { return radius_; }
    /* Several more getters and circle-specific utility functions */

private:
    double radius_;
    /* Several more data members */
};
```

```
//---- <Square.h> ----------------

class Square
{
public:
    explicit Square( double side )
        : side_( side )
    {}

    double side() const { return side_; }
    /* Several more getters and square-specific utility functions */

private:
    double side_;
    /* Several more data members */
};
```

这两个类自我们在讨论 External Polymorphism 时最后一次遇到它们以来没有变化。但仍然值得再次强调，这两个类完全不相关，即彼此不知道对方，最重要的是是非多态的，意味着它们不继承自任何基类或自行引入虚函数。

我们也见过 ShapeConcept 和 OwningShapeModel 类，后者曾用名为 ShapeModel：

```
//---- <Shape.h> ----------------

#include <memory>
#include <utility>

namespace detail {

class ShapeConcept ①
{
public:
    virtual ~ShapeConcept() = default;
    virtual void draw() const = 0;  ②
    virtual std::unique_ptr<ShapeConcept> clone() const = 0;  ③
};





```cpp
template< typename ShapeT
, typename DrawStrategy >
class OwningShapeModel : public ShapeConcept
{
public:
    explicit OwningShapeModel( ShapeT shape, DrawStrategy drawer )
        : shape_{ std::move(shape) }
        , drawer_{ std::move(drawer) }
    {}

    void draw() const override { drawer_(shape_); }

    std::unique_ptr<ShapeConcept> clone() const override
    {
        return std::make_unique<OwningShapeModel>( *this );
    }

private:
    ShapeT shape_;
    DrawStrategy drawer_;
};

} // namespace detail
```

除了名称更改之外，还有一些其他重要的不同。例如，这两个类都已移至 detail 命名空间。命名空间的名称表明这两个类现在正成为实现细节，即它们不再 intended 用于直接使用。ShapeConcept 类仍然引入纯虚函数 draw() 来表示绘制形状的要求。此外，ShapeConcept 现在还引入了纯虚 clone() 函数。“我知道这是什么，这是原型设计模式！”，你解释道。是的，正确。名称 clone() 与原型紧密相关，是该设计模式的强烈指示（但不是保证）。然而，尽管函数名称的选择非常合理且规范，请允许我明确指出 clone() 的函数名称选择，以及 draw() 的函数名称选择，都是我们自己的：这些名称现在是实现细节，与我们从 ShapeT 类型要求的名字没有任何关系。我们同样可以把它们命名为

do_draw() 和 do_clone()，这对 ShapeT 类型没有任何影响。对 ShapeT 类型的真正要求由 draw() 和 clone() 函数的实现定义。

由于 ShapeConcept 再次成为外部层次结构的基类，draw() 函数、clone() 函数以及析构函数代表了所有形状类型的需求集合。这意味着所有形状都必须提供某种绘制行为，它们必须是可拷贝的，并且可析构的。请注意，这三个函数只是此示例的需求选择。特别是，可拷贝性并不是类型擦除所有实现都通用的要求。

OwningShapeModel 类再次代表 ShapeConcept 类的唯一实现。如前所述，OwningShapeModel 在其构造函数中接受具体的形状类型和绘制策略，并在其两个数据成员中进行初始化。由于 OwningShapeModel 继承自 ShapeConcept，它必须实现两个纯虚函数。draw() 函数通过应用给定的绘制策略来实现，而 clone() 函数则实现为返回对应 OwningShapeModel 的精确拷贝。

NOTE
如果你现在正在想：“哦不，std::make_unique()。那意味着动态内存。然后我不能在我的代码中使用它！”，不要担心。
std::make_unique() 仅仅是一个实现细节，是为了保持示例简单而做的选择。在“Guideline 33: Know about the Optimization Potential of Type Erasure”中，你将看到如何通过小缓冲区优化（SBO）来避免动态内存。

“我到目前为止完全不为之印象深刻。我们几乎没有超出外部多态设计模式的实现”。我完全理解这种批评。然而，我们距离将外部多态转变为类型擦除仅一步之遥，距离

从 *引用语义* 切换到 *值语义* 仅一步之遥。我们所需要的是一个值类型，一个围绕由 `ShapeConcept` 和 `OwningShapeModel` 引入的外部层次结构的包装器，它处理我们不希望手动执行的所有细节：`OwningShapeModel` 类模板的实例化，管理指针，执行分配，以及处理生命周期。这个包装器以 `Shape` 类的形式给出：

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
    {
        using Model =
        detail::OwningShapeModel<ShapeT,DrawStrategy>;
        pimpl_ = std::make_unique<Model>( std::move(shape)
                        , std::move(drawer) );
    }
    // ...
private:
    // ...
    std::unique_ptr<detail::ShapeConcept> pimpl_;
};
```

关于 `Shape` 类的第一个，也是也许最重要的细节是模板化构造函数。作为第一个参数，此构造函数接受任何种类的形状（称为 `ShapeT`），作为第二个参数接受期望的 `DrawStrategy`。为了简化相应的 `detail::OwningShapeModel` 类模板的实例化，使用一个方便的类型别名被证明是有帮助的。这个别名用于通过 std::make_unique() 实例化

所需的模型。形状以及绘制策略都被传递给新的模型。

新创建的模型用于初始化 Shape 类的唯一数据成员：pimpl_。“我也认识这个，这是桥接模式！”，你高兴地宣布。是的，再次正确，这是一个桥接设计模式的应用。在构造函数中，我们基于实际给定的类型 ShapeT 和 DrawStrategy 创建一个具体的 OwningShapeModel，但我们以指向 ShapeConcept 的指针形式存储它。通过这样做，你创建了一个通向实现细节的桥梁，一个通向真实形状类型的桥梁。然而，在 pimpl_ 初始化之后，在构造函数完成之后，Shape 不再记住实际类型。Shape 没有模板参数或任何会泄露其存储的具体类型的成员函数，也没有记住给定类型的数据成员。它所持有的只是指向 ShapeConcept 基类的指针。因此，它对真实形状类型的记忆被擦除了。因此该设计模式的名称：类型擦除。

我们 Shape 类中唯一缺少的是真正值类型所需的功能：拷贝和移动操作。幸运的是，由于使用了 std::unique_ptr，我们的努力非常有限。由于编译器生成的析构函数和两个移动操作都可用，我们只需要处理两个拷贝操作：

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    // ...
    Shape( Shape const& other )
        : pimpl_( other.pimpl_->clone() )
    {}
    Shape& operator=( Shape const& other )
```

```
{
    // Copy-and-Swap Idiom
    Shape copy( other );
    pimpl_.swap( copy.pimpl_ );
    return *this;
}

~Shape() = default;
Shape( Shape&& ) = default;
Shape& operator=( Shape&& ) = default;

private:
    friend void draw( Shape const& shape )
    {
        shape.pimpl_->draw();
    }

    // ...
};
```

拷贝构造函数可能是一个非常困难的函数来实现，因为我们不知道存储在另一个 Shape 中的形状的具体类型。然而，由于在 ShapeConcept 基类中提供了 clone() 函数，我们可以请求一个精确的拷贝，而无需知道任何关于具体类型的信息。实现拷贝赋值运算符最短、最无痛、最方便的方式是基于拷贝-交换惯用法构建。

此外，Shape 类提供了一个所谓的隐藏友元 draw()。这个友元函数被称为隐藏友元，因为虽然它是一个自由函数，但它是在 Shape 类的主体内部定义的。作为友元，它被授予对私有数据成员的完全访问权限，并且将被注入到周围的命名空间中。

“你不是说友元是坏的吗？”，你抱怨道。我承认，那是我在“Guideline 4: Design for Testability”中所说的。然而，我也明确指出，隐藏友元是可以的。在这种情况下，draw() 函数是 Shape 类不可分割的一部分，绝对是一个真正的友元（几乎是家庭成员）。“但那它应该是一个成员函数，对吧？”，你争辩道。的确，这将是一个有效的替代方案。如果



你喜欢这样更好，那就去做吧。在这种情况下，我更倾向于使用一个自由函数，因为我们的目标之一是通过提取 draw() 操作来减少依赖。这种目标也应该在 Shape 实现中得到体现。然而，由于该函数需要访问 pimpl_ 数据成员，并且为了不增加 draw() 函数的重载集合，我将其实现为一个隐藏友元。

就是这样。全部如此。让我们看看新的功能有多么美妙：

```
//---- <Main.cpp> ----------------

#include <Circle.h>
#include <Square.h>
#include <Shape.h>
#include <cstdlib>

int main()
{
    // Create a circle as one representative of a concrete shape type
    Circle circle{ 3.14 };

    // Create a drawing strategy in form of a lambda
    auto drawer = []( Circle const& c ){ /*...*/ };

    // Combine the shape and the drawing strategy in a 'Shape' abstraction
    // This constructor call will instantiate a
    //'detail::OwningShapeModel' for
    // the given 'Circle' and lambda types
    Shape shape1( circle, drawer );

    // Draw the shape
    draw( shape1 );

    // Create a copy of the shape by means of the copy constructor
    Shape shape2( shape1 );

    // Drawing the copy will result in the same output
    draw( shape2 );
```

```
return EXIT_SUCCESS;
}
```

我们首先将 shape1 创建为 Circle 和相关联绘制策略的抽象。这感觉很简单，对吧？无需手动分配，也无需处理指针。通过 draw() 函数，我们能够绘制这个 Shape ⑦。紧接着我们创建了形状的副本。一个真正的副本，即“深拷贝”，而不仅仅是指针的拷贝。通过 draw() 函数绘制副本会得到相同的输出 ⑧。再次，这感觉很好：你可以依赖值类型的拷贝操作（在本例中为拷贝构造函数），而无需手动 clone()。

非常惊人，对吧？而且肯定比手动使用外部多态要好得多。我承认，在所有这些实现细节之后，可能一开始很难立刻看出它，但如果你穿过实现细节的丛林，我希望你能认识到这种方法的美妙之处：你不再需要处理指针，没有手动分配，也不再需要处理继承层次结构。所有这些细节都在，但所有证据都被很好地封装在 Shape 类内部。尽管如此，你并没有失去任何解耦的好处：你仍然可以轻松添加新类型，并且具体的形状类型仍然对绘制行为一无所知。它们仅通过 Shape 构造函数与所需功能相连接。

“我在想”，你开始问道。“我们不能让它更简单吗？我设想 main() 函数看起来像这样”：

```
//---- <YourMain.cpp> ----------------

int main()
{
    // Create a circle as one representative of a concrete shape type
    Circle circle{ 3.14 };

    // Bind the circle to some drawing functionality
    auto drawingCircle = [=]() { myCircleDrawer(circle); };
```

```
// Type-erase the circle equipped with drawing behavior
Shape shape( drawingCircle );

// Drawing the shape
draw( shape );

// ...

return EXIT_SUCCESS;
}
```

这是一个很棒的想法。记住，你负责 *Type Erasure* 包装器的所有实现细节，以及如何将类型与其操作的实现结合起来。如果你更喜欢这种形式，那就去做吧！然而，请不要忘记在我们的 `Shape` 示例中，为了简单和代码简洁，我故意只使用了一个具有外部依赖的功能（即绘制）。可能还有更多引入依赖的函数，例如形状的序列化。在这种情况下，lambda 方法就无法工作，因为你需要多个命名函数（例如 `draw()` 和 `serialize()`）。所以，最终取决于情况。它取决于你的 *Type Erasure* 包装器所代表的抽象类型。但无论你偏好哪种实现，请确保不要在不同功能和/或代码之间引入人为的依赖和/或代码重复。换句话说，记住“准则 2：为变化而设计”！这就是我偏爱基于 *Strategy* 设计模式的解决方案的原因，但你不应该将其视为唯一真正的解决方案。相反，你应该努力充分利用 *Type Erasure* 的松耦合潜力。

### 分析 Type Erasure 设计模式的不足

尽管 *Type Erasure* 很美，尽管你从设计角度获得了大量好处，但我并不假装这种设计模式没有缺点。不，这对你不公平，隐瞒潜在的缺点。

第一个，也是可能最明显的问题，对你来说可能是实现复杂性。如前所述，我明确将实现细节的水平保持在合理范围内，希望能帮助你理解思路。我也希望给你留下这样的印象：其实并不那么困难：一个基本的 Type Erasure 实现大约可以在 30 行代码内完成。尽管如此，你可能仍会觉得它太复杂。而且，一旦你开始超越基本实现并开始考虑性能、异常安全等，实现细节会变得非常棘手。在这些情况下，你最安全、最方便的选择是使用第三方库，而不是自己处理所有这些细节。可能的库包括 Louis Dionne 的 dyno 库、Eduardo Madrid 的 zoo 库、Gašper Ažman 的 erasure 库，以及 Steven Watanabe 的 Boost Type Erasure。

在解释 Type Erasure 的意图时，我已经提到了第二个缺点，它更重要也更具限制性：虽然我们现在处理的是可以拷贝和移动的值，但使用 Type Erasure 进行二进制操作并不直接。例如，不容易对这些值进行相等比较，就像你对常规值所期望的那样：

```
int main()
{
    // ...

    if( shape1 == shape2 ) { /*...*/ }  // Does not compile!

    return EXIT_SUCCESS;
}
```

原因在于，归根结底 Shape 只是对具体形状类型的抽象，并且只存储一个指向基类的指针。正如你直接使用外部多态时也会遇到完全相同的问题，这绝对不是 Type Erasure 中的新问题，你甚至可能不将其算作真正的缺点。尽管如此，虽然相等可比性在处理指向基类的指针时不是预期操作，但在值上通常是预期操作。

### 比较两个 Type Erasure 包装器

“这不只是暴露 Shape 接口中必要的功能的问题吗？”你在想。“例如，我们只需向 shapes 的公共接口添加一个 area() 函数并用它来比较两个项目”：

```
bool operator==( Shape const& lhs, Shape const& rhs )
{
    return lhs.area() == rhs.area();
}
```

“这很容易做到。那我错过了什么？”我同意这可能是你所需的一切：如果两个对象在某些公共属性相等时相等，那么这个运算符对你有效。一般来说，答案是“取决于情况”。在这种特定情况下，它取决于 Shape 类所代表的抽象的语义。问题是：什么时候两个 Shapes 相等？考虑以下使用 Circle 和 Square 的示例：

```
#include <Circle.h>
#include <Square.h>
#include <cstdlib>

int main()
{
    Shape shape1( Circle{3.14} );
    Shape shape2( Square{2.71} );

    if( shape1 == shape2 ) { /*...*/ }

    return EXIT_SUCCESS;
}
```

这两个 Shapes 什么时候相等？如果它们的面积相等时相等，还是当抽象背后的实例相等时相等，也就是说



那两种 Shapes 是否属于同一类型并且具有相同的属性？这取决于情况。秉持同样的精神，我也可以问：什么时候两个 Persons 是相等的？如果它们的名相同，它们就相等吗？还是只有当它们的所有特征都相等时才相等？这取决于期望的语义。而且虽然第一个比较很容易完成，但第二个比较却不容易实现。在一般情况下，我假设第二种情况更可能是期望的语义，因此我认为使用类型擦除来进行相等比较，以及更一般地进行二元操作，并不是直接的。

请注意，我并没有说相等比较是不可能的。技术上你可以让它工作，尽管这最终会变成一个相当丑陋的解决方案。因此你必须答应我不要告诉任何人你是从我这里得到这个想法的。“你只是让我更加好奇了”，你狡黠地笑着。好吧，这里就是：

//---- <Shape.h> ----------------
// ...

namespace detail {

class ShapeConcept
{
public:
    // ...
    virtual bool isEqual( ShapeConcept const* c ) const = 0;
};

template< typename ShapeT
        , typename DrawStrategy >
class OwningShapeModel : public ShapeConcept
{
public:
    // ...

    bool isEqual( ShapeConcept const* c ) const override
    {
        using Model = OwningShapeModel<ShapeT,DrawStrategy>;
        auto const* model = dynamic_cast<Model const*>( c );
        return ( model && shape_ == model->shape_ );
    }

}
private:
    // ...
};
} // namespace detail

class Shape
{
    // ...

private:
    friend bool operator==( Shape const& lhs, Shape const& rhs )
    {
        return lhs.pimpl_->isEqual( rhs.pimpl_.get() );
    }

    friend bool operator!=( Shape const& lhs, Shape const& rhs )
    {
        return !( lhs == rhs );
    }

    // ...
};

//---- <Circle.h> ----------------

class Circle
{
    // ...
};

bool operator==( Circle const& lhs, Circle const& rhs )
{
    return lhs.radius() == rhs.radius();
}

//---- <Square.h> ----------------

class Square
{
    // ...
};
```

```
bool operator==( Square const& lhs, Square const& rhs )
{
    return lhs.side() == rhs.side();
}
```

为了使相等比较工作，你可以使用 dynamic_cast (10)。然而，这个相等比较的实现有两个严重的缺点：首先，正如你在“Guideline 18: Beware the Performance of Acyclic Visitors”中看到的那样，dynamic_cast 绝对不算是一个快速操作。因此你必须为每次比较付出相当大的运行时成本。其次，在这个实现中，你只能在两个 Shapes 都配备了相同的 DrawStrategy 时才成功比较它们。虽然这在某些上下文中可能是合理的，但在另一个上下文中也可能被认为是一个不幸的限制。我所知道的唯一解决方案是回到 std::function 来存储绘制策略，这然而会带来另一个性能损失。7 总之，取决于上下文，相等比较可能是可能的，但通常既不容易也不便宜实现。这证明了我之前的说法，即类型擦除不支持二元操作。

### 类型擦除包装器的接口隔离

“那接口隔离原则（ISP）呢？”你在问。“在使用外部多态时，很容易在基类中分离关注点。看起来我们已经失去了这种能力，对吗？”  excellent question。所以你还记得我在“Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”中关于 JSONExportable 和 Serializable 基类的例子。的确，使用类型擦除后我们不再能够使用隐藏的基类，只能使用抽象的值类型。因此 ISP 看起来似乎遥不可及：

```
class Document  // Type-erased 'Document'
{
public:
```
// ...
void exportToJSON( /*...*/ ) const;
void serialize( ByteStream& bs, /*...*/ ) const;
// ...
};
```

```
// Artificial coupling to 'ByteStream', although only the JSON
export is needed
void exportDocument( Document const& doc )
{
    // ...
    doc.exportToJSON( /* pass necessary arguments */ );
    // ...
}
```

然而，幸运的是，这种印象是不正确的。你可以通过提供几种类型擦除抽象来轻松遵守 ISP：⁸

```
Document doc = /*...*/;  // Type-erased 'Document'
doc.exportToJSON( /* pass necessary arguments */ );
doc.serialize( /* pass necessary arguments */ );

JSONExportable jdoc = doc;  // Type-erased 'JSONExportable'
jdoc.exportToJSON( /* pass necessary arguments */ );

Serializable sdoc = doc;  // Type-erased 'Serializable'
sdoc.serialize( /* pass necessary arguments */ );
```

然而，在考虑这一点之前，请先看看“Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers”。

“除了实现复杂性和限制为一元操作之外似乎没有缺点。好吧，那我不得不说这确实是令人惊叹的东西！好处明显超过了缺点。” 好吧，当然一切都取决于具体情况，这意味着在特定上下文中其中一些问题可能会造成一些痛苦。但我同意总体而言类型擦除证明是一个非常有价值的设计模式。从设计角度来看，你获得了强大的解耦程度，这肯定会在更改或扩展软件时带来更少的痛苦。然而，虽然这已经很迷人了，但还有更多。我已经多次提到性能，但还没有展示任何性能数字。所以让我们来看看性能结果。

### 性能基准

在展示 *Type Erasure* 的性能结果之前，让我提醒你关于我们之前用来基准测试 *Visitor* 和 *Strategy* 解决方案的基准场景（参见“Guideline 16: Use Visitors to Extend Operations”中的表 4-2 和“Guideline 23: Prefer a Value-Based Implementation of Strategy and Command”中的表 5-1）。这次我已将基准扩展为基于 `OwningShapeModel` 实现的 *Type Erasure* 解决方案。对于基准，我们仍然使用四种不同类型的形状（圆、方形、椭圆和矩形）。并且再次，我在 10,000 个随机创建的形状上运行 25,000 次平移操作。我使用 GCC 11.1 和 Clang 11.1，并且对两个编译器我只添加了 `-O3` 和 `-DNDEBUG` 编译标志。我使用的平台是 macOS Big Sur（版本 11.4）上的 8 核 Intel Core i7，3.8 Ghz，64 GB 主内存。

表 8-1 显示了性能数字。为方便起见，我重现了 *Strategy* 基准的性能结果。毕竟，*Strategy* 设计模式是旨在同一设计空间的解决方案。最有趣的行是最后一行。它显示了 *Type Erasure* 设计模式的性能结果。

Table 8-1. Performance results for the Type Erasure implementations

|  | GCC 11.1 | Clang 11.1 |  |
|---|---|---|---|
| Object-oriented solution | 1.5205s | 1.1480s |

| std::function | 2.1782s | 1.4884s |
| Manual implementation of std::function | 1.6354s | 1.4465s |
| Classic Strategy | 1.6372s | 1.4046s |
| Type Erasure | 1.5298s | 1.1561s |

“看起来很有趣。类型擦除似乎相当快。显然只有面向对象的解决方案更快。” 是的。在 Clang 的情况下，面向对象解决方案的性能稍好一些。但只是稍好一些。然而请记住，面向对象解决方案是那种不解耦任何东西的解决方案：draw() 函数作为 Shape 层次结构中的虚成员函数实现，因此你会经历对绘制功能的紧密耦合。虽然这可能带来很小的性能开销，但从设计角度来看这是最坏的情况。考虑到这一点，类型擦除的性能数字真是令人惊叹：它比任何 Strategy 实现性能高出 6% 到 20%。因此类型擦除不仅提供了最强的解耦，而且性能也优于所有其他减少耦合的尝试。⁹

### 关于术语的一点说明

总之，类型擦除是一种实现高效且松耦合代码的惊人方法。虽然它可能有一些限制和缺点，但有一件事你可能很难忽视，那就是复杂的实现细节。正因如此，许多人，包括我本人以及 Eric Niebler，认为类型擦除应该成为语言特性：¹⁰



> *如果我能回到过去并且有能力改变 C++，与其添加虚函数，我会添加对类型擦除和概念的语言支持。定义一个单类型概念，并为其自动生成一个类型擦除包装器。*
> 
> —Eric Niebler

然而，要确立 *Type Erasure* 作为真正的设计模式，还有更多工作要做。我已将 *Type Erasure* 引入为由 *External Polymorphism*、*Bridge* 和 *Prototype* 构建的复合设计模式。我已将其介绍为一种基于值的技术，用于提供一组类型与其相关操作之间的强解耦。然而，不幸的是，你可能会看到 *Type Erasure* 的其他“形式”：随着时间的推移，*Type Erasure* 一词已被误用和滥用在各种技术和概念上。例如，有时人们将 `void*` 称为 *Type Erasure*。你也很少会听到关于 *Type Erasure* 在继承层次结构上下文中的讨论，或更具体地说是指向基类的指针。最后，你也可能会在 `std::variant` 的上下文中听到关于 *Type Erasure* 的讨论。$^{11}$

尤其是 `std::variant` 示例表明，这种对 *Type Erasure* 一词的过度使用有多么深层次的缺陷。虽然 *External Polymorphism*，即 *Type Erasure* 背后的主要设计模式，是关于让你能够添加新类型的，而 *Visitor* 设计模式及其在 `std::variant` 形式的现代实现则是关于添加新操作（参见“Guideline 15: Design for the Addition of Types or Operations”）。因此从软件设计角度来看，这两种解决方案彼此完全正交：虽然 *Type Erasure* 真正从具体类型解耦并擦除类型信息，但 `std::variant` 的模板参数揭示了所有可能的替代方案，因此使你依赖于这些类型。对两者使用相同的术语会导致在使用 *Type Erasure* 一词时完全没有信息传递：“我建议我们使用类型擦除来解决这个问题。”“你能更具体一点吗？你是想添加类型还是操作？”因此该术语将不具备设计模式的品质，它将不承载任何意图。因此它将毫无用处。

为了让 *Type Erasure* 在设计模式之殿中获得其应有的地位，为了让它具有任何意义，请只将其用于本准则中讨论的意图。

### GUIDELINE 32: CONSIDER REPLACING INHERITANCE HIERARCHIES WITH *TYPE ERASURE*

- 应用 *Type Erasure* 设计模式，其意图是为一组无关联的、可能非多态的具有相同语义行为的可扩展类型提供基于值、非侵入式的抽象；
- 将 *Type Erasure* 视为由 *External Polymorphism*、*Bridge* 和 *Prototype* 设计模式构建的复合设计模式；
- 理解 *Type Erasure* 的优势，但也要牢记其局限性；
- 仅将 *Type Erasure* 一词用于传达其作为设计模式的意图，即允许轻松添加支持固定操作集的类型。

### Guideline 33: Know about the Optimization Potential of Type Erasure

本书的主要关注点是软件设计。因此所有关于如何构建软件、关于设计原则、关于管理依赖和抽象的工具，以及当然所有关于设计模式的信息都是关注的中心。尽管如此，我已多次提到性能很重要。*非常*重要！毕竟，C++ 是一种以性能为中心的编程语言。因此我现在破例：本准则专门讨论性能。是的，我是认真的：不谈依赖关系，（几乎）没有关注点分离的示例，没有 *value semantics*。只是性能。“终于有一些性能内容了，太好了！”，你欢呼。然而，请注意后果：本准则在实现细节上相当深入。而在 C++ 中，提及一个细节就需要你处理另外两个细节，很快你就会被吸入实现细节的领域。为了避免这种情况（并让我的出版商满意），我不会详细阐述每一个实现细节，也不会展示所有替代方案。我将提供额外的参考资料，帮助你深入挖掘。¹²

在“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”中，你已经看到了我们基础的、未优化的 Type Erasure 实现的出色性能数字。然而，由于我们现在拥有一个值类型，一个包装类，而不仅仅是一个指针，我们获得了大量提升性能的机会。这就是为什么我们将研究两种改进性能的选项：Small Buffer Optimization (SBO) 和手动虚分派。

#### Small Buffer Optimization (SBO)

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





```{    virtual ~Concept() = default;    virtual void draw() const = 0;    virtual void clone( Concept* memory ) const = 0; ⑤    virtual void move( Concept* memory ) = 0; ⑥};template< typename ShapeT, typename DrawStrategy >struct OwningModel : public Concept{    OwningModel( ShapeT shape, DrawStrategy drawer )        : shape_( std::move(shape) )        , drawer_( std::move(drawer) )    {}    void draw() const override    {        drawer_( shape_ );    }    void clone( Concept* memory ) const override ⑤    {        std::construct_at( static_cast<OwningModel*>(memory), *this );        // or:        // auto* ptr = const_cast<void*>(static_cast<void const volatile*>(memory));        // ::new (ptr) OwningModel( *this );    }    void move( Concept* memory ) override ⑥    {        std::construct_at( static_cast<OwningModel*>(memory), std::move(*this) );        // or:        // auto* ptr = const_cast<void*>(static_cast<void const volatile*>(memory));        // ::new (ptr) OwningModel( std::move(*this) );    }    ShapeT shape_;    DrawStrategy drawer_;}; // ...```

alignas(Alignment) std::array<std::byte,Capacity> buffer_;
};

在此上下文中第一个有趣的细节是 clone() 函数（6）：由于 clone() 负责创建副本，因此需要将其适配到类内内存中。因此，它不是通过 std::make_unique() 创建新的 Model，而是通过 std::construct_at() 在原地创建新的 Model。或者，你也可以使用 placement new 在给定的内存位置创建副本。15

“哇，等一下！这段代码实在太难消化了。为什么有这么多强制转换？它们真的有必要吗？”我承认，这些行确实有点具有挑战性。因此我应该详细解释它们。创建实例的传统方法是通过 placement new。然而，使用 new 总会带来有人（无意或恶意）为类特定的 new 运算符提供替换的危险。为了避免任何问题并可靠地在原地构造对象，给定的地址首先通过 static_cast 转换为 void const volatile*，然后通过 const_cast 转换为 void*。得到的地址被传递给全局 placement new 运算符。确实，这不是最直观的代码。因此建议优先使用 C++20 算法 std::construct_at()：它提供了完全相同的功能，但语法好得多。

然而，我们还需要一个函数：clone() 只关心拷贝操作。因此它不适用于移动操作。出于这个原因，我们将 Concept 扩展为包含纯虚 move() 函数，并在 OwningModel 类模板中相应地实现它（6）。

“这真的有必要吗？我们使用的是类内内存，它不能移动到另一个 Shape 实例中。这样 move() 有什么意义？”好吧，你说得对，我们不能将内存本身从一个对象移动到另一个对象，但我们仍然可以移动内部存储的 shape。因此 move() 函数是将 OwningModel 从一个缓冲区移动到另一个缓冲区，而不是复制它。

clone() 和 move() 函数在 Shape 的拷贝构造函数（7）、拷贝赋值运算符（8）、移动构造函数（9）以及移动赋值运算符（10）中使用：

```
template< size_t Capacity = 32U, size_t Alignment = alignof(void*) >
class Shape
{
public:
    // ...

    Shape( Shape const& other )
    {
        other.pimpl()->clone( pimpl() );  7
    }

    Shape& operator=( Shape const& other )
    {
        // Copy-and-Swap Idiom
        Shape copy( other );  8
        buffer_.swap( copy.buffer_ );
        return *this;
    }

    Shape( Shape&& other ) noexcept
    {
        other.pimpl()->move( pimpl() );  9
    }

    Shape& operator=( Shape&& other ) noexcept
    {
        // Copy-and-Swap Idiom
        Shape copy( std::move(other) );  10
        buffer_.swap( copy.buffer_ );
        return *this;
    }

    ~Shape()  11
    {
        std::destroy_at( pimpl() );
        // or: pimpl()->~Concept();
    }

private:
    // ...
```

alignas(Alignment) std::array<std::byte,Capacity> buffer_;
};

值得一提的是 Shape 的析构函数（¶）。由于我们通过 std::construct_at() 或 placement new 在字节缓冲区内手动创建 OwningModel，因此我们也有责任显式调用析构函数。最简单、最优雅的方法是使用 C++17 算法 std::destroy_at()。或者，你也可以显式调用 Concept 析构函数。

Shape 的最后一个但至关重要的细节是模板化构造函数：

```
template< size_t Capacity = 32U, size_t Alignment =
alignof(void*) >
class Shape
{
public:
    template< typename ShapeT, typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
    {
        using Model = OwningModel<ShapeT,DrawStrategy>;

        static_assert( sizeof(Model) <= Capacity, "Given type is too large" );
        static_assert( alignof(Model) <= Alignment, "Given type is misaligned" );

        std::construct_at( static_cast<Model*>(pimpl())
            , std::move(shape), std::move(drawer) );
        // or:
        // auto* ptr = const_cast<void*>(static_cast<void const volatile*>(pimpl()));
        // ::new (ptr) Model( std::move(shape), std::move(drawer) );
    }

    // ...
private:
    // ...
};
```

在经过一对编译期检查，所需 `OwningModel` 是否能放入类内缓冲区并符合对齐限制之后，`OwningModel` 会通过 `std::construct_at()` 实例化到类内缓冲区中。

有了这个实现，我们现在调整并重新运行“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”中的性能基准测试。我们运行完全相同的基准测试，只是这次 Shape 内部不再分配动态内存，也不会因许多微小的分配而导致内存碎片。如预期，性能结果令人印象深刻（见表 8-2）：

### Table 8-2. Performance results for the Type Erasure implementations with Small Buffer Optimization (SBO)

|  | GCC 11.1 | Clang 11.1 |
|---|---|---|
| Object-oriented solution | 1.5205s | 1.1480s |
| `std::function` | 2.1782s | 1.4884s |

| Manual implementation of std::function | 1.6354s | 1.4465s |
| Classic Strategy | 1.6372s | 1.4046s |
| Type Erasure | 1.5298s | 1.1561s |
| Type Erasure (SBO) | 1.3591s | 1.0348s |

“哇，这很快。这……让我算一下…… amazing，大约比最快的 Strategy 实现快 20%，甚至比面向对象的解决方案还要快。”确实如此。非常令人印象深刻，对吧？不过，你应该记住，这些是我在我的系统上得到的数据。你的数据几乎肯定会不同。但即使你的数据可能不一样，总体结论是，通过处理内存分配，有很大的性能优化潜力。

然而，虽然性能现在非常出色，但我们失去了很多灵活性：只有小于或等于指定 Capacity 的 OwningModel 实例才能存储在 Shape 内部。更大的模型将被排除在外。这让我回到了我们可以根据给定 shape 的大小在类内和动态内存之间切换的想法：小的 shape 存储在类内缓冲区中，而大的 shape 动态分配。你现在可以继续更新 Shape 的实现以使用这两种内存。然而，在这一点上，重新强调我们最重要的设计原则之一可能是个好主意：关注点分离。与其将所有逻辑和功能都挤进 Shape 类，不如将实现细节分离，并通过基于策略的设计来实现 Shape（参见“Guideline 19: Use Strategy to Isolate How Things are Done”）：

```
template< typename StoragePolicy >
class Shape;
```





Shape 类模板被重写为接受一个 StoragePolicy。通过该策略，你可以从外部指定类应如何获取内存。当然，你将完全遵守 SRP 和 OCP。一种这样的存储策略可以是 DynamicStorage 策略类：

```
#include <utility>

struct DynamicStorage
{
    template< typename T, typename... Args >
    T* create( Args&&... args ) const
    {
        return new T( std::forward<Args>( args )... );
    }

    template< typename T >
    void destroy( T* ptr ) const noexcept
    {
        delete ptr;
    }
};
```

顾名思义，DynamicPolicy 会通过 new 等方式动态获取内存。或者，如果你有更严格的要求，你可以基于 std::aligned_alloc() 或类似功能构建，以提供具有指定对齐方式的动态内存。与 DynamicStorage 类似，你可以提供一个 InClassStorage 策略：

```
#include <array>
#include <cstddef>
#include <memory>
#include <utility>

template< size_t Capacity, size_t Alignment >
struct InClassStorage
{
    template< typename T, typename... Args >
    T* create( Args&&... args ) const
    {
        static_assert( sizeof(T) <= Capacity, "The given type is too large" );
        static_assert( alignof(T) <= Alignment, "The given type is misaligned" );

        T* memory = const_cast<T*>(reinterpret_cast<T const*> (buffer_.data()));
        return std::construct_at( memory, std::forward<Args>( args )... );

        // or:
        // void* const memory = static_cast<void*>(buffer_.data());
        // return ::new (memory) T( std::forward<Args>( args )...
);
    }

    template< typename T >
    void destroy( T* ptr ) const noexcept
    {
        std::destroy_at(ptr);
        // or: ptr->~T();
    }

    alignas(Alignment) std::array<std::byte,Capacity> buffer_;
};
```

所有这些策略类都提供相同的接口：一个 create() 函数用于实例化类型为 T 的对象，以及一个 destroy() 函数用于执行必要的清理工作。该接口由 Shape 类使用以触发构造和析构，例如在其模板化构造函数 (⑧)¹⁶ 中以及在析构函数 (⑨) 中。

```
template< typename StoragePolicy >
class Shape
{
public:
    template< typename ShapeT >
    Shape( ShapeT shape )
    {
        using Model = OwningModel<ShapeT>;
        pimpl_ = policy_.template create<Model>( std::move(shape) )
⑧
    }

    ~Shape() { policy_.destroy( pimpl_ ); } ⑨
```

```
// ... All other member functions, in particular the
//     special members functions, are not shown

private:
    // ...
    [[no_unique_address]] StoragePolicy policy_{};  ⓐ
    Concept* pimpl_{};
};
```

最后一个不容忽视的细节是数据成员 (ⓐ)：Shape 类现在存储给定 StoragePolicy 的一个实例，并且，不要惊慌，一个指向其 Concept 的原始指针。事实上，已经不需要再存储 std::unique_ptr 了，因为我们会在自己的析构函数中手动销毁对象。你可能还会注意到存储策略上的 [[no_unique_address]] 属性。这是 C++20 的一个特性，它使你有机会节省存储策略的内存。如果策略为空，编译器现在可以不为该数据成员保留任何内存。没有这个属性，就需要为 policy_ 至少保留一个字节，由于对齐限制，这很可能更多。

总之，SBO 是一种有效且最有趣的类型擦除实现的优化。因此，许多标准类型，如 std::function 和 std::any，都使用某种形式的 SBO。不幸的是，C++ 标准库规范并不要求使用 SBO。这就是为什么你只能希望使用 SBO，而不能依赖它。然而，由于性能如此重要，并且由于 SBO 发挥着决定性的作用，已经有一些提案建议也将 inplace_function 和 inplace_any 等类型标准化。时间会证明这些是否能进入标准库。

### 手动实现函数分发

“哇，这很有用。我还能做些什么来提高我的类型擦除实现的性能？”你在问。是的，

你可以做更多。还有第二个潜在的性能优化。这次我们尝试提高虚函数的性能。是的，我说的是虚函数，即由外部继承层次结构引入的，即通过 *外部多态* 设计模式。

“我们怎么能优化虚函数的性能呢？这不是完全取决于编译器吗？”绝对正确，你是对的。然而，我并不是在谈论摆弄后端、编译器特定的实现细节，而是在谈论用更高效的东西替换虚函数。而这确实是可能的。记住，虚函数只不过是存储在虚函数表中的一个函数指针。每个至少有一个虚函数的类型都有这样一个虚函数表。然而，每个类型只有一个虚函数表。换句话说，这个表并不存储在每个实例中。所以为了将虚函数表与该类型的每个实例连接起来，类会存储一个额外的、隐藏的数据成员，我们通常称之为 `vptr`，它是一个指向虚函数表的原始指针。

当你调用一个虚函数时，你首先通过 `vptr` 获取虚函数表。一旦在那里，你就可以从虚函数表中取出相应的函数指针并调用它。因此，总的来说，一个虚函数调用包含两次间接寻址：第一次是 `vptr`，第二次是指向实际函数的指针。因此，大致来说，虚函数调用的成本是普通非内联函数调用的两倍。

这两次间接寻址为我们提供了优化的机会：我们实际上可以将间接寻址次数减少到一次。为了实现这一点，我们将采用一种相当常见的优化策略：用空间换时间。我们将通过在 `Shape` 类内部存储虚函数指针来实现手动虚函数分发。以下代码片段已经很好地说明了细节：

```
//---- <Shape.h> ----------------

#include <cstddef>
#include <memory>

class Shape
{
public:
    // ...

private:
    // ...

    template< typename ShapeT
            , typename DrawStrategy >
    struct OwningModel ⑮
    {
        OwningModel( ShapeT value, DrawStrategy drawer )
            : shape_( std::move(value) )
            , drawer_( std::move(drawer) )
        {}

        ShapeT shape_;
        DrawStrategy drawer_;
    };

    using DestroyOperation = void(void*);      ⑯
    using DrawOperation    = void(void*);      ⑰
    using CloneOperation   = void*(void*);     ⑱

    std::unique_ptr<void,DestroyOperation*> pimpl_;  ⑲
    DrawOperation* draw_ { nullptr };            ⑳
    CloneOperation* clone_ { nullptr };          ㉑
};
```

由于我们替换了所有虚函数，甚至虚析构函数，已经不再需要 Concept 基类了。因此，外部分层结构简化为仅 OwningModel 类模板 (⑮)，它仍然作为特定形状 (即 ShapeT) 和 DrawStrategy 的存储。仍然，它面临同样的命运：所有虚函数都被移除。唯一剩下的细节是构造函数和数据成员。

虚函数被手动函数指针取代。由于函数指针的语法并不最愉快使用，我们为方便起见添加了一些函数类型别名：¹⁷ `DestroyOperation` 代表前面的虚析构函数 (⑲)，`DrawOperation` 代表前面的虚 `draw()` 函数 (⑰)，而 `CloneOperation` 代表前面的虚 `clone()` 函数 (⑲)。
`DestroyOperation` 用于配置 `pimpl_` 数据成员的 `Deleter` (⑲) (并且作为这样的它充当 *策略*)。后两者，即 `DrawOperation` 和 `CloneOperation`，用于两个额外的函数指针数据成员 `draw_` 和 `clone_` (²⁰ 和 ²¹)。

“哦不，void*s！这不是一种古老且超级危险的做法吗？”，你倒吸一口气。好吧，我承认在没有解释的情况下看起来很可疑。然而，坚持下去，我保证一切都会完全正常且类型安全。让它工作的关键现在在于这些函数指针的初始化。它们在 `Shape` 类的模板化构造函数中初始化：

```
//---- <Shape.h> -------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
        : pimpl_( ②
            new OwningModel<ShapeT,DrawStrategy>(
            std::move(shape)
            std::move(drawer) )
            , []( void* shapeBytes ){ ²³
                using Model = OwningModel<ShapeT,DrawStrategy>;
                auto* const model = static_cast<Model*>
                (shapeBytes); ²⁴
                delete model;  ²⁵
            } )
        , draw_( ²⁶


```
[]( void* shapeBytes ) {
    using Model = OwningModel<ShapeT,DrawStrategy>;
    auto* const model = static_cast<Model*>
(shapeBytes);
    (*model->drawer_)( model->shape_ );
}
, clone_( ②
    []( void* shapeBytes ) -> void* {
    using Model = OwningModel<ShapeT,DrawStrategy>;
    auto* const model = static_cast<Model*>
(shapeBytes);
    return new Model( *model );
    } )
{}

// ...
private:
// ...
};
```

让我们聚焦于 pimpl_ 数据成员。它的初始化既通过指向新实例化 OwningModel 的指针 ①，也通过一个无状态的 lambda 表达式 ③ 来完成。你可能记得，无状态的 lambda 可以隐式转换为函数指针。正是这种语言保证被我们利用：我们直接将 lambda 作为 deleter 传递给 unique_ptr 的构造函数，强制编译器应用到 DestroyOperation* 的隐式转换，从而将 lambda 函数绑定到 std::unique_ptr。

“好吧，我明白了，lambda 可以用来初始化函数指针。但它是如何工作的？它做了什么？”好吧，也请记住，我们是在模板化构造函数内部创建这个 lambda。这意味着此时我们完全清楚所传递的 ShapeT 和 DrawStrategy 的实际类型。因此 lambda 是在知道 pimpl_ 内部实例化和存储的 OwningModel 的类型的情况下生成的。最终它会用一个 void* 被调用，即某个 OwningModel 的地址。然而，基于它对 OwningModel 实际类型的了解，它首先可以执行从 void* 到

OwningModel<ShapeT,DrawStrategy>* 的 static_cast（20）。在大多数其他上下文中，这种转换会令人怀疑并且很可能是一种盲目猜测，但在这种上下文中它是完全类型安全的：我们可以确定 OwningModel 的正确类型。因此我们可以使用 resulting 指针来触发正确的清理行为（20）。

draw_ 和 clone_ 数据成员的初始化非常相似（20 和 27）。唯一的差别当然是 lambda 执行的操作：它们分别执行绘制形状的正确操作以及创建模型副本。

我知道，这些内容可能需要一些时间来消化。但我们几乎完成了：唯一缺少的细节是特殊成员函数。对于析构函数和两个移动操作，我们可以再次要求编译器生成的默认版本。然而，我们必须自己处理拷贝构造函数和拷贝赋值运算符：

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    // ...
    Shape( Shape const& other )
        : pimpl_( clone_( other.pimpl_.get() ),
        other.pimpl_.get_deleter() )
        , draw_( other.draw_ )
        , clone_( other.clone_ )
    {}
    Shape& operator=( Shape const& other )
    {
        // Copy-and-Swap Idiom
        using std::swap;
        Shape copy( other );
        swap( pimpl_, copy.pimpl_ );
        swap( draw_, copy.draw_ );
        swap( clone_, copy.clone_ );
        return *this;

}
~Shape() = default;
Shape( Shape&& ) = default;
Shape& operator=( Shape&& ) = default;

private:
    // ...
};
```

这就是我们需要做的全部，我们准备好来尝试了。所以让我们把这个实现放到测试中：再次，我们更新了“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”中的基准测试，并使用我们手动实现的虚函数来运行它。我甚至将手动的虚分派与之前讨论过的 *Small Buffer Optimization (SBO)* 结合起来。表 8-3 显示了性能结果：

### Table 8-3. Performance results for the Type Erasure implementations with manual virtual dispatch

|  | GCC 11.1 | Clang 11.1 |
|---|---|---|
| Object-oriented solution | 1.5205s | 1.1480s |
| `std::function` | 2.1782s | 1.4884s |
| Manual implementation of `std::function` | 1.6354s | 1.4465s |
| Classic Strategy | 1.6372s | 1.4046s |
| Type Erasure | 1.5298s | 1.1561s |
| Type Erasure (SBO) | 1.3591s | 1.0348s |
| Type Erasure (manual virtual dispatch) | 1.1476s | 1.1599s |
| Type Erasure (SBO + manual virtual dispatch) | 1.2538s | 1.2212s |

手动虚分派的性能提升对于 GCC 来说是惊人的。在我的系统上，我降到了 1.1476 秒，与基于的未优化的 *Type Erasure* 实现相比提升了 25%。另一方面，Clang 与基础未优化实现相比并没有显示出任何改进。虽然这可能有点令人失望，但运行时当然仍然很 remarkable。

不幸的是，*SBO* 与手动虚分派的结合并没有带来更好的性能。虽然 GCC 与纯 *SBO* 方法相比显示出小幅提升（这对于没有动态内存的环境可能很有趣），但在 Clang 上，这种组合的表现并不像你希望的那样好。

总之，*Type Erasure* 实现的性能优化潜力很大。如果你之前对 *Type Erasure* 持怀疑态度，这种性能提升应该会给你一个强有力的激励去自行研究。虽然这很惊人且毫无疑问非常令人兴奋，但重要的是要记住这来自何处：正是因为将虚行为的关注点分离并将行为封装到值类型中，我们才获得了这些优化机会。如果我们只有基类指针，就无法实现这一点。

### GUIDELINE 33: KNOW ABOUT THE OPTIMIZATION POTENTIAL OF *TYPE ERASURE*

- 使用 *Small Buffer Optimization (SBO)* 来避免小对象的昂贵拷贝操作；
- 通过手动实现虚分派来减少间接层数。

### Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers

在“Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”和“Guideline 33: Know about the Optimization Potential of Type Erasure”中，我引导你穿过了基本 Type Erasure 实现的实现细节丛林。是的，这很艰难，但绝对值得付出努力：你变得更强、更明智，工具箱里多了一种新的、高效且强解耦的设计模式。太棒了！

然而，我们必须再次回到丛林中。我看到你在翻白眼，但还有更多。而且我必须承认：我撒谎了。至少有一点。但不是告诉你错误的东西，而是遗漏了。还有一个你应该知道的 Type Erasure 的缺点。一个很大的缺点。你可能一点也不喜欢。叹气。

### The Setup Costs of an Owning Type Erasure Wrapper

假设 Shape 再次是一个基类，而 Circle 是众多派生类之一。那么将 Circle 传递给期望 Shape const& 的函数将是简单且廉价的（1）：

```
#include <cstdlib>

class Shape { /*...*/ };  // Classic base class

class Circle : public Shape { /*...*/ };  // Deriving class

void useShape( Shape const& shape )
{
    shape.draw( /*...*/ );
}

int main()
{
    Circle circle{ 3.14 };

    // Automatic and cheap conversion from 'Circle const&' to
    'Shape const&'
useShape( circle ); ①

return EXIT_SUCCESS;
}
```

尽管 *Type Erasure* Shape 抽象有点不同（例如，它始终需要一个绘制 *Strategy*），这种转换仍然是可能的：

```
#include <cstdlib>

class Circle { /*...*/ };  // Non-polymorphic geometric primitive

class Shape { /*...*/ };  // Type erasure wrapper class as shown before

void useShape( Shape const& shape )
{
    draw(shape);
}

int main()
{
    Circle circle{ 3.14 };
    auto drawStrategy = []( Circle const& c ){ /*...*/ };

    // Creates a temporary 'Shape' object, involving
    //   a copy operation and a memory allocation
    useShape( { circle, drawStrategy } ); ②

    return EXIT_SUCCESS;
}
```

不幸的是，它不再廉价。相反：基于我们之前的实现，包括基本实现和优化实现，对 useShape() 函数的调用将涉及一些潜在昂贵的操作（②）：

- 为了将 Circle 转换为 Shape，编译器通过非显式的、模板化的 Shape 构造函数创建一个临时的 Shape；



+   - 构造函数的调用会导致给定形状的拷贝操作（对 `Circle` 来说不昂贵，但对其他形状可能很昂贵）以及给定的绘制 *Strategy* 的拷贝（如果 *Strategy* 无状态则基本免费，但根据对象内部存储的内容可能很昂贵）；
+   - 在 `Shape` 构造函数内部，会创建一个新的形状模型，涉及一次内存分配（隐藏在 `Shape` 构造函数中对 `std::make_unique()` 的调用中，且肯定很昂贵）；
+   - 临时的（右值）`Shape` 通过引用到 const 传递给 `useShape()` 函数。

必须指出，这并不是我们 `Shape` 实现的特定问题。如果你例如使用 `std::function` 作为函数参数，也会遇到同样的问题：

```
#include <cstdlib>
#include <functional>

int compute( int i, int j, std::function<int(int,int)> op )
{
    return op( i, j );
}

int main()
{
    int const i = 17;
    int const j = 10;

    int const sum = compute( i, j, [offset=15]( int x, int y ) {
        return x + y + offset;
    } );

    return EXIT_SUCCESS;
}
```

在此示例中，给定的 lambda 会被转换为一个 `std::function` 实例。此转换会涉及一次拷贝操作，并且可能涉及一次内存分配。它完全取决于给定可调用对象的大小以及

`std::function` 的实现。因此，`std::function` 是一种与例如 `std::string_view` 和 `std::span` 不同的抽象。`std::string_view` 和 `std::span` 是非拥有抽象，拷贝成本低廉，因为它们仅由指向第一个元素的指针和大小组成。由于这两种类型执行浅拷贝，因此非常适合作为函数参数。另一方面，`std::function` 是一个拥有抽象，会执行深拷贝。因此它不是作为函数参数使用的完美类型。不幸的是，我们的 Shape 实现也是如此。¹⁸

“Oh my, I don’t like this. Not at all. That is terrible! I want my money back!”，你抱怨道。我必须同意这可能是在你的代码库中一个严重的问题。然而，你理解到根本问题是 Shape 类的拥有语义：在其值语义背景下，我们当前的 Shape 实现总是会创建给定形状的拷贝并且总是拥有该拷贝。虽然这与“Guideline 22: Prefer Value Semantics to Reference Semantics”中讨论的所有优点完全一致，但在这种上下文中它会导致相当不幸的性能损失。然而，保持冷静，我们可以做些什么：对于这种上下文，我们可以提供一个非拥有的类型擦除实现。

### 一个简单的非拥有类型擦除实现

总体而言，基于值语义的类型擦除实现很美妙，并且完全符合现代 C++ 的精神。然而，性能很重要。它可能重要到有时你可能不在乎值语义部分，而只在乎类型擦除提供的抽象。在这种情况下，你可能希望使用类型擦除的非拥有实现，尽管这会让你回到引用语义的领域。

好消息是，如果你只需要一个简单的类型擦除包装器，一个代表引用到基的、非拥有且可平凡拷贝的包装器，那么所需的代码相当简单。这在

特别如此，因为你已经在“Guideline 33: Know about the Optimization Potential of Type Erasure”中看到了如何手动实现虚分派的方法。有了这种技术，一个简单的非拥有类型擦除实现只是几行代码的问题：

//---- <Shape.h> ----------------

#include <memory>

class ShapeConstRef
{
public:
    template< typename ShapeT, typename DrawStrategy >
    ShapeConstRef( ShapeT& shape, DrawStrategy& drawer ) ⑥
        : shape_{ std::addressof(shape) }
        , drawer_{ std::addressof(drawer) }
        , draw_{ []( void const* shapeBytes, void const* drawerBytes ) {
            auto const* shape = static_cast<ShapeT const*>(shapeBytes);
            auto const* drawer = static_cast<DrawStrategy const*>(drawerBytes);
            (*drawer)( *shape );
        } }
    {}

private:
    friend void draw( ShapeConstRef const& shape )
    {
        shape.draw_( shape.shape_, shape.drawer_ );
    }

    using DrawOperation = void( void const*,void const* );

    void const* shape_{ nullptr };   ③
    void const* drawer_{ nullptr };  ④
    DrawOperation* draw_{ nullptr }; ⑤
};
```

顾名思义，ShapeConstRef 类代表对 const 形状类型的引用。它不是存储给定形状的拷贝，而是仅以 void* 的形式保存指向它的指针（③）。此外，它还保存指向相关联 DrawStrategy 的 void*

（④）。以及作为第三个数据成员，指向手动实现的虚 draw() 函数的函数指针（⑤）（参见“Guideline 33: Know about the Optimization Potential of Type Erasure”）。

ShapeConstRef 以引用到非 const 的形式接收其两个参数，形状和绘制 Strategy，两者都可能 cv 限定（⑥）。¹⁹ 在这种形式下，不可能将右值传递给构造函数，这防止了任何与临时值的生命周期问题。这种方式无法保护你免受所有可能的与左值的生命周期问题，但仍然提供非常合理的保护。²⁰ 如果你想允许右值，你应该重新考虑。并且如果你真的、真的愿意冒临时值的生命周期风险，你可以简单地按引用到 const 接收参数。只要记住你没有从我这里得到这个建议！

就是这样。这就是完整的非拥有实现。它高效、简短、简单，如果你不需要存储任何关联数据或 Strategy 对象，甚至可以更短更简单。有了此功能，你现在能够创建廉价的形状抽象。这在以下代码示例中通过 useShapeConstRef() 函数得以演示。此函数使你能够使用 ShapeConstRef 作为函数参数来绘制任何种类的形状（Circles、Squares、...）以及任何可能的绘制实现。在 main() 函数中，我们通过具体的形状和具体的绘制 Strategy（这里是一个 lambda）调用 useShapeConstRef()（⑦）：

//---- <Main.cpp> ----------------

#include <Circle.h>
#include <Shape.h>
#include <cstdlib>

void useShapeConstRef( ShapeConstRef shape )
{
    draw( shape );
}

int main()
{
    // Create a circle as one representative of a concrete shape type
    Circle circle{ 3.14 };

    // Create a drawing strategy in form of a lambda
    auto drawer = []( Circle const& c ){ /*...*/ };

    // Draw the circle directly via the 'ShapeConstRef' abstraction
    useShapeConstRef( { circle, drawer } );

    return EXIT_SUCCESS;
}
```

此调用触发了期望的效果，尤其是在没有任何内存分配或昂贵拷贝操作的情况下，只是通过将多态行为包装在指向给定形状和绘制 Strategy 的指针集合周围。

### 一个更强大的非拥有类型擦除实现

大多数时候，这个简单的非拥有类型擦除实现应该足够并且满足你的所有需求。有时，然而，只有有时，它可能不够。有时，你可能对稍微不同形式的 Shape 引用感兴趣：

```
#include <Circle.h>
#include <Shape.h>
#include <cstdlib>

int main()
{
    // Create a circle as one representative of a concrete shape type
    Circle circle{ 3.14 };

    // Create a drawing strategy in form of a lambda
    auto drawer = []( Circle const& c ){ /*...*/ };

    // Combine the shape and the drawing strategy in a 'Shape' abstraction
    Shape shape1( circle, drawer );

    // Draw the shape
    draw( shape1 );

    // Create a reference to the shape
    // Works already, but the shape reference will store a pointer
    // to the 'shape1' instance instead of a pointer to the
    'circle'.
    ShapeConstRef shaperef( shape1 );  ⑧

    // Draw via the shape reference, resulting in the same output
    // This works, but only by means of two indirections!
    draw( shaperef );  ⑨

    // Create a deep copy of the shape via the shape reference
    // This is _not_ possible with the simple non-owning
    implementation!
    // With the simple implementation, this creates a copy of the
    'shaperef'
    // instance. 'shape2' itself would act as a reference and
    there would be
    // three indirections... sigh.
    Shape shape2( shaperef );  ⑩

    // Drawing the copy will again result in the same output
    draw( shape2 );

    return EXIT_SUCCESS;
}
```

假设你有一个类型擦除的圆圈叫做 shape1，你可能想将此 Shape 实例转换为 ShapeConstRef ⑧。使用当前实现，这可行，但 shaperef 实例将持有指向 shape1 实例的指针，而不是指向 circle 的指针。因此，任何对 shaperef 的使用都会导致两次间接访问（一次通过 ShapeConstRef，一次通过所引用的 Shape 抽象）⑨。此外，你可能还对将 ShapeConstRef 实例转换为 Shape 实例感兴趣⑩。在这种情况下，你可能会期望，对底层 Circle 进行



created and that the resulting Shape abstraction contains and represents this copy. Unfortunately, with the current implementation, the Shape would create a copy of the ShapeConstRef instance, and thus introduce a third indirection. sigh

If you are in need of a more efficient interaction between owning and non-owning Type Erasure wrappers and if you need a real copy when copying a non-owning wrapper into an owning wrapper, then I can offer you a working solution. Unfortunately, it is more involved than the previous implementation(s), but fortunately not overly complex. The solution will build on the basic Type Erasure implementation from “Guideline 32: Consider Replacing Inheritance Hierarchies with Type Erasure”, which includes the ShapeConcept and OwningShapeModel classes in the detail namespace and the Shape Type Erasure wrapper. You will see that it just requires a few additions, all of which you have already seen before.

The first addition happens in the ShapeConcept base class:

```
//---- <Shape.h> -------------

#include <memory>
#include <utility>

namespace detail {

class ShapeConcept
{
public:
    // ...
    virtual void clone( ShapeConcept* memory ) const = 0;
};

// ...

} // namespace detail
```

The ShapeConcept class is extended with a second clone() function (①). Instead of returning a newly instantiated copy of the according

model, this function is passed the address of the memory location, where the new model needs to be created.

The second addition is a new model class, the NonOwningShapeModel:

```
//---- <Shape.h> ----------------
// ...

namespace detail {
// ...

template< typename ShapeT
        , typename DrawStrategy >
class NonOwningShapeModel : public ShapeConcept
{
public:
    NonOwningShapeModel( ShapeT& shape, DrawStrategy& drawer )
        : shape_{ std::addressof(shape) }
        , drawer_{ std::addressof(drawer) }
    {}

    void draw() const override { (*drawer_)(*shape_); }

    std::unique_ptr<ShapeConcept> clone() const override
    {
        using Model = OwningShapeModel<ShapeT,DrawStrategy>;
        return std::make_unique<Model>( *shape_, *drawer_ );
    }

    void clone( ShapeConcept* memory ) const override
    {
        std::construct_at( static_cast<NonOwningShapeModel*>
            (memory), *this );

        // or:
        // auto* ptr = const_cast<void*>(static_cast<void const volatile*>(memory));
        // ::new (ptr) NonOwningShapeModel( *this );
    }

private:
    ShapeT* shape_{ nullptr };
    DrawStrategy* drawer_{ nullptr };
```

```
// ...
} // namespace detail
```

The NonOwningShapeModel is very similar to the OwningShapeModel implementation, but, as the name suggests, does not store copies of the given shape and strategy. Instead it stores only pointers (⑧ and ⑨). Thus this class represents the reference semantics version of the OwningShapeModel class. Also NonOwningShapeModel needs to override the pure virtual functions of the ShapeConcept class: draw() again forwards the drawing request to the given drawing Strategy (⑩), while the clone() functions perform a copy. The first clone() function is implemented by creating a new OwningShapeModel and copying both the stored shape and drawing Strategy (⑪). The second clone() function is implemented by creating a new NonOwningShapeModel at the specified address by means of std::construct_at() (⑫).

In addition, the OwningShapeModel class needs to provide an implementation of the new clone() function:

```
//---- <Shape.h> ----------------

// ...

namespace detail {

template< typename ShapeT
        , typename DrawStrategy >
class OwningShapeModel : public ShapeConcept
{
public:
    // ...

    void clone( ShapeConcept* memory ) const  ⑬
    {
        using Model = NonOwningShapeModel<ShapeT const, DrawStrategy const>;

std::construct_at( static_cast<Model*>(memory), shape_,
drawer_ );

    // or:
    // auto* ptr =
    //     const_cast<void*>(static_cast<void const volatile*>
(memory));
    // ::new (ptr) Model( shape_, drawer_ );
}
};

// ...
} // namespace detail
```

The clone() function in OwningShapeModel is implemented similarly to the implementation in the NonOwningShapeModel class by creating a new instance of a NonOwningShapeModel by means of std::construct_at() (17).

The next addition is the corresponding wrapper class, that acts as a wrapper around the external hierarchy ShapeConcept and NonOwningShapeModel. This wrapper should take on the same responsibilities as the Shape class (i.e. the instantiation of the NonOwningShapeModel class template and the encapsulation of all pointers handling), but should merely represent a reference to a const concrete shape, not a copy. This wrapper is again given in form of the ShapeConstRef class:

```
//---- <Shape.h> ----------------

#include <array>
#include <cstddef>
#include <memory>

// ...

class ShapeConstRef
{
public:
```

```
// ...
private:
    // ...
    // Expected size of a model instantiation:
    //     sizeof(ShapeT*) + sizeof(DrawStrategy*) + sizeof(vptr)
    static constexpr size_t MODEL_SIZE = 3U*sizeof(void*);
    alignas(void*) std::array<std::byte,MODEL_SIZE> raw_;
};
```

As you will see, the ShapeConstRef class is very similar to the Shape class, but there are a few important differences. The first noteworthy detail is the use of a raw_ storage in form of a properly aligned std::byte array (10). That indicates that ShapeConstRef does not allocate dynamically, but firmly builds on in-class memory. In this case, however, this is easily possible, because we can predict the size of the required NonOwningShapeModel to be equal to the size of three pointers (assuming that the pointer to the virtual function table, i.e. the vptr, has the same size as any other pointer) (10).

The private section of ShapeConstRef also contains a couple of member functions:

```
//---- <Shape.h> ----------------
// ...
class ShapeConstRef
{
public:
    // ...
private:
    friend void draw( ShapeConstRef const& shape )
    {
        shape.pimpl()->draw();
    }
    ShapeConcept* pimpl()
return reinterpret_cast<ShapeConcept*>( raw_.data() );
}

ShapeConcept const* pimpl() const  21
{
    return reinterpret_cast<ShapeConcept const*>( raw_.data()
);
}

// ...
};
```

We also add a draw() function as hidden friend and, just as in the Small Buffer Optimization (SBO) implementation in “Guideline 33: Know about the Optimization Potential of Type Erasure”, we add a pair of pimpl() functions (20 and 21). This will enable us to work conveniently with the in-class std::byte array.

The second noteworthy detail is the signature function of every Type Erasure implementation: the templated constructor:

```
//---- <Shape.h> -------------

// ...

class ShapeConstRef
{
public:
    // Type 'ShapeT' and 'DrawStrategy' are possibly cv qualified;
    // lvalue references prevent references to rvalues
    template< typename ShapeT
            , typename DrawStrategy >
    ShapeConstRef( ShapeT& shape
                  , DrawStrategy& drawer )  22
    {
        using Model =
            detail::NonOwningShapeModel<ShapeT const,DrawStrategy const>;
        static_assert( sizeof(Model) == MODEL_SIZE, "Invalid size detected" );  24
        static_assert( alignof(Model) == alignof(void*), "Misaligned detected" );

std::construct_at( static_cast<Model*>(pimpl()), shape_,
drawer_ );  25

    // or:
    // auto* ptr =
    //     const_cast<void*>(static_cast<void const volatile*>
(pimpl()));
    // ::new (ptr) Model( shape_, drawer_ );
}

// ...

private:
    // ...
};
```

Again, you have the choice to accept the arguments by reference-to-non-const, in order to prevent lifetime issues with temporaries (very much recommended!) (26). Alternatively, you accept the arguments by reference-to-const, which would allow you to pass rvalues, but puts you at risk to experience lifetime issues with temporaries. Inside the constructor we again first use a convenient type alias for the required type of model (23), before checking the actual size and alignment of the model (24). If it does not adhere to the expected MODEL_SIZE or pointer alignment, we create a compile time error. Then we construct the new model inside the in-class memory by means of std::construct_at() (25).

```
//---- <Shape.h> ----------------

// ...

class ShapeConstRef
{
public:
    // ...

    ShapeConstRef( Shape& other )      { other.pimpl_->clone(
pimpl() ); }  26
    ShapeConstRef( Shape const& other ) { other.pimpl_->clone(
pimpl() ); }

    ShapeConstRef( ShapeConstRef const& other )


{
    other.pimpl()->clone( pimpl() );
}

ShapeConstRef& operator=( ShapeConstRef const& other )
{
    // Copy-and-swap idiom
    ShapeConstRef copy( other );
    raw_.swap( copy.raw_ );
    return *this;
}

~ShapeConstRef()
{
    std::destroy_at( pimpl() );
    // or: pimpl()->~ShapeConcept();
}

// Move operations explicitly not declared  27

private:
    // ...
};
```

除了模板化的 ShapeConstRef 构造函数之外，ShapeConstRef 还提供了两个构造函数以实现从 Shape 实例的转换（27）。严格来说这些并非必需，因为我们也可以为 Shape 创建一个 NonOwningShapeModel 实例，但这些构造函数直接为相应的底层形状类型创建 NonOwningShapeModel，从而省去了一层间接引用，这有助于提升性能。需要注意的是，为了使这些构造函数生效，ShapeConstRef 必须成为 Shape 类的友元。尽管如此也不必担心，因为这是一个友元使用的良好示例：Shape 和 ShapeConstRef 确实属于一起、手牵手地协同工作，并且甚至放在同一个头文件中提供。

最后一个值得注意的细节是，两个移动操作既未显式声明也未删除（27）。由于我们已显式定义了两个拷贝操作，编译器既不会创建也不会删除两个移动操作，因此它们消失了。完全消失的意思是，这两个函数永远不会参与重载决议。是的，这与显式删除它们不同：如果它们被删除，它们会参与重载决议，若被选中则会导致编译错误。但在没有这两个函数的情况下，当你尝试移动一个 ShapeConstRef 时，拷贝操作会被使用，而拷贝操作是廉价且高效的，因为 ShapeConstRef 只代表一个引用。因此该类刻意实现了 Rule of 3。

我们几乎完成了。最后一个细节是 Shape 类中的一个额外添加，一个额外的构造函数：

//---- <Shape.h> ----------------

// ...

class Shape
{
public:
    // ...

    Shape( ShapeConstRef const& other )
        : pimpl_{ other.pimpl()->clone() }
    {}

private:
    // ...
}
```

通过此构造函数，Shape 的实例会创建传递进来的 ShapeConstRef 实例中存储的形状的深拷贝。若没有此构造函数，Shape 将存储 ShapeConstRef 实例的拷贝，从而自身也充当引用。

总而言之，两种非拥有实现，简单和更复杂的，都能为你提供类型擦除设计模式的所有设计优势，但同时也将你拉回引用语义的范畴，并伴随其所有缺陷。因此，利用这种非拥有形式的 Type Erasure 的优势，但也要意识到通常的生命周期问题。将它视为与 std::string_view 和 std::span 同一级别。所有这些都可作为函数参数的非常有用的工具，但不要用它们来长期存储任何东西，例如以数据成员的形式。生命周期相关问题的危险性实在太高了。

### GUIDELINE 34: BE AWARE OF THE SETUP COSTS OF OWNING TYPE ERASURE WRAPPERS

- Keep in mind that the setup of owning Type Erasure wrappers may involve copy operations and allocations;
- Be aware of non-owning Type Erasure, but also understand its reference semantics deficiencies;
- Prefer simple Type Erasure implementations, but know their limits;
- Prefer to use non-owning Type Erasure for function arguments, but not for data members or return types.

1 Yes, I consider the manual use of std::unique_ptr manual lifetime management. But of course it could be much worse if we would not reach for the power of RAII.

2 The term Type Erasure is heavily overloaded as it is used in different programming languages and for many different things. Even within the C++ community you hear the term begin used for various purposes: you might have heard it being used to denote void*, pointers-to-base, and std::variant. In the context of software design I consider this a very unfortunate issue. I will address this issue at the end of this guideline.

3 Sean Parent, “Inheritance Is The Base Class of Evil”, GoingNative 2013, YouTube.

4 Kevlin Henney, “Valued Conversions”, C++ Report July-August 2000, citeseer.

5 For an introduction to std::function see “Guideline 23: Prefer a Value-Based Implementation of Strategy and Command”.

6 The placement of ShapeConcept and OwningShapeModel in a namespace is purely an implementation detail of this example implementation. Still, as you will see in “Guideline 34: Be Aware of the Setup Costs of Owning Type Erasure Wrappers” this choice will come in pretty handy. Alternatively, these two classes can be implemented as nested classes. You will see examples for this in “Guideline 33: Know about the Optimization Potential of Type Erasure”.

7 Refer to “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism” for the implementation based on std::function.

8 Many thanks to Arthur O’Dwyer for providing this example.

9 Again, please do not consider these performance numbers as the perfect truth. These are the performance results on my machine and my implementation. Your results will differ for sure. However, the take-away is that Type Erasure performs really well and might perform even better if we take the many optimization options into account (see “Guideline 33: Know about the Optimization Potential of Type Erasure”).

10 Eric Niebler on Twitter, June 19th, 2020 (Twitter).

11 For an introduction of std::variant, see “Guideline 17: Consider std::variant for Implementing Visitors”.

12 You should avoid going too deep, though, as you probably remember what happened to the dwarves of Moria who dug too deep…

13 Alternatively you could use an array of bytes, i.e. std::byte[Capacity], or std::aligned_storage. The advantage of std::array is that it enables you to copy the buffer (if that is applicable!).

14 Note that the choice for the default arguments for Capacity and Alignment are reasonable, but still arbitrary. You can of course use different defaults, that best fit the properties of the expected actual types.

15 You might not have seen a placement new before. If that is the case, rest assured that this form of new does not perform any memory allocation, but only calls a constructor to create an object at the specified address. The only syntactic difference is that you provide an additional pointer argument to new.

16 As a reminder, since you might not see this syntax often: the template keyword in the constructor is necessary because we are trying to call a function template on a dependent name (i.e. a name the meaning of which depends on a template parameter). Therefore you have to make it clear to the compiler that the following is the beginning of a template argument list and not a less-than comparison.

17 There is people that consider functions pointers to be the best feature of C++. In his lightning talk “The Very Best Feature of C++” (YouTube), James McNellis demonstrates their syntactic beauty and enormous flexibility. Please do not take this too serious, though, but rather as a humorous demonstration of a C++ imperfection.

18 At the time of writing there is an active proposal for a std::function_ref type, a non-owning version of std::function.

19 The term cv qualified refers to the const and volatile qualifiers.

20 For a reminder about lvalues and rvalues, refer to Nicolai Josuttis’ book on move semantics: “C++ Move Semantics - The Complete Guide”, NicoJosuttis, Aug. 2020, ISBN: 978-3967309003.
