
## The Role of Routed Bubbling Events

In the current example, if the user clicks the inner yellow oval, the Click event bubbles out to the next level of scope (the Canvas), then to the StackPanel, and finally to the Button where the Click event handler is handled. In a similar way, if the user clicks the Label, the event is bubbled to the StackPanel and then finally to the Button element.

Given this bubbling routed event pattern, you have no need to worry about registering specific Click event handlers for all members of a composite control. However, if you want to perform custom clicking logic for multiple elements within the same object tree, you can do so.

By way of illustration, assume you need to handle the clicking of the outerEllipse control in a unique manner. First, handle the MouseDown event for this subelement (graphically rendered types such as the Ellipse do not support a Click event; however, they can monitor mouse button activity via MouseDown, MouseUp, etc.).

```xml
<Button Name="btnClickMe" Height="75" Width="250"
    Click="btnClickMe_Clicked">
    <StackPanel Orientation="Horizontal">
        <Label Height="50" FontSize="20">Fancy Button!</Label>
        <Canvas Height="50" Width="100">
            <Ellipse Name="outerEllipse" Fill="Green"
                Height="25" MouseDown="outerEllipse_MouseDown"
                Width="50" Cursor="Hand" Canvas.Left="25" Canvas.Top="12"/>
            <Ellipse Name="innerEllipse" Fill="Yellow" Height="15" Width="36"
                Canvas.Top="17" Canvas.Left="32"/>
        </Canvas>
    </StackPanel>
</Button>
```

Then implement an appropriate event handler, which for illustrative purposes will simply change the Title property of the main window, like so:

```csharp
public void outerEllipse_MouseDown(object sender, MouseButtonEventArgs e)
{
    // Change title of window.
    this.Title = "You clicked the outer ellipse!";
}
```

With this, you can now take different courses of action depending on where the end user has clicked (which boils down to the outer ellipse and everywhere else within the button’s scope).

> **Note:** Routed bubbling events always move from the point of origin to the next defining scope. Thus, in this example, if you click the innerEllipse object, the event will be bubbled to the Canvas, not to the outerEllipse, because they are both Ellipse types within the scope of Canvas.

## Continuing or Halting Bubbling

Currently, if the user clicks the outerEllipse object, it will trigger the registered MouseDown event handler for this Ellipse object, at which point the event bubbles to the Button’s Click event. If you want to inform WPF to stop bubbling up the tree of objects, you can set the Handled property of the EventArgs parameter to true, as follows:

```csharp
public void outerEllipse_MouseDown(object sender, MouseButtonEventArgs e)
{
    // Change title of window.
    this.Title = "You clicked the outer ellipse!";
    // Stop bubbling!
    e.Handled = true;
}
```

In this case, you would find that the title of the window is changed, but you will not see the MessageBox displayed by the Click event handler of the Button. In a nutshell, routed bubbling events make it possible to allow a complex group of content to act either as a single logical element (e.g., a Button) or as discrete items (e.g., an Ellipse within the Button).

## The Role of Routed Tunneling Events

Strictly speaking, routed events can be bubbling (as just described) or tunneling in nature. Tunneling events (which all begin with the `Preview` suffix; e.g., `PreviewMouseDown`) drill down from the topmost element into the inner scopes of the object tree. By and large, each bubbling event in the WPF base class libraries is paired with a related tunneling event that fires before the bubbling counterpart. For example, before the bubbling MouseDown event fires, the tunneling PreviewMouseDown event fires first.

