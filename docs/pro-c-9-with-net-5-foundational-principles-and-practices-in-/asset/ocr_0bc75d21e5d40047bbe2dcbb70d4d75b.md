```
case nameof(Color):
    return CheckMakeAndColor();
    case nameof(PetName):
        break;
    }
    return string.Empty;
}
}
```

```
internal string CheckMakeAndColor()
{
    if (Make == "Chevy" && Color == "Pink")
    {
        return $"{Make}'s don't come in {Color}";
    }
    return string.Empty;
}
```

Run the app, select the Red Rider vehicle (the Ford), and change the Make to ModelT. Once you tab out of the field, the red error decorator appears. Now select Kit (which is a Chevy) from the drop-down and click the Change Color button to change the color to Pink. Immediately the red error adorner appears on the Color field but doesn't appear on the Make text box. Now, change Make to Ford, tab out of the text box, and note that the red adorner does not disappear!

This is because the indexer runs only when the PropertyChanged event is fired for a property. As discussed in the “WPP Binding Notification System” section, the PropertyChanged event fires when the source object's property changes, and this happens either through code (such as clicking the Change Color button) or through user interaction (the timing of this is controlled through the UpdateSourceTrigger). When you changed the color, the Make property did not change, so the event did not fire for the Make property. Since the event didn’t fire, the indexer did not get called, so the validation for the Make property didn’t run.

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