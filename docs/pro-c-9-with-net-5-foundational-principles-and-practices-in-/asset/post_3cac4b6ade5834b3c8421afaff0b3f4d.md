
That reference is contained in the first DependencyObject parameter. You can find the new value using the incoming event arguments. Here is the necessary code that will change the Content property of the Label:

```
private static void CurrentNumberChanged(DependencyObject depObj,
DependencyPropertyChangedEventArgs args)
{
    // Cast the DependencyObject into ShowNumberControl.
    ShowNumberControl c = (ShowNumberControl)depObj;
    // Get the Label control in the ShowNumberControl.
    Label theLabel = c.numberDisplay;
    // Set the Label with the new value.
    theLabel.Content = args.NewValue.ToString();
}
```

Whew! That was a long way to go just to change the output of a label. The benefit is that your CurrentNumber dependency property can now be the target of a WPF style, an animation object, the target of a data-binding operation, and so forth. If you run your application once again, you should now see the value change during execution.

That wraps up your look at WPF dependency properties. While I hope you have a much better idea about what these constructs allow you to do and have a better idea of how to make your own, please be aware that there are many details I have not covered here.

If you find yourself in a position where you are building a number of custom controls that support custom properties, please look up the topic “Properties” under the “WPF Fundamentals” node of the .NET Framework 4.7 SDK documentation. In it you will find many more examples of building dependency properties, attached properties, various ways to configure property metadata, and a slew of other details.

## Summary

This chapter examined several aspects of WPF controls, beginning with an overview of the control toolkit and the role of layout managers (panels). The first example gave you a chance to build a simple word processor application that illustrated the integrated spell-checking functionality of WPF, as well as how to build a main window with menu systems, status bars, and toolbars.

More importantly, you examined how to use WPF commands. Recall that you can attach these control-agnostic events to a UI element or an input gesture to inherit out-of-the-box services automatically (e.g., clipboard operations).

You also learned quite a bit about building complex UI’s in XAML, and you learned about the WPF Ink API at the same time. You also received an introduction to WPF data-binding operations, including how to use the WPF DataGrid class to display data from your custom AutoLot database.

Finally, you investigated how WPF places a unique spin on traditional .NET Core programming primitives, specifically properties and events. As you have seen, a dependency property allows you to build a property that can integrate within the WPF set of services (animations, data bindings, styles, etc.). On a related note, routed events provide a way for an event to flow up or down a tree of markup.

## WPF Graphics Rendering Services

In this chapter, you’ll examine the graphical rendering capabilities of WPF. As you’ll see, WPF provides three separate ways to render graphical data: shapes, drawings, and visuals. After you understand the pros and cons of each approach, you will start learning about the world of interactive 2D graphics using the classes within System.Windows.Shapes. After this, you’ll see how drawings and geometries allow you to render 2D data in a more lightweight manner. Finally, you’ll learn how the visual layer gives you the greatest level of power and performance.

Along the way, you will explore several related topics, such as how to create custom brushes and pens, how to apply graphical transformations to your renderings, and how to perform hit-test operations. You’ll see how the integrated tools of Visual Studio and an additional tool named Inkscape can simplify your graphical coding endeavors.

> Note Graphics are a key aspect of WPF development. Even if you are not building a graphics-heavy application (such as a video game or multimedia application), the topics in this chapter are critical when you work with services such as control templates, animations, and data-binding customization.

## Understanding WPF’s Graphical Rendering Services

WPF uses a particular flavor of graphical rendering that goes by the term retained-mode graphics. Simply put, this means that since you are using XAML or procedural code to generate graphical renderings, it is the responsibility of WPF to persist these visual items and ensure that they are correctly redrawn and refreshed in an optimal manner. Thus, when you render graphical data, it is always present, even when the end user hides the image by resizing or minimizing the window, by covering the window with another, and so forth.

In stark contrast, previous Microsoft graphical rendering APIs (including Windows Forms’ GDI+) were immediate-mode graphical systems. In this model, it was up to the programmer to ensure that rendered visuals were correctly “remembered” and updated during the life of the application. For example, in a Windows Forms application, rendering a shape such as a rectangle involved handling the Paint event (or overriding the virtual OnPaint() method), obtaining a Graphics object to draw the rectangle, and, most important, adding the infrastructure to ensure that the image was persisted when the user resized the window (e.g., creating member variables to represent the position of the rectangle and calling Invalidate() throughout your program).

The shift from immediate-mode to retained-mode graphics is indeed a good thing, as programmers have far less grungy graphics code to author and maintain. However, I’m not suggesting that the WPF graphics API is completely different from earlier rendering toolkits. For example, like GDI+, WPF supports various brush types and pen objects, techniques for hit-testing, clipping regions, graphical transformations, and so on. So, if you currently have a background in GDI+ (or C/C++-based GDI), you already know a good deal about how to perform basic renderings under WPF.

## WPF Graphical Rendering Options

As with other aspects of WPF development, you have a number of choices regarding how to perform your graphical rendering, beyond the decision to do so via XAML or procedural C# code (or perhaps a combination of both). Specifically, WPF provides the following three distinct ways to render graphical data:

- *Shapes*: WPF provides the System.Windows.Shapes namespace, which defines a small number of classes for rendering 2D geometric objects (rectangles, ellipses, polygons, etc.). While these types are simple to use and powerful, they do come with a fair amount of memory overhead if used with reckless abandon.
- *Drawings and geometries*: The WPF API provides a second way to render graphical data, using descendants from the System.Windows.Media.Drawing abstract class. Using classes such as GeometryDrawing or ImageDrawing (in addition to various geometry objects), you can render graphical data in a more lightweight (but less feature-rich) manner.
- *Visuals*: The fastest and most lightweight way to render graphical data under WPF is using the visual layer, which is accessible only through C# code. Using descendants of System.Windows.Media.Visual, you can speak directly to the WPF graphical subsystem.

The reason for offering different ways to do the same thing (i.e., render graphical data) has to do with memory use and, ultimately, application performance. Because WPF is such a graphically intensive system, it is not unreasonable for an application to render hundreds or even thousands of different images on a window’s surface, and the choice of implementation (shapes, drawings, or visuals) could have a huge impact.

Do understand that when you build a WPF application, chances are good you’ll use all three options. As a rule of thumb, if you need a modest amount of interactive graphical data that can be manipulated by the user (receive mouse input, display tooltips, etc.), you’ll want to use members in the System.Windows.Shapes namespace.

In contrast, drawings and geometries are more appropriate when you need to model complex, generally noninteractive, vector-based graphical data using XAML or C#. While drawings and geometries can still respond to mouse events, hit-testing, and drag-and-drop operations, you will typically need to author more code to do so.

Last but not least, if you require the fastest possible way to render massive amounts of graphical data, the visual layer is the way to go. For example, let’s say you are using WPF to build a scientific application that can plot out thousands of points of data. Using the visual layer, you can render the plot points in the most optimal way possible. As you will see later in this chapter, the visual layer is accessible only via C# code and is not XAML-friendly.

No matter which approach you take (shapes, drawings and geometries, or visuals), you will make use of common graphical primitives such as brushes (which fill interiors), pens (which draw exteriors), and transformation objects (which, well, transform the data). To begin the journey, you will start working with the classes of System.Windows.Shapes.

> Note WPF also ships with a full-blown API that can be used to render and manipulate 3D graphics, which is not addressed in this text.
