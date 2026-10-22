particular true, because you have already seen how to manually implement the virtual dispatch in “Guideline 33: Know about the Optimization Potential of Type Erasure”. With this technique a simple, non-owning Type Erasure implementation is just a matter of a few lines of code:

```
//---- <Shape.h> ----------------

#include <memory>

class ShapeConstRef
{
public:
    template< typename ShapeT, typename DrawStrategy >
    ShapeConstRef( ShapeT& shape, DrawStrategy& drawer ) ⑥
        : shape_{ std::addressof(shape) }
        , drawer_{ std::addressof(drawer) }
        , draw_{ []( void const* shapeBytes, void const* drawerBytes ) {
            auto const* shape = static_cast<ShapeT const*>(shapeBytes);
            auto const* drawer = static_cast<DrawStrategy const*>(drawerBytes);
            (*drawer)( *shape );
        } }
    {}

private:
    friend void draw( ShapeConstRef const& shape )
    {
        shape.draw_( shape.shape_, shape.drawer_ );
    }

    using DrawOperation = void( void const*,void const* );

    void const* shape_{ nullptr };   ③
    void const* drawer_{ nullptr };  ④
    DrawOperation* draw_{ nullptr }; ⑤
};
```

As the name suggests, the ShapeConstRef class represents a reference to a const shape type. Instead of storing a copy of the given shape, it only holds a pointer to it in form of a void* (③). In addition, it holds a void*