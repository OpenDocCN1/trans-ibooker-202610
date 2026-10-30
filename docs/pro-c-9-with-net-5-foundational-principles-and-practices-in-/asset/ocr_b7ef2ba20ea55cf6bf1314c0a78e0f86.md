```
private Visual AddCircle()
{
    DrawingVisual drawingVisual = new DrawingVisual();
    // Retrieve the DrawingContext in order to create new drawing content.
    using DrawingContext drawingContext =
        drawingVisual.RenderOpen()
    // Create a circle and draw it in the DrawingContext.
    drawingContext.DrawEllipse(Brushes.DarkBlue, null,
        new Point(70, 90), 40, 50);
    return drawingVisual;
}
private Visual AddRect()
{
    DrawingVisual drawingVisual = new DrawingVisual();
    using DrawingContext drawingContext =
        drawingVisual.RenderOpen()
    Rect rect =
        new Rect(new Point(160, 100), new Size(320, 80));
    drawingContext.DrawRectangle(Brushes.Tomato, null, rect);
    return drawingVisual;
}
```

Now, before you can use this custom FrameworkElement in your Window, you must override two key virtual methods mentioned previously, both of which are called internally by WPF during the rendering process. The GetVisualChild() method returns a child at the specified index from the collection of child elements. The read-only VisualChildrenCount property returns the number of visual child elements within this visual collection. Both methods are easy to implement because you can delegate the real work to the VisualCollection member variable.

```
protected override int VisualChildrenCount
=> theVisuals.Count;

protected override Visual GetVisualChild(int index)
{
    // Value must be greater than zero, so do a sanity check.
    if (index < 0 || index >= theVisuals.Count)
    {
        throw new ArgumentOutOfRangeException();
    }
    return theVisuals[index];
}
```

You now have just enough functionality to test your custom class. Update the XAML description of the Window to add one of your CustomVisualFrameworkElement objects to the existing StackPanel. Doing so will require you to add a custom XML namespace that maps to your .NET Core namespace.

```
<Window x:Class="RenderingWithVisuals.MainWindow"
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:local="clr-namespace:RenderingWithVisuals"
```