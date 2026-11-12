We'll add an attribute that lets you draw a help box above variables, like so:

```
```csharp
public class HelpBoxDemo : MonoBehaviour {
    [HelpBox(text = "Here's a help box above the variable!")]
    [SerializeField] int integer;
}
```
```

This code produces the Inspector shown in Figure 12-14.

![](bbox=[0.143, 0.3, 0.856, 0.458])

Figure 12-14. The HelpBox attribute, which we’ll build in this recipe, in action

To create the HelpBox attribute, create a new C# script called HelpBox.cs, and replace the contents as follows:

```
```csharp
#if UNITY_EDITOR
using UnityEditor;
#endif

// A HelpBoxAttribute can be placed above a variable to make it display a
// help box above it in the Inspector.

// Note how we define the HelpBoxAttribute class outside the #if
// UNITY_EDITOR areas. This is because code that refers to the HelpBox will be
// compiled outside of the editor context (that is, with UNITY_EDITOR not
// defined), and it will fail to compile if the class doesn't exist.

public class HelpBoxAttribute : PropertyAttribute
{
    // The text that will appear in the help box.
    public string text;
}

#if UNITY_EDITOR
```
```