

## 表 4-1. 不同编程范式的优缺点

| 编程范式 | 优势 | 劣势 |
|---|---|---|
| 程序式编程 | 添加操作 | 添加（多态）类型 |
| 面向对象编程 | 添加（多态）类型 | 添加操作 |

请注意这些优势：基于对代码库演进的预期，选择合适的设计方法以便于扩展。不要忽视劣势，也不要让自己陷入不幸的维护地狱。

我假设到目前为止，你在想是否可能拥有两个*开放集合*。据我所知，这并非不可能，但通常不切实际。例如，在“准则 18：谨防访问者模式的性能”中，我将向你展示性能可能会显著下降。

由于你可能喜欢基于模板的编程和类似的编译时努力，我还应明确指出，静态多态并不具有相同的限制。虽然在动态多态中，设计轴之一（类型和操作）需要固定，但在静态多态中，这两方面的信息在编译时都可用。因此，如果你做得正确，两方面都可以轻松扩展。⁵

## 准则 15：为类型的添加或操作的添加而设计

- 了解不同编程范式的优缺点；
- 利用范式的优势，但避免其劣势；
- 理解动态多态中添加类型或操作之间的选择；
- 当你主要想要添加类型时，优先采用面向对象的解决方案；
- 当你主要想要添加操作时，优先采用程序式/函数式解决方案。

## 准则 16：使用访问者以扩展操作

在上一节你看到了面向对象编程（OOP）的优势是添加类型，而其劣势是添加操作。但当然 OOP 对此劣势有一个答案：*Visitor* 设计模式。

*Visitor* 设计模式是由四人帮（GoF）描述的经典设计模式之一。其重点在于让你能够频繁地添加操作而不是类型。让我通过之前的玩具示例——绘制形状——来解释 *Visitor* 设计模式。

在图 4-1 中你可以看到 `Shape` 层次结构。`Shape` 类再次是若干具体形状的基类。在此示例中，只有两个类 `Circle` 和 `Square`，但当然可以有更多形状。此外，你可以想象 `Triangle`、`Rectangle` 或 `Ellipse` 类。

![](img/7b1b8ee332752e5702cc436a0cba7c1c_180_0.png)

图 4-1. 形状层次结构的 UML 表示，包含两个派生类（Circle 和 Square）。

### 分析设计问题

假设你确定已经拥有了你所需要的所有形状。那就是说，你认为形状的集合是一个封闭集合。你所缺少的，是额外的操作。例如，你

缺少一个旋转形状的操作。此外，你希望序列化形状，即将形状的实例转换为字节。当然你也想绘制形状。此外你希望任何人都能添加新操作。因此你期望操作集合是开放的。⁶

每新增一个操作，现在就需要你在基类中插入一个新的虚函数。不幸的是，这在不同方面都可能很麻烦。最明显的是，并非每个人都能向 Shape 基类添加虚函数。例如，我就不能简单地去更改你的代码。因此这种方法无法满足“每个人都能添加操作”的期望。虽然你可能已经将其视为最终的负面结论，但让我们仍然更详细地分析虚函数的问题。

如果你决定使用纯虚函数，你就必须在每个派生类中实现该函数。对于你自己的派生类型，你可以将其视为只是多一点额外工作。但你也可能给其他从 Shape 基类继承创建了形状的人造成额外工作。⁷ 而这正是可以预期的，因为这是 OOP 的优势：任何人都可以轻松添加新类型。由于这是可以预期的，这可能是一个不使用纯虚函数的原因。

作为替代方案，你可以引入一个普通的虚函数，即带默认实现的虚函数。虽然 rotate() 函数的默认行为听起来像是一个非常合理的想法，但 serialize() 函数的默认实现听起来就完全不那么容易了。我承认我自己也需要仔细思考如何实现这样的函数。你现在可能会建议默认情况下抛出异常。然而，这本质上意味着派生类必须再次实现缺失的行为，它只不过是伪装的纯虚函数。或者是对里氏替换原则（LSP）的明显违反（参见“准则 6：遵守抽象的预期行为”）。

