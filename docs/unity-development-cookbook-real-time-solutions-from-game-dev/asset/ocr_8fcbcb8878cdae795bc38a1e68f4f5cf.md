1. Create a new C# script called *MultiValue.cs*. Remove the contents of the class, and then add the following code to the top, below the standard includes:

```
#if UNITY_EDITOR
using UnityEditor;
#endif
```

This makes sure we can use the UnityEditor namespace, and the if statement ensures it's included only when this script is being run within the editor environment. For more about this syntax, see Chapter 2.

2. Next, update the class to look as follows:

```
[System.Serializable]
public class MultiValue {
    // The index of the currently selected value.
    [SerializeField] int _selectedIndex = 0;

    // The list of available options
    [SerializeField] string[] options;

    // Manages the selected index, and keeps it from going out of bounds.
    public int SelectedIndex {
        get {
            return _selectedIndex;
        }
        set {
            value = Mathf.Clamp(value, 0, options.Length);
        }
    }
}
```