# Continuing or Halting Bubbling

Currently, if the user clicks the outerEllipse object, it will trigger the registered MouseDown event handler for this Ellipse object, at which point the event bubbles to the button’s Click event. If you want to inform WPF to stop bubbling up the tree of objects, you can set the Handled property of the EventArgs parameter to true, as follows:

```
public void outerEllipse_MouseDown(object sender, MouseButtonEventArgs e)
{
    // Change title of window.
    this.Title = "You clicked the outer ellipse!";
    // Stop bubbling!
    e.Handled = true;
}
```

In this case, you would find that the title of the window is changed, but you will not see the MessageBox displayed by the Click event handler of the Button. In a nutshell, routed bubbling events make it possible to allow a complex group of content to act either as a single logical element (e.g., a Button) or as discrete items (e.g., an Ellipse within the Button).

# The Role of Routed Tunneling Events

Strictly speaking, routed events can be bubbling (as just described) or tunneling in nature. Tunneling events (which all begin with the Preview suffix; e.g., PreviewMouseDown) drill down from the topmost element into the inner scopes of the object tree. By and large, each bubbling event in the WPF base class libraries is paired with a related tunneling event that fires before the bubbling counterpart. For example, before the bubbling MouseDown event fires, the tunneling PreviewMouseDown event fires first.

Handling a tunneling event looks just like the processing of handling any other events; simply assign the event handler name in XAML (or, if needed, use the corresponding C# event-handling syntax in your code file) and implement the handler in the code file. Just to illustrate the interplay of tunneling and bubbling events, begin by handling the PreviewMouseDown event for the outerEllipse object, like so:

```
<Ellipse Name = "outerEllipse" Fill ="Green" Height ="25"
    MouseDown ="outerEllipse_MouseDown"
    PreviewMouseDown ="outerEllipse_PreviewMouseDown"
    Width ="50" Cursor="Hand" Canvas.Left="25" Canvas.Top="12"/>
```

Next, retrofit the current C# class definition by updating each event handler (for all objects) to append data about the current event into a string member variable named mouseActivity, using the incoming event args object. This will allow you to observe the flow of events firing in the background.

```
public partial class MainWindow : Window
{
    string _mouseActivity = string.Empty;
    public MainWindow()
    {
        InitializeComponent();
    }
}
```