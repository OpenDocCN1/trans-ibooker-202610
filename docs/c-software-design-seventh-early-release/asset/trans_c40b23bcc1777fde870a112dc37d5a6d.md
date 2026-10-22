

![](img/7b1b8ee332752e5702cc436a0cba7c1c_387_0.png)

Figure 7-1. The UML representation of the basic Bridge design pattern.

虽然这为我们提供了轻松应用更改的能力，因此实现了 *Bridge* 的思想，但我们还可以再进一步，以进一步解耦并减少重复。假设我们感兴趣的不仅仅是电动汽车，还包括燃油发动机汽车。很自然地，对于我们计划实现的每一种汽车，我们都希望引入与发动机细节相同的解耦，即相同类型的 *Bridge*。因此，为了减少重复并遵循 *Don't Repeat Yourself (DRY)* 原则，我们可以将 *Bridge* 相关的实现细节提取到 `Car` 基类中（见图 7-2）：

![](img/7b1b8ee332752e5702cc436a0cba7c1c_389_0.png)

Figure 7-2. The UML representation of the full Bridge design pattern.

Car 基类封装了到关联 Engine 的 Bridge：

```
//---- <Car.h> ----------------

#include <Engine.h>
#include <memory>
#include <utility>

class Car
{
protected:
    explicit Car( std::unique_ptr<Engine> engine )  ⑦
        : pimpl_( std::move(engine) )
    {}

public:
    virtual ~Car() = default;
    virtual void drive() = 0;
    // ... more car-specific functions

protected:
    Engine*        getEngine()      { return pimpl_.get(); }  ⑨
    Engine const* getEngine() const { return pimpl_.get(); }

private:
    std::unique_ptr<Engine> pimpl_;  // Pointer-to-implementation
    (pimpl)  ⑧

    // ... more car-specific data members (wheels, drivetrain,
    ...)
};
```

有了 Car 类的加入，“抽象”和“实现”都提供了易于扩展的机会，并且可以独立变化。虽然 Engine 基类仍然代表此 Bridge 关系中的“实现”，但 Car 类现在扮演“抽象”的角色。关于 Car 类值得注意的第一点是受保护的构造函数（⑦）。这一选择确保只有派生类才能指定发动机类型。构造函数接受一个指向 Engine 的 std::unique_ptr 并将其移动到其 pimpl_ 数据成员（⑧）。这个指针数据成员是所有种类 Car 的指针到实现，通常称为 pimpl。这个不透明指针代表了到封装实现细节的 Bridge，因此本质上代表了整个 Bridge 设计模式。基于此原因，在代码中使用名称 pimpl 来表明你的意图是一个好主意（记住“Guideline 14: Use a Design Pattern’s Name to Communicate Intent”）。

请注意，尽管派生类必须使用它，pimpl_ 仍声明在类的私有部分。这一选择由 Core Guideline C.133 驱动：

> Avoid protected data.
> 
> —Core Guideline C.133

事实上，经验表明受保护的数据成员几乎与公共数据成员一样糟糕。因此，为了授予对 pimpl 的访问权限，Car 类改为提供受保护的 getEngine() 成员函数 ⑨。

ElectricCar 类相应地进行了调整：

```
//---- <ElectricCar.h> ----------------

#include <Engine.h>
#include <memory>

class ElectricCar : public Car  ①
{
public:
    explicit ElectricCar( /*maybe some engine arguments*/ );

    void drive() override;
    // ...
};
```

```
//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>
#include <ElectricEngine.h>
```

```
ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : Car( std::make_unique<ElectricEngine>( /*engine arguments*/
) )
{}

// ...
```

与其自行实现 *Bridge*，`ElectricCar` 类现在继承自 `Car` 基类 ①。这种继承关系引入了通过指定 `Engine` 来初始化 `Car` 基类的需求。此任务在 `ElectricCar` 构造函数中执行 ①。

## The Pimpl Idiom

*Bridge* 设计模式有一个更简单的形式，在 C 和 C++ 中已被非常普遍和成功地使用数十年。要看一个例子，让我们考虑以下 `Person` 类：

