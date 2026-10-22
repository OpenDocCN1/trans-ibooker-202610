

到 *Observer* 设计模式，这是最著名且最常用的设计模式之一。我们将讨论经典的、GoF 风格的 *Observer*，同时也会讨论如何在现代 C++ 中实现 *Observers*。

在“Guideline 26: Use CRTP to Introduce Static Type Categories”中，我们将把注意力转向 *CRTP*。我将向你展示如何使用 *CRTP* 在一组相关类型之间定义编译时关系，以及如何正确实现一个 *CRTP* 基类。

在“Guideline 27: Use CRTP for Static Mixin Classes”中，我将继续 *CRTP* 的故事，展示如何使用 *CRTP* 创建编译时混入类。我们还将看到语义继承与技术继承之间的区别，前者用于创建抽象，后者则作为实现细节，仅为技术上的优雅和便利而使用。

## Guideline 24: Use Adapters to Standardize Interfaces

假设你已经实现了“Guideline 3: Separate Interfaces to Avoid Artificial Coupling”中的 `Document` 示例，并且由于你正确遵循了 *ISP*，你对它的工作方式相当满意：

```
class JSONExportable
{
public:
    // ...
    virtual ~JSONExportable() = default;

    virtual void exportToJSON( /*...*/ ) const = 0;
    // ...
};

class Serializable
{
public:
    // ...
    virtual ~Serializable() = default;
```

```
virtual void serialize( ByteStream& bs, /*...*/ ) const = 0;
// ...
};

class Document
: public JSONExportable
, public Serializable
{
public:
// ...
};
```

然而，有一天，你被要求引入 Pages 文档格式。¹ 当然，它与你已有的 Word 文档类似，但不幸的是，你对 Pages 格式的细节相当不熟悉。更糟糕的是，你没有太多时间去熟悉该格式，因为你还有太多其他事情要做。幸运的是，你知道有一个相当不错的、开源的实现可供该格式使用：OpenPages 类：

```
class OpenPages
{
public:
// ...
void convertToBytes( /*...*/ );
};

void exportToJSONFormat( OpenPages const& pages, /*...*/ );
```

好消息是，这个类提供了你所需的大部分功能：它提供了一个 convertToBytes() 成员函数来序列化文档内容，并提供了自由函数 exportToJSONFormat() 将 Pages 文档转换为 JSON 格式。不幸的是，它不符合你的接口预期：你期望的是 serialize() 成员函数，而不是 convertToBytes() 成员函数。而你期望的是 exportToJSON() 成员函数，而不是自由的 exportToJSONFormat() 函数。最终，当然第三方类没有继承你的 `Document` 基类，这意味着你无法轻松地将该类纳入现有层次结构中。然而，这个问题有解决方案：通过 *Adapter* 设计模式可以实现无缝集成。

## The Adapter Design Pattern Explained

*Adapter* 设计模式是另一个经典的 GoF 设计模式：它专注于标准化接口，并在不侵入现有继承层次结构的情况下帮助添加功能：

> **THE ADAPTER DESIGN PATTERN**
> 
> Intent: “Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn’t otherwise because of incompatible interfaces.” (GoF)

图 6-1 显示了你的 *Adapter* 场景的 UML 图：你已经有了 `Document` 基类就位（我们暂时忽略 `JSONExportable` 和 `Serializable` 接口），并且已经实现了几种不同的文档（例如通过 `Word` 类）。这个层次结构的新增部分是 `Pages` 类。

![](img/7b1b8ee332752e5702cc436a0cba7c1c_311_0.png)

Figure 6-1. The UML representation of the Adapter design pattern.

Pages 类充当对第三方 OpenPages 类的包装：

```
class Pages : public Document
{
  public:
    // ...
    void exportToJSON( /*...*/ ) const override
    {
      exportToJSONFormat(pages, /*...*/);  ①
    }

    void serialize( ByteStream& bs, /*...*/ ) const override
    {
      pages.convertToBytes(/*...*/);  ②
    }
    // ...

  private:
    OpenPages pages;  // Example of an object adapter
};
```

