Notice nothing has changed from the code that you wrote for the Point class, except of course the class name. The difference is that the values for X or Y cannot be modified once the class is created. For example, the following code will not compile:

```
//The next two lines will not compile
secondReadonlyPoint.X = 10;
secondReadonlyPoint.Y = 10;
```

# Calling Custom Constructors with Initialization Syntax

The previous examples initialized Point types by implicitly calling the default constructor on the type.

```
// Here, the default constructor is called implicitly.
Point finalPoint = new Point { X = 30, Y = 30 };
```

If you want to be clear about this, it is permissible to explicitly call the default constructor as follows:

```
// Here, the default constructor is called explicitly.
Point finalPoint = new Point() { X = 30, Y = 30 };
```

Be aware that when you are constructing a type using initialization syntax, you are able to invoke any constructor defined by the class. Your Point type currently defines a two-argument constructor to set the (x, y) position. Therefore, the following Point declaration results in an X value of 100 and a Y value of 100, regardless of the fact that the constructor arguments specified the values 10 and 16:

```
// Calling a custom constructor.
Point pt = new Point(10, 16) { X = 100, Y = 100 };
```

Given the current definition of your Point type, calling the custom constructor while using initialization syntax is not terribly useful (and more than a bit verbose). However, if your Point type provides a new constructor that allows the caller to establish a color (via a custom enum named PointColor), the combination of custom constructors and object initialization syntax becomes clear.

Add a new class named PointColorEnum.cs to your project, and add the following code to create an enum for the color:

```
namespace ObjectInitializers
{
    enum PointColorEnum
    {
        LightBlue,
        BloodRed,
        Gold
    }
}
```

Now, update the Point class as follows:

```
class Point
{
    public int X { get; set; }
    public int Y { get; set; }
```