## Chapter 10. The Singleton Pattern

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 10th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at sevans@oreilly.com.

In this chapter, we take a look at the (in-)famous *Singleton* pattern. I know, you may already be acquainted with *Singleton* and you may already have a strong opinion about that pattern. It is even possible that you consider *Singleton* an anti-pattern and thus ask yourself how I muster the courage to include it in this book. Well, I am aware that *Singleton* is not particular popular and in many circles has a rather bad reputation, in particular because of the global nature of *Singletons*. From that perspective, however, it might be very surprising to learn, that there is a couple of "Singleton"-like instances in the C++ Standard Library. Seriously! And, honestly, they work fantastically! Therefore we should seriously talk about *what* a *Singleton* is, about *when* *Singleton* works and about *how* to deal with *Singleton* properly.

In “Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern”, I will explain the *Singleton* pattern and demonstrate how it works by means of a very commonly used implementation, the so called

Meyers’ Singleton. I will, however, also make a strong argument to not treat Singleton as a design pattern, but an implementation pattern.

In “Guideline 38: Design Singletons for Change and Testability”, we accept the fact that sometimes we need a solution to represent the few global aspects in your code. This is what the Singleton pattern is often used for.

This also means that we are confronted by the usual problems of Singletons: global state, many strong, artificial dependencies, and an impeded changeability and testability. While this sounds like an excellent reason to avoid Singleton after all, I will show you that by proper software design, you can combine the Singleton benefits with excellent changeability and testability.

### Guideline 37: Treat Singleton as an Implementation Pattern, Not a Design Pattern

Let me start by addressing the elephant in the room:

Singleton is not a design pattern.

If you have not heard about Singleton before, then this might not make any sense at all, but bear with me, I promise to explain Singleton shortly. If you have heard about Singleton before, then I assume your reaction being one of two things: Either you are nodding in agreement, putting up this sympathizing “I know” look, or you are utterly stunned and initially do not know what to say. “But why not”, you eventually dare to ask, “isn’t it one of the original design patterns from the Gang-of-Four (GoF) book?” Yes, you are correct: Singleton is one of the 23 original patterns documented in the GoF book. At the time of writing Wikipedia calls it a design pattern and it is even listed as a design pattern in Steve McConnell’s bestseller “Code Complete”.¹ Nevertheless, it is still not a design pattern, because it does not have the properties of a design pattern. Let me explain.

#### The Singleton Pattern Explained

Sometimes you may want to guarantee that there is only one, and *exactly* one, instance of a particular class. In other words, you have a Highlander situation: “There can be only one”.² This might make sense for the system-wide database, the one and only logger, the system clock, the system configuration, or, in short, any class that should not be instantiated multiple times, since it represents something, that exists only once. That is the intent of the *Singleton* pattern:

#### THE *SINGLETON* PATTERN

Intent: “Ensure a class has only one instance, and provide a global point of access to it.” (GoF)

This intent is visualized by the Gang of Four with the UML diagram in Figure 10-1, which introduces the `instance()` function as the global point of access to the unique instance:

#### Singleton

static instance()
singletonOperation()
getSingletonData()

static uniqueInstance
singletonData

return uniqueInstance

Provides access to the one instance of the Singleton

Figure 10-1. The UML representation of the Singleton pattern.

There are multiple ways to restrict the number of instantiations to exactly one. Definitely one of the most useful and therefore most commonly used forms of Singleton is the Meyers’ Singleton.³ The following Database class is implemented as a Meyers’ Singleton:

```
//---- <Database.h> ----------------

class Database final
{
public:
    static Database& instance() ①
    {
        static Database db;  // The one, unique instance
        return db;
    }

    bool write( /*some arguments*/ );
    bool read( /*some arguments*/ ) const;
    // ... More database-specific functionality

    // ... Potentially access to data members

private:
    Database() { } ②
    Database( Database const& ) = delete;
    Database& operator=( Database const& ) = delete;
    Database( Database&& ) = delete;
    Database& operator=( Database&& ) = delete;

    // ... Potentially some data members
};
```

The Meyers’ Singleton evolves around the fact, that it is only possible to access the single instance of the Database class via the public, static instance() function ①:

```
#include <Database.h>
#include <cstdlib>

int main()
```
```
{    // First access, database object is created    Database& db1 = Database::instance();    // ...    // Second access, returns a reference to the same object    Database& db2 = Database::instance();    assert( &db1 == &db2 );    return EXIT_SUCCESS;}
```

Indeed, this function is the only way to get a Database: all functionality that could possibly be used to create, copy or move an instance, is either declared in the private section, or is explicitly deleted.⁴ Although this appears to be pretty straightforward, one implementation detail is of special interest: note, that the default constructor is explicitly defined and not defaulted ②. The reason is, that in case it was defaulted, up to C++17, it would be possible to create a Database by means of an empty set of braces, i.e. via value initialization:

