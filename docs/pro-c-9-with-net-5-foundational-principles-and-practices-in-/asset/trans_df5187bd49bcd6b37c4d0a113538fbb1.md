

## INotifyDataErrorInfo

.NET 4.5 中引入的 INotifyDataErrorInfo 接口在 IDataErrorInfo 接口的基础上构建，并增加了用于验证的额外功能。当然，功能增强的同时也带来了额外的工作！与之前需要明确选择加入的验证技术相比发生了巨大转变，ValidatesOnNotifyDataErrors 绑定属性默认值为 true，因此在绑定语句中添加该属性是可选的。

INotifyDataErrorInfo 接口非常小，但要使其有效，需要相当多的管道代码，如你很快就会看到。接口如下所示：

```public interface INotifyDataErrorInfo
{
    bool HasErrors { get; }
    event EventHandler<DataErrorsChangedEventArgs>
        ErrorsChanged;
    IEnumerable GetErrors(string propertyName);
}
```

HasErrors 属性由绑定引擎用来确定实例的任何属性是否存在错误。如果使用 null 或空字符串作为 propertyName 参数调用 GetErrors() 方法，它将返回实例中存在的所有错误。如果向方法传入 propertyName，则只返回该特定属性的错误。ErrorsChanged 事件（与 PropertyChanged 和 CollectionChanged 事件类似）通知绑定引擎更新当前错误列表的 UI。

## 实现支持代码

实现 INotifyDataErrorInfo 时，大部分代码通常会推入到基模型类中，因此只需编写一次即可。开始时，在 CarPartial.cs 类中将 IDataErrorInfo 替换为 INotifyDataErrorInfo 并添加接口成员（你可以保留类中 IDataErrorInfo 的代码；稍后会更新它）。

```public partial class Car: INotifyDataErrorInfo, IDataErrorInfo
{
    ...
    public IEnumerable GetErrors(string propertyName)
    {
        throw new NotImplementedException();
    }

    public bool HasErrors { get; }
    public event
        EventHandler<DataErrorsChangedEventArgs> ErrorsChanged;
}
```

接下来，添加一个 Dictionary<string,List<string>> 来按属性名称保存任何错误。你还需要添加 System.Collections.Generic 的 using 语句。两者如下所示：

```using System.Collections.Generic;
private readonly Dictionary<string,List<string>> _errors
    = new Dictionary<string, List<string>>();
```

HasErrors 属性应在字典中存在任何错误时返回 true。这可以通过以下方式轻松实现：

```public bool HasErrors => _errors.Any();
```

接下来，创建一个用于引发 ErrorsChanged 事件的辅助方法（就像引发 PropertyChanged 事件一样），如下所示：

```private void OnErrorsChanged(string propertyName)
{
    ErrorsChanged?.Invoke(this,
        new DataErrorsChangedEventArgs(propertyName));
}
```

如前所述，GetErrors() 方法应在参数为空或 null 时返回字典中的所有错误。如果传入 propertyName 值，它将返回为该属性找到的任何错误。如果参数不匹配（或属性没有错误），则该方法将返回 null。

```public IEnumerable GetErrors(string propertyName)
{
    if (string.IsNullOrEmpty(propertyName))
    {
        return _errors.Values;
    }
    return _errors.ContainsKey(propertyName)
        ? _errors[propertyName]
        : null;
}
```

最后一组辅助方法将为属性添加一个或多个错误，或清除属性（或所有属性）的所有错误。只要字典发生变化，记得调用 OnErrorsChanged() 辅助方法。

```private void AddError(string propertyName, string error)
{
    AddErrors(propertyName, new List<string> { error });
}
```

```private void AddErrors(
    string propertyName, IList<string> errors)
{
    if (errors == null || !errors.Any())
    {
        return;
    }
    var changed = false;
    if (!_errors.ContainsKey(propertyName))
    {
        _errors.Add(propertyName, new List<string>());
        changed = true;
    }

    foreach (var err in errors)
    {
        if (_errors[propertyName].Contains(err)) continue;
        _errors[propertyName].Add(err);
        changed = true;
    }
    if (changed)
    {
        OnErrorsChanged(propertyName);
    }
}

protected void ClearErrors(string propertyName = "")
{
    if (string.IsNullOrEmpty(propertyName))
    {
        _errors.Clear();
    }
    else
    {
        _errors.Remove(propertyName);
    }
    OnErrorsChanged(propertyName);
}
```

