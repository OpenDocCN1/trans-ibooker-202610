```
public void DrawInBoundingBox(int top, int left, int bottom, int right)
{
    Console.WriteLine("Drawing in a box...");
}

public void DrawUpsideDown()
{
    Console.WriteLine("Drawing upside down!");
}
}
```

Now, when you use the BitmapImage, you can invoke each method at the object level (as they are all public), as well as extract a reference to each supported interface explicitly via casting.

```
using System;
using InterfaceHierarchy;

Console.WriteLine("***** Simple Interface Hierarchy *****");

// Call from object level.
BitmapImage myBitmap = new BitmapImage();
myBitmap.Draw();
myBitmap.DrawInBoundingBox(10, 10, 100, 150);
myBitmap.DrawUpsideDown();

// Get IAdvancedDraw explicitly.
if (myBitmap is IAdvancedDraw iAdvDraw)
{
    iAdvDraw.DrawUpsideDown();
}
Console.ReadLine();
```

# Interface Hierarchies with Default Implementations (New 8.0)

When interface hierarchies also include default implementations, downstream interfaces can choose to carry the implementation from the base interface or create a new default implementation. Update the IDrawable interface to the following:

```
public interface IDrawable
{
    void Draw();
    int TimeToDraw() => 5;
}
```