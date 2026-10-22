

并且有一个移动构造函数，因此我们知道它可以被高效地移动。当我们按值传递一个 std::function 时，会调用拷贝构造函数或移动构造函数。如果我们传递的是一个左值，则会调用拷贝构造函数，拷贝该左值。然后我们会将该拷贝移动到数据成员中。总共我们会执行一次拷贝和一次移动来初始化 drawer_ 数据成员。如果我们传递的是一个右值，则会调用移动构造函数，移动该右值。随后得到的 argument strategy 会被移动到数据成员 drawer_ 中。总共我们会执行两次移动操作来初始化 drawer_ 数据成员。因此这种形式代表了一个很好的折中：它优雅且效率几乎没有差别。

一旦我们重构了 Circle 和 Square 类，我们就可以以任何我们喜欢的形式实现不同的绘制策略（即以函数、函数对象或 lambda 的形式）。例如，我们可以像下面这样将 OpenGLCircleStrategy 实现为一个函数对象：

//---- <OpenGLCircleStrategy.h> ----------------

#include <Circle.h>

class OpenGLCircleStrategy
{
public:
    explicit OpenGLCircleStrategy( /* Drawing related arguments */
);

    void operator() ( Circle const& circle, /*...*/ ) const;

private:
    /* Drawing related data members, e.g. colors, textures, ... */
};
```

我们唯一需要遵循的约定是，我们需要提供一个调用操作符，它接受一个 Circle，可能还有几个绘制相关的参数，并且不返回任何内容（即满足函数类型 void(Circle const&, /*...*/)）。

假设 OpenGLSquareStrategy 也有类似的实现，我们现在可以创建不同种类的形状，为它们配置期望的绘制行为，最后绘制它们：

```
#include <Circle.h>
#include <Square.h>
#include <OpenGLCircleStrategy.h>
#include <OpenGLSquareStrategy.h>
#include <memory>
#include <vector>