```
#include <cstdlib>

class Database
{
public:
    // ... As before

private:
    Database() = default;  // Compiler generated default constructor
    // ... As before
};

int main()
{
    Database db;      // Does not compile: Default initialization
    Database db{};    // Works, since value initialization results in aggregate
                      // initialization, because Database is an aggregate type



```
    return EXIT_SUCCESS;
}
```

自 C++17 起，Database 类被视为聚合类型，这意味着值初始化将通过聚合初始化进行。聚合初始化反过来会忽略默认构造函数，包括它为私有的事实，并简单地对对象执行零初始化。因此值初始化仍然使你能够创建实例。然而，如果你自己提供了默认构造函数，那么该类就不再被视为聚合类型，这会阻止聚合初始化。⁵

instance() 函数是基于静态局部变量实现的。这意味着，第一次控制流通过声明时，变量会以线程安全的方式初始化，而在所有后续调用中初始化将被跳过。⁶ 在每次调用中，第一次调用以及所有后续调用，函数都会返回对该静态局部变量的引用。

Database 类的其余部分基本上就是你对表示数据库的类所期望的样子：有一些公开的、与数据库相关的函数（例如 write() 和 read()），并且可能有一些数据成员，包括访问函数。换句话说，除了 instance() 成员函数和特殊成员之外，Database 只是一个普通的类。

### Singleton 并不会管理或减少依赖

现在，有了 Singleton 的一种可能实现后，让我们回到我关于 Singleton 不是设计模式的断言。首先，让我们回顾一下设计模式的特性，我在“准则 11：理解设计模式的目的”中对其进行了定义：

一个设计模式 ...

- ... 有一个名称；
- ... 承载一个意图；
- ... 引入一种抽象；
- ... 已经被证明。

*Singleton* 模式确实有一个名称，并且确实有一个意图。这一点毫无疑问。我也会声称它多年来已被证明（尽管可能存在怀疑的声音，指出 *Singleton* 相当臭名昭著）。然而，这里没有任何抽象：没有基类，没有模板参数，什么都没有。*Singleton* 本身并不代表一种抽象，也没有引入抽象。事实上，它并不关心代码的结构、实体之间的交互和相互依赖，因此它并不旨在管理或减少依赖。⁷ 这，正是我所定义为软件设计不可或缺的一部分。相反，*Singleton* 专注于将实例化数量限制为恰好一个。因此，作为结果，*Singleton* 不是一种设计模式，而仅仅是一种实现模式。

“那么为什么它在许多重要来源中都被列为设计模式呢？”你会问。这是一个公平且好的问题。对此可能有三个答案：首先，在其他编程语言中，特别是每种类都可以自动代表一种抽象的语言中，情况可能不同。虽然我承认这一点，但我仍然认为 *Singleton* 模式的意图主要针对实现细节，而不是依赖和解耦。

其次，*Singleton* 被非常普遍地使用（尽管经常被 *误用*），所以它确实是一种模式。由于在许多不同的编程语言中都有 *Singleton*，它似乎不仅仅是 C++ 编程语言的惯用法。因此，称其为设计模式似乎是合理的。这个论证链可能听起来对你很合理，但我觉得它在区分软件设计和实现细节方面有所不足。这就是为什么在“准则 11：理解设计模式的目的”中，我引入了 *实现模式* 这一术语，以便能够区分 *Singleton* 等不同类型的语言无关模式。⁸

第三，我相信我们仍在理解软件设计和设计模式的过程中。软件设计没有共同的定义。因此，我在“准则 1：理解软件设计的重要性”中提出了一个定义。设计模式也没有共同的定义。这就是为什么我在“准则 11：理解设计模式的目的”中提出了一个定义。我坚信我们必须更多地谈论软件设计和模式，以便就必要的术语达成共同的理解，尤其是在 C++ 中。

总之，你不会使用 Singleton 来解耦软件实体。因此，尽管它在著名的 GoF 书中有所描述，在《Code Complete》中也有提及，甚至在 Wikipedia 上被列为设计模式，但它并不服务于设计模式的目的。Singleton 仅仅处理实现细节，因此你应该将其视为一种实现模式。

### 准则 37：将 SINGLETON 视为实现模式，而非设计模式

- Singleton 的目标不是解耦或管理依赖，因此它不符合设计模式的期望；
- 应用 Singleton 模式的意图是将特定类的实例数量限制为恰好一个。

### 准则 38：为可变性和可测试性设计 Singleton

Singleton 确实是一种相当臭名昭著的模式：外面的许多声音都将 Singleton 描述为代码中的一个普遍问题、一种反模式、危险的，甚至是邪恶的。因此，外面有很多建议要避免该模式，其中包括核心准则 I.3.：⁹

避免单例。

—核心准则 I.3

人们不喜欢 Singleton 的主要原因之一是，它经常导致人为的依赖并阻碍可测试性。如此一来，它与本书中最重要和最通用的两个准则相悖：“准则 2：为变化而设计”和“准则 4：为可测试性而设计”。从这个角度来看，Singleton 确实是代码中的一个问题，应该避免。然而，尽管有所有善意的警告，该模式仍被许多开发人员持续使用。其原因多种多样，但可能主要与两个事实有关：首先，有时（让我们同意有时）表达某事物仅存在一次并且应为代码中的许多实体可用是可取的。其次，有时 Singleton 似乎是适当的解决方案，因为存在需要表示的全局方面。

所以，让我们这样做：与其争论 Singleton 总是坏的和邪恶的，不如让我们专注于那些我们需要在程序中表示全局方面的少数情况，并讨论如何适当地表示这一方面，同时仍为变化和可测试性而设计。

### 单例表示全局状态

单例主要用于表示在程序中逻辑上和/或物理上仅存在一次并且应被许多其他类和函数使用的实体。10 常见的例子是系统范围的数据库、记录器、时钟或配置。这些例子，包括“系统范围”这一术语，表明了这些实体的性质：它们通常代表全局可用的功能或数据，即全局状态。从这个角度来看，Singleton 模式似乎是有意义的：通过阻止每个人创建新实例，通过强制每个人使用唯一实例，你可以保证对所有使用实体的一致且统一的全局状态访问。

然而，这种表示和引入全局状态，解释了为什么 Singleton 通常被认为是一个问题。正如 Michael

Feathers 所表达的那样：¹¹

> *单例模式是人们用来创建全局变量的机制之一。通常，全局变量是一个坏主意，原因有几个。其中之一是透明度。*
> 
> —Michael Feathers

全局变量确实是一个坏主意，尤其有一个重要原因：术语“变量”暗示我们谈论的是 *可变* 的全局状态。而这种状态确实会带来很多麻烦。明确地说，可变的全局状态是不受欢迎的（通常如此，尤其是在多线程环境中），因为控制访问并保证正确性是困难的、昂贵的，并且可能两者兼有。此外，全局（可变）状态很难推理，因为对该状态的读写访问通常在某个函数内部不可见地发生，而基于其接口，该函数并不会透露它使用全局状态的事实。最后但并非最不重要的是，如果你有几个全局变量，它们的生命周期相互依赖并且分布在几个编译单元中，你可能会面临静态初始化顺序灾难（或简称为 *SIOF*）。¹² 所以显然，尽可能避免全局状态是有益的。¹³

然而，全局状态的问题并不是通过避免 *Singleton* 就可以解决的。它是一个普遍问题，与任何特定模式无关。例如，*Monostate* 模式也存在同样的问题，它强制单一的全局状态，但允许任意数量的实例化。¹⁴ 所以相反，*Singleton* 可以帮助处理全局状态，通过限制对其的访问。例如，正如 Miško Hevery 在其 2008 年的文章中所解释的那样，提供单向数据流到 *或* 来自某些全局状态的 *Singleton* 是可以接受的：¹⁵ 实现记录器的 *Singleton* 只允许你写入数据，但不允许读取它。代表系统范围配置或时钟的 *Singleton* 只允许你读取数据，但不允许写入它，从而表示一个全局 *常量*。限制为单向数据流有助于避免全局状态的许多常见问题。或者用 Miško Hevery 的话来说（重点是我加的）：



> *“Global”或半全局状态的适当使用可以极大地简化应用程序的设计 [...]。*
> 
> —Miško Hevery

### 单例阻碍可变性和可测试性

全局状态是 *Singleton* 的固有问题。然而，即使我们觉得用 *Singleton* 来表示全局状态很舒服，也会有严重的后果：使用 *Singleton* 的函数依赖于所表示的全局数据，因此变得更难更改，也更难测试。为了更好地理解这一点，让我们重新唤起“准则 37：把 Singleton 当作实现模式，而不是设计模式”中的 `Database Singleton`，它现在正被几个任意类积极使用，即 `Widget` 和 `Gadget`：

```
//---- <Widget.h> ----------------

