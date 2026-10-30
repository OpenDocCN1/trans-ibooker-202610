
First, notice that you have defined an event trigger for your button to ensure that your storyboard executes when the button has loaded into memory. The StringAnimationUsingKeyFrames class oversees changing the content of the button, via the Storyboard.TargetProperty value.

Within the scope of the `<StringAnimationUsingKeyFrames>` element, you define four DiscreteStringKeyFrame elements, which change the button's Content property over the course of two seconds (note that the duration established by StringAnimationUsingKeyFrames is a total of three seconds, so you will see a slight pause between the final ! and looping 0).

Now that you have a better feel for how to build animations in C# code and XAML, let’s look at the role of WPF styles, which make heavy use of graphics, object resources, and animations.

## Understanding the Role of WPF Styles

When you are building the UI of a WPF application, it is not uncommon for a family of controls to require a shared look and feel. For example, you might want all button types to have the same height, width, background color, and font size for their string content. Although you could handle this by setting each button’s individual properties to identical values, such an approach makes it difficult to implement changes down the road because you would need to reset the same set of properties on multiple objects for every change.

Thankfully, WPF offers a simple way to constrain the look and feel of related controls using styles. Simply put, a WPF style is an object that maintains a collection of property-value pairs. Programmatically speaking, an individual style is represented using the System.Windows.Style class. This class has a property named Setters, which exposes a strongly typed collection of Setter objects. It is the Setter object that allows you to define the property-value pairs.

In addition to the Setters collection, the Style class also defines a few other important members that allow you to incorporate triggers, restrict where a style can be applied, and even create a new style based on an existing style (think of it as “style inheritance”). Be aware of the following members of the Style class:

- Triggers: Exposes a collection of trigger objects, which allow you to capture various event conditions within a style
- BasedOn: Allows you to build a new style based on an existing style
- TargetType: Allows you to constrain where a style can be applied

### Defining and Applying a Style

In almost every case, a Style object will be packaged as an object resource. Like any object resource, you can package it at the window or application level, as well as within a dedicated resource dictionary (this is great because it makes the Style object easily accessible throughout your application). Now recall that the goal is to define a Style object that fills (at minimum) the Setters collection with a set of property-value pairs.

Let’s build a style that captures the basic font characteristics of a control in your application. Start by creating a new WPF application named WpfStyles. Open your App.xaml file and define the following named style:

```
<Application.Resources>
    <Style x:Key="BasicControlStyle">
        <Setter Property="Control.FontSize" Value="14"/>
        <Setter Property="Control.Height" Value="40"/>
        <Setter Property="Control.Cursor" Value="Hand"/>
    </Style>
</Application.Resources>
```

Notice that your BasicControlStyle adds three Setter objects to the internal collection. Now, let’s apply this style to a few controls in your main window. Because this style is an object resource, the controls that want to use it still need to use the {StaticResource} or {DynamicResource} markup extension to locate the style. When they find the style, they will set the resource item to the identically named Style property. Replace the default Grid control with the following markup:

```
<StackPanel>
    <Label x:Name="lblInfo" Content="This style is boring..."
        Style="{StaticResource BasicControlStyle}" Width="150"/>
    <Button x:Name="btnTestButton" Content="Yes, but we are reusing settings!"
        Style="{StaticResource BasicControlStyle}" Width="250"/>
</StackPanel>
```

If you view the Window in the Visual Studio designer (or run the application), you will find that both controls support the same cursor, height, and font size.

### Overriding Style Settings

While both of your controls have opted into the style, if a control wants to apply a style and then change some of the defined settings, that is fine. For example, the Button will now use the Help cursor (rather than the Hand cursor defined in the style).

```
<Button x:Name="btnTestButton" Content="Yes, but we are reusing settings!"
    Cursor="Help" Style="{StaticResource BasicControlStyle}" Width="250" />
```

Styles are processed before the individual property settings of the control using the style; therefore, controls can “override” settings on a case-by-case basis.

