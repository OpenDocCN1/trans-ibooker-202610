
In “Guideline 16: Use Visitors to Extend Operations” I will introduce you to the *Visitor* design pattern. I will explain its intent to extend operations instead of types, show you both the advantages and the shortcomings of the classic *Visitor* pattern.

In “Guideline 17: Consider std::variant for Implementing Visitors” you will make the acquaintance with the modern implementation of the *Visitor* design pattern. I will introduce you to `std::variant` and explain the many advantages of that particular implementation.

In “Guideline 18: Beware the Performance of Acyclic Visitors” I will introduce you to *Acyclic Visitor*. At first glance this approach appears to resolve some fundamental problems of the *Visitor* pattern, but on closer inspection we will find that the runtime overhead may disqualify this implementation.

## Guideline 15: Design for the Addition of Types or Operations

To you the term *Dynamic Polymorphism* may sound like a lot of freedom. It may sound like you felt when you were still a kid: endless possibilities, no limitations! Well, since then you have grown older and have faced reality: you cannot have everything and there is always a choice to be made. Unfortunately it is similar with *Dynamic Polymorphism*. Despite the fact that it sounds like complete freedom, there is a limiting choice: do you want to extend types or operations?

To see what I mean, let us return to the scenario from Chapter 3: we want to draw a given shape.¹ We stick to dynamic polymorphism and for our initial try implement this problem with good old procedural programming.

### A Procedural Solution

The first header file `Point.h` provides a fairly simple `Point` class. This will mainly serve to make the code complete, but also gives us the idea that we are dealing with 2D shapes:

```
//---- <Point.h> ---------------------

struct Point
{
    double x;
    double y;
};
```

The second conceptual header file Shape.h proves to be much more interesting:

```
//---- <Shape.h> ---------------------

enum ShapeType ①
{
    circle,
    square
};

class Shape ②
{
protected:
    explicit Shape( ShapeType type )
        : type_( type ) ⑤
    {}

public:
    virtual ~Shape() = default; ③

    ShapeType getType() const { return type_; } ⑥

private:
    ShapeType type_; ④
};
```

First, we introduce the enumeration ShapeType, which currently lists the two enumerators circle and square (①). Apparently, we are initially dealing with only circles and squares. Second, we introduce the class Shape (②). Given the protected constructor and the virtual destructor (③), you can anticipate that Shape is supposed to work as a base class. But that is not the surprising detail about Shape: Shape has a data member of type ShapeType ④. This data member is initialized via the constructor ⑤ and can be queried via the getType() member function ⑥. Apparently a Shape stores its type in the form of the ShapeType enumeration.

One example for the use of the Shape base class is the Circle class:

```
//---- <Circle.h> ----------------

#include <Point.h>
#include <Shape.h>

class Circle : public Shape ⑦
{
public:
    explicit Circle( double radius )
        : Shape( circle ) ⑧
        , radius_( radius )
    {
        /* Checking that the given radius is valid */
    }

    double radius() const { return radius_; }
    Point center() const { return center_; }

private:
    double radius_;
    Point center_{};
};
```

Circle publicly inherits from Shape ⑦, and for that reason and, due to the lack of a default constructor in Shape, needs to initialize the base class ⑧. Since it is a circle, it uses the circle enumerator as argument to the base class constructor.

As stated before, we want to draw shapes. We therefore introduce the draw() function for circles. Since we do not want to couple too strongly to any implementation details of drawing, the draw() function is declared in the conceptual header file DrawCircle.h and defined in the according source file:

```
//---- <DrawCircle.h> ----------------
class Circle;
void draw( Circle const& );

//---- <DrawCircle.cpp> ----------------
#include <DrawCircle.h>
#include <Circle.h>
#include /* some graphics library */
void draw( Circle const& c )
{
    // ... Implementing the logic for drawing a circle
}
```

Of course there are not only circles. As indicated by the square enumerator, there is also a Square class:

```
//---- <Square.h> ----------------
#include <Point.h>
#include <Shape.h>
class Square : public Shape ⑨
{
public:
    explicit Square( double side )
        : Shape( square ) ⑩
        , side_( side )
    {
        /* Checking that the given side length is valid */
    }
    double side () const { return side_; }
    Point center() const { return center_; }
private:
    double side_;
    Point center_{};  // Or any corner, if you prefer
};
```

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
{
    for( auto const& shape : shapes )
    {
        switch( shape->getType() )
        {
            case circle:
                draw( static_cast<Circle const&>( *shape ) );
                break;
            case square:
                draw( static_cast<Square const&>( *shape ) );
                break;
        }
    }
}
```

drawAllShapes() takes a vector of shapes in the form of std::unique_ptr<Shape> (1). The pointer to the base class is necessary to hold different kinds of concrete shapes, and the std::unique_ptr in particular to automatically manage the shapes via the RAII Idiom. Inside the function, we start by traversing the vector in order to draw every shape. Unfortunately, all we have at this point are Shape pointers. Therefore we have to ask every shape nicely by means of the getType() function (2): what kind of shape are you? If the shape replies with circle, we know that we have to draw it as a Circle and perform the according static_cast. If the shape replies with square, we draw it as a Square.

I can feel that you are not particularly happy about this solution. But before talking about the shortcomings, let us consider the main() function:

```
//---- <Main.cpp> ----------------

#include <Circle.h>
#include <Square.h>
#include <DrawAllShapes.h>
#include <memory>
#include <vector>

int main()
{
    using Shapes = std::vector<std::unique_ptr<Shape>>;

    // Creating some shapes
    Shapes shapes;
    shapes.emplace_back( std::make_unique<Circle>( 2.3 ) );
    shapes.emplace_back( std::make_unique<Square>( 1.2 ) );
    shapes.emplace_back( std::make_unique<Circle>( 4.1 ) );

    // Drawing all shapes
    drawAllShapes( shapes );

    return EXIT_SUCCESS;
}
```

It works! With this main() function, the code compiles and draws three shapes (two circles and a square). Isn’t that great? It is, but it will not stop you from going into a rant: what a primitive solution! Not only is the switch a bad choice for distinguishing between different kinds of shape, it also does not have a default case! And who had this crazy idea to encode the type of the shapes by means of an unscoped enumeration?² You are looking suspiciously in my direction …

Well, I can understand your reaction. But let us analyze the problem in a little more detail. Let me guess: you remember “Guideline 5: Design for Extension”. And you now imagine what you would have to do to add a third kind of shape. First, you would have to extend the enumeration. For instance, we would have to add the new enumerator triangle(⑥):

```
enum ShapeType
{
    circle,
    square,
    triangle ⑮
};
```

Note that this addition would not only have an impact on the switch statement in the drawAllShapes() function (it is now truly incomplete), but on all classes derived from Shape (i.e. Circle and Square). These classes depend on the enumeration since they depend on the Shape base class and also use the enumeration directly. Therefore
