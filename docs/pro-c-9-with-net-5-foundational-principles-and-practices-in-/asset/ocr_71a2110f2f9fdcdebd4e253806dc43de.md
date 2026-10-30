```
using System.Windows.Input;
using Microsoft.Win32;
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    protected void FileExit_Click(object sender, RoutedEventArgs args)
    {
        // Close this window.
        this.Close();
    }

    protected void ToolsSpellingHints_Click(object sender, RoutedEventArgs args)
    {
    }
    protected void MouseEnterExitArea(object sender, RoutedEventArgs args)
    {
    }
    protected void MouseEnterToolsHintsArea(object sender, RoutedEventArgs args)
    {
    }
    protected void MouseLeaveArea(object sender, RoutedEventArgs args)
    {
    }
}
```

# Building Menus Visually

While it is always good to know how to manually define items in XAML, it can be a tad on the tedious side. Visual Studio supports visual design support for menu systems, toolbars, status bars, and many other UI controls. If you right-click the Menu control, you will notice an Add MenuItem option. As the name suggests, this adds a new menu item to the Menu control. After you have added a set of topmost items, you can then add submenu items and separators, expand or collapse the menu itself, and perform other menu-centric operations via a second right-click.

As you see for the remainder of the current MyWordPad example, I’ll typically show you the final generated XAML; however, do take the time to experiment with the visual designers to simplify the task at hand.

# Building the Toolbar

Toolbars (represented by the ToolBar class in WPF) typically provide an alternative manner for activating a menu option. Add the following markup directly after the closing scope of your Menu definition:

```
<!-- Put Toolbar under the Menu -->
<ToolBar DockPanel.Dock ="Top" >
    <Button Content ="Exit" MouseEnter ="MouseEnterExitArea"
            MouseLeave ="MouseLeaveArea" Click ="FileExit_Click"/>

```