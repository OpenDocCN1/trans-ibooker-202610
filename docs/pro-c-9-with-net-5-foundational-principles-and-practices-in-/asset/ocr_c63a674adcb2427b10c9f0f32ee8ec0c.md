You can now define any number of TextBox controls, and they will automatically get the defined look. If a given TextBox does not want this default look and feel, it can opt out by setting the Style property to {x:Null}. For example, txtTest will get the default unnamed style, while txtTest2 is doing things its own way.

```
<TextBox x:Name="txtTest"/>
<TextBox x:Name="txtTest2" Style="{x:Null}" BorderBrush="Black" BorderThickness="5" Height="60" Width="100" Text="Ha!"/>
```

# Subclassing Existing Styles

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

![](bbox=[0.105, 0.57, 0.32, 0.72])

Figure 27-8. Using a derived style