
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

## High-level

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

# Moving Towards Local Dependency Injection

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

