

reason why I said “consider using the name”. You should decide on a case to case basis.

## 标准库中的示例

适配器设计模式的一个有用应用是标准化不同类型容器的接口。让我们假设以下 Stack 基类：

//---- <Stack.h> ----------------

template< typename T >
class Stack
{
public:
    virtual ~Stack() = default;
    virtual T& top() = 0;
    virtual bool empty() const = 0;
    virtual size_t size() const = 0;
    virtual void push( T const& value ) = 0;
    virtual void pop() = 0;
};
```

这个 Stack 类提供了访问栈顶元素、检查栈是否为空、查询栈大小、将元素压入栈以及移除栈顶元素的必要接口。现在这个基类可以用来为各种数据结构实现不同的适配器，例如 std::vector：

//---- <VectorStack.h> ----------------

#include <Stack.h>

template< typename T >
class VectorStack : public Stack<T>
{
public:
    T& top() override { return vec_.back(); }
    bool empty() const override { return vec_.empty(); }
    size_t size() const override { return vec_.size(); }
    void push( T const& value ) override { vec_.push_back(value);
}
    void pop() override { vec_.pop_back(); }
    private:
        std::vector<T> vec_;
};
```

“你真的建议通过抽象基类来实现栈吗？你不担心性能影响吗？每次使用成员函数都要付出一次虚函数调用的代价！”你担心道。不，当然我没有这么建议。显然你是对的，我完全同意你的观点：从 C++ 的角度来看，这种容器感觉很奇怪而且非常低效。出于效率考虑，我们通常通过类模板来实现同样的思想。这是 C++ 标准库采用的方法，以三种 STL 类即容器适配器的形式出现：std::stack、std::queue 和 std::priority_queue：

```
template< typename T
    , typename Container = std::deque<T> >
class stack;
```

```
template< typename T
    , typename Container = std::deque<T> >
class queue;
```

```
template< typename T
    , typename Container = std::vector<T>
    , typename Compare = std::less<typename Container::value_type> >
class priority_queue;
```

这三个类模板的目的是将给定 Container 类型的接口适配到特定用途。例如，std::stack 类模板的目的是将容器的接口适配到栈操作 top()、empty()、size()、push()、emplace()、pop() 和 swap()。默认情况下，你可以使用三种可用的序列容器 std::vector、std::list 和 std::deque。对于任何其他容器类型，你都可以特化 std::stack 类模板。

“这感觉熟悉多了”，你明显松了一口气地说。再次，我完全同意。我也认为标准库的方法对于容器的目的来说是更合适的解决方案。但比较这两种方法仍然很有趣。虽然 Stack 基类和 std::stack 类模板之间存在许多技术差异，但这两种方法的目的和语义却 remarkably 相似：两者都提供了将任何数据结构适配到给定栈接口的能力。而且两者都作为变化点，允许你以非侵入的方式添加新的适配器，而无需修改现有代码。

## 适配器与策略的比较

“这三个 STL 类似乎实现了适配器的意图，但这不是与策略设计模式中配置行为的方式相同吗？这与 std::unique_ptr 及其删除器类似吗？”你问道。是的，你是对的。从结构上看，策略和适配器设计模式非常相似。然而，正如“准则 11：理解设计模式的目的”中所解释的，设计模式的结构可能相似甚至相同，但意图是不同的。在这种情况下，Container 参数不仅仅指定行为的一个方面，而是大部分行为甚至全部行为。类模板仅仅作为给定类型的功能包装器，主要适配接口。因此，适配器的主要关注点是标准化接口并将不兼容的功能集成到现有的一套约定中，而另一方面，策略设计模式的主要关注点是使行为能够从外部进行配置，建立在并提供预期的接口之上。此外，在适配器的情况下，没有必要在任何时候重新配置行为。

## 函数适配器

适配器设计模式的另一个例子是标准库的自由函数 begin() 和 end()。“你认真的吗？”你惊讶地问道。“你声称自由函数是适配器设计模式的例子？这不是类的任务吗？”嗯，不一定。自由 begin() 和 end() 函数的目的是将任何类型的迭代器接口适配到预期的 STL 迭代器接口。因此它将一组可用的函数映射到一组预期的函数，因而服务于与任何其他适配器相同的目的。主要区别在于，与基于继承（运行时多态）或模板（编译时多态）的对象适配器或类适配器不同，begin() 和 end() 汲取其力量于函数重载，这是 C++ 中第二种主要的编译时多态机制。尽管如此，某种形式的抽象仍然在起作用。

> **NOTE**
> 
> 记住所有类型的抽象都代表一组需求，因此必须遵守里氏替换原则（LSP）。这对重载集合也同样适用；请参阅“准则 8：理解重载集合的语义要求”。

考虑以下函数模板：

```
template< typename Range >
void traverseRange( Range const& range )
{
    for( auto&& element : range ) {
        // ...
    }
}
```

在 traverseRange() 函数中，我们通过基于范围的 for 循环迭代给定范围中包含的所有元素。遍历通过迭代器进行，编译器通过自由 begin() 和 end() 函数获取这些迭代器。因此上面的 for 循环等价于以下形式的 for：

```
template< typename Range >
void traverseRange( Range const& range )
{
    {
```

```
using std::begin;
using std::end;

auto first( begin(range) );
auto last ( end(range) );
for( ; first!=last; ++first ) {
    auto&& element = *first;
    // ...
}
}
```

显然基于范围的 for 循环使用起来方便得多。然而，在表面之下，编译器基于自由 begin() 和 end() 函数生成代码。请注意开头的两个 using 声明：它们的目的是为给定 range 类型启用参数依赖查找（ADL）。ADL 是确保调用“正确”的 begin() 和 end() 函数的机制，即使它们是位于用户特定命名空间中的重载。这意味着，你有机会为任何类型重载 begin() 和 end()，并将预期接口映射到不同的、特定用途的函数集。

这种函数适配器在 2004 年由 Matthew Wilson 称为 shim。这种技术的一个有价值的特性是，它完全是非侵入式的：可以为任何类型添加自由函数，甚至是那些你永远无法适配的类型，例如第三方库提供的类型。因此，任何以 shim 形式编写的通用代码，都赋予你将几乎任何类型适配到预期接口的巨大能力。因此，你可以想象，shim 或函数适配器是泛型编程的骨干。

## 分析适配器设计模式的不足

尽管适配器设计模式很有价值，但我应该明确指出该设计模式的一个问题。考虑以下示例，我借鉴自 Eric Freeman 和 Elisabeth Robson：

