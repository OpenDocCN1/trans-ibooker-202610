
大部分工作只是调整已编译的 BAML 资源，将其映射到 XAML 字符串中。图 27-12 显示了你的最终应用程序的运行效果，其中展示了 System.Windows.Controls.DatePicker 控件的默认模板。该图显示了日历，可以通过单击控件右侧的按钮来访问。

![](img/587021f4d5ba039be70a11107130377b_1169_0.png)

图 27-12. 在运行时检查 ControlTemplate

很好！你应该已经更好地理解了逻辑树、视觉树和控件默认模板是如何协同工作的。现在，你可以花本章剩余的时间来学习如何构建自定义模板和用户控件。

## 使用触发器框架构建控件模板

当你为控件构建自定义模板时，你可以仅用 C# 代码来实现。使用这种方法，你需要将数据添加到 ControlTemplate 对象，然后将其分配给控件的 Template 属性。然而，大多数时候，你会使用 XAML 定义 ControlTemplate 的外观，并添加一些代码（或可能相当多的代码）来驱动运行时行为。

在本章剩余部分，你将学习如何使用 Visual Studio 构建自定义模板。在此过程中，你将了解 WPF 的触发器框架和可视化状态管理器（VSM），并学习如何使用动画来为用户加入视觉提示。单靠 Visual Studio 构建复杂模板可能需要大量输入和一些繁重的工作。诚然，生产级模板会得益于 Blend for Visual Studio（现在随 Visual Studio 一起安装的免费配套应用）。然而，鉴于本书这一版没有涵盖 Blend 的内容，是时候卷起袖子，手动编写一些标记了。

首先，创建一个名为 ButtonTemplate 的新 WPF 应用程序。对于这个项目，你更感兴趣的是创建和使用模板的机制，因此用以下标记替换 Grid：

```
<StackPanel Orientation="Horizontal">
    <Button x:Name="myButton" Width="100" Height="100" Click="myButton_Click"/>
</StackPanel>
```

在 Click 事件处理程序中，只需显示一个消息框（通过 MessageBox.Show()）来确认控件的点击。请记住，当你构建自定义模板时，控件的行为是不变的，但外观可能会变化。

目前，这个 Button 使用默认模板渲染，正如前面的示例所示，默认模板是给定 WPF 程序集中的 XAML 资源。当你想要定义自己的模板时，实际上就是用你自己的创建来替换这个默认的可视化树。首先，更新 `<Button>` 元素的定义，使用属性元素语法指定一个新模板。这个模板将使控件呈现圆形外观。

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

在这里，你定义了一个模板，包含一个命名 Grid 控件，其中包含一个命名 Ellipse 和一个 Label。由于你的 Grid 没有定义行或列，每个子元素都堆叠在前一个控件之上，从而使内容居中。如果现在运行应用程序，你会注意到，只有当鼠标光标位于 Ellipse 的边界内时，Click 事件才会触发！这是 WPF 模板架构的一大优点：你无需重新计算命中测试、边界检查或任何其他底层细节。因此，如果你的模板使用 Polygon 对象来渲染某种不规则几何图形，你可以放心，鼠标命中测试的细节是相对于控件的形状，而不是更大的外接矩形。

### 模板作为资源

目前，你的模板嵌入到了特定的 Button 控件中，这限制了复用。理想情况下，你应该将模板放入资源字典中，这样就可以在项目之间复用圆形按钮模板，或者至少将其移动到应用程序资源容器中，以便在此项目内复用。现在，让我们将本地 Button 资源移动到应用程序级别：从 Button 中剪切模板定义，将其粘贴到 App.xaml 文件的 Application.Resources 标记中。添加一个 Key 和 TargetType，如下所示：

```
<Application.Resources>
    <ControlTemplate x:Key="RoundButtonTemplate" TargetType="{x:Type Button}">
        <Grid x:Name="controlLayout">
            <Ellipse x:Name="buttonSurface" Fill="LightBlue"/>
            <Label x:Name="buttonCaption" VerticalAlignment="Center" HorizontalAlignment="Center" FontWeight="Bold" FontSize="20" Content="OK!"/>
        </Grid>
    </ControlTemplate>
</Application.Resources>
```

将 Button 标记更新为以下内容：

```
<Button x:Name="myButton" Width="100" Height="100" Click="myButton_Click" Template="{StaticResource RoundButtonTemplate}">
</Button>
```

