Copy the entire XAML from the 1. fpage file (excluding the outer FixedPage tag) and paste it into the Canvas control. View the Window in design mode, and you will see the sign reproduced in your application.
If you view the Document Outline, you will see that each part of the image is represented as a XAML Path element. If you resize your Window, the image quality stays the same, regardless of how big you make the window. This is because images represented by Path elements are rendered using the drawing engine and math instead of flipping pixels.

# Interacting with the Sign

Recall that routed event tunnel and bubble, so any Path clicked inside the Canvas can be handled by a click event handler on the canvas. Update the Canvas markup to the following:

```
<Canvas MouseLeftButtonDown="Canvas_MouseLeftButtonDown">
```

Add the event handler with the following code:

```
private void Canvas_MouseLeftButtonDown(object sender, MouseButtonEventArgs e)
{
    if (e.OriginalSource is Path p)
    {
        p.Fill = new SolidColorBrush(Colors.Red);
    }
}
```

Now, run your application. Click the lines to see the effects.
You now understand the process of generating Path data for complex graphics and how to interact with the graphical data in code. As you might agree, the ability for professional graphic artists to generate complex graphical data and export the data as XAML is extremely powerful. Once the graphical data has been generated, developers can import the markup and program against the object model.

# Rendering Graphical Data Using the Visual Layer

The final option for rendering graphical data with WPF is termed the visual layer. As mentioned, you can gain access to this layer only through code (it is not XAML-friendly). While a vast majority of your WPF applications will work just fine using shapes, drawings, and geometries, the visual layer does provide the fastest possible way to render huge amounts of graphical data. This low-level graphical layer can also be useful when you need to render a single image over a large area. For example, if you need to fill the background of a window with a plain, static image, the visual layer is the fastest way to do so. It can also be useful if you need to change between window backgrounds quickly, based on user input or whatnot.

I won’t spend too much time delving into the details of this aspect of WPF programming, but let’s build a small sample program to illustrate the basics.

# The Visual Base Class and Derived Child Classes

The abstract System.Windows.Media.Visual class type supplies a minimal set of services (rendering, hit-testing, transformations) to render graphics, but it does not provide support for additional nonvisual services, which can lead to code bloat (input events, layout services, styles, and data binding). The Visual class is an abstract base class. You need to use one of the derived types to perform actual rendering operations. WPF provides a handful of subclasses, including DrawingVisual, Viewport3DVisual, and ContainerVisual.