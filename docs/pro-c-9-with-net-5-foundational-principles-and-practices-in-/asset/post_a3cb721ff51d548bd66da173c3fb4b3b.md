
public static bool operator <(Point p1, Point p2)
    => p1.CompareTo(p2) < 0;

public static bool operator >(Point p1, Point p2)
    => p1.CompareTo(p2) > 0;

public static bool operator <=(Point p1, Point p2)
    => p1.CompareTo(p2) <= 0;

public static bool operator >=(Point p1, Point p2)
    => p1.CompareTo(p2) >= 0;
}

## Final Thoughts Regarding Operator Overloading

As you have seen, C# provides the capability to build types that can respond uniquely to various intrinsic, well-known operators. Now, before you go and retrofit all your classes to support such behavior, you must be sure that the operators you are about to overload make some sort of logical sense in the world at large.

For example, let’s say you overloaded the multiplication operator for the MiniVan class. What exactly would it mean to multiply two MiniVan objects? Not much. In fact, it would be confusing for teammates to see the following use of MiniVan objects:

```
// Huh?! This is far from intuitive...
MiniVan newVan = myVan * yourVan;
```

Overloading operators is generally useful only when you’re building atomic data types. Vectors, matrices, text, points, shapes, sets, etc., make good candidates for operator overloading. People, managers, cars, database connections, and web pages do not. As a rule of thumb, if an overloaded operator makes it harder for the user to understand a type’s functionality, don’t do it. Use this feature wisely.

## Understanding Custom Type Conversions

Let’s now examine a topic closely related to operator overloading: custom type conversions. To set the stage for the discussion, let’s quickly review the notion of explicit and implicit conversions between numerical data and related class types.

## Recall: Numerical Conversions

In terms of the intrinsic numerical types (sbyte, int, float, etc.), an explicit conversion is required when you attempt to store a larger value in a smaller container, as this could result in a loss of data. Basically, this is your way to tell the compiler, “Leave me alone, I know what I am trying to do.” Conversely, an implicit conversion happens automatically when you attempt to place a smaller type in a destination type that will not result in a loss of data.

```
int a = 123;
long b = a;    // Implicit conversion from int to long.
int c = (int) b; // Explicit conversion from long to int.
```

## Recall: Conversions Among Related Class Types

As shown in Chapter 6, class types may be related by classical inheritance (the “is-a” relationship). In this case, the C# conversion process allows you to cast up and down the class hierarchy. For example, a derived class can always be implicitly cast to a base type. However, if you want to store a base class type in a derived variable, you must perform an explicit cast, like so:

```
// Two related class types.
class Base{}
class Derived : Base{}

// Implicit cast between derived to base.
Base myBaseType;
myBaseType = new Derived();
// Must explicitly cast to store base reference
// in derived type.
Derived myDerivedType = (Derived)myBaseType;
```

This explicit cast works because the Base and Derived classes are related by classical inheritance and myBaseType is constructed as an instance of Derived. However, if myBaseType is an instance of Base, the cast throws an InvalidCastException. If there is any doubt that the cast will fail, you should use the as keyword, as discussed in Chapter 6. Here is the sample reworked to demonstrate this:

```
// Implicit cast between derived to base.
Base myBaseType2 = new();
// Throws InvalidCastException
//Derived myDerivedType2 = (Derived)myBaseType2 as Derived;
//No exception, myDerivedType2 is null
Derived myDerivedType2 = myBaseType2 as Derived;
```

However, what if you have two class types in different hierarchies with no common parent (other than System.Object) that require conversions? Given that they are not related by classical inheritance, typical casting operations offer no help (and you would get a compiler error to boot!)

On a related note, consider value types (structures). Assume you have two structures named Square and Rectangle. Given that structures cannot leverage classic inheritance (as they are always sealed), you have no natural way to cast between these seemingly related types.