int main()
{
    using Shapes = std::vector<std::unique_ptr<Shape>>;

    Shapes shapes{};

    // Creating some shapes, each one
    //   equipped with the according OpenGL drawing strategy
    shapes.emplace_back(
        std::make_unique<Circle>( 2.3,
        OpenGLCircleStrategy(/*...red...*/ ) );
    shapes.emplace_back(
        std::make_unique<Square>( 1.2,
        OpenGLSquareStrategy(/*...green...*/ ) );
    shapes.emplace_back(
        std::make_unique<Circle>( 4.1,
        OpenGLCircleStrategy(/*...blue...*/ ) );

    // Drawing all shapes
    for( auto const& shape : shapes )
    {
        shape->draw();
    }

    return EXIT_SUCCESS;
}
```

main() 函数与使用经典 Strategy 实现的原始实现非常相似（参见“Guideline 19: Use Strategy to Isolate How Things are Done”）。然而，这种非侵入式的、即无基类的、基于 std::function 的方法进一步降低了耦合。这在该解决方案的依赖图中变得明显（参见图 5-9）：

我们可以以任何我们想要的形式实现绘制功能（作为自由函数、函数对象或 lambda），并且不必遵守基类的要求。而且，通过 `std::function` 我们自动反转了依赖关系（参见“Guideline 9: Pay Attention to the Ownership of Abstractions”）。

```
class Shape
{
public:
    Shape() = default;
    virtual ~Shape() = default;
    virtual void draw() const = 0;
    // ...
};
```

High-level

Architectural Boundary

```
class Circle : public Shape
{
public:
    Circle( double rad
        , std::function<void(Circle const&)> strategy );

    void draw() const override;
    // ...
};
```

Automatic inversion of dependencies

Architectural Boundary

```
class OpenGLCircleStrategy
{
public:
    void operator()(Circle const& circle) const;
};
```

Low-level

Figure 5-9. Dependency graph for the std::function solution.

## Performance Benchmarks

“我喜欢这种灵活性、自由度。这太棒了！但性能如何呢？” 是的，说得像一个真正的 C++ 开发者。当然性能很重要。在展示性能结果之前，让我提醒你一下我们用来获取“Guideline 16: Use Visitors to Extend Operations”中表 4-2 数字的基准场景。对于基准测试，我实现了四种不同种类的形状（圆、方形、椭圆和矩形）。同样，我在 10,000 个随机创建的形状上运行 25,000 次平移操作。我使用 GCC 11.1 和 Clang 11.1 两个编译器，并且对两个编译器都只添加了 -O3 和 -DNDEBUG 编译标志。我使用的平台是 macOS Big Sur（版本 11.4），搭载 8 核 Intel Core i7，3.8 Ghz，64 GB 主内存。

有了这些信息，你就可以准备查看性能结果了。表 5-1 显示了基于 Strategy 的绘制示例实现以及使用 std::function 的对应解决方案的性能数据。

Table 5-1. Performance results for different Strategy implementations

|  | GCC 11.1 | Clang 11.1 |
|---|---|---|
| Object-oriented solution | 1.5205s | 1.1480s |
| std::function | 2.1782s | 1.4884s |
| Manual implementation of std::function | 1.6354s | 1.4465s |
| Classic Strategy | 1.6372s | 1.4046s |

作为参考，第一行显示了“Guideline 15: Design for the Addition of Types or Operations”中面向对象解决方案的性能。如你所见，该解决方案提供了最佳性能。这并不令人意外：由于 Strategy 设计模式无论实际实现如何都会引入额外开销，因此性能预计会下降。

然而，令人意外的是，std::function 实现带来了性能开销（在 GCC 的情况下甚至是显著的）。但等等，在你把这种方法扔进你的心理垃圾桶之前，请考虑第三行。它显示了通过类型擦除技术手动实现的 std::function，我将在第 8 章中解释该技术。这种实现性能更好，事实上与经典的 Strategy 设计模式实现一样好（或对 Clang 来说几乎一样好，见第四行）。这一结果表明，问题不在于值语义，而在于 std::function 的具体实现细节。36 总而言之，基于值语义的方法在性能方面并不比经典方法差，反而如前所述，提升了代码的许多重要方面。

## Analyzing the Shortcomings of the std::function Solution

总体而言，基于 std::function 的 Strategy 设计模式实现提供了诸多好处。首先，你的代码变得更简洁、更易读，因为你不必处理指针及其相关的生命周期管理（例如通过 std::unique_ptr）。而且你也不会遇到通常的引用语义相关问题（参见“Guideline 22: Prefer Value Semantics to Reference Semantics”）。其次，你促进了松耦合。非常松的耦合。在这种情况下，std::function 就像一个编译防火墙，它保护你免受不同 Strategy 实现的实现细节影响，同时为开发者如何实现不同的 Strategy 解决方案提供了巨大的灵活性。

尽管有这些优点，没有一种解决方案是完美无缺的，即使是基于 std::function 的方法也有其缺点。我已经指出了如果你依赖标准实现可能存在的性能劣势。虽然有方法可以最小化这种影响（参见第 8 章），但它仍然是你在代码库中需要考虑的问题。

还有一个与设计相关的问题。std::function 只能替换单个虚函数。如果需要抽象多个虚函数，例如当你想通过 Strategy 设计模式配置多个方面时，或在 Command 设计模式中需要 undo() 函数时，你就必须使用多个 std::function 实例。这不仅会因多个数据成员而增加类的大小，还会带来接口负担，因为如何优雅地处理多个 std::function 实例的传递成为问题。因此，基于 std::function 的方法最适合替换单个或极少数虚函数。尽管如此，这并不意味着你不能对多个虚函数使用基于值的方法：如果遇到这种情况，请考虑通过将用于 std::function 的技术直接应用于你的类型来推广该方法。我将在第 8 章中解释如何做到这一点。

尽管存在这些不足，基于值语义的方法证明了其是 Strategy 设计模式的出色实现。同样适用于

