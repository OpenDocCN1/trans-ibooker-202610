

向 ListBox 添加一个 DataTemplate，并在 DataTemplate 中添加一个绑定到 ErrorContent 属性的 ListBox。在这种情况下，每个 ListBoxItem 的数据上下文都是一个 ValidationError 对象，因此你不需要设置数据上下文，只需设置路径。将绑定路径设置为 ErrorContent，如下所示：

```
<ListBox.ItemTemplate>
    <DataTemplate>
        <ListBox ItemsSource="{Binding Path=ErrorContent}"/>
    </DataTemplate>
</ListBox.ItemTemplate>
```

运行应用程序，选择 Chevy，并将颜色设置为 Pink。你将看到如图 28-4 所示的错误。

![](img/587021f4d5ba039be70a11107130377b_1194_0.png)

图 28-4. 显示 errors 集合

这只是使用验证以及显示生成的错误所能做到的事情的冰山一角，但它应该能让你很好地开始开发能够改善用户体验的信息性 UI。

## 将支持代码移至基类

正如你可能注意到的那样，CarPartial.cs 类中现在有很多代码。由于此示例只有一个模型类，这并不太糟糕。但随着你在真实应用程序中添加模型，你不希望在每个模型的 partial 类中都添加所有这些管道代码。最好的做法是将所有这些支持代码下推到一个基类中。你现在就这样做。

在 Models 文件夹中添加一个新的类文件，命名为 BaseEntity.cs。添加 System.Collections 和 System.ComponentModel 的 using 语句。使类为 public，并添加 INotifyDataErrorInfo 接口，如下所示：

```
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
```

```
namespace Validations.Models
{
    public class BaseEntity : INotifyDataErrorInfo
}
```

将 CarPartial.cs 中与 INotifyDataErrorInfo 相关的所有代码移至新的基类。任何私有方法和变量都需要改为 protected。接下来，从 CarPartial.cs 类中移除 INotifyDataErrorInfo 接口，并添加 BaseEntity 作为基类，如下所示：

```
public partial class Car : BaseEntity, IDataErrorInfo
{
    //removed for brevity
}
```

现在，你创建的任何额外模型类都将继承所有 INotifyDataErrorInfo 管道代码。

## 使用 WPF 利用数据注解

WPF 也可以利用数据注解进行 UI 验证。让我们向 Car 模型添加一些数据注解。

### 向模型添加数据注解

打开 Car.cs 并添加 System.ComponentModel.DataAnnotations 的 using 语句。向 Make、Color 和 PetName 添加 [Required] 和 [StringLength(50)] 属性。Required 属性添加一条验证规则，即属性不能为空（对于 Id 属性来说这有点冗余，因为它不是可空的 int）。StringLength(50) 属性添加一条验证规则，即属性值的长度不能超过 50 个字符。

### 检查基于数据注解的验证错误

在 WPF 中，你必须以编程方式检查基于数据注解的验证错误。用于注解验证的两个关键类是 ValidationContext 和 Validator 类。ValidationContext 类为检查类是否存在验证错误提供了一个上下文。Validator 类允许你在 ValidationContext 中检查对象是否存在基于属性的错误。

打开 BaseEntity.cs，并添加以下 using 语句：

```
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
```

接下来，创建一个名为 GetErrorsFromAnnotations() 的新方法。该方法为泛型，接受一个字符串类型的属性名和一个类型为 T 的值作为参数，并返回一个字符串数组。确保该方法标记为 protected。其签名如下所示：

```
protected string[] GetErrorsFromAnnotations<T>(
    string propertyName, T value)
{
```

在方法中，创建一个 List<ValidationResult> 变量来保存验证检查的结果，并创建一个作用域限定为传入方法中的属性名的 ValidationContext。当这两个项目就绪后，调用 Validate.TryValidateProperty，它返回一个 bool。如果一切通过（关于数据注解验证），它返回 true。如果不通过，它返回 false 并用错误填充 List<ValidationResult>。完整的代码如下所示：

```
protected string[] GetErrorsFromAnnotations<T>(
    string propertyName, T value)
{
    var results = new List<ValidationResult>();
    var vc = new ValidationContext(this, null, null)
    { MemberName = propertyName };
    var isValid = Validator.TryValidateProperty(
        value, vc, results);
    return (isValid)
        ? null
        : Array.ConvertAll(
            results.ToArray(), o => o.ErrorMessage);
}
```

