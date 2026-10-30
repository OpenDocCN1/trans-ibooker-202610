Next, update the top-level statements to the following:

```
Console.WriteLine("***** Simple Interface Hierarchy *****");
...
if (myBitmap is IAdvancedDraw iAdvDraw)
{
    iAdvDraw.DrawUpsideDown();
    Console.WriteLine($"Time to draw: {iAdvDraw.TimeToDraw()}");
}
Console.ReadLine();
```

Not only does this code compile, but it outputs a value of 5 for the TimeToDraw() method. This is because default implementations automatically carry forward to descendant interfaces. Casting the BitMapImage to the IAdvancedDraw interface provides access to the TimeToDraw() method, even though the BitMapImage instance does not have access to the default implementation. To prove this, enter the following code and see the compile error:

```
//This does not compile
myBitmap.TimeToDraw();
```

If a downstream interface wants to provide its own default implementation, it must hide the upstream implementation. For example, if the IAdvancedDraw TimeToDraw() method takes 15 units to draw, update the interface to the following definition:

```
public interface IAdvancedDraw : IDrawable
{
    void DrawInBoundingBox(
        int top, int left, int bottom, int right);
    void DrawUpsideDown();
    new int TimeToDraw() => 15;
}
```

Of course, the BitMapImage class is also free to implement the TimeToDraw() method. Unlike the IAdvancedDraw TimeToDraw() method, the class only needs to implement the method, not hide it.

```
public class BitmapImage : IAdvancedDraw
{
    ...
    public int TimeToDraw() => 12;
}
```

When casting the BitmapImage instance to either the IAdvancedDraw or IDrawable interface, the method on the instance still is executed. Add this code to the top-level statements:

```
//Always calls method on instance:
Console.WriteLine("***** Calling Implemented TimeToDraw *****");
Console.WriteLine($"Time to draw: {myBitmap.TimeToDraw()}");
Console.WriteLine($"Time to draw: {((IDrawable) myBitmap).TimeToDraw()}");
Console.WriteLine($"Time to draw: {((IAdvancedDraw) myBitmap).TimeToDraw()}");
```