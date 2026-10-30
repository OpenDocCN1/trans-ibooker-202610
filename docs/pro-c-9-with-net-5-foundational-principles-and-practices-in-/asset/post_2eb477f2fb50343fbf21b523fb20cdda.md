
> **Note:** For each of the sections in this chapter, you can continue working in the same project from the previous section, or you can create a copy of the project for each new section. In the repo for this chapter, each section is a different project.

## Updating the Sample for the Validation Examples

In the repo for this chapter, the new project (copied from the previous example) is called WpfValidations. If you are using the same project from the previous section, you just need to make note of the namespace changes when copying code into your project from the examples listed in this section.

## The Validation Class

Before adding validations to your project, it’s important to understand the Validation class. This class is part of the validation framework, and it provides methods and attached properties that can be used to display validation results. There are three main properties of the Validation class commonly used when handling validation errors (shown in Table 28-2). You will use each of these through the rest of this section.

**Table 28-2. Key Members of the Validation Class**

| Member | Meaning in Life |
|---|---|
| HasError | Attached property indicating that a validation rule failed somewhere in the process |
| Errors | Collection of all active ValidationError objects |
| ErrorTemplate | Control template that becomes visible and adorns the bound element when HasError is set to true |

## Validation Options

As mentioned, XAML technologies have several mechanisms for incorporating validation logic into your application. You will examine three of the most commonly used validation choices in the next sections.

### Notify on Exceptions

While exceptions should not be used to enforce business logic, exceptions can and do happen, and they should be handled appropriately. In case they aren’t handled in code, the user should receive visual feedback of the problem. An important change from WinForms is that WPF binding exceptions are not, by default, propagated to the user as exceptions. They are, however, visually indicated, using an adorner (visual layer that resides on top of your controls).

To test this, run the app, select a record from the ComboBox, and clear out the Id value. Since the Id property is defined as an int (not a nullable int), a numeric value is required. When you tab out of the Id field, an empty string is sent to the Id property by the binding framework, and since an empty string can’t be converted to an int, an exception is thrown in the setter. Normally, an unhandled exception would generate a message box to the user, but in this case, nothing like that happened. If you look in the Debug portion of the Output window, you will see the following:

```
System.Windows.Data Error: 7 : ConvertBack cannot convert value '' (type 'String').
BindingExpression:Path=Id; DataItem='Car' (HashCode=52579650); target element is 'TextBox' (Name=''); target property is 'Text' (type 'String') FormatException:'System.FormatException: Input string was not in a correct format.
```

Visual display of the exception is a thin red box around the control, as shown in Figure 28-2.

![](img/587021f4d5ba039be70a11107130377b_1186_0.png)

**Figure 28-2. The default error template**

The red box is the ErrorTemplate property of the Validation object and acts as an adorner for the bound control. While the default error adorner shows that there is indeed an error, there isn't any indication as to what is wrong. The good news is that the ErrorTemplate is completely customizable, as you will see later in this chapter.

### IDataErrorInfo

The IDataErrorInfo interface provides a mechanism for you to add custom validations to your model classes. This interface is added directly to your model (or view model) classes, and the validation code is placed inside your model classes (preferably in partial classes). This centralizes validation code in your project, in direct contrast to WinForms projects, where validation was typically done in the UI itself.

The IDataErrorInfo interface, shown here, contains two properties: an indexer and a string property named Error. Note that the WPF binding engine doesn't use the Error property.

```
public interface IDataErrorInfo
{
    string this[string columnName] { get; }
    string Error { get; }
}
```

You will be adding the Car partial class shortly, but first you need to update the Car.cs class and mark it as partial. Next, add another file to the Models directory named CarPartial.cs. Rename this class Car, make sure the class is marked as partial, and add the IDataErrorInfo interface. Finally, implement the API for the interface. The initial code is listed here:

```
public partial class Car : IDataErrorInfo
{
    public string this[string columnName] => string.Empty;
    public string Error { get; }
}
```