现在你可以更新 CarPartial.cs 中的索引器方法，以检查是否存在基于数据注解的任何错误。如果发现任何错误，将它们添加到支持 INotifyDataErrorInfo 的 errors 集合中。这使我们能够清理错误处理。在索引器方法的开始处，清除该列的错误。然后处理验证，最后处理实体的自定义逻辑。更新后的索引器代码如下所示：

```
public string this[string columnName]
{
    get
    {
        ClearErrors(columnName);
        var errorsFromAnnotations =
            GetErrorsFromAnnotations(columnName,
                typeof(Car)
                    .GetProperty(columnName)?.GetValue(this,null));
        if (errorsFromAnnotations != null)
        {
            AddErrors(columnName, errorsFromAnnotations);
        }
        switch (columnName)
        {
            case nameof(Id):
                break;
            case nameof(Make):
                CheckMakeAndColor();
                if (Make == "ModelT")
                {
                    AddError(nameof(Make), "Too Old");
                }
                break;
```

```
case nameof(Color):
    CheckMakeAndColor();
    break;
case nameof(PetName):
    break;
}
return string.Empty;
}
```

运行应用程序，选择其中一辆车，并为颜色添加超过 50 个字符的文本。当你超过 50 个字符的阈值时，StringLength 数据注解会创建验证错误，并向用户报告，如图 28-5 所示。

![](img/587021f4d5ba039be70a11107130377b_1197_0.png)

图 28-5. 验证 required 数据注解

## 自定义 ErrorTemplate

最后一个主题是创建一个在控件处于错误状态时应用的样式，并更新 ErrorTemplate 以显示更有意义的错误信息。正如你在第 27 章中学到的那样，控件可以通过样式和控件模板进行自定义。

首先在 MainWindow.xaml 的 Windows.Resources 部分添加一个新的样式，其目标类型为 TextBox。接下来，添加一个触发器到样式上，当 Validation.HasError 属性设置为 true 时设置属性。属性及其要设置的值为 Background（Pink）、Foreground（Black）以及 ToolTip 为 ErrorContent。Background 和 Foreground 设置器没什么新意，但设置 ToolTip 的语法需要一些解释。绑定指向此样式所应用的控件指向自身，在本例中为 TextBox。路径是 Validation.Errors 集合的第一个 ErrorContent 值。标记如下：

```
<Window.Resources>
    <Style TargetType="{x:Type TextBox}">
        <Style.Triggers>
            <Trigger Property="Validation.HasError" Value="true">
                <Setter Property="Background" Value="Pink" />
```

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

运行应用程序并创建一个错误条件。结果将类似于图 28-6，完整显示了显示错误消息的工具提示。

![](img/587021f4d5ba039be70a11107130377b_1198_0.png)

图 28-6. 显示自定义 ErrorTemplate

之前的样式更改了任何处于错误条件的 TextBox 的外观。接下来，你将创建一个自定义控件模板来更新 Validation 类的 ErrorTemplate，以显示红色感叹号并为感叹号设置工具提示。ErrorTemplate 是一个装饰器，位于控件顶部。虽然刚刚创建的样式更新了控件本身，但 ErrorTemplate 将位于控件顶部。

在你刚刚创建的样式中，在 Style.Triggers 关闭标签之后立即放置一个 setter。你将创建一个控件模板，它由一个 TextBlock（用于显示感叹号）和一个 BorderBrush 组成，用于环绕包含错误(s)的 TextBox。XAML 中有一个特殊的标签用于被 ErrorTemplate 装饰的控件，名为 AdornedElementPlaceholder。通过为此控件添加名称，你可以访问与控件关联的错误。在此示例中，你希望访问 Validation.Errors 属性以获取 ErrorContent（就像你在 Style.Trigger 中所做的那样）。以下是 setter 的完整标记：

```
<Setter Property="Validation.ErrorTemplate">
    <Setter.Value>
        <ControlTemplate>
            <DockPanel LastChildFill="True">
                <TextBlock Foreground="Red" FontSize="20" Text="!"
                    ToolTip="{Binding ElementName=controlWithError,
                    Path=AdornedElement.(Validation.Errors)[0].ErrorContent}"/>
```