#include <Database.h>

class Widget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().read( /*some arguments*/ );
        // ...
    }
};
```

```
//---- <Gadget.h> ----------------

#include <Database.h>

class Gadget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().write( /*some arguments*/ );
    }
};
// ...
}
};
```

Widget 和 Gadget 都需要访问系统范围的 Database。因此它们调用 Database::instance() 函数，随后调用 read() 和 write() 函数。

由于它们使用了 Database 因此依赖于它，我们希望它们位于 Database Singleton 层级之下的架构层级中。因为，正如你从“准则 2：为变化而设计”中记得的，只有当所有依赖箭头都指向高层级时，我们才能称之为适当的架构（参见图 10-2）。

高层级

低层级

架构边界

架构边界

```
class Database
{
public:
    static Database& instance();
    // ...
    bool write( /* some arguments */ );
    bool read( /* some arguments */ );
private:
    Database() {}
    Database( Database const& ) = default;
};
```

```
class Widget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().read( /*some arguments*/ );
        // ...
    }
};
```

```
class Gadget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().write( /*some arguments*/ );
        // ...
    }
};
```

图 10-2. 数据库作为 Singleton 实现时期望的依赖图。

尽管这种依赖结构可能是理想的，但不幸的是它只是一种幻觉：Database 类不是抽象，而是具体的实现，表示对非常特定的数据库的依赖！因此实际的依赖结构是倒置的，看起来像图 10-3。

高层级

```
class Widget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().read( /*some arguments*/ );
        // ...
    }
};
```

架构边界

```
class Gadget
{
public:
    void doSomething( /*some arguments*/ )
    {
        // ...
        Database::instance().write( /*some arguments*/ );
        // ...
    }
};
```

架构边界

```
class Database
{
public:
    static Database& instance();
    // ...
    bool write( /* some arguments */ );
    bool read( /* some arguments */ );
private:
    Database() {}
    Database( Database const& ) = default;
};
```

低层级

图 10-3. 数据库作为 Singleton 实现时实际的依赖图。

实际的依赖结构完全违背了依赖倒置原则 DIP（参见“准则 9：注意抽象的所有权”）：所有依赖箭头都指向低层级。换句话说，现在根本没有软件架构！

由于 Database 是一个具体类而不是抽象，因此从代码各处到 Database 类的具体实现细节和设计选择都存在强烈的而且不幸的是不可见的依赖关系。这在最坏的情况下可能包括对供应商特定细节的依赖，这些细节会在代码中显现，体现在许多不同的地方，并使后续的更改变得极其困难甚至不可能。因此，代码变得更难更改。

同时考虑这种依赖对测试的破坏程度：所有使用依赖于 Database Singleton 的函数之一的测试本身都依赖于 Singleton。这意味着，例如，对于每个使用 Widget::doSomething() 函数的测试，你总是必须提供唯一的一个 Database 类。不幸但简单的理由是，这些函数都没有为你提供用其他东西替换 Database 的方式：任何类型的存根、模拟或假对象。16 它们都把 Database Singleton 当作它们闪亮珍贵的秘密。可测试性因此受到严重阻碍，编写测试变得如此困难，以至于你可能会被诱使根本不写测试。17

这个例子确实展示了 Singleton 通常存在的问题，以及它们引入的不幸的人为依赖。这些依赖使系统更不灵活、更僵化，从而更难更改、更难测试。当然，这不应该如此。相反：应该很容易用另一个数据库实现替换数据库实现，也应该很容易测试使用数据库的功能。正因为这些确切的原因，我们必须确保 Database 成为一个适当架构低层级的真正的实现细节。18

“等等：你刚才说如果 Database 是实现细节，就没有架构，对吧？” 是的，我是这么说的。而且在现状下我们无能为力：Database Singleton 不代表任何抽象，也不能让我们处理依赖关系。Singleton 根本不是设计模式。因此，为了消除对 Database 类的依赖，使架构发挥作用，我们必须通过引入抽象并使用真正的设计模式来为变化和可测试性而设计。为了实现这一点，让我们看一个处理全局方面的良好方式的例子，该例子使用了 C++ 标准库中的 Singletons。

### 反转对 Singleton 的依赖

我再次回到一个我多次用来演示不同设计模式的真正的设计模式伊甸园：C++17 多态内存资源。

```
#include <array>
#include <cstddef>
#include <cstdlib>
#include <memory_resource>
#include <string>
#include <vector>
// ...