无论如何，向 Shape 基类添加新操作都是困难的，甚至根本不可能。根本原因是添加虚函数违反了开放封闭原则（OCP）。如果你真的需要频繁添加新操作，那么你应该设计成使操作的扩展变得容易。而这正是 *Visitor* 设计模式试图实现的目标。

### Visitor 设计模式详解

> **THE *VISITOR* DESIGN PATTERN**
> 
> Intent: “Represent an operation to be performed on the elements of an object structure. *Visitor* lets you define a new operation without changing the classes of the elements on which it operates.” (GoF)

除了 `Shape` 层次结构，我们现在在图 4-2 左侧引入了 `ShapeVisitor` 层次结构。`ShapeVisitor` 基类代表了形状操作的抽象。因此，你可以认为 `ShapeOperation` 可能是该类的更好名称。然而，应用“准则 14：使用设计模式的名称来传达意图”是有益的。*Visitor* 这个名称将帮助他人理解设计。

![](img/7b1b8ee332752e5702cc436a0cba7c1c_183_0.png)

图 4-2. Visitor 设计模式的 UML 表示。

ShapeVisitor 基类为 Shape 层次结构中的每个具体形状提供一个纯虚 visit() 函数：

```
class ShapeVisitor
{
public:
virtual ~ShapeVisitor() = default;

virtual void visit( Circle const&, /*...*/ ) const = 0; ①
virtual void visit( Square const&, /*...*/ ) const = 0; ②
// Possibly more visit() functions, one for each concrete shape
};
```

在此示例中，有一个 visit() 函数用于 Circle（①）和一个用于 Square（②）。当然可能有更多 visit() 函数，例如一个用于 Triangle，一个用于 Rectangle，一个用于 Ellipse，前提是这些也是从 Shape 基类派生的类。

有了 ShapeVisitor 基类，你现在可以轻松添加新操作。你要做的只是添加一个新派生类。例如，为了启用旋转形状，你可以引入 Rotate 类并实现所有 visit() 函数。为了启用绘制形状，你所要做的就是引入一个 Draw 类：

```
class Draw : public ShapeVisitor
{
public:
void visit( Circle const& c, /*...*/ ) const override;
void visit( Square const& s, /*...*/ ) const override;
// Possibly more visit() functions, one for each concrete shape
};
```

你还可以考虑为每个需要支持的图形库引入多个 Draw 类。你可以轻松做到这一点，因为你

不需要修改任何*现有代码*。只需通过添加*新代码*来扩展 `ShapeVisitor` 层次结构即可。因此该设计在添加操作方面满足了 *OCP*。

为了完全理解 *Visitor* 的软件设计特性，重要的是要理解为什么 *Visitor* 设计模式能够满足 *OCP*。最初的问题是，每个新操作都需要对 `Shape` 基类进行更改。*Visitor* 将操作的添加识别为*变化点*。通过提取这个*变化点*，即将其作为单独的类，你遵循了*单一职责原则*（*SRP*）：`Shape` 不必为每个新操作而更改。这避免了对 `Shape` 层次结构的频繁修改，并使新操作的轻松添加成为可能。*SRP* 因此成为 *OCP* 的使能者。

为了能够在形状上使用访问者（即从 `ShapeVisitor` 基类派生的类），你现在必须向 `Shape` 层次结构添加最后一个函数：`accept()` 函数（③）：⁸

```
class Shape
{
  public:
    virtual ~Shape() = default;
    virtual void accept( ShapeVisitor const& v ) = 0;  ③
    // ...
};
```

`accept()` 函数作为纯虚函数引入到基类中，因此必须在每个派生类中实现（④ 和 ⑤）：

```
class Circle : public Shape
{
  public:
    explicit Circle( double radius )
      : radius_( radius )
    {
      /* Checking that the given radius is valid */
    }
```
