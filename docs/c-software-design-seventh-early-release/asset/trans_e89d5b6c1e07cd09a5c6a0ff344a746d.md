

## 装饰器设计模式

意图：“动态地向对象附加额外的职责。装饰器为扩展功能提供了一种灵活的替代子类化的方法。”（GoF）

图 9-5 显示了给定 Item 问题的 UML 图。如前所述，Item 基类表示所有可能项目的抽象。而派生的 CppBook 类则充当 Item 不同实现的代表。该层次结构中的问题是难以为现有的 price() 函数添加新的修饰符。在装饰器设计模式中，这种添加新的“职责”被识别为一个变化点，并以 DecoratedItem 类的形式提取出来。该类是 Item 基类的一个独立的、特殊的实现，表示对任何给定项目的附加职责。一方面，DecoratedItem 派生自 Item，因此必须遵守 Item 抽象的所有期望（参见“准则 6：遵守抽象的预期行为”）。另一方面，它也包含一个 Item（通过组合或聚合）。因此，DecoratedItem 充当每个项目的包装器，可能本身也可以扩展功能。因此，它为修饰符的层次化应用提供了基础。两种可能的修饰符由 Discounted 类表示，该类表示特定项目的折扣，以及 Taxed 类，该类表示某种税。⁴

![](img/7b1b8ee332752e5702cc436a0cba7c1c_538_0.png)

*图 9-5. 装饰器设计模式的 UML 表示。*

通过引入 `DecoratedItem` 类并分离需要更改的方面，你遵循了 *单一职责原则 (SRP)*。通过分离这一关注点并因此允许轻松添加新的价格修饰符，你也遵循了 *开放-封闭原则 (OCP)*。由于 `DecoratedItem` 类的层次化、递归性质，以及获得轻松重用和组合不同修饰符的能力，你也遵循了 *不要重复自己 (DRY)* 原则的建议。最后但同样重要的是，由于 *Decorator* 的包装器方法，没有必要以 *空对象* 的形式定义任何默认行为。任何不需要修饰符的 `Item` 都可以照常使用。

图 9-6 说明了 *Decorator* 设计模式的依赖图。在此图中，`Item` 类位于架构的最高层。所有其他类都依赖于它，包括位于其下一层的 `DecoratedItem` 类。当然这不是一个要求：如果 `Item` 和 `DecoratedItem` 在同一架构层引入也是完全可以接受的。然而，此示例演示了，任何时候、任何地方都可以引入新的 *Decorator* 而无需修改现有代码。`Items` 的具体类型在架构的最低层实现。注意，这些项目之间没有依赖关系：所有项目，包括 `Discounted` 等修饰符，都可以由任何人在任何时候独立引入，并且由于 *Decorator* 的结构，可以灵活地任意组合。

```
class Item
{
public:
    virtual ~Item() = default;
    virtual Money price() const = 0;
};
```

High-level

Architectural
Boundary

```
class DecoratedItem : public Item
{
public:
    explicit DecoratedItem( std::unique_ptr<Item>&& item );
    // ...
protected:
    // ... Access functions for item_
private:
    std::unique_ptr<Item> item_;
};
```

Architectural
Boundary

```
class CppBook : public Item
{
public:
    CppBook( std::string title, double price );
    Money price() const noexcept override;
private:
    // ...
};
```

```
class Discounted : public DecoratedItem
{
public:
    Discounted( std::unique_ptr<Item>&& item, double discount );
    Money price() const override;
private:
    // ...
};
```

Low-level

图 9-6. 装饰器设计模式的依赖图。

## 装饰器设计模式的经典实现

让我们通过给定的 Item 示例来看一下装饰器设计模式的完整、GoF 风格的实现：

```
//---- <Item.h> ----------------

#include <Money.h>

class Item
{
public:
    virtual ~Item() = default;
    virtual Money price() const = 0;
};
```

Item 基类表示所有可能项目的抽象。唯一的要求是由纯虚 price() 函数定义，可用于查询给定项目的价格。DecoratedItem 类表示 Item 类的一种可能实现（1）：

