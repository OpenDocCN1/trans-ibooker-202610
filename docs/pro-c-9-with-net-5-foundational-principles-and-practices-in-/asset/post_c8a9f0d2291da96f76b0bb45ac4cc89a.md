
![](img/587021f4d5ba039be70a11107130377b_1054_0.png)

# Figure 25-11. The Document Outline window allows you to convert to new panel types

Another handy trick is the ability to select a set of controls on the visual designer and group them into a new, nested layout manager. Assume you have a Grid that contains a set of random objects. Now, select a set of items on the designer by holding down the Ctrl key and clicking each item with the left mouse button. If you then right-click the selection, you can group the selected items into a new subpanel (see Figure 25-12).

![](img/587021f4d5ba039be70a11107130377b_1055_0.png)

Figure 25-12. Grouping items into a new subpanel

After you have done so, examine the Document Outline window once again to verify the nested layout system. As you build full-featured WPF windows, you will most likely always need to make use of a nested layout system, rather than simply picking a single panel for all of the UI display (in fact, the remaining WPF examples in the text will typically do so). On a final note, the nodes in the Document Outline window are all drag and droppable. For example, if you wanted to move a control currently in the DockPanel into the parent panel, you could do so as suggested in Figure 25-13.

![](img/587021f4d5ba039be70a11107130377b_1055_1.png)

Figure 25-13. Relocating items via the Document Outline window

As you work through the remaining WPF chapters, I’ll point out additional layout shortcuts where possible. However, it’s definitely worth your time to experiment and test various features yourself. To keep you moving in the right direction, the next example in the chapter will illustrate how to build a nested layout manager for a custom text processing application (with spell-checking!).

## Building a Window’s Frame Using Nested Panels

As mentioned, a typical WPF window will not use a single panel control but instead will nest panels within other panels to gain the desired layout system. Begin by creating a new WPF application named MyWordPad.

Your goal is to construct a layout where the main window has a topmost menu system, a toolbar under the menu system, and a status bar mounted on the bottom of the window. The status bar will contain a pane to hold text prompts that are displayed when the user selects a menu item (or toolbar button), while the menu system and toolbar will offer UI triggers to close the application and display spelling suggestions in an Expander widget. Figure 25-14 shows the initial layout you are shooting for; it also shows the spell-checking capabilities within WPF.

![](img/587021f4d5ba039be70a11107130377b_1056_0.png)

Figure 25-14. Using nested panels to establish a window’s UI

To begin building this UI, update the initial XAML definition for your Window type so it uses a DockPanel child element, rather than the default Grid, as follows:

```
<Window x:Class="MyWordPad.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
```

## Building the Menu System

Menu systems in WPF are represented by the Menu class, which maintains a collection of MenuItem objects. When building a menu system in XAML, you can have each MenuItem handle various events. The most notable of these events is Click, which occurs when the end user selects a subitem. In this example, you begin by building the two topmost menu items (File and Tools; you will build the Edit menu later in this example), which expose Exit and Spelling Hints subitems, respectively.

In addition to handling the Click event for each subitem, you also need to handle the MouseEnter and MouseExit events, which you will use to set the status bar text in a later step. Add the following markup within your DockPanel scope:

```
<!-- Dock menu system on the top -->
<Menu DockPanel.Dock ="Top"
    HorizontalAlignment="Left" Background="White" BorderBrush ="Black">
    <MenuItem Header="_File">
        <Separator/>
        <MenuItem Header ="_Exit" MouseEnter ="MouseEnterExitArea"
            MouseLeave ="MouseLeaveArea" Click ="FileExit_Click"/>
    </MenuItem>
    <MenuItem Header="_Tools">
        <MenuItem Header ="_Spelling Hints"
            MouseEnter ="MouseEnterToolsHintsArea"
            MouseLeave ="MouseLeaveArea" Click ="ToolsSpellingHints_Click"/>
    </MenuItem>
</Menu>
```