int main()
{
    std::array<std::byte,1000> raw;  // Note: not initialized!

    std::pmr::monotonic_buffer_resource
        buffer{ raw.data(), raw.size(),
    std::pmr::null_memory_resource() };

    std::pmr::vector<std::pmr::string> strings{ &buffer };

    // ...

    return EXIT_SUCCESS;
}
```

在这个例子中，我们配置一个名为 buffer 的 std::pmr::monotonic_buffer_resource，使其仅与给定 std::array raw ① 中包含的静态内存一起工作。如果该内存耗尽，buffer 将尝试通过其上游分配器获取新内存，我们将其指定为 std::pmr::null_memory_resource()。通过此分配器分配永远不会返回任何内存，而是总是通过 std::bad_alloc() 异常失败。因此，buffer 被限制在 raw 提供的 1000 字节内。

你应该立即记住并认出这是一个装饰器设计模式的例子，同时这也是 Singleton 模式的例子：std::pmr::null_memory_resource() 函数每次调用都返回指向同一分配器的指针，因此充当对 std::pmr::null_memory_resource 的唯一实例的单一访问点。因此，返回的分配器充当 Singleton。尽管此 Singleton 不提供单向的数据流（毕竟，我们既可以分配内存也可以归还），Singleton 仍然感觉像是一个合理的选择，因为它代表了一种全局状态：内存。

特别有趣且重要的是要注意，这个 Singleton 并不会让你依赖分配器的具体实现细节。恰恰相反：std::pmr::null_memory_resource() 函数返回一个指向 std::pmr::memory_resource 的指针。此类代表所有分配器的基类（至少在 C++17 的范围内），因此充当抽象。尽管如此，std::pmr::null_memory_resource() 代表一个特定的分配器，一个特定的选择，我们现在依赖于它。由于此功能在标准库中，我们往往不会将其识别为依赖，但从一般意义上讲它确实是：我们没有机会替换标准特定的实现。

如果我们用对





### std::pmr::get_default_resource() ②：

```
#include <memory_resource>
// ...

int main()
{
    // ...

    std::pmr::monotonic_buffer_resource
        buffer{ raw.data(), raw.size(),
    std::pmr::get_default_resource() };  ②

    // ...

    return EXIT_SUCCESS;
}
```

std::pmr::get_default_resource() 函数同样返回一个指向 std::pmr::memory_resource 的指针，该指针代表系统范围默认分配器的抽象。默认情况下，返回的分配器是由 std::new_delete_resource() 函数返回的那个。然而令人惊讶的是，这个默认值可以通过 std::pmr::set_default_resource() 函数进行自定义：

```
namespace std::pmr {

memory_resource* set_default_resource(memory_resource* r)
noexcept;

} // namespace std::pmr
```

借助此函数，我们可以将 std::pmr::null_memory_resource() 定义为新的系统范围默认分配器 ③：

```
// ...