```
//---- <DecoratedItem.h> ----------------

#include <Item.h>
#include <memory>
#include <stdexcept>
#include <utility>

class DecoratedItem : public Item 1
{
public:
    explicit DecoratedItem( std::unique_ptr<Item> item ) 3
        : item_( std::move(item) )
    {
        if( !item_ ) {
            throw std::invalid_argument( "Invalid item" );
        }
    }

```

```
protected:
    Item& item() { return *item_; }
    Item const& item() const { return *item_; }

private:
    std::unique_ptr<Item> item_;
};
```

DecoratedItem 派生自 Item 类，但也包含一个 item_（2）。此 item_ 通过构造函数指定，该构造函数接受指向另一个 Item 的非空 std::unique_ptr（3）。请注意，此 DecoratedItem 类仍然是抽象的，因为纯虚 price() 函数尚未定义。DecoratedItem 仅提供存储 Item 并通过受保护成员函数访问 Item 的必要功能（4）。

配备了这两个类，可以实现具体的 Items：

```
//---- <CppBook.h> -------------

#include <Item.h>
#include <string>
#include <utility>

class CppBook : public Item  5
{
public:
    CppBook( std::string title, Money price )
        : title_{ std::move(title) }
        , price_{ price }
    {}

    std::string const& title() const { return title_; }
    Money price() const override { return price_; }

private:
    std::string title_{};
    Money price_{};
};
```

```
//---- <ConferenceTicket.h> ----------------

#include <Item.h>
#include <string>
#include <utility>

class ConferenceTicket : public Item ⑥
{
public:
    ConferenceTicket( std::string name, Money price )
    : name_{ std::move(name) }
    , price_{ price }
    {}

    std::string const& name() const { return name_; }
    Money price() const override { return price_; }

private:
    std::string name_{};
    Money price_{};
};
```

CppBook 和 ConferenceTicket 类表示可能的特定 Item 实现（⑤ 和 ⑥）。虽然 C++ 书籍通过书籍的标题表示，C++ 会议通过会议的名称表示。最重要的是，这两个类都通过返回指定的 price_ 来重写 price() 函数。

CppBook 和 ConferenceTicket 对任何税或折扣都一无所知。但显然，这两种 Item 都可能同时受其影响。这些价格修饰符由 Discounted 和 Taxed 类实现：

```
//---- <Discounted.h> ----------------

#include <DecoratedItem.h>

class Discounted : public DecoratedItem
{
public:
    Discounted( double discount, std::unique_ptr<Item> item )  ⑦
```

```
: DecoratedItem( std::move(item) )
, factor_( 1.0 - discount )
{
    if( !std::isfinite(discount) || discount < 0.0 || discount > 1.0 ) {
        throw std::invalid_argument( "Invalid discount" );
    }
}

Money price() const override
{
    return item().price() * factor_; ⑧
}

private:
    double factor_;
};
```

Discounted 类（⑦）通过传递指向 Item 的 std::unique_ptr 和一个折扣值进行初始化，折扣值由介于 0.0 和 1.0 之间的 double 值表示。虽然给定的 Item 立即传递给 DecoratedItem 基类，但给定的折扣值用于计算折扣因子 factor_。此因子在 price() 函数的实现中用于修改给定项目的价格（⑧）。请注意，此项目可以是 CppBook 或 ConferenceTicket 等特定项目，也可以是 Discounted 等装饰器，后者反过来修改另一个 Item 的价格。因此，price() 函数是装饰器层次结构完全被利用的地方。

```
//---- <Taxed.h> ----------------

#include <DecoratedItem.h>

class Taxed : public DecoratedItem
{
public:
    Taxed( double taxRate, std::unique_ptr<Item> item )  ⑨
    : DecoratedItem( std::move(item) )
    , factor_( 1.0 + taxRate )
    {
        if( !std::isfinite(taxRate) || taxRate < 0.0 ) {
```

