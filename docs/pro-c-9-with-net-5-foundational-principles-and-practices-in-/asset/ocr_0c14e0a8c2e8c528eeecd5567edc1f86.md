Here, you set the Content property of the Button to a simple string with the value "Click Me!". However, thanks to the WPF control content model, you could fashion a Button that contains the following complex content:

```
<Button x:Name="btnMyButton" Height="121" Width="156" Click="btnMyButton_Click">
    <Button.Content>
        <StackPanel Height="95" Width="128" Orientation="Vertical">
            <Ellipse Fill="Red" Width="52" Height="45" Margin="5"/>
            <Label Width="59" FontSize="20" Content="Click!" Height="36" />
        </StackPanel>
    </Button.Content>
</Button>
```

You might also recall that the immediate child element of a ContentControl-derived class is the implied content; therefore, you do not need to define a Button.Content scope explicitly when specifying complex content. You could simply author the following:

```
<Button x:Name="btnMyButton" Height="121" Width="156" Click="btnMyButton_Click">
    <StackPanel Height="95" Width="128" Orientation="Vertical">
        <Ellipse Fill="Red" Width="52" Height="45" Margin="5"/>
        <Label Width="59" FontSize="20" Content="Click!" Height="36" />
    </StackPanel>
</Button>
```

In either case, you set the button’s Content property to a StackPanel of related items. You can also author this sort of complex content using the Visual Studio designer. After you define the layout manager for a content control, you can select it on the designer to serve as a drop target for the internal controls. At this point, you can edit each using the Properties window. If you were to use the Properties window to handle the Click event for the Button control (as shown in the previous XAML declarations), the IDE would generate an empty event handler, to which you could add your own custom code, like so:

```
private void btnMyButton_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("You clicked the button!");
}
```

# Working with the Document Outline Editor

You should recall from the previous chapter that the Document Outline window of Visual Studio (which you can open using the View ► Other Windows menu) is useful when designing a WPF control that has complex content. The logical tree of XAML is displayed for the Window you are building, and if you click any of these nodes, it is automatically selected in the visual designer and the XAML editor for editing.

With the current edition of Visual Studio, the Document Outline window has a few additional features that you might find useful. To the right of any node you will find an icon that looks similar to an eyeball. When you toggle this button, you can opt to hide or show an item on the designer, which can be helpful when you want to focus in on a particular segment to edit (note that this will not hide the item at runtime; this only hides items on the designer surface).

Right next to the “eyeball icon” is a second toggle that allows you to lock an item on the designer. As you might guess, this can be helpful when you want to make sure you (or your co-workers) do not accidentally change the XAML for a given item. In effect, locking an item makes it read-only at design time (however, you can change the object’s state at runtime).