
//---- <Duck.h> ----------------

class Duck
{
public:
    virtual ~Duck() = default;
    virtual void quack() = 0;
    virtual void fly() = 0;
};


//---- <MallardDuck.h> ----------------

#include <Duck.h>

class MallardDuck : public Duck
{
public:
    void quack() override { /*...*/ }
    void fly() override { /*...*/ }
};

我们从抽象的 Duck 类开始，它引入了两个纯虚函数 quack() 和 fly()。的确，这看起来是一个相当预期且自然的 Duck 类接口，并且当然会产生一些期望：鸭子发出非常特征性的声音并且能很好地飞行。这个接口被许多可能的 Duck 类型实现，例如 MallardDuck 类。现在，出于某种原因，我们还必须处理火鸡：

//---- <Turkey.h> ----------------

class Turkey
{
public:
    virtual ~Turkey() = default;
    virtual void gobble() = 0;  // Turkeys don't quack, they gobble!
    virtual void fly() = 0;     // Turkeys can fly (a short distance)
};


//---- <WildTurkey.h> ----------------

class WildTurkey : public Turkey
{
public:
    void gobble() override { /*...*/ }
    void fly() override { /*...*/ }
};

火鸡由抽象的 Turkey 类表示，当然它被许多不同种类的具体火鸡实现，例如 WildTurkey。事情变得更糟的是，出于某种原因，鸭子和火鸡被期望一起使用。⁷ 使其工作的一种可能方式是假装火鸡是鸭子。毕竟，火鸡与鸭子非常相似。好吧，它不会嘎嘎叫，但它可以咕咕叫（典型的火鸡声音），并且它也可以飞行（不是很远，但确实可以飞行）。因此，你可以做的是，通过 TurkeyAdapter 将火鸡适配为鸭子：

//---- <TurkeyAdapter.h> -------------

#include <memory>

class TurkeyAdapter : public Duck
{
public:
    explicit TurkeyAdapter( std::unique_ptr<Turkey> turkey )
        : turkey_{ std::move(turkey) }
    {}

    void quack() override { turkey_->gobble(); }
    void fly() override { turkey_->fly(); }

private:
    std::unique_ptr<Turkey> turkey_;  // This is an example for an object adapter
};

虽然这是对鸭子类型的有趣解释，但这个例子很好地证明了，将外来事物集成到现有层次结构中是多么容易。Turkey 根本就不是 Duck，即使我们希望它成为。 我认为很可能 quack() 和 fly() 函数都违反了 LSP。 这两个函数实际上并没有做我期望它们做的事情（至少我相当确定我想要一只嘎嘎叫的，而不是咕咕叫的生物，并且我想要一只能够像鸭子一样真正飞行的生物）。 当然这取决于具体的上下文，但不可否认的是，适配器设计模式使得将不属于一起的东西组合起来变得非常容易。 因此，在应用此设计模式时，考虑预期的行为并检查 LSP 违规非常重要：

#include <MallardDuck.h>
#include <WildTurkey.h>
#include <TurkeyAdapter.h>
#include <memory>
#include <vector>

using DuckChoir = std::vector<std::unique_ptr<Duck>>;

void give_concert( DuckChoir const& duck_choir )
{
    for( auto const& duck : duck_choir ) {
        duck->quack();
    }
}

int main()
{
    DuckChoir duck_choir{};

    // Let us hire the world's best ducks for the choir
    duck_choir.push_back( std::make_unique<MallardDuck>() );
    duck_choir.push_back( std::make_unique<MallardDuck>() );
    duck_choir.push_back( std::make_unique<MallardDuck>() );

    // Unfortunately we also hire a turkey in disguise
    auto turkey = std::make_unique<WildTurkey>();
    auto turkey_in_disguise = std::make_unique<TurkeyAdapter>(
    std::move(turkey) );
    duck_choir.push_back( std::move(turkey_in_disguise) );

    // The concert is going to be a musical disaster...
    give_concert( duck_choir );

    return EXIT_SUCCESS;
}

总之，*Adapter* 设计模式可以被认为是最有价值的设计模式之一，用于组合不同的功能片段并使它们协同工作。我保证它将在你的日常工作中证明是有价值的工具。 尽管如此，不要在一些英雄式的努力中滥用 *Adapter* 的强大功能来组合苹果和橙子（甚至橙子和葡萄柚：它们相似，但并不相同）。 始终注意 *LSP* 的期望。