Notice that you dock the menu system to the top of the DockPanel. Also, you use the Separator element to insert a thin horizontal line in the menu system, directly before the Exit option. Also notice that the Header values for each MenuItem contain an embedded underscore token (e.g., _Exit). You use this token to establish which letter will be underlined when the end user presses the Alt key (for keyboard shortcuts). This is a change from the & character used in Windows Forms since XAML is based on XML, and the & character has meaning in XML.

So far you’ve implemented the complete menu system definition; next, you need to implement the various event handlers. First, you have the File Exit handler, FileExit_Click(), which simply closes the window, which in turn terminates the application because this is your topmost window. The MouseEnter and MouseExit event handlers for each subitem will eventually update your status bar; however, for now, you will simply provide shells. Finally, the ToolsSpellingHints_Click() handler for the Tools Spelling Hints menu item will also remain a shell for the time being. Here are the current updates to your code-behind file (including the updated using statements):

```
using System.IO;
using System.Windows;
using System.Windows.Controls;
```

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

## Building Menus Visually

While it is always good to know how to manually define items in XAML, it can be a tad on the tedious side. Visual Studio supports visual design support for menu systems, toolbars, status bars, and many other UI controls. If you right-click the Menu control, you will notice an Add MenuItem option. As the name suggests, this adds a new menu item to the Menu control. After you have added a set of topmost items, you can then add submenu items and separators, expand or collapse the menu itself, and perform other menu-centric operations via a second right-click.

As you see for the remainder of the current MyWordPad example, I’ll typically show you the final generated XAML; however, do take the time to experiment with the visual designers to simplify the task at hand.

## Building the Toolbar

Toolbars (represented by the ToolBar class in WPF) typically provide an alternative manner for activating a menu option. Add the following markup directly after the closing scope of your Menu definition:

```
<!-- Put Toolbar under the Menu -->
<ToolBar DockPanel.Dock ="Top" >
    <Button Content ="Exit" MouseEnter ="MouseEnterExitArea"
            MouseLeave ="MouseLeaveArea" Click ="FileExit_Click"/>

```

```
<Separator/>
<Button Content ="Check" MouseEnter ="MouseEnterToolsHintsArea"
    MouseLeave ="MouseLeaveArea" Click ="ToolsSpellingHints_Click"
    Cursor="Help" />
</ToolBar>
```

Your ToolBar control consists of two Button controls, which just so happen to handle the same events and are handled by the same methods in your code file. Using this technique, you can double up your handlers to serve both menu items and toolbar buttons. Although this toolbar uses the typical push buttons, you should appreciate that the ToolBar type “is-a” ContentControl; therefore, you are free to embed any types into its surface (e.g., drop-down lists, images, and graphics). The only other point of interest here is that the Check button supports a custom mouse cursor through the Cursor property.

> Note You can optionally wrap the ToolBar element within a ToolBarTray element, which controls layout, docking, and drag-and-drop operations for a set of ToolBar objects.

## Building the Status Bar

A StatusBar control will be docked to the lower portion of the DockPanel and contain a single TextBlock control, which you have not used prior to this point in the chapter. You can use a TextBlock to hold text that supports numerous textual annotations, such as bold text, underlined text, line breaks, and so forth. Add the following markup directly after the previous ToolBar definition:

```
<!-- Put a StatusBar at the bottom -->
<StatusBar DockPanel.Dock ="Bottom" Background="Beige" >
    <StatusBarItem>
        <TextBlock Name="statBarText" Text="Ready"/>
    </StatusBarItem>
</StatusBar>
```

## Finalizing the UI Design

The final aspect of your UI design is to define a splittable Grid that defines two columns. On the left, place an Expander control that will display a list of spelling suggestions, wrapped within a StackPanel. On the right, place a TextBox control that supports multiple lines and scrollbars and includes enabled spell-checking. You mount the entire Grid to the left of the parent DockPanel. Add the following XAML markup directly under the StatusBar markup to complete the definition of your window’s UI:

```
<Grid DockPanel.Dock ="Left" Background ="AliceBlue">
    <!-- Define the rows and columns -->
    <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
    </Grid.ColumnDefinitions>
```
