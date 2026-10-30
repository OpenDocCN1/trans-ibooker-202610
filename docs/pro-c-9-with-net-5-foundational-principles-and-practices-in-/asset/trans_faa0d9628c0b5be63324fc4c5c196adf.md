

## 以编程方式检查逻辑树

在运行时分析窗口的逻辑树并不是一个非常常见的 WPF 编程活动，但值得一提的是，System.Windows 命名空间定义了一个名为 LogicalTreeHelper 的类，它允许你在运行时检查逻辑树的结构。为了说明逻辑树、视觉树和控件模板之间的联系，创建一个名为 TreesAndTemplatesApp 的新 WPF 应用程序。

用以下包含两个 Button 控件和一个启用了滚动条的大型只读 TextBox 的标记替换 Grid。确保使用 IDE 处理每个按钮的 Click 事件。以下 XAML 就很不错：

```
<DockPanel LastChildFill="True">
    <Border Height="50" DockPanel.Dock="Top" BorderBrush="Blue">
        <StackPanel Orientation="Horizontal">
            <Button x:Name="btnShowLogicalTree" Content="Logical Tree of Window" Margin="4" BorderBrush="Blue" Height="40" Click="btnShowLogicalTree_Click"/>
            <Button x:Name="btnShowVisualTree" Content="Visual Tree of Window" BorderBrush="Blue" Height="40" Click="btnShowVisualTree_Click"/>
        </StackPanel>
    </Border>
    <TextBox x:Name="txtDisplayArea" Margin="10" Background="AliceBlue" IsReadOnly="True" BorderBrush="Red" VerticalScrollBarVisibility="Auto" HorizontalScrollBarVisibility="Auto" />
</DockPanel>
```

在你的 C# 代码文件中，定义一个名为 _dataToShow 的字符串成员变量。现在，在 btnShowLogicalTree 对象的 Click 处理程序中，调用一个递归调用自身的辅助函数，以使用 Window 的逻辑树填充字符串变量。为此，你将调用 LogicalTreeHelper 的静态 GetChildren() 方法。以下是代码：

```
private string _dataToShow = string.Empty;

private void btnShowLogicalTree_Click(object sender, RoutedEventArgs e)
{
    _dataToShow = "";
    BuildLogicalTree(0, this);
    txtDisplayArea.Text = _dataToShow;
}

void BuildLogicalTree(int depth, object obj)
{
    // Add the type name to the dataToShow member variable.
    _dataToShow += new string(' ', depth) + obj.GetType().Name + "\n";
    // If an item is not a DependencyObject, skip it.
    if (!(obj is DependencyObject))
        return;
    // Make a recursive call for each logical child.
    foreach (var child in LogicalTreeHelper.GetChildren((DependencyObject)obj))
    {
        BuildLogicalTree(depth + 5, child);
    }
}
```

```
private void btnShowVisualTree_Click(
    object sender, RoutedEventArgs e)
{
}
```

如果运行应用程序并单击第一个按钮，你将在文本区域看到一棵树打印出来，这几乎是原始 XAML 的精确复制品（参见图 27-10）。

![](img/587021f4d5ba039be70a11107130377b_1165_0.png)

图 27-10. 在运行时查看逻辑树

## 以编程方式检查视觉树

Window 的视觉树也可以使用 System.Windows.Media 的 VisualTreeHelper 类在运行时进行检查。以下是第二个按钮控件 btnShowVisualTree 的 Click 实现，它执行类似的递归逻辑来构建视觉树的文本表示：

```
using System.Windows.Media;

private void btnShowVisualTree_Click(object sender, RoutedEventArgs e)
{
    _dataToShow = "";
    BuildVisualTree(0, this);
    txtDisplayArea.Text = _dataToShow;
}
void BuildVisualTree(int depth, DependencyObject obj)
{
    // Add the type name to the dataToShow member variable.
    _dataToShow += new string(' ', depth) + obj.GetType().Name + "\n";
    // Make a recursive call for each visual child.
    for (int i = 0; i < VisualTreeHelper.GetChildrenCount(obj); i++)
```

```
{
    BuildVisualTree(depth + 1, VisualTreeHelper.GetChild(obj, i));
}
}
```

正如你在图 27-11 中看到的，视觉树公开了几个较低级别的渲染代理，例如 ContentPresenter、AdornerDecorator、TextBoxLineDrawingVisual 等。

![](img/587021f4d5ba039be70a11107130377b_1166_0.png)

图 27-11. 在运行时查看视觉树

## 以编程方式检查控件的默认模板

