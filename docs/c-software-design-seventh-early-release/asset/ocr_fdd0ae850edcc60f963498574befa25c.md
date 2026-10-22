switching from *reference semantics* to *value semantics*. All we need is a value type, a wrapper around the external hierarchy introduced by `ShapeConcept` and `OwningShapeModel`, that handles all the details that we do not want to perform manually: the instantiation of the `OwningShapeModel` class template, managing pointers, performing allocations, and dealing with lifetime. This wrapper is given in form of the `Shape` class:

```
//---- <Shape.h> ----------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )  10
    {
        using Model =
        detail::OwningShapeModel<ShapeT,DrawStrategy>;  11
        pimpl_ = std::make_unique<Model>( std::move(shape)  12
                        , std::move(drawer) );
    }
    // ...
private:
    // ...
    std::unique_ptr<detail::ShapeConcept> pimpl_;  13
};
```

The first, and perhaps most important, detail about the `Shape` class is the templated constructor (10). As the first argument, this constructor takes any kind of shape (called `ShapeT`) and as the second argument the desired `DrawStrategy`. In order to simplify the instantiation of the according `detail::OwningShapeModel` class template, it proves to be helpful to use a convenient type alias (11). This alias is used to instantiate the