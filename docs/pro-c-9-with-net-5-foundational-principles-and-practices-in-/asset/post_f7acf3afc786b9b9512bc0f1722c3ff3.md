
## The {StaticResource} Markup Extension

The {StaticResource} markup extension applies the resource only once (on initialization) and stays “connected” to the original object during the life of the application. Some properties (such as gradient stops) will update, but if you create a new Brush, for example, the control will not be updated. To see this in action, add a Name and Click event handler to each Button control, as follows:

```
<Button Name="Ok" Margin="25" Height="200" Width="200" Content="OK"
    FontSize="20" Background="{StaticResource myBrush}" Click="Ok_OnClick"/>
<Button Name="Cancel" Margin="25" Height="200" Width="200" Content="Cancel"
    FontSize="20" Background="{StaticResource myBrush}" Click="Cancel_OnClick"/>
```

Next, add the following code to the Ok_OnClick() event handler:

```
private void Ok_OnClick(object sender, RoutedEventArgs e)
{
    // Get the brush and make a change.
    var b=(RadialGradientBrush)Resources["myBrush"];
    b.GradientStops[1]=new GradientStop(Colors.Black, 0.0);
}
```

> Note You are using the Resources indexer to locate a resource by name here. Be aware, however, that this will throw a runtime exception if the resource cannot be found. You could also use the TryFindResource() method, which will not throw a runtime error; it will simply return null if the specified resource cannot be located.

When you run the program and click the OK Button, you see the gradients change appropriately. Now add the following code to the Cancel_OnClick() event handler:

```
private void Cancel_OnClick(object sender, RoutedEventArgs e)
{
    // Put a totally new brush into the myBrush slot.
    Resources["myBrush"]=new SolidColorBrush(Colors.Red);
}
```

Run the program again, click the Cancel Button, and nothing happens!

## The {DynamicResource} Markup Extension

It is also possible for a property to use the DynamicResource markup extension. To see the difference, change the markup for the Cancel Button to the following:

```
<Button Name="Cancel" Margin="25" Height="200" Width="200" Content="Cancel"
    FontSize="20" Background="{DynamicResource myBrush}" Click="Cancel_OnClick"/>
```

This time, when you click the Cancel Button, the background for the Cancel Button changes, but the background for the OK Button remains the same. This is because the {DynamicResource} markup extension can detect whether the underlying keyed object has been replaced with a new object. As you might guess, this requires some extra runtime infrastructure, so you should typically stick to using {StaticResource} unless you know you have an object resource that will be swapped with a different object at runtime and you want all items using that resource to be informed.

## Application-Level Resources

When you have object resources in a window’s resource dictionary, all items in the window are free to make use of it, but other windows in the application cannot. The solution to share resources across your application is to define the object resource at the application level, rather than at the window level. There is no way to automate this within Visual Studio, so simply cut the current brush object out of the <Window.Resources> scope and place it in the <Application.Resources> scope in your App.xaml file.

Now any additional window or control in your application is free to use this same brush. If you want to set the Background property for a control, the application-level resources are available for selection, as shown in Figure 27-5.

![](img/587021f4d5ba039be70a11107130377b_1145_0.png)

Figure 27-5. Applying application-level resources

> Note Placing the resources at the application level and assigning it to a control's property will freeze the resource, preventing changing values at runtime. The resource can be cloned, and the clone can be updated.

## Defining Merged Resource Dictionaries

Application-level resources are quite often good enough, but they do not help reuse across projects. In this case, you want to define what is known as a merged resource dictionary. Think of it as a class library for WPF resources; it is nothing more than a XAML file that contains a collection of resources. A single project can have as many of these files as required (one for brushes, one for animations, etc.), each of which can be inserted using the Add New Item dialog box activated via the Project menu (see Figure 27-6).

![](img/587021f4d5ba039be70a11107130377b_1146_0.png)

Figure 27-6. Inserting a new merged resource dictionary

In the new MyBrushes.xaml file, cut the current resources in the Application.Resources scope and move them into your dictionary, like so:

```
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:local="clr-namespace:ObjectResourcesApp"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <RadialGradientBrush x:Key="myBrush">
        <GradientStop Color="#FFC44EC4" Offset="0" />
        <GradientStop Color="#FF829CEB" Offset="1" />
        <GradientStop Color="#FF793879" Offset="0.669" />
    </RadialGradientBrush>
</ResourceDictionary>
```

