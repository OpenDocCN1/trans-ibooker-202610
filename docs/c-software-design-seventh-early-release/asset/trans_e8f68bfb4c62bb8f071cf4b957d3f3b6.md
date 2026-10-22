

template< typename T >
using Meter = StrongType<T,struct MeterTag>;

template< typename T >
using Kilometer = StrongType<T,struct KilometerTag>;

// ...

//---- <Person.h> ----------------

#include <StrongType.h>

using Surname = StrongType<std::string,struct SurnameTag>;

// ...

使用别名模板 Meter 和 Kilometer 使你能够选择例如 long 或 double 来表示距离。然而，尽管这些类型基于基本类型或标准库类型构建，如 Surname 情况中的 std::string，它们代表的是不同的类型（即强类型），具有语义含义，不能（意外地）在诸如加法之类的算术运算中组合：

```
//---- <Main.cpp> ----------------

#include <Distances.h>
#include <cstdlib>

int main()
{
    auto const m1 = Meter<long>{ 120L };
    auto const m2 = Meter<long>{  50L };
    auto const km = Kilometer<long>{ 30L };
    auto const surname1 = Surname{ "Stroustrup" };
    auto const surname2 = Surname{ "Iglberger" };
    // ...

    m1 + km;        // 正确地无法编译！ ①
    surname1 + surname2;  // 也正确地无法编译！ ②
    m1 + m2;        // 不便的是这也无法编译
```

either. ③
    return EXIT_SUCCESS;
}

尽管 Meter 和 Kilometer 都是通过 long 表示的，但 Meter 和 Kilometer 之间不能直接相加 ①。这非常好：它不会留下任何让意外错误潜入的空隙。同样也不能相加两个 Surname，尽管 std::string 本身提供了用于字符串连接的加法运算符 ②。但这也很好：强类型有效地限制了底层类型不期望的操作。不幸的是，这个“特性”也阻止了两个 Meter 实例的相加 ③。然而这个操作是期望的：它是直观的、自然的，而且由于操作的结果仍将是 Meter 类型，因此在物理上是准确的。为了使其工作，我们可以为 Meter 类型实现一个加法运算符。然而，显然，这不会是唯一的加法运算符。我们还需要为所有其他强类型提供，例如 Kilometer、Mile、Foot 等。由于所有这些实现看起来都一样，这将违反不要重复自己（DRY）原则。因此，将 StrongType 类模板扩展为加法运算符似乎是合理的：

```
template< typename T, typename Tag >
StrongType<T,Tag>
operator+( StrongType<T,Tag> const& a, StrongType<T,Tag> const& b )
{
    return StrongType<T,Tag>( a.get() + b.get() );
}
```

由于这个加法运算符的表述方式，不可能将 StrongType 的两个不同实例化相加（例如 Meter 和 Kilometer），但它将允许相加 StrongType 的同一实例化的两个实例。“哦，但我看到一个问题：虽然现在可以相加两个 Meter 或两个 Kilometer，但也可以相加两个 Surname。我们不希望这样！”你是对的：这

将是不期望的。我们真正需要的是对 StrongType 的特定实例化有意地添加操作。这就是 CRTP 发挥作用的地方。

## 使用 CRTP 作为实现模式

我们将不会直接为 StrongType 类模板配备操作，而是通过 mixin 类提供操作：作为“注入”所需操作的基类。这些 mixin 类将以 CRTP 的形式实现。例如 Addable 类模板，它表示加法操作：

```
//---- <Addable.h> ----------------

template< typename Derived >
struct Addable
{
    friend Derived& operator+=( Derived& lhs, Derived const& rhs )
    {
        ④
        lhs.get() += rhs.get();
        return lhs;
    }

    friend Derived operator+( Derived const& lhs, Derived const& rhs ) {
        ⑤
        return Derived{ lhs.get() + rhs.get() };
    }
};
```

模板参数的名称已经说明了一切：Addable 是一个 CRTP 基类。Addable 只提供两个函数，作为隐藏友元实现：一个加法赋值运算符 ④ 和一个加法运算符 ⑤。两个运算符都是为指定的 Derived 类型定义的，并将被注入到周围的命名空间中。20 因此，任何从该 CRTP 基类派生的类都将“继承”两个自由加法运算符：

```
//---- <StrongType.h> ----------------

#include <stdlib>
```

