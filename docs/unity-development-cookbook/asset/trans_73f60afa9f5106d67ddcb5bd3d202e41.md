

这段脚本看起来有点令人困惑，但其实相对简单。只是需要更多上下文来解释，所以请继续阅读。

+   3. 添加另一个类，如下所示：

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
```

```
for (int i = 0; i < labels.Length; i++) {
    labels[i] = valuesProperty.GetArrayElementAtIndex(i).stringValue;
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
```

```
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

这段很长的代码创建了一个名为 MultiValuePropertyDrawer 的新 PropertyDrawer，它定义了我们刚刚创建的类型为 MultiValue 的对象在编辑器中如何绘制。代码获取了该属性所需的所有必要部分的引用，然后绘制属性检查器的实际 UI。

我们最先创建的 MultiValue 类定义了 MultiValuePropertyDrawer 实际可以编辑的属性的结构：它包含一个字符串数组作为其可能选项，以及一个 int，指向当前选中的选项。MultiValuePropertyDrawer 中的代码会呈现这些选项，并让用户从检查器中选择一个。

让我们来测试一下：

+   1. 创建一个新的空游戏对象，并将其命名为“Demo。”
+   2. 创建一个新的 C# 脚本，命名为 MultiValueDemo.cs。将以下代码添加到其中：

```
// A simple component to demo the MultiValueChooser property drawer.
public class MultiValueDemo : MonoBehaviour {

    [SerializeField]
    MultiValue multiValue =
        new MultiValue("One", "Two", "Three", "Four", "Five", "Six");
}
```

注意 MultiValue 属性看起来像图 12-12。

![](img/e54c00769db669e3746fa1e50090241f_378_0.png)

图 12-12. MultiValue 属性的检查器

当你返回 Unity 时，检查器将看起来像图 12-13。你可以从属性顶部的栏中选择一个字符串，也可以修改可用的字符串。如果你在 MultiValueDemo 类中添加更多属性，它们将按预期显示。

![](img/e54c00769db669e3746fa1e50090241f_378_1.png)

图 12-13. 带有自定义属性绘制器的相同 MultiValue 属性的检查器

## 讨论

Unity 通常会在检查器中绘制类的子属性。如果你想覆盖此行为，你需要自己完成所有绘制。

属性绘制器不能使用更高级的 EditorGUI.Layout，而必须使用 EditorGUI。这意味着你需要自己计算和绘制矩形，就像我们在本教程中所做的那样。

![](img/e54c00769db669e3746fa1e50090241f_379_0.png)

你可以在 Unity 文档中了解更多关于 Unity Editor API 的信息。

## 12.9 属性绘制器

### 问题

你希望在某个属性附加到变量时自定义检查器如何绘制该类型的变量。例如，Header 属性会导致 Unity 在检查器中变量上方绘制一个标签。

### 解决方案

我们将添加一个属性，让你可以在变量上方绘制一个帮助框，如下所示：

```
public class HelpBoxDemo : MonoBehaviour {

    [HelpBox(text = "Here's a help box above the variable!")]
    [SerializeField] int integer;

}
```

此代码会生成图 12-14 所示的检查器。

![](img/e54c00769db669e3746fa1e50090241f_379_1.png)

图 12-14. 我们将在本教程中构建的 HelpBox 属性的实际效果

要创建 HelpBox 属性，创建一个名为 HelpBox.cs 的新 C# 脚本，并将内容替换如下：

```
#if UNITY_EDITOR
using UnityEditor;
#endif

// A HelpBoxAttribute can be placed above a variable to make it display a
// help box above it in the Inspector.

// Note how we define the HelpBoxAttribute class outside the #if
// UNITY_EDITOR areas. This is because code that refers to the HelpBox will be
```

```
// compiled outside of the editor context (that is, with UNITY_EDITOR not
// defined), and it will fail to compile if the class doesn't exist.

public class HelpBoxAttribute : PropertyAttribute
{
    // The text that will appear in the help box.
    public string text;
}

#if UNITY_EDITOR
// The code that draws the help box, as well as the original property.
[CustomPropertyDrawer(typeof(HelpBoxAttribute))]
public class HelpBoxAttributePropertyDrawer : PropertyDrawer {
    public override void OnGUI(
        Rect position, SerializedProperty property, GUIContent label)
    {

        // Let's start by calculating the rectangle in which we'll draw the
        // help box.

        // 'position' is the rectangle that we've been given to draw
        // everything to do with this property. It's calculated by taking
        // the width of the Inspector tab, and the height returned by
        // GetPropertyHeight.

        // The help box will be at the top of the property, so we just take
        // the original position, and reduce the height.
        var helpBoxPosition = position;
        helpBoxPosition.height = HelpBoxHeight;

        // Next, we figure out the rectangle we need to draw the property in.
        // We'll start with the entire available area...
        var propertyPosition = position;

        // Shift it down by the help box's height, plus line spacing
        propertyPosition.y += EditorGUIUtility.standardVerticalSpacing +
            helpBoxPosition.height;

        // And update its height to be however tall the property wants to
        // be, including any child properties.
        propertyPosition.height =
            EditorGUI.GetPropertyHeight(property, includeChildren: true);

        // Get the text from the HelpBoxAttribute.
        HelpBoxAttribute helpBox = (attribute as HelpBoxAttribute);
        string text = helpBox.text;

        // Draw the help box itself.
        EditorGUI.HelpBox(helpBoxPosition, text, MessageType.Info);

        // Draw the original property underneath.
        EditorGUI.PropertyField(
```