int main()
{
    // ...
    std::pmr::set_default_resource(
    std::pmr::null_memory_resource() );  3

    std::pmr::monotonic_buffer_resource
        buffer{ raw.data(), raw.size(),
    std::pmr::get_default_resource() };

    // ...

    return EXIT_SUCCESS;
}
```

通过 std::pmr::set_default_resource()，你能够自定义系统范围的分配器。换句话说，该函数为你提供了注入对该分配器依赖的能力。这是否让你有所联想？是否听起来很熟悉？我非常希望这会让你想到另一个基本的设计模式：……鼓点……没错：策略设计模式。19 确实，这就是一种策略。使用这种设计模式是一个很棒的选择，因为它对架构产生了惊人的影响：虽然 std::pmr::memory_resource 代表了所有可能分配器的抽象，因此可以位于架构的高层，但任何分配器的具体实现，包括所有（厂商）特定的实现细节，都可以位于架构的最底层。作为演示，考虑例如 CustomAllocator 类的这个草图：

```
//---- <CustomAllocator.h> ----------------

#include <memory_resource>

class CustomAllocator : public std::pmr::memory_resource
{
public:
    // There is no need to enforce a single instance
    CustomAllocator( /*...*/ );
    // No explicitly declared copy or move operations

private:
    void* do_allocate( size_t bytes, size_t alignment ) override;
```

```
void do_deallocate( void* ptr, size_t bytes,
    size_t alignment ) override;

bool do_is_equal(
    std::pmr::memory_resource const& other ) const noexcept override;
    // ...
};
```

注意，CustomAllocator 公共继承自 std::pmr::memory_resource 以便符合 C++17 分配器的资格。正因如此，你可以通过 std::pmr::set_default_resource() 函数将 CustomAllocator 的实例建立为新的系统范围默认分配器 (4)：

```
#include <CustomAllocator.h>

int main()
{
    // ...
    CustomAllocator custom_allocator{ /*...*/ };

    std::pmr::set_default_resource( &custom_allocator );  4
    // ...
}
```

虽然 std::pmr::memory_resource 基类位于架构的最高层，但 CustomAllocator 在逻辑上是在最低的架构层引入的（参见图 10-4）。因此，策略模式导致了依赖倒置（参见“准则 9：注意抽象的所有权”）：尽管分配器具有单例特性，尽管代表全局状态，你依赖的是抽象而不是具体的实现细节。

High-level

```
namespace std::pmr {

class memory_resource { /*...*/ };

memory_resource* set_default_resource(memory_resource* r) noexcept;

} // namespace std::pmr
```

Architectural Boundary

Architectural Boundary

```
class CustomAllocator : public std::pmr::memory_resource
{
public:
    CustomAllocator( /*...*/ );

private:
    void* do_allocate( size_t bytes, size_t alignment ) override;
    void do_deallocate( void* ptr, size_t bytes,
                        size_t alignment ) override;
    bool do_is_equal(
        std::pmr::memory_resource const& other ) const noexcept override;

    // ...
};
```

Low-level

图 10-4. 通过 std::pmr::memory_resource 抽象实现的依赖倒置。

作为旁注，值得指出的是，通过这种方法你可以轻松避免对全局变量初始化顺序的任何依赖（即 SIOF），因为你可以通过在堆栈上并在单个编译单元中创建所有单例来显式管理初始化顺序：

```
int main()
{
    // The one and only system-wide clock has no lifetime dependencies.
    // Thus it is created first
    SystemClock clock{ /*...*/ };

    // The one and only system-wide configuration depends on the clock.
    SystemConfiguration config{ &clock, /*...*/ };

    // ...
}
```

### 应用策略设计模式

基于前面的示例，你现在应该有了如何修复我们的 Database 示例的思路。作为提醒，目标是保留 Database 类作为默认数据库实现，但将其作为实现细节，即移除对具体实现的所有依赖。你需要做的就是应用策略设计模式，在架构的高层引入一个抽象，以及一个全局访问点和一个全局依赖注入点。这将使任何人（我真的意思是任何人，因为你也遵循开闭原则 OCP；参见“准则 5：为扩展而设计”）能够在最低层引入自定义数据库实现（包括具体实现以及测试桩、模拟或假对象）。

因此让我们引入以下 PersistenceInterface 抽象 (5)：

```
//---- <PersistenceInterface.h> ----------------

class PersistenceInterface  5
{
public:
    virtual ~PersistenceInterface() = default;

    bool read( /*some arguments*/ ) const  6
    {
        return do_read( /*...*/ );
    }
    bool write( /*some arguments*/ )  7
    {
        return do_write( /*...*/ );
    }

    // ... More database specific functionality

private:
    virtual bool do_read( /*some arguments*/ ) const = 0;  6
    virtual bool do_write( /*some arguments*/ ) = 0;  7
};