```
#include <utility>

template< typename T, typename Tag >
struct StrongType : private Addable< StrongType<T,Tag> >
{ /* ... */ };

//---- <Distances.h> ----------------

#include <StrongType.h>

template< typename T >
using Meter = StrongType<T,struct MeterTag>;

// ...

//---- <Main.cpp> -------------------

#include <Distances.h>
#include <cstdlib>

int main()
{
    auto const m1 = Meter<long>{ 100 };
    auto const m2 = Meter<long>{ 50 };

    auto const m3 = m1 + m2;  // 编译通过且结果为 150 米
    // ...

    return EXIT_SUCCESS;
}
```

“我理解 mixin 类的目的，但在此形式下，所有 StrongType 的实例化都会继承加法运算符，即使在不需要加法的地方也是如此，对吧？” 是的，确实如此。因此我们还没有完成。我们想要做的是选择性地将 mixin 类添加到需要该操作的那些 StrongType 实例化中。我们选择的解决方案是以可选的模板参数形式提供 mixins。为此，我们通过一组可变参数模板模板参数扩展 StrongType 类模板：²¹

```
//---- <StrongType.h> ----------------

#include <utility>

template< typename T, typename Tag, template<typename> class... Skills >
struct StrongType
  : private Skills< StrongType<T,Tag,Skills...> >...
{ /* ... */ };
```

此扩展使我们能够为每个单独的强类型单独指定所需的技能。例如下面两个额外的技能 Printable 和 Swappable：

```
//---- <Printable.h> ----------------

template< typename Derived >
struct Printable
{
  friend std::ostream& operator<<( std::ostream& os, const Derived& d )
  {
    os << d.get();
    return os;
  }
};
```

```
//---- <Swappable.h> ----------------

template< typename Derived >
struct Swappable
{
  friend void swap( Derived& lhs, Derived& rhs )
  {
    using std::swap;  // 启用 ADL
    swap( lhs.get(), rhs.get() );
  }
};
```

与 Addable 技能一起，我们现在可以组装出配备所需和期望技能的强类型：

```
//---- <Distances.h> ----------------

#include <StrongType.h>

template< typename T >
using Meter =
    StrongType<T,struct MeterTag,Addable,Printable,Swappable>; ⑥

template< typename T >
using Kilometer =
    StrongType<T,struct KilometerTag,Addable,Printable,Swappable>;
⑦

// ...

//---- <Person.h> --------------------

#include <StrongType.h>
#include <string>

using Surname =
    StrongType<std::string,struct SurnameTag,Printable,Swappable>;
⑧

// ...
```

Meter 和 Kilometer 都可以相加、打印和交换（参见 ⑥ 和 ⑦），而 Surname 可打印、可交换，但不可相加（即未接收 Addable mixin，因此未从中派生）（⑧）。

“太好了。我理解了在此上下文中 CRTP mixin 类的目的。但这种 CRTP 示例与之前的示例有何不同？” 非常好的问题。你是对的，实现细节非常相似。但有几个显著的区别：注意 CRTP 基类不提供虚或受保护的析构函数。因此，与之前的示例不同，它不是设计为多态基类。还要注意在此示例中，将 CRTP 基类用作私有基类是足够且更可取的，而不是公有基类（⑨）。

因此在此上下文中，*CRTP* 基类不代表抽象，而只是一个实现细节。因此 *CRTP* 不具备设计模式的属性，也不作为设计模式发挥作用。它仍然是一个模式，无可置疑，但在这种情况下它仅作为实现模式发挥作用。

*CRTP* 示例在实现上的主要区别在于我们使用继承的方式。对于 *CRTP* 设计模式，我们使用继承来实现根据 *LSP* 的抽象：基类代表派生类的需求以及因此可用的和期望的行为。用户代码直接通过指向基类的指针或引用访问操作，这就要求我们提供 `virtual` 或 `protected` 析构函数。以这种方式实现时，*CRTP* 成为软件设计的一个真正元素，即一种设计模式。

相比之下，对于 *CRTP* 实现模式，我们使用继承来实现技术上的优雅和便利。基类成为实现细节，不必被调用代码所知或使用。因此它不需要 `virtual` 或 `protected` 析构函数。以这种方式实现时，*CRTP* 停留在实现细节的层面上，因此是一种实现模式。在这种形式下，*CRTP* 并不会与 C++20 概念竞争。相反：在这种形式下 *CRTP* 无可匹敌，因为它代表了一种提供静态 mixin 功能的独特技术。因此，*CRTP* 至今仍有价值，是每个 C++ 开发者工具箱中的宝贵补充。

总之，*CRTP* 并未过时，但其价值已经改变。在 C++20 中，*CRTP* 被概念取代，因此作为设计模式的地位下降。然而，它作为 mixin 类的实现模式仍然很有价值。

