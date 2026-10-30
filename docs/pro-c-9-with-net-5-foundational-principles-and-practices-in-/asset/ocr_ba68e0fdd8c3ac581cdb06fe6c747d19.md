To begin, create a new WPF application named ButtonTemplate. For this project, you are more interested in the mechanics of creating and using templates, so replace the Grid with the following markup:

```
<StackPanel Orientation="Horizontal">
    <Button x:Name="myButton" Width="100" Height="100" Click="myButton_Click"/>
</StackPanel>
```

In the Click event handler, simply display a message box (via MessageBox.Show()) to show a message confirming the clicking of the control. Remember, when you build custom templates, the behavior of the control is constant, but the look may vary.

Currently, this Button is rendered using the default template, which, as the previous example illustrated, is a XAML resource within a given WPF assembly. When you want to define your own template, you essentially replace this default visual tree with your own creation. To begin, update the definition of the <Button> element to specify a new template using the property-element syntax. This template will give the control a round appearance.

```
<Button x:Name="myButton" Width="100" Height="100" Click="myButton_Click">
    <Button.Template>
        <ControlTemplate>
            <Grid x:Name="controlLayout">
                <Ellipse x:Name="buttonSurface" Fill="LightBlue"/>
                <Label x:Name="buttonCaption"
                    VerticalAlignment="Center"
                    HorizontalAlignment="Center"
                    FontWeight="Bold" FontSize="20" Content="OK!"/>
            </Grid>
        </ControlTemplate>
    </Button.Template>
</Button>
```

Here, you have defined a template that consists of a named Grid control containing a named Ellipse and a Label. Because your Grid has no defined rows or columns, each child stacks on top of the previous control, which centers the content. If you run your application now, you will notice that the Click event will fire only when the mouse cursor is within the bounds of the Ellipse! This is a great feature of the WPF template architecture: you do not need to recalculate hit-testing, bounds checking, or any other low-level detail. So, if your template used a Polygon object to render some oddball geometry, you can rest assured that the mouse hit-testing details are relative to the shape of the control, not the larger bounding rectangle.

# Templates as Resources

Currently, your template is embedded to a specific Button control, which limits reuse. Ideally, you would place your template into a resource dictionary so you can reuse your round button template between projects or, at minimum, move it into the application resource container for reuse within this project. Let’s move the local Button resource to the application level by cutting the template definition from the Button and pasting it into the Application.Resources tag in the App.xaml file. Add in a Key and a TargetType, as follows:

```
<Application.Resources>
    <ControlTemplate x:Key="RoundButtonTemplate" TargetType="{x:Type Button}">
        <Grid x:Name="controlLayout">
            <Ellipse x:Name="buttonSurface" Fill="LightBlue"/>
```