PersistenceInterface* get_persistence_interface();  8
void set_persistence_interface( PersistenceInterface* persistence );  9

// Declaration of the one 'instance' variable
extern PersistenceInterface* instance;  10
```

PersistenceInterface 基类为所有可能的数据库实现提供接口。例如，它引入了 read() 和 write() 函数，分为公共接口部分和私有实现部分，基于 std::pmr::memory_resource 类的示例 (6 和 7)。20 当然，在现实中它会引入更多数据库特定的函数，但对于本示例 read() 和 write() 已足够。

除了 PersistenceInterface 之外，你还会引入一个名为 get_persistence_interface() 的全局访问点 (8) 和一个用于启用依赖注入的函数

set_persistence_interface() ⑨。这些两个函数允许你访问和设置全局持久化系统 ⑩。

Database 类现在继承自 PersistenceInterface 基类并实现所需接口（希望遵循里氏替换原则 LSP；参见“准则 6：遵循抽象的预期行为”）：

```
//--- <Database.h> -------------

class Database : public PersistenceInterface
{
public:
    // ... Potentially access to data members

    // Make the class immobile by deleting the copy and move operations
    Database( Database const& ) = delete;
    Database& operator=( Database const& ) = delete;
    Database( Database&& ) = delete;
    Database& operator=( Database&& ) = delete;

private:
    bool do_read( /*some arguments*/ ) const override;
    bool do_write( /*some arguments*/ ) override;
    // ... More database-specific functionality

    // ... Potentially some data members
};
```

在我们的特殊设置中，Database 类代表默认数据库实现。我们需要创建一个数据库的默认实例，以防未通过 set_persistence_interface() 函数指定其他持久化系统。然而，如果在 Database 创建之前已将其他持久化系统建立为系统范围的数据库，我们就必须不创建实例，因为这会造成不必要的且不幸的开销。这种行为是通过使用两个静态局部变量和一个立即调用的 Lambda 表达式 IILE 实现 get_persistence_interface() 函数来实现的 ⑪：




//---- <PersistenceInterface.cpp> ----------------

#include <Database.h>

// Definition of the one 'instance' variable
PersistenceInterface* instance = nullptr;

PersistenceInterface* get_persistence_interface()
{
    // Local object, initialized by an 'Immediately Invoked Lambda Expression (IILE)'
    static bool init = [](){
        if (!instance) {
            static Database db;
            instance = &db;
        }
        return true; // or false, as the actual value does not matter.
    }(); // Note the '()' after the lambda expression. This invokes the lambda.

    return instance;
}

void set_persistence_interface( PersistenceInterface* persistence )
{
    instance = persistence;
}
```

第一次执行流程进入 `get_persistence_interface()` 函数时，`init` 静态局部变量会被初始化。此时如果 `instance` 已经设置，则不会创建 `Database`。然而如果未设置，则 `Database` 实例会在 lambda 内部作为另一个静态局部变量创建并绑定到 `instance` 变量。

```
#include <PersistenceInterface.h>
#include <cstdlib>

int main()
{
    // First access, database object is created
    PersistenceInterface* persistence =
get_persistence_interface();

// ...

return EXIT_SUCCESS;
}
```

此实现实现了期望的效果：`Database` 成为一个实现细节，没有其他代码依赖于它，并且可以随时通过自定义数据库实现进行替换（参见图 10-5）。因此，尽管 `Database` 具有单例特性，它并不会引入依赖，可以轻松更改，也便于用于测试目的。

#### High-level

```
class PersistenceInterface
{
public:
    virtual ~PersistenceInterface() = default;
    // ...
private:
    virtual bool do_read( /* some arguments */ ) const;
    virtual bool do_write( /* some arguments */ );
};
```

```
class Database : public PersistenceInterface
{
public:
    // ...
private:
    bool do_read( /*...*/ ) const override;
    bool do_write( /*...*/ ) override;
    // ...
};
```

Architectural Boundary

```
class Widget
{
    // ...
};
```

Architectural Boundary

```
class Gadget
{
    // ...
};
```

```
class CustomPersistence
: public PersistenceInterface
{
public:
    // ...
private:
    bool do_read( /*...*/ ) const override;
    bool do_write( /*...*/ ) override;
    // ...
};
```

Low-level

No dependencies on the Database class!

Figure 10-5. The dependency graph for the refactored, non-Singleton Database.

“哇，这是一个很棒的解决方案。我敢打赌我可以在我自己的代码库的几个地方使用它！”你带着钦佩和赞赏的表情说道。“但我看到一个潜在的问题：由于我必须继承自一个接口类，这是一个侵入式的解决方案。如果我不能更改某个给定的单例类，我该怎么办？”好吧，在这种情况下，你有两个非侵入式设计模式可供选择：要么你已经有一个继承层次结构，就可以在其中引入一个适配器来包装给定的单例（参见“准则 24：使用适配器来标准化接口”）。要么你还没有继承层次结构，在这种情况下，你可以很好地利用外部多态设计模式（参见“准则 31：使用外部多态实现非侵入式运行时多态”）。

