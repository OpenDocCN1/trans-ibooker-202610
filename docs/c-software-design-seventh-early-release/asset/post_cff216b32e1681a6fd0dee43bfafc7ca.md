
the classic design patterns do not draw their power from inheritance, but rather from composition.

In “Guideline 21: Use Commands to Isolate What Things are Done” I will give an introduction to the *Command* design pattern. I will show you how to use that design pattern productively and also give you an idea of how *Command* and *Strategy* compare.

In “Guideline 22: Prefer Value Semantics to Reference Semantics” we take a trip into the realm of *reference semantics*. However, we will find that this realm is not particularly friendly and hospitable and makes us worry about the quality of our code. Thus we will resettle into the realm of *value semantics*, which will welcome us with many benefits for our code base.

In “Guideline 23: Prefer a Value-Based Implementation of Strategy and Command”, we will revisit the *Strategy* and *Command* patterns. I will demonstrate, how we can apply the insight we gained in the realm of *value semantics* and implement both design patterns based on
`std::function`.

## Guideline 19: Use Strategy to Isolate How Things are Done

Let us imagine that you and your team are about to implement a new 2D graphics tool. Amongst other, it needs to deal with simple geometric primitives, such as circles, squares, and others, which need to be drawn (see Figure 5-1).

![](img/7b1b8ee332752e5702cc436a0cba7c1c_225_0.png)

Figure 5-1. The initial Shape inheritance hierarchy.

A couple of classes have already been implemented, such as a Shape base class, a Circle and a Square class:

```
//---- <Shape.h> ----------------

class Shape
{
public:
    virtual ~Shape() = default;

    virtual void draw( /*some arguments*/ ) const = 0;  ①
};
```
//---- <Circle.h> ----------------

#include <Point.h>
#include <Shape.h>

class Circle : public Shape
{
public:
    explicit Circle( double radius )
        : radius_( radius )
    {
        /* Checking that the given radius is valid */
    }

    double radius() const { return radius_; }
    Point center() const { return center_; }

    void draw( /*some arguments*/ ) const override;

private:
    double radius_;
    Point center_{};
};

//---- <Circle.cpp> ----------------

#include <Circle.h>
#include /* some graphics library */

void Circle::draw( /*some arguments*/ ) const
{
    // ... Implementing the logic for drawing a circle
}

//---- <Square.h> ----------------

#include <Point.h>
#include <Shape.h>

