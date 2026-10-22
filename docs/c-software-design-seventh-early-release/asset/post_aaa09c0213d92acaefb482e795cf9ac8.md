
etc.), it has the opportunity to use *Type Erasure* to literally erase the type of the deleter, i.e. to remove any kind of possible dependency.

## The Type Erasure Design Pattern Explained

“Wow, that truly sounds intriguing. This makes me even more excited to learn about *Type Erasure*“. Ok then, here we go. However, please do not expect any magic or any revolutionary new idea. *Type Erasure* is nothing but a compound design pattern, meaning that it is a very clever and elegant combination of three other design patterns. The three design patterns of choice are *External Polymorphism* (which is the key ingredient for achieving the decoupling effect and for the non-intrusive nature of *Type Erasure*; see “Guideline 31: Use External Polymorphism for Non-Intrusive Runtime Polymorphism”), *Bridge* (which proves to be key to create a value semantics based implementation; see “Guideline 28: Build Bridges to Remove Physical Dependencies”), and (optionally) *Prototype* (which is required to deal with the copy semantics of the resulting values; see “Guideline 30: Apply Prototype for Abstract Copy Operations”). These three design patterns form the core of *Type Erasure*, but of course, keep in mind that different interpretations and implementations exist, mainly to adapt to specific contexts. The point of combining these three design patterns is to to create a wrapper type, which represents a loosely coupled, non-intrusive abstraction:

### THE *TYPE ERASURE* COMPOUND DESIGN PATTERN

Intent: “Provide a value-based, non-intrusive abstraction for an extendable set of unrelated, potentially non-polymorphic types with the same semantic behavior.”

The purpose of this formulation is to be as short as possible, but as precise as necessary. However, every detail of this intent carries meaning. Thus it may be helpful to elaborate:

- **Value-based**: the intent of *Type Erasure* is to create value types, i.e. types that may be copyable, movable, and, most importantly, are easily reasoned about. However, such a value type is not of the same quality as a regular value type, there are some limitations. In particular, *Type Erasure* works best for unary operations, but has its limits for binary operations.
- **Non-intrusive**: the intent of *Type Erasure* is to create an external, non-intrusive abstraction based on the example set by the *External Polymorphism* design pattern. All types providing the behavior expected by the abstraction are automatically supported, without the need to apply any modifications to them.
- **Extendable, unrelated set of types**: *Type Erasure* is firmly based on object-oriented principles, i.e. it enables you to add types easily. These types, though, should not be connected in any way. That includes that they do not have to share common behavior via some base class. Instead it should be possible to add any fitting type, without any intrusive measure, to this set of types.
- **Potentially non-polymorphic**: as demonstrated with the *External Polymorphism* design pattern, types should not have to buy into the set by means of inheritance. They should also not have to provide virtual functionality on their own, but they should be decoupled from their polymorphic behavior. However, of course types with base classes or virtual functions are not excluded.
- **Same semantic behavior**: the goal is not to provide an abstraction for all possible types, but to provide a semantic abstraction for a set of types that provide the same operations (including same syntax) and adhere to some expected behavior according to the *Liskov Substitution Principle (LSP)* (see “Guideline 6: Adhere to the Expected Behavior of Abstractions”). If possible, for any type that does not provide the expected functionality, a compile time error should be created.

With this formulation of the intent in mind, let us take a look at the dependency graph of *Type Erasure* (see Figure 8-1). The graph should look very familiar, as the structure of the pattern is dominated by the inherent structure of the *External Polymorphism* design pattern (see Figure 7-8). The most important difference and addition is the `Shape` class on the highest level of the architecture. This class serves as a wrapper around the external hierarchy introduced by *External Polymorphism*. Primarily, since this external hierarchy will not be used directly anymore, but also to reflect the fact that they are storing, say “owning”, a concrete type, the name of the `ShapeModel` class has been adapted to `OwningShapeModel`.

```
class ShapeConcept
{
    // ...
};

template< typename ShapeT
        , typename DrawStrategy >
class OwningShapeModel
    : public ShapeConcept
{
    // ...
};
```

```
class Shape
{
public:
    template< typename ShapeT, typename DrawStrategy >
    Shape( ShapeT const&, DrawStrategy );
    // ...
private:
    std::unique_ptr<ShapeConcept> pimpl_;
};

void draw( Shape const& shape );
```

High-level

Architectural Boundary

```
class Circle
{
    // ...
};
```

```
class Square
{
    // ...
};
```

Architectural Boundary

```
class OwningShapeModel<Circle,OpenGLDrawStrategy>
{
    // ...
};
```

```
class OwningShapeModel<Square,OpenGLDrawStrategy>
{
    // ...
};
```

```
class OpenGLDrawStrategy
{
public:
    void operator()( Circle const& circle ) const;
    void operator()( Square const& square ) const;
    // ...
};
```

Low-level

Figure 8-1. Dependency graph for the Type Erasure design pattern.

## An Owning Type Erasure Implementation

Ok, but now, with the structure of Type Erasure in mind, let us take a look at the implementation details of Type Erasure. Still, despite the fact that you have seen all ingredients in action before, the implementation details are not particularly beginner-friendly and not for the faint-hearted. And that is despite the fact that I have picked the simplest Type Erasure implementation I am aware of. Therefore I will try to keep everything at a reasonable level and will not stray too much into the realm of implementation details. Among other things, this means that I will not try to squeeze out every tiny bit of performance. For instance, I will not use forwarding references and I will not avoid dynamic memory allocations. Also, I will favor readability and code clarity. While this may be a disappointment to you, I believe that will save us a lot of headache. However, in case you want to dig deeper into the implementation details and optimization options, I recommend to take a look at “Guideline 33: Know about the Optimization Potential of Type Erasure”.

We again start with the Circle and Square classes:

```
//---- <Circle.h> ----------------

class Circle
{
public:
    explicit Circle( double radius )
        : radius_( radius )
    {}

    double radius() const { return radius_; }
    /* Several more getters and circle-specific utility functions */

private:
    double radius_;
    /* Several more data members */
};
```

```
//---- <Square.h> ----------------

class Square
{
public:
    explicit Square( double side )
        : side_( side )
    {}

    double side() const { return side_; }
    /* Several more getters and square-specific utility functions */

private:
    double side_;
    /* Several more data members */
};
```

These two classes have not changed since we last encountered them in the discussion of External Polymorphism. But it still pays off to again stress, that these two are completely unrelated, i.e. do not know about each other, and most importantly are non-polymorphic, meaning that they do not inherit from any base class or introduce virtual function on their own.

We have also seen the ShapeConcept and OwningShapeModel classes before, the latter under the name ShapeModel:

```
//---- <Shape.h> ----------------

#include <memory>
#include <utility>

namespace detail {

class ShapeConcept ①
{
public:
    virtual ~ShapeConcept() = default;
    virtual void draw() const = 0;  ②
    virtual std::unique_ptr<ShapeConcept> clone() const = 0;  ③
};
