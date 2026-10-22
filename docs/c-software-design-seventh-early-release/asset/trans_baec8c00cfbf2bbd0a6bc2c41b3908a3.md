

可能会让你失望，我的意图是主要停留在软件设计的层面。⁸

与之前的设计模式一样，我们从 *Observer* 设计模式的经典实现开始。核心元素是 `Observer` 基类：

//---- <Observer.h> ----------------

class Observer
{
public:
    virtual ~Observer() = default;

    virtual void update( /*...*/ ) = 0;  ①
};
```

此类最重要的实现细节是纯虚 `update()` 函数（①），每当观察者被通知到某种状态变化时就会调用它。⁹ 定义 `update()` 函数有三种替代方案，它们提供了合理的实现和设计灵活性。第一种替代方案是通过一个甚至几个 `update()` 函数推送更新后的状态：

```
class Observer
{
public:
    // ...
    virtual void update1( /*arguments representing the updated state*/ ) = 0;
    virtual void update2( /*arguments representing the updated state*/ ) = 0;
    // ...
};
```

这种形式的观察者通常被称为 *push observer*。在此形式中，观察者由主题提供所有必要的信息，因此无需自行从主题中提取信息。这可以显著降低与主题的耦合，并创造出将 Observer 类用于多个主题的机会。此外，还可以选择为每种状态变化使用单独的重载。上面的代码片段中有两个 update() 函数，每个可能的两种状态变化各一个。而且由于总是清楚哪种状态发生了变化，观察者无需“搜索”任何状态变化，这被证明是高效的。

“请原谅”，你开始提问，“但这不是违反了接口隔离原则吗？我们不应该通过将 update() 函数分离到几个基类中来分离关注点吗？” 这是个好问题！显然你正在提防人为的耦合。非常好！而且你是对的：我们可以将具有多个 update() 函数的 Observer 分成更小的 Observer 类：

```
class Observer1
{
public:
    // ...
    virtual void update1( /*arguments representing the updated state*/ ) = 0;
    // ...
};

class Observer2
{
public:
    // ...
    virtual void update2( /*arguments representing the updated state*/ ) = 0;
    // ...
};
```

理论上，这种方法可能有助于减少对特定主题的耦合，并更轻松地将观察者重用于不同的主题。它也可能有所帮助，因为不同的观察者可能对不同的状态变化感兴趣，因此将所有可能的状态变化人为耦合起来可能违反 ISP。当然，这也可能带来效率提升，如果可以避免大量不必要的状态变化通知。

不幸的是，特定主题不太可能区分不同种类的观察者。首先，因为这需要它存储不同种类的指针（这对主题来说处理起来很不方便），其次，因为不同的状态变化可能以某种方式关联。在这种情况下，主题会期望观察者对 *所有* 可能的状态变化感兴趣。从这个角度来看，将几个 `update()` 函数组合到一个基类中是合理的。无论如何，很可能一个具体的观察者必须处理各种状态变化。我知道，处理多个 `update()` 函数可能很麻烦，即使其中只有很小一部分是有趣的。但仍然，请确保你没有通过不遵守某些预期行为（如果有的话）而意外违反 *Liskov Substitution Principle*。

*push observer* 还有几个潜在的缺点。首先，观察者 *总是* 被提供 *所有* 信息，无论它们是否需要。因此这种推送风格仅在观察者大多数时候需要信息时才有效。否则在不必要的通知上会浪费大量精力。其次，推送会产生对传递给观察者的参数数量和种类的依赖。对这些参数的任何更改都需要对派生的观察者类进行大量后续更改。

第二种 `Observer` 替代方案解决了其中一些缺点。可以只将主题的引用传递给观察者：¹⁰

```
class Observer
{
public:
    // ...
    virtual void update( Subject const& subject ) = 0;
    // ...
};
```

由于没有向观察者传递特定信息，从 `Observer` 基类派生的类需要自行从主题中提取新信息。因此，这种形式的观察者通常被称为 *pull observer*。优点是减少了对参数数量和种类的依赖。派生的观察者可以自由查询任何信息，而不仅仅是已更改的状态。另一方面，这种设计在从 `Observer` 派生的类与主题之间创建了强有力的直接依赖。因此主题的任何更改都很容易反映到观察者上。此外，如果可能更改多个细节，观察者可能不得不“搜索”状态变化。这可能被证明是不必要低效的。

如果你只考虑将单个信息片视为变化状态，性能劣势可能不会对你构成限制。尽管如此，请记住软件会发生变化：主题可能会增长，随着它希望通知不同种类的变化。相应地调整观察者将导致大量额外工作。从这个角度来看，*push observer* 似乎是更好的选择。

幸运的是，有第三种替代方案，它消除了许多之前的缺点，因此成为我们的首选方法：除了传递主题的引用外，我们还传递一个标签以提供有关主题的哪个属性已更改的信息：

//---- <Observer.h> ----------------

class Observer
{
public:
    virtual ~Observer() = default;

    virtual void update( Subject const& subject
        , /*Subject-specific type*/ property ) = 0;
};
```

标签可以帮助观察者自行决定某些状态变化是否有趣。它通常由某种主题特定的枚举类型表示，该类型列出了所有可能的状态变化。这不幸地增加了 `Observer` 类与特定主题的耦合。

“我们是否可以通过将 Observer 基类实现为类模板来消除对特定 Subject 的依赖？请看下面的代码片段：”

//---- <Observer.h> ----------------

template< typename Subject, typename StateTag >  ②
class Observer
{
public:
    virtual ~Observer() = default;

    virtual void update( Subject const& subject, StateTag property
) = 0;
};
```

这是一个很好的建议：通过以类模板的形式定义 Observer 类（②），我们可以轻松地将 Observer 提升到更高的架构层面：在这种形式下，该类不依赖于任何特定主题，因此可以被许多希望定义一对多关系的主题重用。然而，你不应该对这种改进期望太多：效果仅限于 Observer 类。具体主题将期望此观察者类的具体实例化，因此具体实现的 Observer 仍将强烈依赖于主题。

为了更好地理解为什么会这样，让我们看一个可能的主题实现。在你关于社交媒体的初始评论之后，我建议我们为人物实现一个 Observer。好吧，这个例子在道德上可能值得怀疑，但它会发挥其作用，所以让我们继续吧。至少我们知道谁要为此负责。

以下 Person 类代表一个被观察的人：

//---- <Person.h> ----------------

#include <Observer.h>
#include <string>