Handling a tunneling event looks just like the processing of handling any other events; simply assign the event handler name in XAML (or, if needed, use the corresponding C# event-handling syntax in your code file) and implement the handler in the code file. Just to illustrate the interplay of tunneling and bubbling events, begin by handling the PreviewMouseDown event for the outerEllipse object, like so:

```xml
<Ellipse Name="outerEllipse" Fill="Green" Height="25"
    MouseDown="outerEllipse_MouseDown"
    PreviewMouseDown="outerEllipse_PreviewMouseDown"
    Width="50" Cursor="Hand" Canvas.Left="25" Canvas.Top="12"/>
```

Next, retrofit the current C# class definition by updating each event handler (for all objects) to append data about the current event into a string member variable named `_mouseActivity`, using the incoming event args object. This will allow you to observe the flow of events firing in the background.

```csharp
public partial class MainWindow : Window
{
    string _mouseActivity = string.Empty;

    public MainWindow()
    {
        InitializeComponent();
    }
}
```

```csharp
public void btnClickMe_Clicked(object sender, RoutedEventArgs e)
{
    AddEventInfo(sender, e);
    MessageBox.Show(_mouseActivity, "Your Event Info");
    // Clear string for next round.
    _mouseActivity = "";
}

private void AddEventInfo(object sender, RoutedEventArgs e)
{
    _mouseActivity += string.Format(
        "{0} sent a {1} event named {2}.\n", sender,
        e.RoutedEvent.RoutingStrategy,
        e.RoutedEvent.Name);
}

private void outerEllipse_MouseDown(object sender, MouseButtonEventArgs e)
{
    AddEventInfo(sender, e);
}

private void outerEllipse_PreviewMouseDown(object sender, MouseButtonEventArgs e)
{
    AddEventInfo(sender, e);
}
```

Notice that you are not halting the bubbling of an event for any event handler. If you run this application, you will see a unique message box display based on where you click the button. Figure 25-15 shows the result of clicking the outer Ellipse object.

![](img/587021f4d5ba039be70a11107130377b_1070_0.png)

Figure 25-15. Tunneling first, bubbling second

So, why do WPF events typically tend to come in pairs (one tunneling and one bubbling)? The answer is that by previewing events, you have the power to perform any special logic (data validation, disable bubbling action, etc.) before the bubbling counterpart fires. By way of an example, assume you have a TextBox that should contain only numerical data. You could handle the PreviewKeyDown event, and if you see the user has entered non-numerical data, you could cancel the bubbling event by setting the Handled property to true.

As you would guess, when you are building a custom control that contains custom events, you could author the event in such a way that it can bubble (or tunnel) through a tree of XAML. For the purpose of this chapter, I will not be examining how to build custom routed events (however, the process is not that different from building a custom dependency property). If you are interested, check out the topic “Routed Events Overview” within the .NET Framework 4.7 SDK documentation. In it you will find a number of tutorials that will help you on your way.

## A Deeper Look at WPF APIs and Controls

The remainder of this chapter will give you a chance to build a new WPF application using Visual Studio. The goal is to create a UI that consists of a TabControl widget containing a set of tabs. Each tab will illustrate some new WPF controls and interesting APIs you might want to make use of in your software projects. Along the way, you will also learn additional features of the Visual Studio WPF designers.

## Working with the TabControl

To get started, create a new WPF application named WpfControlsAndAPIs. As mentioned, your initial window will contain a TabControl with three different tabs, each of which shows off a set of related controls and/or WPF APIs. Update the window’s Width to 800 and Height to 350.

Locate the TabControl control in the Visual Studio Toolbox, drop one onto your designer, and update the markup to the following:

```xml
<TabControl Name="MyTabControl" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
    <TabItem Header="TabItem">
        <Grid Background="#FFFE5E5E"/>
    </TabItem>
    <TabItem Header="TabItem">
        <Grid Background="#FFFE5E5E"/>
    </TabItem>
</TabControl>
```

You will notice that you are given two tab items automatically. To add additional tabs, you simply need to right-click the TabControl node in the Document Outline window and select the Add TabItem menu option (you can also right-click the TabControl on the designer to activate the same menu option) or just start typing in the XAML editor. Add one additional tab using either approach.

Now, update each TabItem control through the XAML editor and change the Header property for each tab, naming them Ink API, Data Binding, and DataGrid. At this point, your window designer should look like what you see in Figure 25-16.
