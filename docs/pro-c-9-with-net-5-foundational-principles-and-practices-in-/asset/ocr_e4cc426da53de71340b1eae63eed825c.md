# Figure 27-4. Naming the object resource

When you are done, you will see the brush has been moved inside the Window.Resources tag.

```
<Window.Resources>
<RadialGradientBrush x:Key="myBrush">
<GradientStop Color="#FFC44EC4" Offset="0" />
<GradientStop Color="#FF829CEB" Offset="1" />
<GradientStop Color="#FF793879" Offset="0.669" />
</RadialGradientBrush>
</Window.Resources>
```

And the Button control's Background has been updated to use a new resource.

```
<Button Margin="25" Height="200" Width="200" Content="OK"
FontSize="20" Background="{DynamicResource myBrush}"/>
```

The Create Resource Wizard creates the new resource as a DynamicResource. You will learn about DynamicResources later in the text, but for now, change it to a StaticResource, like this:

```
<Button Margin="25" Height="200" Width="200" Content="OK"
FontSize="20" Background="{StaticResource myBrush}"/>
```

To see the benefit, update the Cancel Button's Background property to the same StaticResource, and you can see the reuse in action.

```
<Button Margin="25" Height="200" Width="200" Content="Cancel"
FontSize="20" Background="{StaticResource myBrush}"/>
```

![](bbox=[0.107, 0.108, 0.867, 0.39])