```
//---- <Shape.h> ----------------

#include <cstddef>
#include <memory>

class Shape
{
public:
    // ...

private:
    // ...

    template< typename ShapeT
            , typename DrawStrategy >
    struct OwningModel ⑮
    {
        OwningModel( ShapeT value, DrawStrategy drawer )
            : shape_( std::move(value) )
            , drawer_( std::move(drawer) )
        {}

        ShapeT shape_;
        DrawStrategy drawer_;
    };

    using DestroyOperation = void(void*);      ⑯
    using DrawOperation    = void(void*);      ⑰
    using CloneOperation   = void*(void*);     ⑱

    std::unique_ptr<void,DestroyOperation*> pimpl_;  ⑲
    DrawOperation* draw_ { nullptr };            ⑳
    CloneOperation* clone_ { nullptr };          ㉑
};
```

Since we are replacing all virtual functions, even the virtual destructor, there is no need for a Concept base class anymore. So consequently, the external hierarchy is reduced to just the OwningModel class template (⑮), which still acts as storage for a specific kind of shape (i.e. ShapeT) and DrawStrategy. Still, it meets the same fate: all virtual functions are removed. The only remaining details are the constructor and the data members.