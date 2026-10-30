You could interact with this method as follows:

```
Console.WriteLine("***** Fun with Interfaces *****
");
// Make an array of Shapes.
Shape[] myShapes = { new Hexagon(), new Circle(),
    new Triangle("Joe"), new Circle("JoJo")};

// Get first pointy item.
IPointy firstPointyItem = FindFirstPointyShape(myShapes);
// To be safe, use the null conditional operator.
Console.WriteLine("The item has {0} points",
    firstPointyItem?.Points);
```

# Arrays of Interface Types

Recall that the same interface can be implemented by numerous types, even if they are not within the same class hierarchy and do not have a common parent class beyond System.Object. This can yield some powerful programming constructs. For example, assume you have developed three new class types within your current project that model kitchen utensils (via Knife and Fork classes) and another modeling gardening equipment (à la PitchFork). The relevant code for the classes is shown here, and the updated class diagram is shown in Figure 8-3:

```
//Fork.cs
namespace CustomInterfaces
{
    class Fork : IPointy
    {
        public byte Points => 4;
    }
}
//PitchFork.cs
namespace CustomInterfaces
{
    class PitchFork : IPointy
    {
        public byte Points => 3;
    }
}
//Knife.cs.cs
namespace CustomInterfaces
{
    class Knife : IPointy
    {
        public byte Points => 1;
    }
}
```