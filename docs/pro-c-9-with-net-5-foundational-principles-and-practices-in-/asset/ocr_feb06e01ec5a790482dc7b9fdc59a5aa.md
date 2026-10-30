```
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

Now the question is “how is this code activated?” The binding engine listens for the ErrorsChanged event and will update the UI if there is a change in the errors collection for a binding statement. But the validation code still needs a trigger to execute. There are two mechanisms for this, and they will be discussed next.

# Use INotifyDataErrorInfo for Validations

One place to check for errors is in the property setters, like the following example, simplified to just check for the ModelT validation:

```
public string Make
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
    }
}
```