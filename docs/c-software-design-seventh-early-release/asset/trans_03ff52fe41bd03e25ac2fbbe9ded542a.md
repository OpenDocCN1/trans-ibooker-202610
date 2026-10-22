

```cpp
template< typename ShapeT
, typename DrawStrategy >
class OwningShapeModel : public ShapeConcept
{
public:
    explicit OwningShapeModel( ShapeT shape, DrawStrategy drawer )
        : shape_{ std::move(shape) }
        , drawer_{ std::move(drawer) }
    {}

    void draw() const override { drawer_(shape_); }

    std::unique_ptr<ShapeConcept> clone() const override
    {
        return std::make_unique<OwningShapeModel>( *this );
    }

private:
    ShapeT shape_;
    DrawStrategy drawer_;
};

} // namespace detail
```

除了名称更改之外，还有一些其他重要的不同。例如，这两个类都已移至 detail 命名空间。命名空间的名称表明这两个类现在正成为实现细节，即它们不再 intended 用于直接使用。ShapeConcept 类仍然引入纯虚函数 draw() 来表示绘制形状的要求。此外，ShapeConcept 现在还引入了纯虚 clone() 函数。“我知道这是什么，这是原型设计模式！”，你解释道。是的，正确。名称 clone() 与原型紧密相关，是该设计模式的强烈指示（但不是保证）。然而，尽管函数名称的选择非常合理且规范，请允许我明确指出 clone() 的函数名称选择，以及 draw() 的函数名称选择，都是我们自己的：这些名称现在是实现细节，与我们从 ShapeT 类型要求的名字没有任何关系。我们同样可以把它们命名为

do_draw() 和 do_clone()，这对 ShapeT 类型没有任何影响。对 ShapeT 类型的真正要求由 draw() 和 clone() 函数的实现定义。

由于 ShapeConcept 再次成为外部层次结构的基类，draw() 函数、clone() 函数以及析构函数代表了所有形状类型的需求集合。这意味着所有形状都必须提供某种绘制行为，它们必须是可拷贝的，并且可析构的。请注意，这三个函数只是此示例的需求选择。特别是，可拷贝性并不是类型擦除所有实现都通用的要求。

OwningShapeModel 类再次代表 ShapeConcept 类的唯一实现。如前所述，OwningShapeModel 在其构造函数中接受具体的形状类型和绘制策略，并在其两个数据成员中进行初始化。由于 OwningShapeModel 继承自 ShapeConcept，它必须实现两个纯虚函数。draw() 函数通过应用给定的绘制策略来实现，而 clone() 函数则实现为返回对应 OwningShapeModel 的精确拷贝。

NOTE
如果你现在正在想：“哦不，std::make_unique()。那意味着动态内存。然后我不能在我的代码中使用它！”，不要担心。
std::make_unique() 仅仅是一个实现细节，是为了保持示例简单而做的选择。在“Guideline 33: Know about the Optimization Potential of Type Erasure”中，你将看到如何通过小缓冲区优化（SBO）来避免动态内存。

“我到目前为止完全不为之印象深刻。我们几乎没有超出外部多态设计模式的实现”。我完全理解这种批评。然而，我们距离将外部多态转变为类型擦除仅一步之遥，距离

从 *引用语义* 切换到 *值语义* 仅一步之遥。我们所需要的是一个值类型，一个围绕由 `ShapeConcept` 和 `OwningShapeModel` 引入的外部层次结构的包装器，它处理我们不希望手动执行的所有细节：`OwningShapeModel` 类模板的实例化，管理指针，执行分配，以及处理生命周期。这个包装器以 `Shape` 类的形式给出：

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
    {
        using Model =
        detail::OwningShapeModel<ShapeT,DrawStrategy>;
        pimpl_ = std::make_unique<Model>( std::move(shape)
                        , std::move(drawer) );
    }
    // ...
private:
    // ...
    std::unique_ptr<detail::ShapeConcept> pimpl_;
};
```

关于 `Shape` 类的第一个，也是也许最重要的细节是模板化构造函数。作为第一个参数，此构造函数接受任何种类的形状（称为 `ShapeT`），作为第二个参数接受期望的 `DrawStrategy`。为了简化相应的 `detail::OwningShapeModel` 类模板的实例化，使用一个方便的类型别名被证明是有帮助的。这个别名用于通过 std::make_unique() 实例化

所需的模型。形状以及绘制策略都被传递给新的模型。

新创建的模型用于初始化 Shape 类的唯一数据成员：pimpl_。“我也认识这个，这是桥接模式！”，你高兴地宣布。是的，再次正确，这是一个桥接设计模式的应用。在构造函数中，我们基于实际给定的类型 ShapeT 和 DrawStrategy 创建一个具体的 OwningShapeModel，但我们以指向 ShapeConcept 的指针形式存储它。通过这样做，你创建了一个通向实现细节的桥梁，一个通向真实形状类型的桥梁。然而，在 pimpl_ 初始化之后，在构造函数完成之后，Shape 不再记住实际类型。Shape 没有模板参数或任何会泄露其存储的具体类型的成员函数，也没有记住给定类型的数据成员。它所持有的只是指向 ShapeConcept 基类的指针。因此，它对真实形状类型的记忆被擦除了。因此该设计模式的名称：类型擦除。

我们 Shape 类中唯一缺少的是真正值类型所需的功能：拷贝和移动操作。幸运的是，由于使用了 std::unique_ptr，我们的努力非常有限。由于编译器生成的析构函数和两个移动操作都可用，我们只需要处理两个拷贝操作：

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    // ...
    Shape( Shape const& other )
        : pimpl_( other.pimpl_->clone() )
    {}
    Shape& operator=( Shape const& other )
```

```
{
    // Copy-and-Swap Idiom
    Shape copy( other );
    pimpl_.swap( copy.pimpl_ );
    return *this;
}

~Shape() = default;
Shape( Shape&& ) = default;
Shape& operator=( Shape&& ) = default;

private:
    friend void draw( Shape const& shape )
    {
        shape.pimpl_->draw();
    }

    // ...
};
```

拷贝构造函数可能是一个非常困难的函数来实现，因为我们不知道存储在另一个 Shape 中的形状的具体类型。然而，由于在 ShapeConcept 基类中提供了 clone() 函数，我们可以请求一个精确的拷贝，而无需知道任何关于具体类型的信息。实现拷贝赋值运算符最短、最无痛、最方便的方式是基于拷贝-交换惯用法构建。

此外，Shape 类提供了一个所谓的隐藏友元 draw()。这个友元函数被称为隐藏友元，因为虽然它是一个自由函数，但它是在 Shape 类的主体内部定义的。作为友元，它被授予对私有数据成员的完全访问权限，并且将被注入到周围的命名空间中。

“你不是说友元是坏的吗？”，你抱怨道。我承认，那是我在“Guideline 4: Design for Testability”中所说的。然而，我也明确指出，隐藏友元是可以的。在这种情况下，draw() 函数是 Shape 类不可分割的一部分，绝对是一个真正的友元（几乎是家庭成员）。“但那它应该是一个成员函数，对吧？”，你争辩道。的确，这将是一个有效的替代方案。如果

