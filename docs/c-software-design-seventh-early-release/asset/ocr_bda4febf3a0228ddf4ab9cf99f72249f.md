```
{ public:
    explicit Square( double side )
        : side_( side )
    {
        /* Checking that the given side length is valid */
    }

    double side () const { return side_; }
    Point center() const { return center_; }

    void draw() const override;

private:
    double side_;
    Point center_{};
};
```

```
//---- <Square.cpp> ----------------

#include <Square.h>
#include /* some graphics library */

void Square::draw() const
{
    // ... Implementing the logic for drawing a square
}
```

Once the virtual draw() function is in place and implemented by all derived classes, it can be used to refactor the drawAllShapes() function.

```
//---- <DrawAllShapes.h> ----------------

#include <memory>
#include <vector>
class Shape;

void drawAllShapes( std::vector< std::unique_ptr<Shape> > const&
shapes );
```

```
//---- <DrawAllShapes.cpp> ----------------
```