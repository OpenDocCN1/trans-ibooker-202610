```
//---- <DrawSquare.h> -------------------

class Square;

void draw( Square const& );

//---- <DrawSquare.cpp> -------------------

#include <DrawSquare.h>
#include <Square.h>
#include /* some graphics library */

void draw( Square const& s )
{
    // ... Implementing the logic for drawing a square
}
```

The Square class looks very similar to the Circle class (9). The major difference is that a Square initializes its base class by means of the square enumerator (10).

With both circles and squares available, we now want to draw an entire vector of different shapes. For that reason we introduce the drawAllShapes() function:

```
//---- <DrawAllShapes.h> -------------------

#include <memory>
#include <vector>
class Shape;

void drawAllShapes( std::vector<std::unique_ptr<Shape>> const&
shapes );  11
```

```
//---- <DrawAllShapes.cpp> -------------------

#include <DrawAllShapes.h>
#include <Circle.h>
#include <Square.h>

void drawAllShapes( std::vector<std::unique_ptr<Shape>> const&
shapes )
```