While you could create helper methods in the structures (such as Rectangle.ToSquare()), C# lets you build custom conversion routines that allow your types to respond to the () casting operator. Therefore, if you configured the structures correctly, you would be able to use the following syntax to explicitly convert between them as follows:

```
// Convert a Rectangle to a Square!
Rectangle rect = new Rectangle
{
    Width = 3,
    Height = 10
}
Square sq = (Square)rect;
```

## Creating Custom Conversion Routines

Begin by creating a new Console Application project named CustomConversions. C# provides two keywords, explicit and implicit, that you can use to control how your types respond during an attempted conversion. Assume you have the following structure definitions:

```
using System;

namespace CustomConversions
{
    public struct Rectangle
    {
        public int Width {get; set;}
        public int Height {get; set;}

        public Rectangle(int w, int h)
        {
            Width = w;
            Height = h;
        }

        public void Draw()
        {
            for (int i = 0; i < Height; i++)
            {
                for (int j = 0; j < Width; j++)
                {
                    Console.Write("*");
                }
                Console.WriteLine();
            }
        }

        public override string ToString()
            => $"[Width = {Width}; Height = {Height}]";
    }
}

using System;

namespace CustomConversions
{
    public struct Square
    {
        public int Length {get; set;}
        public Square(int l) : this()
        {
            Length = l;
        }

        public void Draw()
        {
            for (int i = 0; i < Length; i++)
            {
                for (int j = 0; j < Length; j++)
                {
                    Console.Write("*");
                }
                Console.WriteLine();
            }
        }

        public override string ToString() => $"[Length = {Length}]";

        // Rectangles can be explicitly converted into Squares.
        public static explicit operator Square(Rectangle r)
        {
            Square s = new Square {Length = r.Height};
            return s;
        }
    }
}
```

Notice that this iteration of the Square type defines an explicit conversion operator. Like the process of overloading an operator, conversion routines make use of the C# operator keyword, in conjunction with the explicit or implicit keyword, and must be defined as static. The incoming parameter is the entity you are converting from, while the operator type is the entity you are converting to.

In this case, the assumption is that a square (being a geometric pattern in which all sides are of equal length) can be obtained from the height of a rectangle. Thus, you are free to convert a Rectangle into a Square, as follows:

```
using System;
using CustomConversions;

Console.WriteLine("***** Fun with Conversions *****
");
// Make a Rectangle.
Rectangle r = new Rectangle(15, 4);
Console.WriteLine(r.ToString());
r.Draw();

Console.WriteLine();

// Convert r into a Square,
// based on the height of the Rectangle.
Square s = (Square)r;
Console.WriteLine(s.ToString());
s.Draw();
Console.ReadLine();
```

You can see the output here:

```
**** Fun with Conversions ****
[Width = 15; Height = 4]

***************
***************
***************
***************

[Length = 4]
****
****
****
****
```

While it may not be all that helpful to convert a Rectangle into a Square within the same scope, assume you have a function that has been designed to take Square parameters.

```
// This method requires a Square type.
static void DrawSquare(Square sq)
{
    Console.WriteLine(sq.ToString());
    sq.Draw();
}
```

Using your explicit conversion operation on the Square type, you can now pass in Rectangle types for processing using an explicit cast, like so:

```
// Convert Rectangle to Square to invoke method.
Rectangle rect = new Rectangle(10, 5);
DrawSquare((Square)rect);
Console.ReadLine();
```

## Additional Explicit Conversions for the Square Type

Now that you can explicitly convert Rectangles into Squares, let's examine a few additional explicit conversions. Given that a square is symmetrical on all sides, it might be helpful to provide an explicit conversion routine that allows the caller to cast from an integer type into a Square (which, of course, will have a side length equal to the incoming integer). Likewise, what if you were to update Square such that the caller can cast from a Square into an int? Here is the calling logic:

```
// Converting an int to a Square.
Square sq2 = (Square)90;
Console.WriteLine("sq2 = {0}", sq2);
```
