+   1. Create a new C# script called ListItem.cs. Remove the contents of the class, and add the following code to it:

```
[SerializeField] UnityEngine.UI.Text labelText;
```

This creates a label to store the text that will be used for a list item entry, and exposes it to the Unity Inspector.

+   2. Add the following property:

```
public string Label
{
    get
    {
        return labelText.text;
    }
    set
    {
        labelText.text = value;
    }
}
```

This exposes a string, allowing you to get or set the contents of the label.

TIP
For more information on the syntax for creating getters and setters, check out Chapter 2.

Now we need to add the script we’ve just created to the prototype object:

+   1. Add a ListItem script to the List Item.

+   2. Drag the Text object into the Label Text field.