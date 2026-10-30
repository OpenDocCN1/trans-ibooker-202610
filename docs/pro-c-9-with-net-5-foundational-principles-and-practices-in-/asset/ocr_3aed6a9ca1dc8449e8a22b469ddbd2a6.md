```
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

# Intercepting Mouse Events

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