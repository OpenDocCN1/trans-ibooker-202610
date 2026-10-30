just shorthand notation for the syntax used to create a class variable using a default constructor and to set the state data property by property.

> Note It's important to remember that the object initialization process is using the property setter implicitly. If the property setter is marked private, this syntax cannot be used.

# Using init-Only Setters (New 9.0)

A new feature added in C# 9.0 is init-only setters. These setters enable a property to have its value set during initialization, but after construction is complete on the object, the property becomes read-only. These types of properties are call immutable. Add a new class file named ReadOnlyPointAfterCreation.cs to your project, and add the following code:

```
using System;

namespace ObjectInitializers
{
    class PointReadOnlyAfterCreation
    {
        public int X { get; init; }
        public int Y { get; init; }

        public void DisplayStats()
        {
            Console.WriteLine("InitOnlySetter: [{0}, {1}]", X, Y);
        }
        public PointReadOnlyAfterCreation(int xVal, int yVal)
        {
            X = xVal;
            Y = yVal;
        }
        public PointReadOnlyAfterCreation() { }
    }
}
```

Use the following code to take this new class for a test-drive:

```
//Make readonly point after construction
PointReadOnlyAfterCreation firstReadonlyPoint = new PointReadOnlyAfterCreation(20, 20);
firstReadonlyPoint.DisplayStats();

// Or make a Point using object init syntax.
PointReadOnlyAfterCreation secondReadonlyPoint = new PointReadOnlyAfterCreation { X = 30, Y = 30 };
secondReadonlyPoint.DisplayStats();
```