“好的，但我看到另一个更严重的问题：这段代码真的线程安全吗？”老实说：不是。为了举一个可能问题的例子：可能在第一次调用 `get_persistence_interface()` 期间，由于 `Database` 实例的设置可能需要一些时间，`set_persistence_interface()` 被调用。在这种情况下，要么 `Database` 被徒劳创建，要么对 `set_persistence_interface()` 的调用丢失。然而，也许令人惊讶的是，这并不是我们需要解决的问题。原因如下：请记住，`instance` 代表全局状态。如果我们假设 `set_persistence_interface()` 可以在代码中的任何地方随时调用，通常我们不能期望在调用 `set_persistence_interface()` 之后对 `get_persistence_interface()` 的调用会返回设置的值。因此，从代码中的任何地方调用 `set_persistence_interface()` 函数就像从某人脚下抽走地毯。这与对任何左值调用 `std::move()` 可比：

```
template< typename T >
void f( T& value )
{
    // ...
    T other = std::move(value);  // Very bad move (literally)!
    // ...
}
```

从这个角度来看，`set_persistence_interface()` 函数应该在程序的非常开始或单个测试的开始时使用——而不是随意使用。

“我们不应该确保 `set_persistence_interface()` 函数只能被调用一次吗？”你问道。我们当然可以这样做，但这会人为地限制其用于测试的目的：我们将无法在每个单独测试开始时重置持久化系统。

### Moving Towards Local Dependency Injection

“好的，我明白了。最后一个问题：由于这个解决方案涉及可以更改的全局状态，是否最好对低层类使用更直接、更局部的依赖注入？考虑以下对 `Widget` 类的修改，它在构造时获得其依赖关系”：

//---- <Widget.h> ----------------

#include <PersistenceInterface.h>

class Widget
{
public:
    Widget( PersistenceInterface* persistence )  // Dependency injection
        : persistence_(persistence)
    {}

    void doSomething( /*some arguments*/ )
    {
        // ...
        persistence_->read( /*some arguments*/ );
        // ...
    }

}
private:
    PersistenceInterface* persistence_{};
};
```

我完全同意你的观点。这可能是解决全局状态问题的下一步。然而，在分析这种方法之前，你应该意识到，这个想法之所以可行，是因为我们已经反转了依赖关系：多亏在架构的高层引入了抽象，我们突然有了选择，可以讨论替代方案。因此，第一步也是最重要的一步是正确管理依赖关系。但回到你的建议：我真的很喜欢这种方法。`Widget` 类的接口变得更“诚实”，清楚地展示了它的所有依赖关系。而且由于依赖关系是通过构造函数参数传递的，依赖注入变得更直观、更自然。

或者，你也可以直接将依赖关系传递给 `Widget::doSomething()` 函数：

//--- <Widget.h> -------------

#include <PersistenceInterface.h>

class Widget
{
public:
    void doSomething( PersistenceInterface* persistence, /*some arguments*/ )
    {
        // ...
        persistence->read( /*some arguments*/ );
        // ...
    }
};
```

虽然这种方法对于成员函数可能不是最佳选择，但对于自由函数来说，这可能是你唯一的选项。而且再次，函数变得更“诚实”了一些，因为它明确说明了其依赖关系。

然而，直接依赖注入也有另一面：这种方法在大型调用栈中可能会很快变得难以处理：将依赖关系通过软件栈的多个层传递以便在需要时可用，既不方便也不直观。此外，尤其是在存在多个单例的情况下，该解决方案很快就会变得笨重：例如，通过许多层函数调用传递 `PersistenceInterface`、`Allocator` 和系统范围的 `Configuration` 只是为了能够在最低层使用它们，真的不是最优雅的方法。因此，你可能希望结合提供全局访问点（毕竟它们是全局方面）和局部依赖注入的思想，例如通过引入包装函数：

//---- <Widget.h> ----------------

#include <PersistenceInterface.h>

class Widget
{
public:
    void doSomething( /*some arguments*/ )  ⑫
    {
        doSomething( get_persistence_interface(), /*some arguments*/ );
    }

