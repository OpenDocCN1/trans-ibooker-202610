alternatively forward to a member function of the GeomType shape (which does not have to be named draw()!), or you could forward to a free function of the shape. You just need to make sure that you do not impose artificial requirements on either ShapeModel or the ShapeConcept abstraction. Either way, any type used to instantiate ShapeModel must fulfill these requirements in order to make the code compile.

# NOTE

From a design perspective, building on a member function would introduce a more restrictive requirement on the given type and therefore introduce stronger coupling. Building on a free function, however, would enable you to invert dependencies, similar to the use of the Strategy design pattern (see “Guideline 9: Pay Attention to the Ownership of Abstractions”). In case you prefer the free function approach, just remember “Guideline 8: Understand the Semantic Requirements of Overload Sets”).

“Isn’t ShapeModel some kind of generalization of the initial Circle and Square classes? The ones that were also holding a std::function instance?” Yes, this is an excellent realization. Indeed, you could say that ShapeModel is kind of a templated version of the initial shape classes. For this reason it helps to reduce the boilerplate code necessary to introduce the Strategy behavior and improves the implementation with respect to the DRY principle (see “Guideline 2: Design for Change”). However, you gain a lot more: for instance, since ShapeModel is already a class template, you can easily switch from the current runtime Strategy implementation to a compile time Strategy implementation (i.e. Policy-Based Design; see “Guideline 19: Use Strategy to Isolate How Things are Done”):

```
template< typename ShapeT
, typename DrawStrategy > ①
class ShapeModel : public ShapeConcept
{
public:
    explicit ShapeModel( ShapeT shape, DrawStrategy drawer )
        : shape_{ std::move(shape) }
        , drawer_{ std::move(drawer) }
```