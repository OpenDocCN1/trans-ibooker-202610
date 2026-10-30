
// This struct implicitly derives from System.ValueType and
// implements two interfaces.
public struct PitchFork : ICloneable, IPointy
{...}

Understand that implementing an interface is an all-or-nothing proposition for interface items that do not include a default implementation. The supporting type is not able to selectively choose which members it will implement. Given that the IPointy interface defines a single read-only property, this is not too much of a burden. However, if you are implementing an interface that defines ten members (such as the IDbConnection interface shown earlier), the type is now responsible for fleshing out the details of all ten abstract members.

For this example, insert a new class type named Triangle that “is-a” Shape and supports IPointy. Note that the implementation of the read-only Points property (implemented using the expression-bodied member syntax) simply returns the correct number of points (three).

```
using System;
namespace CustomInterfaces
{
    // New Shape derived class named Triangle.
    class Triangle : Shape, IPointy
    {
        public Triangle() { }
        public Triangle(string name) : base(name) { }
        public override void Draw()
        {
            Console.WriteLine("Drawing {0} the Triangle", PetName);
        }

        // IPointy implementation.
        //public byte Points
        //{
        //    get { return 3; }
        //}
        public byte Points => 3;
    }
}
```

Now, update your existing Hexagon type to also support the IPointy interface type.

```
using System;
namespace CustomInterfaces
{
    // Hexagon now implements IPointy.
    class Hexagon : Shape, IPointy
    {
        public Hexagon(){ }
        public Hexagon(string name) : base(name){ }
        public override void Draw()
        {
            Console.WriteLine("Drawing {0} the Hexagon", PetName);
        }
```
```
// IPointy implementation.
public byte Points => 6;
}
```

To sum up the story so far, the Visual Studio class diagram shown in Figure 8-1 illustrates IPointy-compatible classes using the popular “lollipop” notation. Notice again that Circle and ThreeDCircle do not implement IPointy, as this behavior makes no sense for these classes.

![](img/587021f4d5ba039be70a11107130377b_351_0.png)

Figure 8-1. The shapes hierarchy, now with interfaces

Note To display or hide interface names in the class designer, right-click the interface icon and select the Collapse or Expand option.

## Invoking Interface Members at the Object Level

Now that you have some classes that support the IPointy interface, the next question is how you interact with the new functionality. The most straightforward way to interact with functionality supplied by a given interface is to invoke the members directly from the object level (provided the interface members are not implemented explicitly; you can find more details later in the section “Implementing Explicit Interfaces”). For example, consider the following code:

```
Console.WriteLine("***** Fun with Interfaces *****
");
// Call Points property defined by IPointy.
Hexagon hex = new Hexagon();
Console.WriteLine("Points: {0}", hex.Points);
Console.ReadLine();
```

This approach works fine in this case, given that you understand the Hexagon type has implemented the interface in question and, therefore, has a Points property. Other times, however, you might not be able to determine which interfaces are supported by a given type. For example, suppose you have an array containing 50 Shape-compatible types, only some of which support IPointy. Obviously, if you attempt to invoke the Points property on a type that has not implemented IPointy, you will receive an error. So, how can you dynamically determine whether a class or structure supports the correct interface?

One way to determine at runtime whether a type supports a specific interface is to use an explicit cast. If the type does not support the requested interface, you receive an InvalidCastException. To handle this possibility gracefully, use structured exception handling as in the following example:

```
...
// Catch a possible InvalidCastException.
Circle c = new Circle("Lisa");
IPointy itfPt = null;
try
{
    itfPt = (IPointy)c;
    Console.WriteLine(itfPt.Points);
}
catch (InvalidCastException e)
{
    Console.WriteLine(e.Message);
}
Console.ReadLine();
```

While you could use try/catch logic and hope for the best, it would be ideal to determine which interfaces are supported before invoking the interface members in the first place. Let’s see two ways of doing so.

## Obtaining Interface References: The as Keyword

You can determine whether a given type supports an interface by using the as keyword, introduced in Chapter 6. If the object can be treated as the specified interface, you are returned a reference to the interface in question. If not, you receive a null reference. Therefore, be sure to check against a null value before proceeding.

```
...
// Can we treat hex2 as IPointy?
Hexagon hex2 = new Hexagon("Peter");
IPointy itfPt2 = hex2 as IPointy;
if(itfPt2 != null)
{
    Console.WriteLine("Points: {0}", itfPt2.Points);
}
```
```
else
{
    Console.WriteLine("OOPS! Not pointy...");
}
Console.ReadLine();
```

Notice that when you use the as keyword, you have no need to use try/catch logic; if the reference is not null, you know you are calling on a valid interface reference.

## Obtaining Interface References: The is Keyword (Updated 7.0)

You may also check for an implemented interface using the is keyword (also first discussed in Chapter 6). If the object in question is not compatible with the specified interface, you are returned the value false. If you supply a variable name in the statement, the type is assigned into the variable, eliminating the need to do the type check and perform a cast. The previous example is updated here:

```
Console.WriteLine("***** Fun with Interfaces *****
");
...
if(hex2 is IPointy itfPt3)
{
    Console.WriteLine("Points: {0}", itfPt3.Points);
}
else
{
    Console.WriteLine("OOPS! Not pointy...");
}
Console.ReadLine();
```

## Default Implementations (New 8.0)

As mentioned earlier, C# 8.0 added the ability for interface methods and properties to have a default implementation. Add a new interface named IRegularPointy to represent a regularly shaped polygon. The code is shown here:

```
namespace CustomInterfaces
{
    interface IRegularPointy : IPointy
    {
        int SideLength { get; set; }
        int NumberOfSides { get; set; }
        int Perimeter => SideLength * NumberOfSides;
    }
}
```

Add a new class named Square.cs to the project, inherit the Shape base class, and implement the IRegularPointy interface, as follows:

```
namespace CustomInterfaces
{
    class Square: Shape,IRegularPointy
    {
        public Square() { }
        public Square(string name) : base(name) { }
        //Draw comes from the Shape base class
        public override void Draw()
        {
            Console.WriteLine("Drawing a square");
        }

        //This comes from the IPointy interface
        public byte Points => 4;
        //These come from the IRegularPointy interface
        public int SideLength { get; set; }
        public int NumberOfSides { get; set; }
        //Note that the Perimeter property is not implemented
    }
}
```

Here we have unwittingly introduced the first “catch” of using default implementations in interfaces. The Perimeter property, defined on the IRegularPointy interface, is not defined in the Square class, making it inaccessible from an instance of Square. To see this in action, create a new instance of a Square class and output the relevant values to the console, like this:

```
Console.WriteLine("\n**** Fun with Interfaces ****\n");
...
var sq = new Square("Boxy")
    {NumberOfSides = 4, SideLength = 4};
sq.Draw();
//This won’t compile
//Console.WriteLine($"{sq.PetName} has {sq.NumberOfSides} of length {sq.SideLength} and a perimeter of {sq.Perimeter}");
```

Instead, the Square instance must be explicitly cast to the IRegularPointy interface (since that is where the implementation lives), and then the Perimeter property can be accessed. Update the code to the following:

```
Console.WriteLine($"{sq.PetName} has {sq.NumberOfSides} of length {sq.SideLength} and a perimeter of {((IRegularPointy)sq).Perimeter}");
```

One option to get around this problem is to always code to the interface of a type. Change the definition of the Square instance to IRegularPointy instead of Square, like this:

```
IRegularPointy sq = new Square("Boxy") {NumberOfSides = 4, SideLength = 4};
```

The problem with this approach (in our case) is that the Draw() method and PetName property are not defined on the interface, causing compilation errors.