    void doSomething( PersistenceInterface* persistence, /*some arguments*/ )  ⑬
    {
        // ...
        persistence->read( /*some arguments*/ );
        // ...
    }
};
```

虽然我们仍然提供之前的 `doSomething()` 函数（⑫），但我们现在额外提供了一个接受 `PersistenceInterface` 作为函数参数的重载（⑬）。第二个函数执行所有工作，而第一个函数现在仅仅充当一个


wrapper，它注入了全局设置的 `PersistenceInterface`。在这种组合中，可以做出本地决策并在本地注入所需的依赖，同时又不必通过多层函数调用传递依赖。

然而，说实话，虽然这些解决方案在数据库示例中以及在管理内存的上下文中可能效果非常好，但它可能并不是解决每一个 *Singleton* 问题的正确方法。所以不要认为这是唯一可能的解决方案。毕竟，这取决于具体情况。然而，它是软件设计一般流程的一个很好的例子：识别发生变化或导致依赖的方面，然后通过提取合适的抽象来分离关注点。根据你的意图，你只是在应用一种设计模式。因此请相应地命名你的解决方案，并借此为他人留下你的推理痕迹。

总之：*Singleton* 模式绝对不是那些光鲜的设计模式之一。它带来的缺点实在太多，最重要的是全局状态的常见缺陷。但即便如此，尽管存在诸多负面方面，如果谨慎使用，*Singleton* 在某些情况下可以成为表示代码中少数全局方面的正确解决方案。如果是这样，优先使用具有单向数据流的 *Singletons*，并通过反转依赖关系并利用 *Strategy* 设计模式实现 *dependency injection* 来为变化和可测试性设计你的 *Singletons*。

### GUIDELINE 38: DESIGN *SINGLETONS* FOR CHANGE AND TESTABILITY

- 要意识到 *Singleton* 代表全局状态，有其所有缺陷；
- 尽可能避免全局状态；
- 谨慎使用 *Singleton*，仅用于我们代码中少数全局方面；
- 优先使用具有单向数据流的 *Singletons*；
- 使用 *Strategy* 设计模式反转对 *Singleton* 的依赖，以消除对可变性和可测试性的通常阻碍。

1 Steve McConnell: “Code Complete: A Practical Handbook of Software Construction”，第 2 版，Microsoft Press，2004 年 7 月，ISBN: 978-0735619678。

2 “There can be only one” 是 1986 年电影“Highlander”的宣传语，主演为 Christopher Lambert（Wikipedia）。

3 Meyers’ Singleton 在 Scott Meyers 的“Effective C++，第三版：55 种改进程序和设计的特定方法”，Addison-Wesley，2005 年 5 月，ISBN: 978-0321334879 的第 4 条中进行了说明。

4 我知道显式处理拷贝和移动赋值运算符似乎有些过度，但这给了我提醒你关于 Rule of 5 的机会。

5 这种行为在 C++20 中已经改变，因为用户声明的任何构造函数现在足以使一个类型成为非聚合类型。

6 为了精确并避免投诉：在静态局部变量为零或常量初始化的情况下，初始化可以在进入函数之前发生。在我们的示例中，变量确实是在第一次通过时创建的。

7 事实上，Singleton 的天真实现本身就会产生大量人为依赖；参见“Guideline 38: Design Singletons for Change and Testability”

8 不展开细节，我认为还有几种所谓的“设计模式”，属于实现模式的范畴，例如 Monostate 模式、Memento 模式，或 RAII 惯用法，Wikipedia 也将其列为设计模式。虽然这在 C++ 以外的语言中可能有意义，但 RAII 的意图最肯定不是减少依赖，而是自动化清理并封装责任。

9 另一条类似的建议是 Peter Muldoon 在 CppCon 2020 的演讲“Retiring the Singleton Pattern: Concrete Suggestions for What to use Instead”（Youtube），它提供了许多有用的技术，说明如何在代码库中处理 Singletons。

10 如果 Singleton 用于任何其他用途，你应该非常怀疑并将其视为 Singleton 模式的误用。

11 Michael Feathers: “Working Effectively with Legacy Code”，Addison-Wesley，第 14 版，2013 年 1 月，ISBN: 978-0131177055。

12 我所知对 SIOF 最好的总结是由 Jonathan Müller 在其同名 Meeting C++ 2020 演讲（YouTube）中给出的。

13 “Globals are bad, m’kay?”，如 Guy Davidson 和 Kate Gregory 在“Beautiful C++: 30 Core Guidelines for Writing Clean, Safe, and Fast Code”，Addison Wesley，2021 年 12 月，ISBN: 978-0137647842 中所述。

14 Monostate 模式，据我所知首次在 1996 年 C++ 报告 9 月刊的文章“Monostate Classes: The Power of One”中由 Steve Ball 和 John Crawford 提出（参见“More C++ Gems”，Cambridge University Press，2000 年 2 月，ISBN: 978-0521786188）。它也在 Martin Reddy 的“API Design for C++”，Morgan Kaufmann，2011 年 2 月，ISBN: 978-0123850034 中有所描述。Monostate 与 Singleton 相比，允许类型的任意数量实例，但确保所有实例只有一个状态。因此，该模式不应与 std::monostate 混淆，后者用作 std::variant 中的一种良好行为的空替代。

15 Miško Hevery，“Root Cause of Singletons”，2008 年 8 月，博客。

16 有关不同种类测试替身的解释，请参阅 Martin Fowler 的文章“Mocks Aren’t Stubs”（martinfowler.com）。有关如何在 C++ 中使用这些的示例，请参阅 Jeff Langr 的“Modern C++ Programming with Test-Driven Development”，O’Reilly，2013 年 10 月，ISBN: 978-1937785482。

17 但我确信你不会因此而放弃编写测试，尽管它很困难。

18 这也是 Robert C. Martin 的“Clean Architecture”，Addison-Wesley，2017 年 9 月，ISBN: 978-0134494166 中的有力论点之一。

19 对于设计模式专家，我应该明确指出，std::pmr::get_default_resource() 函数本身就实现了另一种设计模式的意图：Facade 设计模式。不幸的是，本书中我没有详细介绍 Facade。

20 将公共接口与私有实现分离是 Template Method 设计模式的一个例子。不幸的是，在本书中我无法详细介绍这种设计模式的诸多好处。