现在问题是“如何激活此代码？”绑定引擎会监听 ErrorsChanged 事件，如果绑定语句的错误集合发生变化，它将更新 UI。但验证代码仍需要触发器来执行。有两种机制可用于此，下文将讨论。

## 使用 INotifyDataErrorInfo 进行验证

检查错误的一个地方是在属性 setter 中，如以下示例所示，简化为仅检查 ModelT 验证：

```public string Make
{
    get { return _make; }
    set
    {
        if (value == _make) return;
        _make = value;
        if (Make == "ModelT")
        {
            AddError(nameof(Make), "Too Old");
        }
        else
        {
            ClearErrors(nameof(Make));
        }
        OnPropertyChanged(nameof(Make));
        OnPropertyChanged(nameof(Color));
    }
}
```

这种方法的主要问题是你必须将验证逻辑与属性 setter 结合起来，使代码更难阅读和维护。

## 将 IDataErrorInfo 与 INotifyDataErrorInfo 结合用于验证

你在上一节中看到 IDataErrorInfo 可以添加到 partial 类中，这意味着你不必更新 setter。你还看到索引器在属性上引发 PropertyChanged 时会自动被调用。将 IDataErrorInfo 和 INotifyDataErrorInfo 结合使用可为你提供 INotifyDataErrorInfo 的验证附加功能，以及 IDataErrorInfo 提供的与 setter 的分离。

使用 IDataErrorInfo 的目的是不是运行验证，而是确保利用 INotifyDataErrorInfo 的验证代码在对象上每次引发 PropertyChanged 时都会被调用。由于你不使用 IDataErrorInfo 进行验证，始终从索引器返回 string.Empty。将索引器和 CheckMakeAndColor() 辅助方法更新为以下代码：

```public string this[string columnName]
{
    get
    {
        ClearErrors(columnName);
        switch (columnName)
        {
            case nameof(Id):
                break;
            case nameof(Make):
                CheckMakeAndColor();
                if (Make == "ModelT")
                {
                    AddError(nameof(Make), "Too Old");
                    hasError = true;
                }
                break;
            case nameof(Color):
                CheckMakeAndColor();
                break;
            case nameof(PetName):
                break;
        }
        return string.Empty;
    }
}

internal bool CheckMakeAndColor()
{
    if (Make == "Chevy" && Color == "Pink")
    {
        AddError(nameof(Make), $"{Make}'s don't come in {Color}");
        AddError(nameof(Color),
            $"{Make}'s don't come in {Color}");
        return true;
    }
    return false;
}
```

运行应用，选择 Chevy，然后将颜色更改为 Pink。除了 Make 和 Model 文本框周围的红色装饰器外，你还会看到包含 Car 详细信息字段的整个网格周围出现红色框装饰器（见图 28-3）。

![](img/587021f4d5ba039be70a11107130377b_1193_0.png)

图 28-3. 更新后的错误装饰器

这是使用 INotifyDataErrorInfo 的另一个优势。除了出错的控件外，定义数据上下文的控件也会被错误模板装饰。

## 显示所有错误

Validation 类上的 Errors 属性以 ValidationError 对象的形式返回特定对象上的所有验证错误。每个 ValidationError 对象都有一个 ErrorContent 属性，其中包含该属性的错误消息列表。这意味着你想要显示的错误消息位于列表中的列表内。要正确显示它们，你需要创建一个包含 ListBox 的 ListBox 来显示数据。听起来有点递归，但一旦你看到它就会明白。

首先向 DetailsGrid 添加另一行，并确保 Window 的 Height 至少为 300。在最后一行添加一个 ListBox，并将 ItemsSource 绑定到 DetailsGrid，使用 Validation.Errors 作为路径，如下所示：

```<ListBox Grid.Row="6" Grid.Column="0" Grid.ColumnSpan="2"
    ItemsSource="{Binding ElementName=DetailsGrid, Path=(Validation.Errors)}">
</ListBox>
```

