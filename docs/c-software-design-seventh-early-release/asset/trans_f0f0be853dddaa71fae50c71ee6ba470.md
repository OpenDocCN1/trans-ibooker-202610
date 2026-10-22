

Person 构造函数通过 std::make_unique() ⑮ 初始化 pimpl_ 数据成员。当然，这涉及一次动态内存分配，这意味着动态内存需要再次被清理。“这就是我们使用 std::unique_ptr 的原因”，你说。正确。但也许令人惊讶的是，虽然我们出于该目的使用 std::unique_ptr，但仍然有必要手动处理析构函数 ⑯。“为什么我们非要这么做？std::unique_ptr 的意义不就是我们不需要处理清理吗？”好吧，我们仍然需要。让我解释一下：如果不编写析构函数，编译器会觉得有义务为你生成析构函数。不幸的是，它会在 <Person.h> 头文件中生成析构函数。Person 的析构函数会触发 std::unique_ptr 数据成员的析构函数的实例化，而这反过来又需要 Impl 类的析构函数的定义。然而，Impl 的定义在头文件中不可用。相反，它需要在源文件中定义，否则 Bridge 的目的就被破坏了。因此编译器会发出关于不完整类型 Impl 的错误。幸运的是，你不必放弃 std::unique_ptr 来解决这个问题（事实上，你不应该放弃它）。问题其实很简单就能解决。你所要做的就是将 Person 析构函数的定义移到源文件：你在类定义中声明析构函数，并在源文件中通过 =default 定义它。

由于 std::unique_ptr 不能被复制，你将不得不实现拷贝构造函数以保留 Person 类的拷贝语义 ⑰。拷贝赋值运算符 ⑱ 也是如此。请注意，该运算符的实现基于这样一个假设，即 Person 的每个实例始终都有一个有效的 pimpl_。这一假设解释了移动构造函数的实现：它不是简单地移动 std::unique_ptr，而是通过 std::make_unique() 执行一次可能失败的，即会抛出的动态内存分配。因此，它没有被声明为 noexcept ⑲。³ 这一假设也解释了为什么 pimpl_ 数据成员被声明为 const：一旦初始化，指针将不再被更改，甚至在移动操作中也不会更改，包括移动赋值运算符 (20)。

最后值得注意的一个细节是，year_of_birth() 成员函数的定义位于源文件中 (21)。尽管这个简单的 getter 函数是很好的内联候选，但定义必须移到源文件。原因是头文件中 Impl 是一个不完整类型。这意味着在头文件中，你无法访问任何成员（数据和函数）。这只有在源文件中，或者更一般地说，只要编译器知道 Impl 的定义时才有可能。

## Bridge 与 Strategy 的比较

“我有一个问题”，你说。“我看到 Bridge 和 Strategy 设计模式之间有很强的相似性。我知道你说过设计模式在结构上有时非常相似，唯一的区别是它们的意图。但这两者到底有什么区别？”4 我理解你的问题。这两者之间的相似性确实有点令人困惑。然而，有一件事可以帮助你区分它们：相关数据成员的初始化方式，是判断你使用的是哪一个的强有力指标。

如果一个类不想知道某些实现细节，并且出于这个原因，它提供了通过从外部传入细节来配置行为的机会，例如通过构造函数或 setter 函数，那么你很可能是在处理 Strategy 设计模式。因为灵活配置行为，即减少逻辑依赖，是其主要关注点，Strategy 属于行为型设计模式。例如，在下面的代码片段中，Database 类的构造函数是一个明显的迹象：

```
class DatabaseEngine
{
    public:
        virtual ~DatabaseEngine() = default;
```

```
// ... Many database-specific functions
};

class Database
{
public:
    explicit Database( std::unique_ptr<DatabaseEngine> engine );
    // ... Many database-specific functions

private:
    std::unique_ptr<DatabaseEngine> engine_;
};

// The database is unaware of any implementation details and
// requests them
//   via its constructor from outside -> Strategy design pattern
Database::Database( std::unique_ptr<DatabaseEngine> engine )
    : engine_{ std::move(engine) }
{}
```

`DatabaseEngine` 的实际类型是从外部传入的 (22)，这使得它成为 *Strategy* 设计模式的一个很好的例子。

