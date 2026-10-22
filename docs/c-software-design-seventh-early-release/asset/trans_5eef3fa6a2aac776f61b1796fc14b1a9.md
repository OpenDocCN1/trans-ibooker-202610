

此外，你已经看到了实现细节并意识到代码复杂度有所增加。由于代码的简洁性和可读性是一种美德，这应该被视为一个缺点。确实，这只会影响类的内部，而不会影响用户代码。但即便如此，其中一些细节（例如需要在源文件中定义析构函数）可能会让经验不足的开发者感到困惑。

总之，*Bridge* 设计模式是减少物理依赖的最有价值且最常用的解决方案之一。尽管如此，你应该意识到 *Bridge* 带来的开销和复杂性。

## GUIDELINE 28: BUILD *BRIDGES* TO REMOVE PHYSICAL DEPENDENCIES

- 注意数据成员或包含引入的物理依赖；
- 采用 *Bridge* 设计模式的意图是将物理依赖与实现细节隔离开来；
- 优先使用 *pimpl* 数据成员来传达使用 *Bridge* 的意图；
- 了解 *Bridge* 设计模式的优势，但也要了解其弱点；
- 知道减少物理依赖（*Bridge*）与逻辑依赖（*Strategy*）之间的区别。

## Guideline 29: Be Aware of the Performance Gains and Losses of Bridges

在“Guideline 28: Build Bridges to Remove Physical Dependencies”中，我们详细研究了 *Bridge* 设计模式。虽然我想象 *Bridge* 的设计和解耦方面给你留下了积极的印象，但我必须让你意识到使用这种模式可能会带来性能损失。“是的，这让我担心。性能对我很重要，听起来 Bridge 会造成巨大的性能开销。”，你说道。这是一个相当常见的预期。所以既然性能很重要，我确实应该给你一个使用 Bridge 时需要预期多少开销的想法。然而，我也应该演示如何明智地使用 Bridges 甚至提高代码的性能。听起来不可思议？好吧，让我来展示给你看。

### The Performance Impact of Bridges

如“Guideline 28: Build Bridges to Remove Physical Dependencies”中所讨论的，Bridge 实现的性能受许多因素影响：通过间接访问、虚函数调用、内联、动态内存分配等。由于所有这些因素以及可能的组合数量巨大，无法给出 Bridge 会让你付出多少性能的明确答案。根本没有捷径，没有替代方案可以替代为你自己的代码组装几个基准测试并运行它们以评估明确答案。我想演示的，不过是确实存在通过间接访问的性能损失，但你仍然可以使用 Bridge 来实际提高性能。

那么，让我们开始给你一个关于基准测试的想法。为了形成对指针间接访问有多昂贵的看法，让我们比较以下两种 Person 类的实现：

```
#include <string>

//---- <Person1.h> ----------------

class Person1
{
public:
    // ...
private:
    std::string forename_;
    std::string surname_;
```

```
std::string address_;
std::string city_;
std::string country_;
std::string zip_;
int year_of_birth_;
};
```

Person1 结构体表示一个未使用 Bridge 实现的类型。所有七个数据成员（六个 std::string 和一个 int）都是结构体本身的直接组成部分。总共，假设 64 位机器，使用 Clang 11.1 时一个 Person1 实例的总大小为 152 字节，使用 GCC 11.1 时为 200 字节。⁶

另一方面，Person2 结构体是通过 Pimpl 惯用法实现的：

```
//---- <Person2.h> ----------------

#include <memory>

class Person2
{
public:
    explicit Person2( /*...various person arguments...*/ );
    ~Person2();
    // ...

private:
    struct Impl;
    std::unique_ptr<Impl> pimpl_;
};
```

```
//---- <Person2.cpp> ----------------

#include <Person2.h>
#include <string>

struct Person2::Impl
{
    std::string forename;
    std::string surname;
    std::string address;
    std::string city;
```

```
std::string country;
std::string zip;
int year_of_birth;
};

Person2::Person2( /*...various person arguments...*/ )
  : pimpl{ std::make_unique<Impl>( /*...various person arguments...*/ ) }
{}

Person2::~Person2() = default;
```

所有七个数据成员都已移入嵌套的 Impl 结构体中，因此只能通过 pimpl 指针访问。虽然嵌套 Impl 结构体的总大小与 Person1 的大小相同，但 Person2 结构体的大小仅为 8 字节（同样假设 64 位机器）。

### NOTE

通过 Bridge 设计，你可以减小类型的大小，有时甚至显著减小。这可能非常有价值，例如如果你想将该类型用作 std::variant 中的替代项（参见“Guideline 17: Consider std::variant for Implementing Visitors”）。

那么让我概述一下基准测试：我将创建两个包含 25,000 个人员的 std::vectors，每个 Person 实现各一个。这个元素数量将确保我们的工作超出底层 CPU 内部缓存的大小（即我们将使用 Clang-11.1 时总共 3.2MB，GCC-11.1 时 4.2MB）。所有这些人员都拥有任意的名字和地址，但出生年份也在 1957 年至 2004 年之间（在撰写本文时，这将代表一个组织员工的合理年龄范围）。然后我们将遍历这两个人员向量 5000 次，每次通过 std::min_element() 确定最年长的人员。计算结果由于基准测试的重复性而相当无趣。最晚在 100 次迭代后你就会感到厌倦。唯一重要的是，看到直接访问数据成员（`Person1`）与间接访问（`Person2`）之间的性能差异。表 7-1 显示了归一化到 `Person1` 实现性能的性能结果：

### Table 7-1. Performance results for different Person implementations

| | GCC 11.1 | Clang 11.1 |
|---|---|---|
| Person1 (No pimpl) | 1.0 | 1.0 |
| Person2 (Complete *Pimpl Idiom*) | 1.1099 | 1.1312 |

很明显，在这个特定基准测试中，*Bridge* 实现带来了相当显著的性能损失：GCC 为 11.0%，Clang 为 13.1%。这听起来很多！然而，不要太认真对待这些数字：显然结果在很大程度上取决于实际的元素数量、实际的数据成员数量和类型、我们运行的系统，以及基准测试中实际执行的计算。如果你改变其中任何一个细节，数字也会改变。因此，这些数字只证明了由于对数据成员的间接访问而存在一些，甚至可能更多的开销。

### Improving Performance with Partial Bridges

“好吧，但这是一个预期的结果，对吧？我应该从中学习什么？”，你在问。好吧，我承认这个基准测试相当具体，并不能回答所有问题。然而，它确实为我们提供了实际使用 *Bridge* 来提高性能的机会。如果你仔细查看 `Person1` 的实现，你可能会意识到对于给定的基准测试，可实现的性能相当有限：虽然 `Person1` 的总大小为 152 字节（Clang-11.1）或 200 字节（GCC-11.1），但我们只使用了 4 字节，即一个 `int`。这被证明是相当浪费和低效的：由于在基于缓存的架构上，内存总是以缓存行的形式加载，我们从内存中加载的大量数据实际上根本没有被使用。事实上，我们从内存中加载的几乎 *所有* 数据都没有被使用：假设缓存行长度为 64 字节，我们只使用了大约 6% 的加载数据。因此，尽管我们根据所有人员的出生年份确定最年长的人员，这听起来像是一个计算密集型操作，但我们实际上完全受内存限制：机器根本无法足够快地提供数据，整数单元大部分时间处于空闲状态。

