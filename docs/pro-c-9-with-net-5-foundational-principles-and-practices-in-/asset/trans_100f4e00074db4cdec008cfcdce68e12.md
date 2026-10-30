
首先，请注意你已经为按钮定义了一个事件触发器，以确保当按钮加载到内存中时，你的情节提要能够执行。`StringAnimationUsingKeyFrames` 类负责通过 `Storyboard.TargetProperty` 值来更改按钮的内容。

在 `<StringAnimationUsingKeyFrames>` 元素的作用域内，你定义了四个 `DiscreteStringKeyFrame` 元素，它们会在两秒内更改按钮的 Content 属性（请注意，`StringAnimationUsingKeyFrames` 建立的持续时间总共为三秒，因此你会看到最后一个 `!` 与循环显示的 `0` 之间有一小段暂停）。

现在你已经对如何在 C# 代码和 XAML 中构建动画有了更好的感觉，接下来让我们看看 WPF 样式的作用，它们大量使用了图形、对象资源和动画。

## 理解 WPF 样式的作用

在构建 WPF 应用程序的 UI 时，一组控件通常需要共享相同的外观和感觉。例如，你可能希望所有按钮类型都具有相同的高度、宽度、背景色以及字符串内容的字体大小。虽然你可以通过将每个按钮的各个属性设置为相同的值来处理，但这种方法使得后续更改变得困难，因为每次更改都需要在多个对象上重置相同的属性集。

幸运的是，WPF 提供了一种使用样式来约束相关控件外观和感觉的简单方法。简而言之，WPF 样式是一个维护属性-值对集合的对象。从编程角度来说，单个样式使用 `System.Windows.Style` 类来表示。该类有一个名为 `Setters` 的属性，它公开了一个强类型的 `Setter` 对象集合。正是 `Setter` 对象允许你定义属性-值对。

除了 `Setters` 集合之外，`Style` 类还定义了一些其他重要成员，这些成员允许你合并触发器、限制样式的应用范围，甚至基于现有样式创建新样式（可以将其视为“样式继承”）。请注意 `Style` 类的以下成员：

- `Triggers`：公开一个触发器对象集合，允许你在样式内捕获各种事件条件
- `BasedOn`：允许你基于现有样式构建新样式
- `TargetType`：允许你约束样式的应用范围

### 定义并应用样式

在几乎所有情况下，`Style` 对象都会作为对象资源打包。与任何对象资源一样，你可以在窗口或应用程序级别打包它，也可以放在专用的资源字典中（这很棒，因为这使得 `Style` 对象在整个应用程序中易于访问）。现在回想一下，目标就是定义一个 `Style` 对象，该对象用一组属性-值对填充（至少）`Setters` 集合。

让我们构建一个样式，捕获应用程序中控件的基本字体特征。首先创建一个名为 `WpfStyles` 的新 WPF 应用程序。打开 `App.xaml` 文件并定义以下命名样式：

```
<Application.Resources>
    <Style x:Key="BasicControlStyle">
        <Setter Property="Control.FontSize" Value="14"/>
        <Setter Property="Control.Height" Value="40"/>
        <Setter Property="Control.Cursor" Value="Hand"/>
    </Style>
</Application.Resources>
```

请注意，你的 `BasicControlStyle` 向内部集合添加了三个 `Setter` 对象。现在，让我们将此样式应用到主窗口中的一些控件。因为此样式是一个对象资源，所以要使用它的控件仍然需要使用 `{StaticResource}` 或 `{DynamicResource}` 标记扩展来定位样式。当它们找到样式时，会将资源项设置为同名的 `Style` 属性。用以下标记替换默认的 `Grid` 控件：

```
<StackPanel>
    <Label x:Name="lblInfo" Content="This style is boring..."
        Style="{StaticResource BasicControlStyle}" Width="150"/>
    <Button x:Name="btnTestButton" Content="Yes, but we are reusing settings!"
        Style="{StaticResource BasicControlStyle}" Width="250"/>
</StackPanel>
```

如果你在 Visual Studio 设计器中查看窗口（或运行应用程序），你会看到两个控件支持相同的鼠标指针、高度和字体大小。

