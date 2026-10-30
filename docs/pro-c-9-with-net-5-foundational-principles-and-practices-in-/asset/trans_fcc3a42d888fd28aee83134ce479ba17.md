

## WPF 绑定通知系统

WinForms 绑定系统的一个显著缺陷是缺乏通知。如果视图中表示的数据以编程方式更新，UI 也必须以编程方式刷新才能保持同步。这会导致对控件调用大量 Refresh()，通常比为安全所必需的要多。虽然通常包含过多的 Refresh() 调用并不是重大的性能问题，但如果包含的调用不足，用户的体验可能会受到负面影响。

XAML 基于应用程序中内置的绑定系统通过让您将数据对象和集合挂钩到通知系统中来纠正此问题，通过将它们开发为可观察对象。每当可观察模型上的属性值发生变化或可观察集合上的集合发生变化（例如，添加、删除或重新排序项目）时，就会引发事件（NotifyPropertyChanged 或 NotifyCollectionChanged）。绑定框架会自动监听这些事件的发生，并在事件触发时更新绑定的控件。甚至更好的是，作为开发人员，您可以控制哪些属性会引发通知。听起来很完美，对吧？嗯，还不完全完美。如果您完全手工操作，为可观察模型设置此功能可能涉及相当多的代码。幸运的是，有一个开源框架可以让它变得简单得多，正如您很快就会看到的那样。

## 可观察模型和集合

在本节中，您将创建一个使用可观察模型和集合的应用程序。首先，创建一个名为 WpfNotifications 的新 WPF 应用程序。该应用程序将是一个主从表单，允许用户使用 ComboBox 选择特定的汽车，然后汽车的详细信息将显示在下面的 TextBox 控件中。通过以下标记替换默认的 Grid 来更新 MainWindow.xaml：

```
<Grid IsSharedSizeScope="True" Margin="5,0,5,5">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Grid Grid.Row="0">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" SharedSizeGroup="CarLabels"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <Label Grid.Column="0" Content="Vehicle"/>
        <ComboBox Name="cboCars" Grid.Column="1" DisplayMemberPath="PetName" />
    </Grid>
    <Grid Grid.Row="1" Name="DetailsGrid">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" SharedSizeGroup="CarLabels"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
```

```
<Label Grid.Column="0" Grid.Row="0" Content="Id"/>
<TextBox Grid.Column="1" Grid.Row="0" />
<Label Grid.Column="0" Grid.Row="1" Content="Make"/>
<TextBox Grid.Column="1" Grid.Row="1" />
<Label Grid.Column="0" Grid.Row="2" Content="Color"/>
<TextBox Grid.Column="1" Grid.Row="2" />
<Label Grid.Column="0" Grid.Row="3" Content="Pet Name"/>
<TextBox Grid.Column="1" Grid.Row="3" />
<StackPanel Grid.Column="0" Grid.ColumnSpan="2" Grid.Row="4" HorizontalAlignment="Right" Orientation="Horizontal" Margin="0,5,0,5">
    <Button x:Name="btnAddCar" Content="Add Car" Margin="5,0,5,0" Padding="4, 2" />
    <Button x:Name="btnChangeColor" Content="Change Color" Margin="5,0,5,0" Padding="4, 2"/>
</StackPanel>
</Grid>
</Grid>
```

您的窗口将类似于图 28-1。

![](img/587021f4d5ba039be70a11107130377b_1178_0.png)

图 28-1. 显示汽车详细信息的从属窗口

Grid 控件上的 IsSharedSizeScope 标签设置了子网格共享尺寸。标记为 SharedSizeGroup 的 ColumnDefinitions 将自动调整为相同的宽度，而无需任何编程。在此示例中，如果将 Pet Name 标签更改为更长的内容，Vehicle 列（位于不同的 Grid 控件中）将被调整大小以匹配它，保持窗口的外观整洁有序。

接下来，在解决方案资源管理器中右键单击项目名称，选择 添加 ► 新文件夹，并将文件夹命名为 Models。在此新文件夹中，创建一个名为 Car 的类。初始类如下所示：

```
public class Car
{
    public int Id { get; set; }
    public string Make { get; set; }
    public string Color { get; set; }
    public string PetName { get; set; }
}
```

## 添加绑定和数据

下一步是为控件添加绑定语句。请记住，数据绑定语句围绕数据上下文展开，该上下文可以设置在控件本身或父控件上。在这里，您将上下文设置在 DetailsGrid 上，因此包含的每个控件都会继承该数据上下文。将 DataContext 设置为 ComboBox 的 SelectedItem 属性。将保存详细信息控件的 Grid 更新为以下内容：

