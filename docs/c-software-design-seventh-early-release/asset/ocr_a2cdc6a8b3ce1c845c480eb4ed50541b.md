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
```