图 7-3 显示了此示例的依赖图。最重要的是，`Database` 类与 `DatabaseEngine` 抽象处于同一架构级别，从而为其他人提供了实现行为的机会（例如以 `ConcreteDatabaseEngine` 的形式）。由于 `Database` 只依赖于抽象，因此不存在对任何特定实现的依赖。

```
class Database
{
public:
Database( std::unique_ptr<DatabaseEngine> engine );

// ... Many data-base specific functions
};
```

```
class DatabaseEngine
{
public:
virtual ~DatabaseEngine() = default;

// ... Many data-base specific functions
};
```

```
class ConcreteDatabaseEngine : public DatabaseEngine
{
public:
// ... Many data-base specific functions
};
```

High-level

Architectural
Boundary

Inversion of
dependencies

Low-level

Figure 7-3. Dependency graph for the Strategy design pattern.

如果一个类知道实现细节，但主要想减少对这些细节的物理依赖，那么你很可能是在处理 Bridge 设计模式。在这种情况下，类不会提供任何从外部设置指针的机会，即指针更像是一个实现细节，并在内部设置。由于

![](img/7b1b8ee332752e5702cc436a0cba7c1c_399_0.png)

Bridge 设计模式主要关注对实现细节的物理依赖，而非逻辑依赖，Bridge 属于结构型设计模式。作为示例，请考虑以下代码片段：

```
class Database
{
public:
  explicit Database();
  // ...
private:
  std::unique_ptr<DatabaseEngine> pimpl_;
};

// The database knows about the required implementation details,
but does
//   not want to depend too strongly on it -> Bridge design
pattern
Database::Database()
  : pimpl_{ std::make_unique<ConcreteDatabaseEngine>( /*some arguments*/ ) }  23
{}
```

再次，Bridge 设计模式应用的一个明显迹象是：构造函数不是从外部接受 engine，而是 Database 类的构造函数知道 ConcreteDatabaseEngine 并在内部设置它 (23)。

图 7-4 显示了 Database 示例的 Bridge 实现的依赖图。最值得注意的是，Database 类与 ConcreteDatabaseEngine 类处于同一架构级别，因此不给其他人提供不同实现的任何机会。这表明，与 Strategy 设计模式相反，Bridge 在逻辑上与特定实现耦合，但仅通过 DatabaseEngine 抽象在物理上解耦。

## High-level

```
class DatabaseEngine
{
public:
    virtual ~DatabaseEngine() = default;

    // ... Many data-base specific functions
};
```

## Architectural
Boundary

Inversion of dependencies

```
class Database
{
public:
    Database();

    // ... Many data-base specific functions
};
```

```
class ConcreteDatabaseEngine : public DatabaseEngine
{
public:
    // ... Many data-base specific functions
};
```

## Low-level

Figure 7-4. Dependency graph for the Bridge design pattern.

## 分析 Bridge 设计模式的不足

“我完全可以看到为什么 Bridge 设计模式在社区中如此受欢迎。解耦特性真的很棒！”，你惊呼道。“不过，你一直告诉我每个设计都有利有弊。我猜会有性能损失？”很好，你记得总是有一些缺点。当然这也包括 Bridge 设计模式，尽管它被证明非常有用。是的，你正确地假设其中存在一些性能开销。

五种开销类型中的第一种源于 Bridge 引入了额外的间接：pimpl 指针。由于这个指针，对实现细节的所有访问都会变得更昂贵。然而，这个指针造成的性能损失有多大，是一个我将在“准则 29：注意 Bridge 的性能增益和损失”中单独讨论的问题。这并不是唯一的性能开销来源，还有更多：取决于你是否使用抽象，你可能还必须为虚函数调用开销买单。此外，由于无法内联访问数据成员的最简单函数，你也必须付出更多代价。当然，每当你创建一个以 Bridge 实现的类的新实例时，你还必须为一次额外的动态内存分配买单。⁵ 最后但同样重要的是，你还应考虑到引入 pimpl 指针造成的内存开销。因此，是的，隔离物理依赖和隐藏实现细节并非免费，而是会产生相当大的开销。尽管如此，这不应成为普遍抛弃 Bridge 解决方案的理由：这始终取决于具体情况。例如，如果底层实现执行缓慢、昂贵的任务，例如系统调用，那么这种开销可能根本无法测量。换句话说，是否使用 Bridge 应该逐案决定并以性能基准测试为依据。