## GUIDELINE 24: USE *ADAPTERS* TO STANDARDIZE INTERFACES

- Apply the *Adapter* design pattern with the intent to adapt interfaces, so that otherwise incompatible pieces can work together;
- Be aware that *Adapter* is useful for both dynamic and static polymorphism;
- Distinguish between *object adapters*, *class adapters*, and *function adapters*;
- Understand the differences between the *Adapter* and *Strategy* design pattern;
- Pay attention to *LSP* violations when using the *Adapter* design pattern.

## Guideline 25: Apply Observers as an Abstract Notification Mechanism

很有可能你以前听说过观察者。 “哦，是的当然，我不是听说所谓的社交媒体平台就是用它来对付我们的吗？” 你在问。 好吧，不完全是我想要的，但我相信我们可以称这些平台为观察者。 而且，是的，他们所做的事情也有一种模式，即使它不是设计模式。 但我实际上是在想 GoF 最受欢迎的设计模式之一，*Observer* 设计模式。 即使你还不熟悉这个想法，你很可能已经有过来自现实生活的 helpful observers 的一些经验。 例如，你可能已经注意到在你最喜欢的即时通讯应用中，文本消息的发送者在你阅读新文本消息后会立即收到通知。 这意味着消息显示为“已读”而不是仅仅“已送达”。 这项小服务本质上是现实生活中的 *Observer* 的工作：只要新消息的状态发生变化，发送者就会收到通知，从而有机会对状态变化做出响应。

### The Observer Design Pattern Explained

在许多软件情况下，在发生某些状态变化时获得反馈是可取的：任务队列中添加了新作业，配置对象中的设置发生了更改，结果已准备好被提取，等等。 但同时，在主题（即发生变化的被观察实体）和其观察者（即基于状态变化而被通知的回调）之间引入显式依赖关系将是极其不可取的。 相反：主题应该对可能存在的许多不同类型的观察者一无所知。 这仅仅是因为任何直接依赖关系都会使软件更难更改和扩展。 主题与其潜在的众多观察者之间的这种解耦是 *Observer* 设计模式的意图：

> **THE *OBSERVER* DESIGN PATTERN**
> 
> Intent: “Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.” (GoF)

与所有设计模式一样，*Observer* 设计模式将一个方面识别为 *variation point*（即发生变化或预期发生变化的方面）并以抽象的形式提取它。 因此它有助于解耦软件实体。 在 *Observer* 的情况下，引入新观察者的需要，将一对多依赖扩展的需要，被认为是 *variation point*。 如图 6-2 所示，此 *variation point* 以 `Observer` 基类的形式实现：

Figure 6-2. The UML representation of the Observer design pattern.

Observer 类代表所有可能的观察者实现的抽象。 这些观察者附加到特定的主题，由 ConcreteSubject 类表示。 为了减少观察者与其主题之间的耦合，或者为了通过为不同观察者提供 attach() 和 detach() 的所有通用服务来简单地减少代码重复，可以使用 Subject 抽象。 这个 Subject 还可以 notify() 所有附加的观察者关于状态变化并触发它们相应的 update() 功能。

“引入 Observer 基类不是另一个单一职责原则（SRP）的例子吗？” 你问。 而且是的，你是 100% 正确的：提取 Observer 类，提取一个变化点，就是 SRP 在行动（参见“Guideline 2: Design for Change”）。 再次，SRP 充当开放封闭原则（OCP）的使能器（参见“Guideline 5: Design for Extension”）：通过引入 Observer 抽象，任何人都可以添加新类型的观察者（例如 ConcreteObserver）而无需修改现有代码。 如果你注意 Observer 基类的所有权并确保 Observer 类位于架构的高层，那么你也满足了依赖倒置原则（DIP）。

### A Classic Observer Implementation

“太好了，我明白了！ 看到这些设计原则再次付诸实践很不错，但我想看一个具体的 Observer 示例。” 我理解。 所以让我们看一个具体的实现。 然而，在我们开始查看代码之前，我应该在开始之前明确说明以下示例的局限性。 你可能已经熟悉 Observer，因此你可能正在寻找有关 Observer 许多棘手实现细节的帮助和更深入的建议：如何处理附加和分离观察者的顺序，如何处理多次附加同一个观察者，以及特别是在并发环境中如何处理观察者。 我应该诚实地提前说明，我无意提供这些问题的答案。 这种讨论就像打开了一个潘多拉魔盒，很快就会把我们吸入实现细节的领域。 不，虽然你