```
<Grid Grid.Row="1" Name="DetailsGrid"
    DataContext="{Binding ElementName=cboCars, Path=SelectedItem}">
```

DetailsGrid 中的文本框将显示所选汽车的各个属性。像这样向 TextBox 控件添加适当的文本属性和相关绑定：

```
<TextBox Grid.Column="1" Grid.Row="0" Text="{Binding Path=Id}" />
<TextBox Grid.Column="1" Grid.Row="1" Text="{Binding Path=Make}" />
<TextBox Grid.Column="1" Grid.Row="2" Text="{Binding Path=Color}" />
<TextBox Grid.Column="1" Grid.Row="3" Text="{Binding Path=PetName}" />
```

最后，向 ComboBox 添加数据。在 MainWindow.xaml.cs 中，创建一个新的 Car 记录列表，并将 ComboBox 的 ItemsSource 设置为该列表。同时，添加 Notifications.Models 命名空间的 using 语句。

```
using WpfNotifications.Models;
//omitted for brevity
public partial class MainWindow : Window
{
    readonly IList<Car> _cars = new List<Car>();
    public MainWindow()
    {
        InitializeComponent();
        _cars.Add(new Car {Id = 1, Color = "Blue", Make = "Chevy", PetName = "Kit"});
        _cars.Add(new Car {Id = 2, Color = "Red", Make = "Ford", PetName = "Red Rider"});
        cboCars.ItemsSource = _cars;
    }
}
```

运行应用程序。您会看到车辆选择器中有两辆车可供选择。选择其中一辆，文本框将自动填充车辆详细信息。更改其中一辆车辆的颜色，选择另一辆车辆，然后返回您编辑的车辆。您会看到新颜色确实仍然附加在车辆上。这没有什么特别之处；您在之前的示例中已经见过 XAML 数据绑定的强大功能。

## 以编程方式更改车辆数据

虽然前面的示例按预期工作，但如果数据以编程方式更改，UI 不会反映更改，除非您编程应用程序刷新数据。为了演示这一点，像这样为 btnChangeColor 按钮添加事件处理程序：

```
<Button x:Name="btnChangeColor" Content="Change Color" Margin="5,0,5,0" Padding="4, 2" Click="BtnChangeColor_OnClick"/>
```

在 BtnChangeColor_Click() 事件处理程序中，使用 ComboBox 的 SelectedItem 属性从 cars 列表中定位所选记录，并将颜色更改为粉红色。代码如下所示：

```
private void BtnChangeColor_OnClick(object sender, RoutedEventArgs e)
{
    _cars.First(x => x.Id == ((Car)cboCars.SelectedItem)?.Id).Color = "Pink";
}
```

运行应用程序，选择一辆车辆，然后单击更改颜色按钮。什么也没有明显变化。选择另一辆车辆，然后返回您最初选择的车辆。现在您会看到更新后的值。这对用户来说不是一个好的体验！

现在像这样为 btnAddCar 按钮添加事件处理程序：

```
<Button x:Name="btnAddCar" Content="Add Car" Margin="5,0,5,0" Padding="4, 2" Click="BtnAddCar_OnClick" />
```

在 BtnAddCar_Click 事件处理程序中，向 Car 列表添加一条新记录。

```
private void BtnAddCar_Click(object sender, RoutedEventArgs e)
{
    var maxCount = _cars?.Max(x => x.Id) ?? 0;
    _cars?.Add(new Car { Id=++maxCount,Color="Yellow",Make="VW",PetName="Birdie"});
}
```

运行应用程序，单击添加汽车按钮，并检查 ComboBox 的内容。即使您知道列表中有三辆车，也只有两辆被显示！为了解决这两个问题，您将把 Car 类转换为可观察模型，并使用可观察集合来保存所有 Car 实例。

## 可观察模型

模型属性上的数据更改且未在 UI 中显示的问题通过在 Car 模型类上实现 INotifyPropertyChanged 接口来解决。INotifyPropertyChanged 接口包含一个事件：PropertyChangedEvent。XAML 绑定引擎会为实现 INotifyPropertyChanged 接口的类上的每个绑定属性监听此事件。接口如下所示：

```
public interface INotifyPropertyChanged
{
    event PropertyChangedEventHandler PropertyChanged;
}
```

向 Car.cs 类添加以下 using 语句：

```
using System.ComponentModel;
using System.Runtime.CompilerServices;
```

