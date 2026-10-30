
> Note: Depending on a number of variables with your system configuration, the generated file will have either the `.xps` or `.oxps` extension. Either way, the process works the same.

The `.xps` and `.oxps` formats are actually `.zip` files. Rename the extension of the file to `.zip`, and you can open the file in File Explorer (or 7-Zip, or your favorite archive tool). You will see that it contains the hierarchy shown in Figure 26-13.

![](img/587021f4d5ba039be70a11107130377b_1126_0.png)

Figure 26-13. The folder hierarchy of the printed XPS file

The file that you need is in the Pages directory (`Documents/1/Pages`) and is named `1.fpage`. Open the file with a text editor and copy everything except the `<FixedPage>` opening and closing tags. The path data can then be copied into the XAML and placed inside a Canvas in the main Window. Your image will show in the XAML window.

> Note: The latest version of Inkscape has an option to save the file as Microsoft XAML. Unfortunately, at the time of this writing, it is not compatible with WPF.

## Importing the Graphical Data into a WPF Project

At this point, create a new WPF application named `InteractiveLaserSign`. Resize the Window to a Height of 600 and Width of 650, and replace the Grid with a Canvas.

```xml
<Window x:Class="InteractiveLaserSign.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    
    xmlns:local="clr-namespace:InteractiveLaserSign"
    mc:Ignorable="d"
    Title="MainWindow" Height="600" Width="650">
    <Canvas>
    </Canvas>
</Window>
```

Copy the entire XAML from the `1.fpage` file (excluding the outer `FixedPage` tag) and paste it into the Canvas control. View the Window in design mode, and you will see the sign reproduced in your application.

If you view the Document Outline, you will see that each part of the image is represented as a XAML Path element. If you resize your Window, the image quality stays the same, regardless of how big you make the window. This is because images represented by Path elements are rendered using the drawing engine and math instead of flipping pixels.

## Interacting with the Sign

Recall that routed events tunnel and bubble, so any Path clicked inside the Canvas can be handled by a click event handler on the Canvas. Update the Canvas markup to the following:

```xml
<Canvas MouseLeftButtonDown="Canvas_MouseLeftButtonDown">
```

Add the event handler with the following code:

```csharp
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

## Rendering Graphical Data Using the Visual Layer

The final option for rendering graphical data with WPF is termed the visual layer. As mentioned, you can gain access to this layer only through code (it is not XAML-friendly). While a vast majority of your WPF applications will work just fine using shapes, drawings, and geometries, the visual layer does provide the fastest possible way to render huge amounts of graphical data. This low-level graphical layer can also be useful when you need to render a single image over a large area. For example, if you need to fill the background of a window with a plain, static image, the visual layer is the fastest way to do so. It can also be useful if you need to change between window backgrounds quickly, based on user input or whatnot.

I won’t spend too much time delving into the details of this aspect of WPF programming, but let’s build a small sample program to illustrate the basics.

## The Visual Base Class and Derived Child Classes

The abstract `System.Windows.Media.Visual` class type supplies a minimal set of services (rendering, hit-testing, transformations) to render graphics, but it does not provide support for additional nonvisual services, which can lead to code bloat (input events, layout services, styles, and data binding). The `Visual` class is an abstract base class. You need to use one of the derived types to perform actual rendering operations. WPF provides a handful of subclasses, including `DrawingVisual`, `Viewport3DVisual`, and `ContainerVisual`.

In this example, you will focus only on `DrawingVisual`, a lightweight drawing class that is used to render shapes, images, or text.

## A First Look at Using the DrawingVisual Class

To render data onto a surface using `DrawingVisual`, you need to take the following basic steps:

- Obtain a `DrawingContext` object from the `DrawingVisual` class.
- Use the `DrawingContext` to render the graphical data.

These two steps represent the bare minimum necessary for rendering some data to a surface. However, if you want the graphical data you’ve rendered to be responsive to hit-testing calculations (which would be important for adding user interactivity), you will also need to perform these additional steps:

- Update the logical and visual trees maintained by the container upon which you are rendering.
- Override two virtual methods from the `FrameworkElement` class, allowing the container to obtain the visual data you have created.

You will examine these final two steps in a bit. First, to illustrate how you can use the `DrawingVisual` class to render 2D data, create a new WPF application named `RenderingWithVisuals`. Your first goal is to use a `DrawingVisual` to dynamically assign data to a WPF Image control. Begin by updating the XAML of your window to handle the Loaded event, like so:

```xml
<Window x:Class="RenderingWithVisuals.MainWindow"
    <!--omitted for brevity -->
    Title="Fun With Visual Layer" Height="450" Width="800"
    Loaded="MainWindow_Loaded">
```

Next, replace the Grid with a StackPanel and add an Image in the StackPanel, like this:

```xml
<StackPanel Background="AliceBlue" Name="myStackPanel">
    <Image Name="myImage" Height="80"/>
</StackPanel>
```

Your `<Image>` control does not yet have a Source value because that will happen at runtime. The Loaded event will do the work of building the in-memory graphical data, using a `DrawingBrush` object. Make sure the following namespaces are at the top of `MainWindow.cs`:

```csharp
using System;
using System.Windows;
using System.Windows.Media;
using System.Windows.Media.Imaging;
```

Here is the implementation of the Loaded event handler:

```csharp
private void MainWindow_Loaded(
    object sender, RoutedEventArgs e)
{
    const int TextFontSize = 30;
    // Make a System.Windows.Media.FormattedText object.
```

```csharp
FormattedText text = new FormattedText(
    "Hello Visual Layer!",
    new System.Globalization.CultureInfo("en-us"),
    FlowDirection.LeftToRight,
    new Typeface(this.FontFamily, FontStyles.Italic,
    FontWeights.DemiBold, FontStretches.UltraExpanded),
    TextFontSize,
    Brushes.Green,
    null,
    VisualTreeHelper.GetDpi(this).PixelsPerDip);
// Create a DrawingVisual, and obtain the DrawingContext.
DrawingVisual drawingVisual = new DrawingVisual();
using(DrawingContext drawingContext =
    drawingVisual.RenderOpen())
{
    // Now, call any of the methods of DrawingContext to render data.
    drawingContext.DrawRoundedRectangle(
        Brushes.Yellow, new Pen(Brushes.Black, 5),
        new Rect(5, 5, 450, 100), 20, 20);
    drawingContext.DrawText(text, new Point(20, 20));
}
// Dynamically make a bitmap, using the data in the DrawingVisual.
RenderTargetBitmap bmp = new RenderTargetBitmap(
    500, 100, 100, 90, PixelFormats.Pbgra32);
bmp.Render(drawingVisual);
// Set the source of the Image control!
myImage.Source = bmp;
}
```

This code introduces a number of new WPF classes, which I will briefly comment on here. The method begins by creating a new `FormattedText` object that represents the textual portion of the in-memory image you are constructing. As you can see, the constructor allows you to specify numerous attributes such as font size, font family, foreground color, and the text itself.

Next, you obtain the necessary `DrawingContext` object via a call to `RenderOpen()` on the `DrawingVisual` instance. Here, you are rendering a colored, rounded rectangle into the `DrawingVisual`, followed by your formatted text. In both cases, you are placing the graphical data into the `DrawingVisual` using hard-coded values, which is not necessarily a great idea for production but is fine for this simple test.

The last few statements map the `DrawingVisual` into a `RenderTargetBitmap` object, which is a member of the `System.Windows.Media.Imaging` namespace. This class will take a visual object and transform it into an in-memory bitmap image. After this point, you set the Source property of the Image control, and sure enough, you will see the output in Figure 26-14.
