# Working with Binary Resources

Before getting to the topic of object resources, let’s quickly examine how to package up binary resources such as icons or image files (e.g., company logos or images for an animation) into your applications. If you would like to follow along, create a new WPF application named BinaryResourcesApp. Update the markup for your initial window to handle the Window Loaded event and to use a DockPanel as the layout root, like so:

```
<Window x:Class="BinaryResourcesApp.MainWindow"
    <!-- Omitted for brevity -->
    Title="Fun with Binary Resources" Height="500" Width="649" Loaded="MainWindow_OnLoaded">
    <DockPanel LastChildFill="True">
    </DockPanel>
</Window>
```

Now, let’s say your application needs to display one of three image files inside part of the window, based on user input. The WPF Image control can be used to display not only a typical image file (*.bmp, *.gif, *.ico, *.jpg, *.png, *.wdp, or *.tiff) but also data in a DrawingImage (as you saw in Chapter 26). You might build a UI for your window that supports a DockPanel containing a simple toolbar with Next and Previous buttons. Below this toolbar you can place an Image control, which currently does not have a value set to the Source property, like so:

```
<DockPanel LastChildFill="True">
    <ToolBar Height="60" Name="picturePickerToolbar" DockPanel.Dock="Top">
        <Button x:Name="btnPreviousImage" Height="40" Width="100" BorderBrush="Black" Margin="5" Content="Previous" Click="btnPreviousImage_Click"/>
        <Button x:Name="btnNextImage" Height="40" Width="100" BorderBrush="Black" Margin="5" Content="Next" Click="btnNextImage_Click"/>
    </ToolBar>
    <!-- We will fill this Image in code. -->
    <Border BorderThickness="2" BorderBrush="Green">
        <Image x:Name="imageHolder" Stretch="Fill" />
    </Border>
</DockPanel>
```

Next, add the following empty event handlers:

```
private void MainWindow_OnLoaded(
    object sender, RoutedEventArgs e)
{
}
private void btnPreviousImage_Click(
    object sender, RoutedEventArgs e)
{
}
private void btnNextImage_Click(
    object sender, RoutedEventArgs e)
{
}
```

When the window loads, images will be added to a collection that the Next and Previous buttons will cycle through. Now that the application framework is in place, let’s examine the different options for implementing this.