For a bound control to opt in to the IDataErrorInfo interface, it must add ValidatesOnDataErrors to the binding expression. Update the binding expression for the Make text box to the following (and update the rest of the binding statements in the same way):

```
<TextBox Grid.Column="1" Grid.Row="1" Text="{Binding Path=Make, ValidatesOnDataErrors=True}" />
```

Once this update is made to the binding statements, the indexer on the model gets called each time the PropertyChanged event is raised. The property name from the event is used as the columnName parameter in the indexer. If the indexer returns string.Empty, the framework assumes that all validations passed, and no error condition exists. If the indexer returns anything but string.Empty, an error is presumed to exist on the property for that object instance, and each control that is bound to the property being validated on this specific instance of the class is considered to have an error, the HasError property of the Validation object is set to true, and the ErrorTemplate adorner is activated for the controls affected.

Next, you will add some simple validation logic to the indexer in CarPartial.cs. The validation rules are simple.

- If Make equals ModelT, set the error equal to "Too Old".
- If Make equals Chevy and Color equals Pink, set the error equal to `$"{Make}'s don't come in {Color}"`.

Start by adding a switch statement for each of the properties. To avoid using magic strings in the case statements, you will again use the nameof method. If the code falls through the switch statement, return string.Empty. Next, add the validation rules. In the proper case statements, add a check of the property value based on the rules listed earlier. In the case statement for the Make property, first check to make sure the value isn't ModelT. If it is, return the error. If that passes, the next line will call into a helper method that returns an error if the second rule is violated, or it will return string.Empty if it is not. In the case statement for the Color property, also call the helper method. The code is as follows:

```
public string this[string columnName]
{
    get
    {
        switch (columnName)
        {
            case nameof(Id):
                break;
            case nameof(Make):
                return Make == "ModelT"
                    ? "Too Old"
                    : CheckMakeAndColor();
            case nameof(Color):
                return CheckMakeAndColor();
            case nameof(PetName):
                break;
        }
        return string.Empty;
    }
}

internal string CheckMakeAndColor()
{
    if (Make == "Chevy" && Color == "Pink")
    {
        return $"{Make}'s don't come in {Color}";
    }
    return string.Empty;
}
```

Run the app, select the Red Rider vehicle (the Ford), and change the Make to ModelT. Once you tab out of the field, the red error adorner appears. Now select Kit (which is a Chevy) from the drop-down and click the Change Color button to change the color to Pink. Immediately the red error adorner appears on the Color field but doesn't appear on the Make text box. Now, change Make to Ford, tab out of the text box, and note that the red adorner does not disappear!

This is because the indexer runs only when the PropertyChanged event is fired for a property. As discussed in the "WPF Binding Notification System" section, the PropertyChanged event fires when the source object's property changes, and this happens either through code (such as clicking the Change Color button) or through user interaction (the timing of this is controlled through the UpdateSourceTrigger). When you changed the color, the Make property did not change, so the event did not fire for the Make property. Since the event didn’t fire, the indexer did not get called, so the validation for the Make property didn’t run.

There are two ways to fix this. The first is to change PropertyChangedEventArgs to update every bound property by passing in string.Empty instead of a field name. As discussed, this causes the binding engine to update every property on that instance. Update the OnPropertyChanged() method in the Car.cs class like this:

```
protected virtual void OnPropertyChanged([CallerMemberName] string propertyName = "")
{
    if (propertyName != nameof(IsChanged))
    {
        IsChanged = true;
    }
    //PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    PropertyChanged?.Invoke(this,
        new PropertyChangedEventArgs(string.Empty));
}
```

Now, when you run the same test, you see that both the Make and Color text boxes are adorned with the error template when one of them is updated. So, why not always raise the event in this manner? It's largely a matter of performance. It's possible that refreshing every property on an object could hamper performance. Of course, there's no way to know without testing, and your mileage may (and probably will) vary.

The other solution is to raise the PropertyChanged event for the other dependent field(s) when one changes. The downside to using this mechanism is that you (or other developers who support your app) must know that the Make and Color properties are related through the validation code.