回想一下，WPF 使用视觉树来理解如何渲染 Window 和所有包含的元素。每个 WPF 控件在其默认模板中存储自己的渲染命令集。从编程角度来说，任何模板都可以表示为 ControlTemplate 类的实例。此外，你还可以通过恰当地命名的 Template 属性获得控件的默认模板，如下所示：

```
// Get the default template of the Button.
Button myBtn = new Button();
ControlTemplate template = myBtn.Template;
```

同样，你也可以在代码中创建一个新的 ControlTemplate 对象并将其插入到控件的 Template 属性中，如下所示：

```
// Plug in a new template for the button to use.
Button myBtn = new Button();
ControlTemplate customTemplate = new ControlTemplate();

// Assume this method adds all the code for a star template.
MakeStarTemplate(customTemplate);
myBtn.Template = customTemplate;
```

虽然你可以在代码中构建新模板，但更常见的做法是在 XAML 中完成它。然而，在开始构建自己的模板之前，让我们完成当前示例并添加在运行时查看 WPF 控件默认模板的能力。这是一种查看模板整体构成的有用方法。用停靠在主 DockPanel 左侧的新 StackPanel 控件更新窗口的标记，定义如下（放置在 `<TextBox>` 元素之前）：

```
<Border DockPanel.Dock="Left" Margin="10" BorderBrush="DarkGreen" BorderThickness="4" Width="358">
    <StackPanel>
        <Label Content="Enter Full Name of WPF Control" Width="340" FontWeight="DemiBold" />
        <TextBox x:Name="txtFullName" Width="340" BorderBrush="Green" Background="BlanchedAlmond" Height="22" Text="System.Windows.Controls.Button" />
        <Button x:Name="btnTemplate" Content="See Template" BorderBrush="Green" Height="40" Width="100" Margin="5" Click="btnTemplate_Click" HorizontalAlignment="Left" />
        <Border BorderBrush="DarkGreen" BorderThickness="2" Height="260" Width="301" Margin="10" Background="LightGreen">
            <StackPanel x:Name="stackTemplatePanel" />
        </Border>
    </StackPanel>
</Border>
```

添加一个空的 btnTemplate_Click() 事件处理程序函数，如下所示：

```
private void btnTemplate_Click(
    object sender, RoutedEventArgs e)
{
}
```

左上方的文本区域允许你输入位于 PresentationFramework.dll 程序集中的 WPF 控件的完全限定名称。一旦库加载，你将动态创建对象的实例并将其显示在左下角的大方块中。最后，控件的默认模板将显示在右侧的文本区域。首先，将一个类型为 Control 的新成员变量添加到你的 C# 类中，如下所示：

```
private Control _ctrlToExamine = null;
```

以下是剩余的代码，这将要求你导入 System.Reflection、System.Xml 和 System.Windows.Markup 命名空间：

```
private void btnTemplate_Click(
    object sender, RoutedEventArgs e)
{
    _dataToShow = "";
    ShowTemplate();
    txtDisplayArea.Text = _dataToShow;
}

private void ShowTemplate()
{
    // Remove the control that is currently in the preview area.
    if (_ctrlToExamine != null)
        stackTemplatePanel.Children.Remove(_ctrlToExamine);
    try
    {
        // Load PresentationFramework, and create an instance of the
        // specified control. Give it a size for display purposes, then add to the
        // empty StackPanel.
        Assembly asm = Assembly.Load("PresentationFramework, Version=4.0.0.0," +
            "Culture=neutral, PublicKeyToken=31bf3856ad364e35");
        _ctrlToExamine = (Control)asm.CreateInstance(txtFullName.Text);
        _ctrlToExamine.Height = 200;
        _ctrlToExamine.Width = 200;
        _ctrlToExamine.Margin = new Thickness(5);
        stackTemplatePanel.Children.Add(_ctrlToExamine);
        // Define some XML settings to preserve indentation.
        var xmlSettings = new XmlWriterSettings { Indent = true };
        // Create a StringBuilder to hold the XAML.
        var strBuilder = new StringBuilder();
        // Create an XmlWriter based on our settings.
        var xWriter = XmlWriter.Create(strBuilder, xmlSettings);
        // Now save the XAML into the XmlWriter object based on the ControlTemplate.
        XamlWriter.Save(_ctrlToExamine.Template, xWriter);
        // Display XAML in the text box.
        _dataToShow = strBuilder.ToString();
    }
    catch (Exception ex)
    {
        _dataToShow = ex.Message;
    }
}
```