### 覆盖样式设置

虽然你的两个控件都选择了样式，但如果某个控件想要应用样式然后再更改某些定义设置，那也是可以的。例如，按钮现在将使用 `Help` 鼠标指针（而不是样式中定义的 `Hand` 鼠标指针）。

```
<Button x:Name="btnTestButton" Content="Yes, but we are reusing settings!"
    Cursor="Help" Style="{StaticResource BasicControlStyle}" Width="250" />
```

样式在控件的各个属性设置之前处理，因此控件可以逐例“覆盖”设置。

### TargetType 对样式的影响

当前，你的样式的定义方式使得任何控件都可以采用它（并且必须通过设置控件的 `Style` 属性来显式采用），因为每个属性都由 `Control` 类限定。对于定义数十个设置的程序，这会产生大量的重复代码。稍微简化此样式的一种方法是使用 `TargetType` 属性。当你将此属性添加到 `Style` 的开始元素时，你可以恰好一次标记它的应用位置（在本例中是在 `App.xaml` 中）。

```
<Style x:Key="BasicControlStyle" TargetType="Control">
    <Setter Property="FontSize" Value="14"/>
    <Setter Property="Height" Value="40"/>
    <Setter Property="Cursor" Value="Hand"/>
</Style>
```

> 注意：当你构建使用基类类型的样式时，如果为派生类型不支持的依赖属性分配了值，则无需担心。如果派生类型不支持给定的依赖属性，该属性将被忽略。

这有点帮助，但你仍然拥有一个可以应用于任何控件的样式。当你想要定义一个只能应用于特定类型控件的样式时，`TargetType` 属性更有用。将以下新样式添加到应用程序的资源字典中：

```
<Style x:Key="BigGreenButton" TargetType="Button">
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="Height" Value="100"/>
    <Setter Property="Width" Value="100"/>
    <Setter Property="Background" Value="DarkGreen"/>
    <Setter Property="Foreground" Value="Yellow"/>
</Style>
```

此样式仅适用于 `Button` 控件（或 `Button` 的子类）。如果你将其应用于不兼容的元素，将会收到标记和编译器错误。添加一个使用此新样式的新 `Button`，如下所示：

```
<Button x:Name="btnAnotherButton" Content="OK!" Margin="0,10,0,0"
    Style="{StaticResource BigGreenButton}" Width="250" Cursor="Help"/>
```

你将看到类似于图 27-7 所示的输出。

![](img/587021f4d5ba039be70a11107130377b_1158_0.png)

图 27-7. 具有不同样式的控件

`TargetType` 的另一个作用是，如果不存在 `x:Key` 属性，该样式将应用于样式定义作用域内该类型的所有元素。

下面是另一个应用程序级别的样式，它将自动应用于当前应用程序中的所有 `TextBox` 控件：

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

现在你可以定义任意数量的 `TextBox` 控件，它们将自动获得定义的外观。如果某个 `TextBox` 不想要这种默认外观和感觉，它可以通过将 `Style` 属性设置为 `{x:Null}` 来退出。例如，`txtTest` 将获取默认的未命名样式，而 `txtTest2` 则按自己的方式处理。

```
<TextBox x:Name="txtTest"/>
<TextBox x:Name="txtTest2" Style="{x:Null}" BorderBrush="Black" BorderThickness="5" Height="60" Width="100" Text="Ha!"/>
```

### 对现有样式进行子类化

你还可以通过 `BasedOn` 属性使用现有样式构建新样式。要扩展的样式必须在字典中有一个合适的 `x:Key`，因为派生样式将通过 `{StaticResource}` 或 `{DynamicResource}` 标记扩展按名称引用它。下面是一个基于 `BigGreenButton` 的新样式，它将按钮元素旋转 20 度：

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

要使用此新样式，请将按钮的标记更新为以下内容：

```
<Button x:Name="btnAnotherButton" Content="OK!" Margin="0,10,0,0" Style="{StaticResource TiltButton}" Width="250" Cursor="Help"/>
```

这将外观更改为图 27-8 所示的图像。

![](img/587021f4d5ba039be70a11107130377b_1159_0.png)

图 27-8. 使用派生样式
