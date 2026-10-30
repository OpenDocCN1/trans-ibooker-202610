
```
this.StartupUri = new System.Uri("MainWindow.xaml", System.UriKind.Relative);
#line default
#line hidden
}
```

## Interacting with Application-Level Data

Recall that the Application class defines a property named Properties, which allows you to define a collection of name-value pairs via a type indexer. Because this indexer has been defined to operate on type System.Object, you are able to store any sort of item within this collection (including your custom classes) to be retrieved at a later time using a friendly moniker. Using this approach, it is simple to share data across all windows in a WPF application.

To illustrate, you will update the current Startup event handler to check the incoming command-line arguments for a value named /GODMODE (a common cheat code for many PC video games). If you find this token, you will establish a bool value set to true within the properties collection of the same name (otherwise, you will set the value to false).

This sounds simple enough, but how are you going to pass the incoming command-line arguments (typically obtained from the Main() method) to your Startup event handler? One approach is to call the static Environment.GetCommandLineArgs() method. However, these same arguments are automatically added to the incoming StartupEventArgs parameter and can be accessed via the Args property. Here is the first update to the current code base:

```
private void App_OnStartup(object sender, StartupEventArgs e)
{
    Application.Current.Properties["GodMode"] = false;
    // Check the incoming command-line arguments and see if they
    // specified a flag for /GODMODE.
    foreach (string arg in e.Args)
    {
        if (arg.Equals("/godmode",StringComparison.OrdinalIgnoreCase))
        {
            Application.Current.Properties["GodMode"] = true;
            break;
        }
    }
}
```

Application-wide data can be accessed from anywhere within the WPF application. All you are required to do is obtain an access point to the global application object (via Application.Current) and investigate the collection. For example, you could update the Click event handler of the Button as so:

```
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Did user enable /godmode?
    if (((bool)Application.Current.Properties["GodMode"])
    {
        MessageBox.Show("Cheater!");
    }
}
```

With this, if you enter the /godmode command-line argument into the Debug tab in the project properties and then run the program, you will be shamed, and the program will exit. You can also run the program from a command line by entering the following command (open a command prompt and navigate to the bin/debug directory):

```
WpfAppAllCode.exe  /godmode
```

You will see the shameful message box displayed when terminating the application.

Note Recall that you can supply command-line arguments within Visual Studio. Simply double-click the Properties icon within Solution Explorer, click the Debug tab from the resulting editor, and enter /godmode within the “Command line arguments” editor.

## Handling the Closing of a Window Object

End users can shut down a window by using numerous built-in system-level techniques (e.g., clicking the X close button on the window’s frame) or by indirectly calling the Close() method in response to some user interaction element (e.g., File ► Exit). In either case, WPF provides two events that you can intercept to determine whether the user is truly ready to shut down the window and remove it from memory. The first event to fire is Closing, which works in conjunction with the CancelEventHandler delegate.

This delegate expects target methods to take System.ComponentModel.CancelEventArgs as the second parameter. CancelEventArgs provides the Cancel property, which when set to true will prevent the window from actually closing (this is handy when you have asked the user if he really wants to close the window or if perhaps he would like to save his work first).

If the user does indeed want to close the window, CancelEventArgs.Cancel can be set to false (which is the default setting). This will then cause the Closed event to fire (which works with the System.EventHandler delegate), making it the point at which the window is about to be closed for good.

Update the MainWindow class to handle these two events by adding these code statements to the current constructor, like so:

```
public MainWindow()
{
    InitializeComponent();
    this.Closed+=MainWindow_Closed;
    this.Closing += MainWindow_Closing;
}
```

Now, implement the corresponding event handlers as so:

```
private void MainWindow_Closing(object sender, System.ComponentModel.CancelEventArgs e)
{
    // See if the user really wants to shut down this window.
    string msg = "Do you want to close without saving?";
    MessageBoxResult result = MessageBox.Show(msg,
        "My App", MessageBoxButton.YesNo, MessageBoxImage.Warning);
    if (result == MessageBoxResult.No)
    {
        // If user doesn't want to close, cancel closure.
        e.Cancel = true;
    }
}

private void MainWindow_Closed(object sender, EventArgs e)
{
    MessageBox.Show("See ya!");
}
```

Now, run your program and attempt to close the window, either by clicking the X icon in the upper right of the window or by clicking the button control. You should see the confirmation dialog asking if you really want to leave. If you answer Yes, you will then see the farewell message. Clicking the No button will keep the window in memory.

## Intercepting Mouse Events

The WPF API provides several events you can capture to interact with the mouse. Specifically, the UIElement base class defines mouse-centric events such as MouseMove, MouseUp, MouseDown, MouseEnter, MouseLeave, and so forth.

Consider, for example, the act of handling the MouseMove event. This event works in conjunction with the System.Windows.Input.MouseEventHandler delegate, which expects its target to take a System.Windows.Input.MouseEventArgs type as the second parameter. Using MouseEventArgs, you can extract the (x, y) position of the mouse and other relevant details. Consider the following partial definition:

```
public class MouseEventArgs : InputEventArgs
{
...
    public Point GetPosition(IInputElement relativeTo);
    public MouseButtonState LeftButton { get; }
    public MouseButtonState MiddleButton { get; }
    public MouseDevice MouseDevice { get; }
    public MouseButtonState RightButton { get; }
    public StylusDevice StylusDevice { get; }
    public MouseButtonState XButton1 { get; }
    public MouseButtonState XButton2 { get; }
}
```

> Note The XButton1 and XButton2 properties allow you to interact with “extended mouse buttons” (such as the “next” and “previous” buttons found on some mouse controls). These are often used to interact with a browser’s history list to navigate between visited pages.

The GetPosition() method allows you to get the (x, y) value relative to a UI element on the window. If you are interested in capturing the position relative to the activated window, simply pass in this. Handle the MouseMove event in the constructor of your MainWindow class, like so:

```
public MainWindow(string windowTitle, int height, int width)
{
...
    this.MouseMove += MainWindow_MouseMove;
}
```

Here is an event handler for MouseMove that will display the location of the mouse in the window’s title area (notice you are translating the returned Point type into a text value via ToString()):

```
private void MainWindow_MouseMove(object sender,
    System.Windows.Input.MouseEventArgs e)
{
    // Set the title of the window to the current (x,y) of the mouse.
    this.Title = e.GetPosition(this).ToString();
}
```

## Intercepting Keyboard Events

Processing keyboard input for the focused window is also straightforward. UIElement defines events that you can capture to intercept keypresses from the keyboard on the active element (e.g., KeyUp, KeyDown). The KeyUp and KeyDown events both work with the System.Windows.Input.KeyEventHandler delegate, which expects the target’s second event handler to be of type KeyEventArgs, which defines several public properties of interest, shown here:

```
public class KeyEventArgs : KeyboardEventArgs
{
...
    public bool IsDown { get; }
    public bool IsRepeat { get; }
    public bool IsToggled { get; }
    public bool IsUp { get; }
    public Key Key { get; }
    public KeyStates KeyStates { get; }
    public Key SystemKey { get; }
}
```

To illustrate handling the KeyDown event in the constructor of MainWindow (just like you did for the previous events), implement the following event handler that changes the content of the button with the currently pressed key:

```
private void MainWindowOs_KeyDown(object sender, System.Windows.Input.KeyEventArgs e)
{
    // Display key press on the button.
    ClickMe.Content = e.Key.ToString();
}
```
