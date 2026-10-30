
![](img/587021f4d5ba039be70a11107130377b_1130_0.png)

Figure 26-14. Using the visual layer to render an in-memory bitmap

> Note The System.Windows.Media.Imaging namespace contains a number of additional encoding classes that let you save the in-memory RenderTargetBitmap object to a physical file in a variety of formats. Check out the JpegBitmapEncoder class (and friends) for more information.

## Rendering Visual Data to a Custom Layout Manager

While it is interesting to use DrawingVisual to paint onto the background of a WPF control, it is perhaps more common to build a custom layout manager (Grid, StackPanel, Canvas, etc.) that uses the visual layer internally to render its content. After you have created such a custom layout manager, you can plug it into a normal Window (or Page or UserControl) and have a part of the UI using a highly optimized rendering agent while the noncritical aspects of the hosting Window are using shapes and drawings for the remainder of the graphical data.

If you don’t require the extra functionality provided by a dedicated layout manager, you could opt to simply extend FrameworkElement, which does have the necessary infrastructure to also contain visual items. To illustrate how this could be done, insert a new class to your project named CustomVisualFrameworkElement. Extend this class from FrameworkElement and import the System, System.Windows, System.Windows.Input, System.Windows.Media, and System.Windows.Media.Imaging namespaces.

This class will maintain a member variable of type VisualCollection, which contains two fixed DrawingVisual objects (of course, you could add new members to this collection via a mouse operation, but this example will keep it simple). Update your class with the following new functionality:

```
public class CustomVisualFrameworkElement : FrameworkElement
{
    // A collection of all the visuals we are building.
    VisualCollection theVisuals;
    public CustomVisualFrameworkElement()
    {
        // Fill the VisualCollection with a few DrawingVisual objects.
        // The ctor arg represents the owner of the visuals.
        theVisuals = new VisualCollection(this)
            {AddRect(),AddCircle()};
    }
}
```

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

```
Title="Fun with the Visual Layer" Height="350" Width="525"
Loaded="Window_Loaded" WindowStartupLocation="CenterScreen">
    <StackPanel Background="AliceBlue" Name="myStackPanel">
        <Image Name="myImage" Height="80"/>
        <local:CustomVisualFrameworkElement/>
    </StackPanel>
</Window>
```

When you run the program, you will see the result shown in Figure 26-15.

![](img/587021f4d5ba039be70a11107130377b_1132_0.png)

Figure 26-15. Using the visual layer to render data to a custom FrameworkElement

## Responding to Hit-Test Operations

Because DrawingVisual does not have any of the infrastructures of UIElement or FrameworkElement, you will need to programmatically add in the ability to calculate hit-test operations. Thankfully, this is fairly easy to do in the visual layer because of the concept of logical and visual trees. As it turns out, when you author a blob of XAML, you are essentially building a logical tree of elements. However, behind every logical tree is a much richer description known as the visual tree, which contains lower-level rendering instructions.

Chapter 27 will delve into these trees in more detail, but for now, just understand that until you register your custom visuals with these data structures, you will not be able to perform hit-testing operations. Luckily, the VisualCollection container does this on your behalf (which explains why you needed to pass in a reference to the custom FrameworkElement as a constructor argument).

First, update the CustomVisualFrameworkElement class to handle the MouseDown event in the class constructor using standard C# syntax, like so:

```
this.MouseDown += CustomVisualFrameworkElement_MouseDown;
```

The implementation of this handler will call the VisualTreeHelper.HitTest() method to see whether the mouse is within the boundaries of one of the rendered visuals. To do this, you specify as a parameter to HitTest() a HitTestResultCallback delegate that will perform the calculations. If you click a visual, you will toggle between a skewed rendering of the visual and the original rendering. Add the following methods to your CustomVisualFrameworkElement class:

```
void CustomVisualFrameworkElement_MouseDown(object sender, MouseButtonEventArgs e)
{
    // Figure out where the user clicked.
    Point pt = e.GetPosition((UIElement)sender);
    // Call helper function via delegate to see if we clicked on a visual.
    VisualTreeHelper.HitTest(this, null,
        new HitTestResultCallback(myCallback), new PointHitTestParameters(pt));
}
```

```
public HitTestResultBehavior myCallback(HitTestResult result)
{
    // Toggle between a skewed rendering and normal rendering,
    // if a visual was clicked.
    if (result.VisualHit.GetType() == typeof(DrawingVisual))
    {
        if (((DrawingVisual)result.VisualHit).Transform == null)
        {
            ((DrawingVisual)result.VisualHit).Transform = new SkewTransform(7, 7);
        }
        else
        {
            ((DrawingVisual)result.VisualHit).Transform = null;
        }
    }
    // Tell HitTest() to stop drilling into the visual tree.
    return HitTestResultBehavior.Stop;
}
```

Now, run your program once again. You should now be able to click either rendered visual and see the transformation in action! While this is just a simple example of working with the visual layer of WPF, remember that you make use of the same brushes, transformations, pens, and layout managers as you would when working with XAML. As a result, you already know quite a bit about working with this Visual-derived classes.

That wraps up your investigation of the graphical rendering services of Windows Presentation Foundation. While you learned a number of interesting topics, the reality is that you have only scratched the surface of WPF's graphical capabilities. I will leave it in your hands to dig deeper into the topics of shapes, drawings, brushes, transformations, and visuals (and, to be sure, you will see some additional details of these topics in the remaining WPF chapters).
