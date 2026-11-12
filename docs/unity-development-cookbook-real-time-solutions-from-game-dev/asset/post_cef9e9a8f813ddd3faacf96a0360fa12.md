

.InstantiatePrefab(wall.brickPrefab.gameObject)
as GameObject;

// Give it a name appropriate to its position
brick.name = string.Format("{0} ({1},{2})",
    wall.brickPrefab.name,
    column,
    row);

// Place it in the scene
brick.transform.SetParent(wall.transform, false);

// Update its position, relative to its parent
brick.transform.localPosition = columnPosition;

// Don't rotate it, relative to its parent
brick.transform.localRotation = Quaternion.identity;
}
}

Phew. That's a long piece of code. Now we can use it:

- 1. Create a new, empty game object, and name it “Wall.” Add a Wall component to it.
- 2. Drag the Brick prefab into the Brick Prefab slot.
- 3. Click the Create Wall button. A wall of bricks will appear! You can see an example of the input in the custom Inspector, and the output, in Figure 12-11.

![](img/4dc1688a9a64dca9b820486c73104ca2_483_0.png)

Figure 12-11. Click the Create Wall button and a brick wall appears

## Discussion

EditorGUILayout and GUILayout both provide similar versions of common editor UI elements. It’s important that, when designing Unity editors, you consider which set of controls you want to use.

> **TIP**
>
> When you customize a Unity Inspector component, you replace the default layout with the script you create.

## 12.8 Property Drawers

## Problem

You want to customize how the Inspector draws variables of a certain type. For example, you’ve got a custom class, and you want to customize how it appears.

## Solution

We’ll demonstrate how to do this by defining a new type, called MultiValue. This will contain a list of strings, one of which can be chosen in the Inspector:

- 1. Create a new C# script called *MultiValue.cs*. Remove the contents of the class, and then add the following code to the top, below the standard includes:

```
#if UNITY_EDITOR
using UnityEditor;
#endif
```

This makes sure we can use the UnityEditor namespace, and the if statement ensures it's included only when this script is being run within the editor environment. For more about this syntax, see Chapter 2.

- 2. Next, update the class to look as follows:

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

    // Creates a new chooser, using the specified values.
    public MultiValue(params string[] values) {
        this.options = values;
    }

    // Returns the text of the currently selected value.
    public string SelectedValue {
        get {
            if (options.Length > 0) {
                return options[_selectedIndex];
            } else {
                return null;
            }
        }
    }
}
```

This script may look a little confusing, but it's relatively simple. It needs a little more context to explain though, so read on.

- 3. Add another class, as follows:

```
#if UNITY_EDITOR
// Overrides how Unity will draw a MultiValue property.
[CustomPropertyDrawer(typeof(MultiValue))]
public class MultiValuePropertyDrawer : PropertyDrawer {

    // Called by Unity when it needs to draw a MultiValue property in
    // the Inspector.
    public override void OnGUI(Rect position,
        SerializedProperty property, GUIContent label)
    {
        // Ensure that the controls found in the GUI class behave
        // properly. This also tells Unity that any edit to any field in
        // here should be recorded for the purposes of Undoing them.
        EditorGUI.BeginProperty(position, label, property);

        // Get a reference to the variables that store the info we need
        var indexProperty =
            property.FindPropertyRelative("_selectedIndex");

        var valuesProperty = property.FindPropertyRelative("options");

        // Calculate the rectangle to draw the first line in. This will
        // hold our toolbar (our list of buttons).
        var firstLinePosition = position;
        firstLinePosition.height =
            EditorGUI.GetPropertyHeight(indexProperty);

        // Use this to calculate the rectangle to draw the second
        // property in. (This will vary, depending on whether the user
        // has elected to expand the list in the Inspector or not.)
        var secondLinePosition = firstLinePosition;
        secondLinePosition.y += 2 + firstLinePosition.height;
        secondLinePosition.height =
            EditorGUI.GetPropertyHeight(valuesProperty);

        // Display the label in front of the toolbar, and get back a new
        // rectangle to draw the toolbar in.
        firstLinePosition = EditorGUI.PrefixLabel(
            firstLinePosition, new GUIContent(property.displayName));

        // Get every string inside the "options" property, as an array
        string[] labels = new string[valuesProperty.arraySize];

        for (int i = 0; i < labels.Length; i++) {
            labels[i] =
                valuesProperty.GetArrayElementAtIndex(i).stringValue;
        }

        // Because Toolbar is not in the EditorGUI class, it won't
        // automatically report to the editor that it was updated in a
        // way that the editor can track for the purposes of the Undo
        // system. So, we use BeginChangeCheck before drawing the
        // toolbar, and call EndChangeCheck. If EndChangeCheck returns
        // true, the user made a change.
        EditorGUI.BeginChangeCheck();
        var index = indexProperty.intValue;
        var newValue = GUI.Toolbar(firstLinePosition, index, labels);
        if (EditorGUI.EndChangeCheck()) {
            // The toolbar was changed.
            indexProperty.intValue = newValue;
        }

        // Draw the 'options' list as a regular list. This will also
        // draw things like the expand arrow, the items in the list,
        // and the number of items in the list.
        EditorGUI.indentLevel += 1;
        EditorGUI.PropertyField(secondLinePosition,
            valuesProperty, true);
        EditorGUI.indentLevel -= 1;

        // We're done editing this property.
        EditorGUI.EndProperty();
    }

    // Called by Unity to determine the height of the
    // MultiValue property.
    public override float GetPropertyHeight(SerializedProperty property,
        GUIContent label)
    {
        // The height of a MultiValue property is the height of both of
        // its two child properties, plus the spacing between them.
        float lineSpacing = EditorGUIUtility.standardVerticalSpacing;

        // Get the child properties
        var indexProperty =
            property.FindPropertyRelative("_selectedIndex");

        var valuesProperty = property.FindPropertyRelative("options");

        // Calculate the height of this property by getting the height
        // of both properties (including the strings inside the options,
        // if it's been expanded), plus the line spacing
        float indexHeight = EditorGUI.GetPropertyHeight(indexProperty);
        float optionsHeight =
            EditorGUI.GetPropertyHeight(valuesProperty, true);

        return indexHeight + lineSpacing + optionsHeight;
    }
}

#endif
```

This long piece of code creates a new PropertyDrawer named MultiValuePropertyDrawer that defines how something of type MultiValue, which we created a moment ago, is drawn in the editor. The code gets a reference to all the necessary bits of the property in question, and then draws the actual UI for the property Inspector.

The MultiValue class we created first defines the structure of the property that MultiValuePropertyDrawer can actually edit: it contains an array of strings as its possible options, and an int, which points to the option that's currently selected. The code in MultiValuePropertyDrawer presents the options, and lets the user pick one from the Inspector.

Let's test it out:

- 1. Create a new, empty game object, and call it “Demo.”
- 2. Create a new C# script called MultiValueDemo.cs. Add the following code to it:

```
// A simple component to demo the MultiValueChooser property drawer.
public class MultiValueDemo : MonoBehaviour {

    [SerializeField]
    MultiValue multiValue =
        new MultiValue("One", "Two", "Three", "Four", "Five", "Six");
}
```

Note how the MultiValue property looks like Figure 12-12.

![](img/4dc1688a9a64dca9b820486c73104ca2_489_0.png)

Figure 12-12. The Inspector for a MultiValue property

When you return to Unity, the Inspector will look like Figure 12-13. You can select a string from the bar at the top of the property, and modify the available strings as well. If you add more properties to the MultiValueDemo class, they will be displayed as you’d expect.