```
class Person
{
public:
    // ...
    int year_of_birth() const;
    // ... Many more access functions

private:
    std::string forename_;
    std::string surname_;
    std::string address_;
    std::string city_;
    std::string country_;
    std::string zip_;
    int year_of_birth_;
    // ... Potentially many more data members
};
```

一个人包含大量数据成员：`forename`、`surname`、完整的邮寄地址、`year_of_birth`，以及可能还有更多。而且未来可能需要添加更多数据成员：手机号码、Twitter 账号，或下一个最热门社交媒体的账号信息。换句话说，很合理地认为 Person 类需要随着时间的推移而扩展或更改，可能甚至频繁更改。这会给 Person 的使用者带来很多不便：每当 Person 发生变化时，Person 的使用者都必须重新编译他们的代码。更不用说 ABI 稳定性：Person 实例的大小将会改变！

为了隐藏对 Person 实现细节的所有更改并获得 ABI 稳定性，你可以使用 Bridge 设计模式。在这种特定情况下，然而，并不需要以基类的形式提供抽象：Person 只有一个且唯一一个实现。因此，我们所做的只是引入一个私有、嵌套的类，称为 Impl（⓪）：

```
//---- <Person.h> ----------------

#include <memory>

class Person
{
public:
    // ...

private:
    struct Impl;
    std::unique_ptr<Impl> const pimpl_;
};
```

```
//---- <Person.cpp> ----------------

#include <Person.h>
#include <string>

struct Person::Impl
{
    std::string forename;
    std::string surname;
    std::string address;
    std::string city;
    std::string country;
    std::string zip;
    int year_of_birth;
```

```
// ... Potentially many more data members
};
```

嵌套 Impl 类的唯一任务是封装 Person 的实现细节。因此，Person 类中剩下的唯一数据成员是对 Impl 实例的 std::unique_ptr（⑧）。所有其他数据成员，以及可能的一些非虚辅助函数，都从 Person 类移入 Impl 类。请注意，Impl 类仅在 Person 类中声明，但未定义。相反，它在对应的源文件中定义（⑨）。正因如此，你对细节所做的所有更改，即添加或删除数据成员、更改数据成员的类型等，都对 Person 的使用者隐藏。

这个 Person 的实现使用了 Bridge 设计模式的最简单形式：这种本地的、非多态的 Bridge 形式称为 Pimpl Idiom。它具有 Bridge 模式的所有解耦优势，但尽管其简单，仍然导致 Person 类的实现变得稍微复杂一些：

```
//---- <Person.h> --------------------

// #include <memory>

class Person
{
public:
    // ...
    Person();      ⑮
    ~Person();      ⑯

    Person( Person const& other );      ⑰
    Person& operator=( Person const& other );      ⑱

    Person( Person&& other );      ⑲
    Person& operator=( Person&& other );      ⑳

    int year_of_birth() const;      ㉑
    // ... Many more access functions

private:
    struct Impl;
    std::unique_ptr<Impl> const pimpl_;
};

//---- <Person.cpp> ----------------

//#include <Person.h>
//#include <string>

struct Person::Impl
{
    // ...
};

Person::Person()  ⑮
    : pimpl_{ std::make_unique<Impl>() }
{}

Person::~Person() = default;  ⑯

Person::Person( Person const& other )  ⑰
    : pimpl_{ std::make_unique<Impl>(*other.pimpl_) }
{}

Person& Person::operator=( Person const& other )  ⑱
{
    *pimpl_ = *other.pimpl_;
    return *this;
}

Person::Person( Person&& other )  ⑲
    : pimpl_{ std::make_unique<Impl>(std::move(*other.pimpl_)) }
{}

Person& Person::operator=( Person&& other )  ⑳
{
    *pimpl_ = std::move(*other.pimpl_);
    return *this;
}

int Person::year_of_birth() const  ㉑
{
    return pimpl_->year_of_birth;
}

// ... Many more Person member functions
```