Pages 通过将调用转发到相应的 OpenPages 函数来实现 Document 接口：对 exportToJSON() 的调用被转发到自由函数 exportToJSONFormat() ①，而对 serialize() 的调用被转发到 convertToBytes() 成员函数 ②。

通过 Pages 类，你可以轻松地将第三方实现集成到现有层次结构中。非常轻松：你可以在不以任何方式修改它的情况下进行集成。Adapter 设计模式的这种非侵入性是你应该视为其最大优势之一：任何人都可以添加一个 Adapter 来将一个接口适配到另一个现有接口。

在此上下文中，Pages 类充当了对 OpenPages 类实际实现细节的抽象。因此，Adapter 设计模式将接口的关注点与实现细节分离开来。这很好地满足了单一职责原则（SRP），并与开放封闭原则（OCP）的意图相契合（参见“Guideline 2: Design for Change” 和 “Guideline 5: Design for Extension”）。

某种程度上，Pages Adapter 充当了一个间接层，并将一组函数映射到另一组函数。请注意，严格地从一个函数映射到恰好另一个函数并不是必要的。相反，你在如何将期望的函数集映射到可用的函数集上拥有完全的灵活性。因此 Adapter 不一定代表 1 对 1 的关系，也可以支持 1 对 N 的关系。²

## Object Adapters vs. Class Adapters

Pages 类是所谓的 object adapter 的一个例子。这个术语指的是你存储被包装类型的实例。或者，鉴于被包装的类型是继承层次结构的一部分，你也可以存储指向该层次结构基类的指针。这将允许你对属于该层次结构的所有类型使用 object adapter，从而使 object adapter 的灵活性大大提高。

相比之下，还有实现所谓的 class adapter 的选项：

```
class Pages : public Document
            , private OpenPages  // Example of a class adapter  ③
{
public:
    // ...
    void exportToJSON( /*...*/ ) const override
    {
        exportToJSONFormat(*this, /*...*/);
    }

    void serialize( ByteStream& bs, /*...*/ ) const override
    {
        this->convertToBytes(/*...*/);
    }
    // ...
};
```

而不是存储被适配类型的实例，你将从它继承（如果可能为非公开继承），并相应地实现期望的接口 ③。然而，正如在“Guideline 20: Favor Composition Over Inheritance”中所讨论的，优先使用组合构建是更好的选择。通常，*object adapters* 比 *class adapters* 更灵活，因此应该是你的首选。只有在少数情况下你会更喜欢 *class adapter*：

- 如果你必须重写一个虚函数；
- 如果你需要访问 `protected` 成员函数；
- 如果你要求被适配的类型在另一个基类之前构造；
- 如果你需要共享一个共同的虚基类或重写虚基类的构造；
- 如果你能从 *Empty Base Optimization (EBO)* 中获得 *显著* 的优势。³

否则，在大多数情况下，你应该优先选择 *object adapter*。

“我喜欢这个设计模式，它很强大。然而，我刚想起你建议在代码中使用设计模式的名称来传达意图。类不应该叫做 `PagesAdapter` 吗？” 你提出了一个极好的观点。我很高兴你还记得“Guideline 14: Use a Design Pattern’s Name to Communicate Intent”，其中我确实认为模式的名称有助于理解代码。我承认在这种情况下，我对两种命名约定都持开放态度。虽然我确实看到了 `PagesAdapter` 这个名称的优点，因为它立即传达了你基于 *Adapter* 设计模式构建的事实，但我不认为有必要传达这个类代表一个适配器的事实。对我来说，在这种情况下，*Adapter* 更像是一个实现细节：我不需要知道 `Pages` 类没有自行实现所有细节，而是使用 `OpenPages` 类来实现。那是

