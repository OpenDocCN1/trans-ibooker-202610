```
std::unique_ptr<DrawSquareStrategy> drawer_;
};
```

For the Circle class, you have to introduce the DrawCircleStrategy base class (①), for the Square class it is the DrawSquareStrategy (②) base class. And with the addition of a Triangle class, you will also have to add a DrawTriangleStrategy base class. Only this way, you can properly separate concerns and still allow everyone to add new types and new implementations for the drawing of shapes.

With this functionality in place, you can easily implement new Strategy classes for drawing circles, squares, and eventually triangles. As an example, consider the OpenGLCircleStrategy, which implements the DrawCircleStrategy interface:

```
//---- <OpenGLCircleStrategy.h> -------------

#include <Circle.h>
#include <DrawCircleStrategy.h>
#include /* OpenGL graphics library */

class OpenGLCircleStrategy : public DrawCircleStrategy
{
public:
    explicit OpenGLCircleStrategy( /* Drawing related arguments */
    );

    void draw( Circle const& circle, /*...*/ ) const override;

private:
    /* Drawing related data members, e.g. colors, textures, ... */
};
```

In Figure 5-5 you can see the dependency graph for the Circle class. Note that Circle and the DrawCircleStrategy classes are on the same architectural level. Even more noteworthy is the cyclic dependency between them: Circle depends on the DrawCircleStrategy, but the DrawCircleStrategy also depends on Circle. But do not worry: although this may look like a problem at first sight, it is not. It is a