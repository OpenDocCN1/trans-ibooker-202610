```
<Setter Property="Foreground" Value="Black" />
<Setter Property="ToolTip"
    Value="{Binding RelativeSource={RelativeSource Self},
    Path=(Validation.Errors)[0].ErrorContent}"/>
</Trigger>
</Style.Triggers>
</Style>
</Window.Resources>
```

Run the app and create an error condition. The result will be similar to Figure 28-6, complete with tooltip showing the error message.

![](bbox=[0.07, 0.295, 0.86, 0.54])

Figure 28-6. Showing a custom ErrorTemplate

The previous style changed the appearance of any TextBox that has an error condition. Next, you will create a custom control template to update the ErrorTemplate of the Validation class to show a red exclamation mark and set the tooltips for the exclamation mark. The ErrorTemplate is an adorner, which sits on top of the control. While the style just created updates the control itself, the ErrorTemplate will sit on top of the control.

Place a setter immediately after the Style.Triggers closing tag within the style you just created. You will be creating a control template that consists of a TextBlock (to show the exclamation mark) and a BorderBrush to surround the TextBox that contains the error(s). There is a special tag in XAML for the control that is being adorned with the ErrorTemplate named AdornedElementPlaceholder. By adding a name to this control, you can access the errors that are associated with the control. In this example, you want to access the Validation.Errors property so you can get the ErrorContent (just like you did in the Style.Trigger). Here is the full markup for the setter:

```
<Setter Property="Validation.ErrorTemplate">
    <Setter.Value>
        <ControlTemplate>
            <DockPanel LastChildFill="True">
                <TextBlock Foreground="Red" FontSize="20" Text="!"
                    ToolTip="{Binding ElementName=controlWithError,
                    Path=AdornedElement.(Validation.Errors)[0].ErrorContent}"/>

```