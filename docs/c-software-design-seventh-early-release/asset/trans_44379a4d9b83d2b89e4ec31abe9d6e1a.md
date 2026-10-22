

设计模式恰恰相反：它帮助你减少物理依赖，促进解耦，即让两个需要协同工作但不应了解彼此太多细节的功能保持距离。

## 一个激励性的示例

为了说明我的想法，请考虑以下 ElectricCar 类：

//---- <ElectricEngine.h> ----------------

class ElectricEngine
{
public:
    void start();
    void stop();

private:
    // ...
};

//---- <ElectricCar.h> ----------------

#include <ElectricEngine.h>
// ...

class ElectricCar
{
public:
    ElectricCar( /*maybe some engine arguments*/ );

    void drive();
    // ...
private:
    ElectricEngine engine_;  ①

    // ... more car-specific data members (wheels, drivetrain, ...)
};
```

```
//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
  : engine_{ /*engine arguments*/ }
  // ... Initialization of the other data members
{}
```

// ...

如名称所示，ElectricCar 类配备了 ElectricEngine（1）。然而，现实中这样的汽车可能相当有吸引力，但当前实现细节令人担忧：由于 engine_ 数据成员，<ElectricCar.h> 头文件需要包含 <ElectricEngine.h> 头文件。编译器需要看到 ElectricEngine 的类定义，因为否则它将无法确定 ElectricCar 实例的大小。包含 <ElectricEngine.h> 头文件很容易导致传递的物理耦合：每个包含 <ElectricCar.h> 头文件的<|message|>文件都会在物理上依赖于 <ElectricEngine.h> 头文件。因此，只要头文件中的内容发生变化，ElectricCar 类以及可能许多其他类都会受到影响。它们可能需要重新编译、重新测试，在最坏的情况下甚至需要重新部署……唉。

除此之外，这种设计会向所有人暴露所有实现细节。“你是什么意思？类的私有部分隐藏和封装实现细节难道不是重点吗？” 是的，它可能是私有的，但 private 标签仅仅是一个访问标签。它不是可见性标签。因此，你类定义中的所有内容（我确实是说所有内容）对看到 ElectricCar 类定义的人都是可见的。这意味着，你无法在不被任何人注意到的情况下更改该类的实现细节。特别是，如果你需要提供 ABI 稳定性，即你的类的内存表示不得更改，这可能是一个问题。¹

一个稍微好一点的方法是只存储指向 ElectricEngine 的指针（2）：²

```
//---- <ElectricCar.h> ----------------

#include <memory>
// ...
struct ElectricEngine;  // Forward declaration

class ElectricCar
{
public:
    ElectricCar( /*maybe some engine arguments*/ );

    void drive();
    // ...
private:
    std::unique_ptr<ElectricEngine> engine_;  2

    // ... more car-specific data members (wheels, drivetrain,
    ...)
};

//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>
#include <ElectricEngine.h>  3

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : engine_{ std::make_unique<ElectricEngine>( /*engine
arguments*/ ) }
    // ... Initialization of the other data members
{}

// ... Other 'ElectricCar' member functions, using the pointer to an
//     'ElectricEngine'.

```

在这种情况下，只需提供 ElectricEngine 类的前向声明就足够了，因为编译器不需要知道类定义就能确定 ElectricCar 实例的大小。此外，物理依赖也消失了，因为

<ElectricEngine.h> 头文件已移至源文件 ③。因此，从依赖关系的角度来看，这个解决方案要好得多。仍然存在的问题是实现细节的可见性。每个人仍然能够看到 ElectricCar 基于 ElectricEngine 构建，因此每个人仍然隐式依赖于这些实现细节。因此，对这些细节的任何更改，例如升级到新的 PowerEngine，都会影响使用 <ElectricCar.h> 头文件的任何类。“这不好，对吧？” 确实不好，因为变化是必然的（参见“准则 2：为变化而设计”）。为了摆脱这种依赖，并在任何时候轻松更改实现细节而不被任何人察觉，我们必须引入抽象。抽象的经典形式是引入抽象类：

//---- <Engine.h> ----------------

class Engine ④
{
public:
    virtual ~Engine() = default;
    virtual void start() = 0;
    virtual void stop() = 0;
    // ... more engine-specific functions

private:
    // ...
};
```

```
//---- <ElectricCar.h> ----------------

#include <Engine.h>
#include <memory>

class ElectricCar
{
public:
    void drive();
    // ...
private:
    std::unique_ptr<Engine> engine_;  ⑤
```

```
// ... more car-specific data members (wheels, drivetrain,
...)
};

//---- <ElectricEngine.h> ----------------

#include <Engine.h>

class ElectricEngine : public Engine
{
public:
    void start() override;
    void stop() override;

private:
    // ...
};

//---- <ElectricCar.cpp> ----------------

#include <ElectricCar.h>
#include <ElectricEngine.h>

ElectricCar::ElectricCar( /*maybe some engine arguments*/ )
    : engine_{ std::make_unique<ElectricEngine>( /*engine arguments*/ ) } ⑤
    // ... Initialization of the other data members
{}

// ... Other 'ElectricCar' member functions, primarily using the
'Engine'
//      abstraction, but potentially also explicitly dealing with
an
//      'ElectricEngine'.
```

有了 Engine 基类 ④，我们就可以使用该抽象来实现我们的 ElectricCar 类 ⑤。没有人需要知道我们使用的引擎的实际类型。也没有人需要知道我们何时升级引擎。通过这种实现，我们只需修改源文件 ⑥，就能随时轻松更改实现细节。因此，通过这种方法，我们真正最小化了对 ElectricEngine 实现的依赖。我们已将对该细节的了解变成了我们自己的秘密实现细节。通过这样做，我们建立了自己的 Bridge。

> NOTE
> 正如引言中所说，违反直觉的是，这个 Bridge 并不是为了让 ElectricCar 和 Engine 类更接近。相反，它是关于分离关注点和松耦合。另一个例子表明命名是困难的。

## Bridge 设计模式详解

Bridge 设计模式是 1994 年引入的经典 GoF 设计模式之一。Bridge 的目的是通过将某些实现细节封装在抽象之后来最小化物理依赖。因此，在 C++ 中，它充当编译防火墙，从而实现轻松更改：

> THE BRIDGE DESIGN PATTERN
> Intent: “Decouple an abstraction from its implementation so that the two can vary independently.” (GoF)

在这种意图表述中，四人帮谈到了“抽象”和“实现”。在我们的示例中，ElectricCar 类代表“抽象”，而 Engine 类代表“实现”（参见图 7-1）。两者都应能够独立变化，即对其中一方的更改不应影响另一方。阻碍轻松更改的是 ElectricCar 类与其引擎之间的物理依赖。因此，想法是将这些依赖提取并隔离：通过以 Engine 抽象的形式隔离它们，通过分离关注点并满足单一职责原则（SRP），你获得了以任何你想要的方式更改、调整或升级引擎的灵活性（参见“准则 2：为变化而设计”）。更改在 `ElectricCar` 类中不再可见。因此，现在可以轻松添加新的引擎类型，而“抽象”不会察觉到。这符合 *开放封闭原则（OCP）* 的思想（参见“准则 5：为扩展而设计”）。