Even though this resource dictionary is part of your project, all resource dictionaries must be merged (typically at the application level) into an existing resource dictionary to be used. To do this, use the following format in the App.xaml file (note that multiple resource dictionaries can be merged by adding multiple <ResourceDictionary> elements within the <ResourceDictionary.MergedDictionaries> scope):

```
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="MyBrushes.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

The problem with this approach is that each resource file must be added to each project that needs the resources. A better approach for sharing resources is to define a .NET Core class library to share between projects, which you will do next.

## Defining a Resource-Only Assembly

The easiest way to build a resource-only assembly is to begin with a WPF User Control Library (.NET Core) project. Add such a project (named MyBrushesLibrary) to your current solution via the Add ► New Project menu option of Visual Studio and add a project reference to it from the ObjectResourcesApp project.

Now, delete the UserControl1.xaml file from the project. Next, drag and drop the MyBrushes.xaml file into your MyBrushesLibrary project and delete it from the ObjectResourcesApp project. Finally, open MyBrushes.xaml in the MyBrushesLibrary project and change the xmlns:local namespace in the file to clr-namespace:MyBrushesLibrary. Your MyBrushes.xaml file should look like this:

```
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="clr-namespace:MyBrushesLibrary">
    <RadialGradientBrush x:Key="myBrush">
        <GradientStop Color="#FFC44EC4" Offset="0" />
        <GradientStop Color="#FF829CEB" Offset="1" />
        <GradientStop Color="#FF793879" Offset="0.669" />
    </RadialGradientBrush>
</ResourceDictionary>
```

Compile your User Control Library project. Now, merge these binary resources into the application-level resource dictionary of the ObjectResourcesApp project. Doing so, however, requires some rather funky syntax, shown here:

```
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- The syntax is /NameOfAssembly;Component/NameOfXamlFileInAssembly.xaml -->
            <ResourceDictionary Source="/MyBrushesLibrary;Component/MyBrushes.xaml"/>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

First, be aware that this string is space sensitive. If you have extra whitespace around your semicolon or forward slashes, you will generate errors. The first part of the string is the friendly name of the external library (no file extension). After the semicolon, type in the word Component followed by the name of the compiled binary resource, which will be identical to the original XAML resource dictionary.

That wraps up the examination of WPF’s resource management system. You will make good use of these techniques for most (if not all) of your applications. Next up, let’s investigate the integrated animation API of Windows Presentation Foundation.

## Understanding WPF’s Animation Services

In addition to the graphical rendering services you examined in Chapter 26, WPF supplies a programming interface to support animation services. The term animation may bring to mind visions of spinning company logos, a sequence of rotating image resources (to provide the illusion of movement), text bouncing across the screen, or specific types of programs such as video games or multimedia applications.

While WPF’s animation APIs could certainly be used for such purposes, animation can be used any time you want to give an application additional flair. For example, you could build an animation for a button on a screen that magnifies slightly when the mouse cursor hovers within its boundaries (and shrinks back once the mouse cursor moves beyond the boundaries). Or you could animate a window so that it closes using a particular visual appearance, such as slowly fading into transparency. A more business application-centric use is to fade in error messages on an application screen to improve the user experience. In fact, WPF’s animation support can be used within any sort of application (a business application, multimedia programs, video games, etc.) whenever you want to provide a more engaging user experience.

As with many other aspects of WPF, the notion of building animations is nothing new. What is new is that, unlike other APIs you might have used in the past (including Windows Forms), developers are not required to author the necessary infrastructure by hand. Under WPF, there is no need to create the background threads or timers used to advance the animation sequence, define custom types to represent the animation, erase and redraw images, or bother with tedious mathematical calculations. Like other aspects of WPF, you can build an animation entirely using XAML, entirely using C# code, or using a combination of the two.

> Note Visual Studio has no support for authoring animations using GUI animation tools. If you author an animation with Visual Studio, you will do so by typing in the XAML directly. However, Blend for Visual Studio (the companion product that ships with Visual Studio 2019) does indeed have a built-in animation editor that can simplify your life a good deal.

### The Role of the Animation Class Types

To understand WPF’s animation support, you must begin by examining the animation classes within the System.Windows.Media.Animation namespace of PresentationCore.dll. Here, you will find more than 100 different class types that are named using the Animation token.

These classes can be placed into one of three broad categories. First, any class that follows the name convention DataTypeAnimation (ByteAnimation, ColorAnimation, DoubleAnimation, Int32Animation, etc.) allows you to work with linear interpolation animations. This enables you to change a value smoothly over time from a start value to a final value.