现在，由于此资源可用于整个应用程序，你可以通过简单应用该模板来定义任意数量的圆形按钮。再创建两个使用此模板的 Button 控件以进行测试（无需为这些新项处理 Click 事件）。

```
<StackPanel>
    <Button x:Name="myButton" Width="100" Height="100" Click="myButton_Click" Template="{StaticResource RoundButtonTemplate}"/>
    <Button x:Name="myButton2" Width="100" Height="100" Template="{StaticResource RoundButtonTemplate}"/>
    <Button x:Name="myButton3" Width="100" Height="100" Template="{StaticResource RoundButtonTemplate}"/>
</StackPanel>
```

### 使用触发器加入视觉提示

当你定义自定义模板时，默认模板的视觉提示也会被移除。例如，默认的按钮模板包含标记，这些标记告知控件在某些 UI 事件发生时如何呈现，比如当它获得焦点、被鼠标点击、被启用（或禁用）等等。用户非常习惯这类视觉提示，因为它给控件带来了一种类似触觉的响应。然而，你的 RoundButtonTemplate 没有定义任何此类标记，因此无论鼠标如何操作，控件的外观都是相同的。理想情况下，当控件被点击时，它应该看起来稍有不同（例如通过颜色变化或投影），让用户知道视觉状态已经改变。

正如你已经学到的，这可以使用触发器来完成。对于简单操作，触发器非常适用。还有其他方法超出了本书的范围，但你可以在 https://docs.microsoft.com/en-us/dotnet/desktop-wpf/themes/how-to-create-apply-template 找到更多信息。

举个例子，用以下标记更新你的 RoundButtonTemplate，其中添加了两个触发器。第一个触发器会在鼠标悬停在表面上时将控件颜色改为蓝色，并将前景色改为黄色。第二个触发器会在控件被鼠标按下时缩小 Grid（以及所有子元素）的尺寸。

```
<ControlTemplate x:Key="RoundButtonTemplate" TargetType="Button" >
    <Grid x:Name="controlLayout">
        <Ellipse x:Name="buttonSurface" Fill="LightBlue" />
        <Label x:Name="buttonCaption" Content="OK!" FontSize="20" FontWeight="Bold"
               HorizontalAlignment="Center"
               VerticalAlignment="Center" />
    </Grid>
    <ControlTemplate.Triggers>
        <Trigger Property="IsMouseOver" Value="True">
            <Setter TargetName="buttonSurface" Property="Fill" Value="Blue"/>
            <Setter TargetName="buttonCaption" Property="Foreground" Value="Yellow"/>
        </Trigger>
        <Trigger Property="IsPressed" Value="True">
            <Setter TargetName="controlLayout" Property="RenderTransformOrigin" Value="0.5,0.5"/>
            <Setter TargetName="controlLayout" Property="RenderTransform">
                <Setter.Value>
                    <ScaleTransform ScaleX="0.8" ScaleY="0.8"/>
                </Setter.Value>
            </Setter>
        </Trigger>
    </ControlTemplate.Triggers>
</ControlTemplate>
```

### {TemplateBinding} 标记扩展的作用

控件模板的问题在于，每个按钮看起来都一样，显示的内容也相同。将标记更新为以下内容不会产生任何效果：

```
<Button x:Name="myButton" Width="100" Height="100" Background="Red" Content="Howdy!" Click="myButton_Click" Template="{StaticResource RoundButtonTemplate}" />
<Button x:Name="myButton2" Width="100" Height="100" Background="LightGreen" Content="Cancel!" Template="{StaticResource RoundButtonTemplate}" />
<Button x:Name="myButton3" Width="100" Height="100" Background="Yellow" Content="Format" Template="{StaticResource RoundButtonTemplate}" />
```

这是因为控件的默认属性（如 Background 和 Content）在模板中被覆盖了。若要启用这些属性，必须将它们映射到模板中的相关属性。你可以在构建模板时使用 {TemplateBinding} 标记扩展来解决这些问题。这允许你捕获使用该模板的控件所定义的属性设置，并使用它们在模板本身中设置值。

以下是 RoundButtonTemplate 的重写版本，现在它使用此标记扩展将 Button 的 Background 属性映射到 Ellipse 的 Fill 属性；同时还确保 Button 的 Content 确实传递给 Label 的 Content 属性：

```
<Ellipse x:Name="buttonSurface" Fill="{TemplateBinding Background}"/>
<Label x:Name="buttonCaption" Content="{TemplateBinding Content}" FontSize="20" FontWeight="Bold" HorizontalAlignment="Center" VerticalAlignment="Center" />
```

