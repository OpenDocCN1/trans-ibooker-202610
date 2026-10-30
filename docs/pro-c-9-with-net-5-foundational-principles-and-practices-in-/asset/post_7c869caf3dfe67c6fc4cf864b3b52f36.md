
# Overloading Binary Operators

To illustrate the process of overloading binary operators, assume the following simple Point class is defined in a new Console Application project named OverloadedOps:

```
using System;
namespace OverloadedOps
{
    // Just a simple, everyday C# class.
    public class Point
    {
        public int X {get; set;}
        public int Y {get; set;}

        public Point(int xPos, int yPos)
        {
            X = xPos;
            Y = yPos;
        }
        public override string ToString()
        => $"[{this.X}, {this.Y}]";
    }
}
```

Now, logically speaking, it makes sense to “add” Points together. For example, if you added together two Point variables, you should receive a new Point that is the summation of the X and Y values. Of course, it might also be helpful to subtract one Point from another. Ideally, you would like to be able to author the following code:

```
using System;
using OverloadedOps;

// Adding and subtracting two points?
Console.WriteLine("***** Fun with Overloaded Operators *****");

// Make two points.
Point ptOne = new Point(100, 100);
Point ptTwo = new Point(40, 40);
Console.WriteLine("ptOne = {0}", ptOne);
Console.WriteLine("ptTwo = {0}", ptTwo);
// Add the points to make a bigger point?
Console.WriteLine("ptOne + ptTwo: {0} ", ptOne + ptTwo);

// Subtract the points to make a smaller point?
Console.WriteLine("ptOne - ptTwo: {0} ", ptOne - ptTwo);
Console.ReadLine();
```

However, as your Point now stands, you will receive compile-time errors, as the Point type does not know how to respond to the + or - operator. To equip a custom type to respond uniquely to intrinsic operators, C# provides the operator keyword, which you can use only in conjunction with the static keyword. When you overload a binary operator (such as + and -), you will most often pass in two arguments that are the same type as the defining class (a Point in this example), as illustrated in the following code update:

```
// A more intelligent Point type.
public class Point
{
...
    // Overloaded operator +.
    public static Point operator + (Point p1, Point p2)
        => new Point(p1.X + p2.X, p1.Y + p2.Y);

    // Overloaded operator -.
    public static Point operator - (Point p1, Point p2)
        => new Point(p1.X - p2.X, p1.Y - p2.Y);
}
```

The logic behind operator + is simply to return a new Point object based on the summation of the fields of the incoming Point parameters. Thus, when you write pt1 + pt2, under the hood you can envision the following hidden call to the static operator + method:

```
// Pseudo-code: Point p3 = Point.operator+ (p1, p2)
Point p3 = p1 + p2;
```

Likewise, p1-p2 maps to the following:

```
// Pseudo-code: Point p4 = Point.operator- (p1, p2)
Point p4 = p1 - p2;
```

With this update, your program now compiles, and you find you can add and subtract Point objects, as shown in the following output:

```
***** Fun with Overloaded Operators *****
ptOne = [100, 100]
ptTwo = [40, 40]
ptOne + ptTwo: [140, 140]
ptOne - ptTwo: [60, 60]
```

When you are overloading a binary operator, you are not required to pass in two parameters of the same type. If it makes sense to do so, one of the arguments can differ. For example, here is an overloaded operator + that allows the caller to obtain a new Point that is based on a numerical adjustment:

```
public class Point
{
...
    public static Point operator + (Point p1, int change)
        => new Point(p1.X + change, p1.Y + change);

    public static Point operator + (int change, Point p1)
        => new Point(p1.X + change, p1.Y + change);
}
```

Notice that you need both versions of the method if you want the arguments to be passed in either order (i.e., you can’t just define one of the methods and expect the compiler to automatically support the other one). You are now able to use these new versions of operator + as follows:

```
// Prints [110, 110].
Point biggerPoint = ptOne + 10;
Console.WriteLine("ptOne + 10 = {0}", biggerPoint);

// Prints [120, 120].
Console.WriteLine("10 + biggerPoint = {0}", 10 + biggerPoint);
Console.WriteLine();
```

## What of the += and -= Operators?

If you are coming to C# from a C++ background, you might lament the loss of overloading the shorthand assignment operators (+=, -=, etc.). Don’t despair. In terms of C#, the shorthand assignment operators are automatically simulated if a type overloads the related binary operator. Thus, given that the Point structure has already overloaded the + and - operators, you can write the following:

```
// Overloading binary operators results in a freebie shorthand operator.
...
// Freebie +=
Point ptThree = new Point(90, 5);
Console.WriteLine("ptThree = {0}", ptThree);
Console.WriteLine("ptThree += ptTwo: {0}", ptThree += ptTwo);

// Freebie -=
Point ptFour = new Point(0, 500);
Console.WriteLine("ptFour = {0}", ptFour);
Console.WriteLine("ptFour -= ptThree: {0}", ptFour -= ptThree);
Console.ReadLine();
```

## Overloading Unary Operators

C# also allows you to overload various unary operators, such as ++ and --. When you overload a unary operator, you also must use the static keyword with the operator keyword; however, in this case, you simply pass in a single parameter that is the same type as the defining class/structure. For example, if you were to update the Point with the following overloaded operators:

```
public class Point
{
...
// Add 1 to the X/Y values for the incoming Point.
public static Point operator ++(Point p1)
=> new Point(p1.X+1, p1.Y+1);

// Subtract 1 from the X/Y values for the incoming Point.
public static Point operator --(Point p1)
=> new Point(p1.X-1, p1.Y-1);
}
```

you could increment and decrement Point's x and y values like this:

```
...
// Applying the ++ and -- unary operators to a Point.
Point ptFive = new Point(1, 1);
Console.WriteLine("+ +ptFive = {0}", ++ptFive);  // [2, 2]
Console.WriteLine("--ptFive = {0}", --ptFive);  // [1, 1]

// Apply same operators as postincrement/decrement.
Point ptSix = new Point(20, 20);
Console.WriteLine("ptSix++ = {0}", ptSix++);    // [20, 20]
Console.WriteLine("ptSix-- = {0}", ptSix--);    // [21, 21]
Console.ReadLine();
```

Notice in the preceding code example you are applying the custom ++ and -- operators in two different manners. In C++, it is possible to overload pre- and post-increment/decrement operators separately. This is not possible in C#. However, the return value of the increment/decrement is automatically handled "correctly" free of charge (i.e., for an overloaded ++ operator, pt++ has the value of the unmodified object as its value within an expression, while ++pt has the new value applied before use in the expression).

## Overloading Equality Operators

As you might recall from Chapter 6, System.Object.Equals() can be overridden to perform value-based (rather than referenced-based) comparisons between reference types. If you choose to override Equals() (and the often-related System.Object.GetHashCode() method), it is trivial to overload the equality operators (== and !=). To illustrate, here is the updated Point type:

```
// This incarnation of Point also overloads the == and != operators.
public class Point
{
...
public override bool Equals(object o)
    => o.ToString() == this.ToString();

public override int GetHashCode()
    => this.ToString().GetHashCode();

// Now let's overload the == and != operators.
public static bool operator ==(Point p1, Point p2)
    => p1.Equals(p2);

public static bool operator !=(Point p1, Point p2)
    => !p1.Equals(p2);
}
```

Notice how the implementation of operator == and operator != simply makes a call to the overridden Equals() method to get the bulk of the work done. Given this, you can now exercise your Point class as follows:

```
// Make use of the overloaded equality operators.
...
Console.WriteLine("ptOne == ptTwo : {0}", ptOne == ptTwo);
Console.WriteLine("ptOne != ptTwo : {0}", ptOne != ptTwo);
Console.ReadLine();
```

As you can see, it is quite intuitive to compare two objects using the well-known == and != operators, rather than making a call to Object.Equals(). If you do overload the equality operators for a given class, keep in mind that C# demands that if you override the == operator, you must also override the != operator (if you forget, the compiler will let you know).

## Overloading Comparison Operators

In Chapter 8, you learned how to implement the IComparable interface to compare the relationship between two like objects. You can, in fact, also overload the comparison operators (<, >, <=, and >=) for the same class. As with the equality operators, C# demands that if you overload <, you must also overload >. The same holds true for the <= and >= operators. If the Point type overloaded these comparison operators, the object user could now compare Points, as follows:

```
// Using the overloaded < and > operators.
...
Console.WriteLine("ptOne < ptTwo : {0}", ptOne < ptTwo);
Console.WriteLine("ptOne > ptTwo : {0}", ptOne > ptTwo);
Console.ReadLine();
```

Assuming you have implemented the IComparable interface (or better yet, the generic equivalent), overloading the comparison operators is trivial. Here is the updated class definition:

```
// Point is also comparable using the comparison operators.
public class Point : IComparable<Point>
{
...
public int CompareTo(Point other)
{
    if (this.X > other.X && this.Y > other.Y)
    {
        return 1;
    }
    if (this.X < other.X && this.Y < other.Y)
    {
        return -1;
    }
    return 0;
}
```
