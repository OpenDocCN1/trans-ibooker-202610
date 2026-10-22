The virtual functions are replaced by manual function pointers. Since the syntax for function pointers is not the most pleasant to use, we add a couple of function type aliases for our convenience:¹⁷ `DestroyOperation` represents the former virtual destructor (⑲), `DrawOperation` represents the former virtual `draw()` function (⑰), and `CloneOperation` represents the former virtual `clone()` function (⑲).
`DestroyOperation` is used to configure the `Deleter` of the `pimpl_` data member (⑲) (and yes, as such it acts as a *Strategy*). The latter two, i.e. `DrawOperation` and `CloneOperation`, are used for the two additional function pointer data members `draw_` and `clone_` (²⁰ and ²¹).

“Oh no, void*s! Isn’t that an archaic and super dangerous way of doing things?”, you gasp. Ok, I admit that without explanation it looks very suspicious. However, stay with me, I promise that everything will be perfectly fine and type-safe. The key to making this work now lies in the initialization of these function pointers. They are initialized in the templated constructor of the `Shape` class:

```
//---- <Shape.h> -------------
// ...
class Shape
{
public:
    template< typename ShapeT
            , typename DrawStrategy >
    Shape( ShapeT shape, DrawStrategy drawer )
        : pimpl_( ②
            new OwningModel<ShapeT,DrawStrategy>(
            std::move(shape)
            std::move(drawer) )
            , []( void* shapeBytes ){ ²³
                using Model = OwningModel<ShapeT,DrawStrategy>;
                auto* const model = static_cast<Model*>
                (shapeBytes); ²⁴
                delete model;  ²⁵
            } )
        , draw_( ²⁶
```