### The Effect of TargetType on Styles

Currently, your style is defined in such a way that any control can adopt it (and must do so explicitly by setting the control’s Style property), given that each property is qualified by the Control class. For a program that defines dozens of settings, this would entail a good amount of repeated code. One way to clean this style up a bit is to use the TargetType attribute. When you add this attribute to a Style’s opening element, you can mark exactly once where it can be applied (in this example, in App.xaml).

```
<Style x:Key="BasicControlStyle" TargetType="Control">
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="Height" Value="40"/>
    <Setter Property="Cursor" Value="Hand"/>
</Style>
```

> Note When you build a style that uses a base class type, you needn’t be concerned if you assign a value to a dependency property not supported by derived types. If the derived type does not support a given dependency property, it is ignored.

This is somewhat helpful, but you still have a style that can apply to any control. The TargetType attribute is more useful when you want to define a style that can be applied to only a particular type of control. Add the following new style to the application’s resource dictionary:

```
<Style x:Key="BigGreenButton" TargetType="Button">
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="Height" Value="100"/>
    <Setter Property="Width" Value="100"/>
    <Setter Property="Background" Value="DarkGreen"/>
    <Setter Property="Foreground" Value="Yellow"/>
</Style>
```

This style will work only on Button controls (or a subclass of Button). If you apply it to an incompatible element, you will get markup and compiler errors. Add a new Button that uses this new style, as follows:

```
<Button x:Name="btnAnotherButton" Content="OK!" Margin="0,10,0,0"
    Style="{StaticResource BigGreenButton}" Width="250" Cursor="Help"/>
```

You will see output like that shown in Figure 27-7.

![](img/587021f4d5ba039be70a11107130377b_1158_0.png)

Figure 27-7. Controls with different styles

Another effect of TargetType is that the style will get applied to all elements of that type within the scope of the style definition if the x:Key property does not exist.

Here is another application-level style that will apply automatically to all TextBox controls in the current application:

```
<!-- The default style for all text boxes. -->
<Style TargetType="TextBox">
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="Width" Value="100"/>
    <Setter Property="Height" Value="30"/>
    <Setter Property="BorderThickness" Value="5"/>
    <Setter Property="BorderBrush" Value="Red"/>
    <Setter Property="FontStyle" Value="Italic"/>
</Style>
```

You can now define any number of TextBox controls, and they will automatically get the defined look. If a given TextBox does not want this default look and feel, it can opt out by setting the Style property to {x:Null}. For example, txtTest will get the default unnamed style, while txtTest2 is doing things its own way.

```
<TextBox x:Name="txtTest"/>
<TextBox x:Name="txtTest2" Style="{x:Null}" BorderBrush="Black" BorderThickness="5" Height="60" Width="100" Text="Ha!"/>
```

### Subclassing Existing Styles

You can also build new styles using an existing style, via the BasedOn property. The style you are extending must have been given a proper x:Key in the dictionary, as the derived style will reference it by name using the {StaticResource} or {DynamicResource} markup extension. Here is a new style based on BigGreenButton, which rotates the button element by 20 degrees:

```
<!-- This style is based on BigGreenButton. -->
<Style x:Key="TiltButton" TargetType="Button" BasedOn="{StaticResource BigGreenButton}">
    <Setter Property="Foreground" Value="White"/>
    <Setter Property="RenderTransform">
        <Setter.Value>
            <RotateTransform Angle="20"/>
        </Setter.Value>
    </Setter>
</Style>
```

To use this new style, update the markup for the button to this:

```
<Button x:Name="btnAnotherButton" Content="OK!" Margin="0,10,0,0" Style="{StaticResource TiltButton}" Width="250" Cursor="Help"/>
```

This changes the appearance to the image shown in Figure 27-8.

![](img/587021f4d5ba039be70a11107130377b_1159_0.png)

Figure 27-8. Using a derived style