class Square : public Shape
{
public:
    explicit Square( double side )
        : side_( side )
```
/* Checking that the given side length is valid */
}

double side () const { return side_; }
Point center() const { return center_; }

void draw( /*some arguments*/ ) const override;  ③

private:
    double side_;
    Point center_ {};
};
```

```
//---- <Square.cpp> -------------

#include <Square.h>
#include /* some graphics library */

void Square::draw( /*some arguments*/ ) const
{
    // ... Implementing the logic for drawing a square
}
```

The most important aspect is the pure virtual draw() member function of the Shape base class ①. While you were on vacation, one of your team members has already implemented this draw() member function for both the Circle and the Square classes by means of OpenGL ② and ③. This means the tool is already able to draw circles and squares and the entire team agrees that the resulting graphics look pretty neat. Everyone is happy!

## Analyzing the Design Issues

Everyone, except you, that is. After returning from your vacation, you of course immediately realize that the implemented solution violates the Single-Responsibility Principle (SRP).¹ As it is, the Shape hierarchy is not designed for change: first, it is not easy to change the way a shape is drawn. In the current implementation there is only one fixed way of drawing shapes and it is not possible to change these details non-intrusively. Since you already predict that the tool will have to support multiple graphic libraries, this is definitely a problem.² And second, if you eventually

perform the change, it is necessary to change the behavior in multiple, unrelated places.

But there is more: since the drawing functionality is implemented inside `Circle` and `Square`, the `Circle` and `Square` classes depend on the implementation details of `draw()`, meaning they depend on OpenGL. Despite the fact that circles and squares should primarily be some simply geometric primitives, these two classes now carry the burden of having to use OpenGL everywhere they are used.

When pointing this out to your colleagues, they are, at first, a little dumbfounded. And also a little annoyed, since they did not expect you to point out any flaw in their beautiful solution. However, you have a very nice way of explaining the problem and so eventually, they agree with you and start to think about a better solution.

It does not take them long to come up with a better approach. In the next team meeting a few days later, they present their new idea: another layer in the inheritance hierarchy (see Figure 5-2).

![](img/7b1b8ee332752e5702cc436a0cba7c1c_229_0.png)

Figure 5-2. The extended Shape inheritance hierarchy.

In order to demonstrate the idea, they have even already implemented the OpenGLCircle and OpenGLSquare classes:

```
//---- <Circle.h> ----------------

#include <Shape.h>

class Circle : public Shape
{
public:
    // ... No implementation of the draw() member function anymore
};

//---- <OpenGLCircle.h> ----------------

#include <Circle.h>

class OpenGLCircle : public Circle
{
public:
    explicit OpenGLCircle( double radius )
        : Circle( radius )
    {}

    void draw( /*some arguments*/ ) const override;
};

//---- <OpenGLCircle.cpp> ----------------

#include <OpenGLCircle.h>
#include /* OpenGL graphics library headers */

void OpenGLCircle::draw( /*some arguments*/ ) const
{
    // ... Implementing the logic for drawing a circle by means of OpenGL
}

//---- <Square.h> ----------------

#include <Shape.h>

class Square : public Shape
```
```
{
  public:
    // ... No implementation of the draw() member function anymore
};
```

```
//---- <OpenGLSquare.h> ----------------
```
```
#include <Square.h>

class OpenGLSquare : public Square
{
  public:
    explicit OpenGLSquare( double side )
      : Square( side )
    {}

    void draw( /*some arguments*/ ) const override;
};
```

```
//---- <OpenGLSquare.cpp> ----------------
```
```
#include <OpenGLSquare.h>
#include /* OpenGL graphics library headers */

void OpenGLSquare::draw( /*some arguments*/ ) const
{
  // ... Implementing the logic for drawing a square by means of OpenGL
}
```

Inheritance! Of course! By simply deriving from Circle and Square and by moving the implementation of the draw() function further down the hierarchy, it is easily possible to implement the drawing in different ways. For instance, there could be a MetalCircle and a VulkanCircle, assuming that the Metal and Vulkan libraries need to be supported. Suddenly change is easy, right?

While your colleagues are still very proud about their new solution, you already realize that this approach will not work well for long. And it is easy to demonstrate the shortcomings: all you have to do is considering another requirement, for instance a serialize() member function:

```
class Shape
{
    public:
        virtual ~Shape() = default;

        virtual void draw(/*some arguments*/ ) const = 0;
        virtual void serialize(/*some arguments*/ ) const = 0;
};
```

The serialize() member function (4) is supposed to transform a shape into a byte sequence, which can be stored in a file or a data base. From there, it will be possible to deserialize the byte sequence to recreate the exact same shape. And just as the draw() member function, the serialize() member function can be implemented in various different ways. For instance, you could reach for the protobuf or Boost.serialization libraries.

Using the same strategy of moving the implementation details down the inheritance hierarchy, this will quickly lead to a pretty complex and rather artificial hierarchy (see Figure 5-3). Consider the class names: OpenGLProtobufCircle, MetalBoostSerialSquare, ... ridiculous, right? And how should we structure this: should we add another layer in the hierarchy (see the Square branch)? That approach would quickly lead to a deep and complex hierarchy. Or should we rather flatten the hierarchy out (as in the Circle branch of the hierarchy)? And what about reusing implementation details? For instance, how would it be possible to reuse the OpenGL code between the OpenGLProtobufCircle and the OpenGLBoostSerialCircle classes?
