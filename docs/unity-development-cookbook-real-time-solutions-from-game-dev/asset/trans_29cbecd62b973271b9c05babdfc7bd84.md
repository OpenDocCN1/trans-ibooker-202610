


```csharp
// Only draw this if we have a quest, and it has objectives
if (quest != null && quest.objectives.Count > 0)
{
    // Get the name of every objective, as an array
    var objectiveNames =
        quest.objectives.Select(o => o.name).ToArray();

    // Get the index of the currently selected objective
    var selectedObjective = objectiveNumberProperty.intValue;

    // If we somehow are referring to an object that's not
    // present in the list, reset it to the first objective
    if (selectedObjective >= quest.objectives.Count) {
        selectedObjective = 0;
    }

    // Draw the pop-up, and get back the new selection
    var newSelectedObjective = EditorGUI.Popup(thirdLinePosition,
        selectedObjective,
        objectiveNames);

    // If it was different, store it in the property
    if (newSelectedObjective != selectedObjective)
    {
        objectiveNumberProperty.intValue = newSelectedObjective;
    }
} else {
    // Draw a disabled pop-up as a visual placeholder
    using (new EditorGUI.DisabledGroupScope(true)) {
        // Show a pop-up with a single entry: the string "-".
        // Ignore its return value, since it's not interactive
        // anyway.
    }
}
```

```csharp
EditorGUI.Popup(thirdLinePosition, 0, new[] { "-" });
}
}

EditorGUI.EndProperty();
}

// Called by Unity to figure out the height of this property.
public override float GetPropertyHeight(SerializedProperty property,
    GUIContent label)
{
    // The number of lines in this property
    var lineCount = 3;

    // The number of pixels between each line
    var lineSpacing = 2;

    // The height of each line
    var lineHeight = base.GetPropertyHeight(property, label);

    // The height of this property is the number of lines times the
    // height of each line, plus the spacing between each line
    return (lineHeight * lineCount) +
        (lineSpacing * (lineCount - 1));
}
}
#endif
```

最后，我们将设置方块，使其在被点击时完成或失败目标：

- 1. 创建一个名为 *TriggerObjectiveOnClick.cs* 的新 C# 脚本，并向其中添加以下代码：

```csharp
using UnityEngine.EventSystems;

// Triggers an objective when an object enters it.
public class TriggerObjectiveOnClick :
    MonoBehaviour, IPointerClickHandler {

    // The objective to trigger, and how to trigger it.
    [SerializeField] ObjectiveTrigger objective = new ObjectiveTrigger();

    // Called when the player clicks on this object
    void IPointerClickHandler.OnPointerClick(PointerEventData eventData)
    {
        // We just completed or failed this objective!
        objective.Invoke();

        // Disable this component so that it doesn't get run twice
        this.enabled = false;
    }
}
```

- 2. 将 TriggerObjectiveOnClick 组件添加到每个方块。对每个方块，将任务资产拖入其 Quest 字段，并为目标选择适当的状态（即，将蓝色方块设置为 Failed，其余设置为 Complete）。

图 9-7 显示了配置完成后红色方块的 TriggerObjectiveOnClick 脚本示例。

> **提示**
> 画布本身可能会阻止射线检测，除非你将文本作为射线检测目标禁用。

- 3. 运行游戏。任务状态会在屏幕上显示，并在你点击不同方块时发生变化。

![](img/4dc1688a9a64dca9b820486c73104ca2_329_0.png)

图 9-7. TriggerObjectiveOnClick 的 Inspector

## 讨论

那一段相当长了！别担心，讨论部分不会和解决方案一样长。

> **提示**
> 你可以在第 12 章中了解更多关于 UI 的内容。

在这里创建任务系统涉及创建充当任务的对象（方块）、一个显示任务状态的 UI，然后是一些实际任务。在构建这样的系统时，重要的是要考虑对象可能涉及的不同状态组合。想想调皮、恶意、困惑或运气不佳的玩家可能会做什么：他们可能会做出顺序颠倒的操作、跳过内容，或找到你的代码未预料到的方法来做事情。

## 9.5 拖动方框以选择对象

### 问题

你想要点击并拖动鼠标，在屏幕上创建一个可见的矩形。当你释放鼠标时，你想要知道哪些对象位于该矩形内。

首先，我们将创建绘制方框的画布，以及方框对象本身：

- 1. 打开 GameObject 菜单，选择 UI → Canvas。
- 2. 从 GameObject 菜单中，选择 UI → Image。
- 3. 将新图像命名为“Selection Box.”

接下来，我们将创建一个在被选中时输出消息的脚本：

- 1. 创建一个名为 BoxSelectable.cs 的新 C# 脚本，并向其中添加以下代码：

```csharp
// Handles the input and display of a selection box.
public class BoxSelectable: MonoBehaviour {
    public void Selected() {
        Debug.LogFormat("{0} was selected!", gameObject.name);
    }
}
```

- 2. 创建一些对象（它们是什么不重要；立方体即可），并将它们放置在摄像机可见的位置。
- 3. 将 BoxSelectable.cs 脚本添加到每个对象。

现在，我们将创建一个脚本，用于更新选择框的位置和大小，并报告哪些对象被选中：

- 1. 创建一个名为 BoxSelection.cs 的新 C# 脚本，并向其中添加以下代码：

```csharp
// Handles the input and display of a selection box.
public class BoxSelection : MonoBehaviour
{

    // Draggable inspector reference to the Image GameObject's
    // RectTransform.
    public RectTransform selectionBox;

    // This variable will store the location of wherever we first click
    // before dragging.
    private Vector2 initialClickPosition = Vector2.zero;

    // The rectangle that the box has dragged, in screen space.
    public Rect SelectionRect { get; private set; }

    // If true, the user is actively dragging a box
    public bool IsSelecting { get; private set; }
}

// Configure the visible box
private void Start()
{

    // Setting the anchors to be positioned at zero-zero means that
    // the box's size won't change as its parent changes size
    selectionBox.anchorMin = Vector2.zero;
    selectionBox.anchorMax = Vector2.zero;

    // Setting the pivot point to zero means that the box will pivot
    // around its lower-left corner
    selectionBox.pivot = Vector2.zero;

    // Hide the box at the start
    selectionBox.gameObject.SetActive(false);
}

void Update()
{

    // When we start dragging, record the position of the mouse, and
    // start showing the box
    if (Input.GetMouseButtonDown(0))
    {
        // Get the initial click position of the mouse. No need to
        // convert to GUI space since we are using the lower left as
        // anchor and pivot.
        initialClickPosition =
            new Vector2(Input.mousePosition.x,
                Input.mousePosition.y);

        // Show the box
        selectionBox.gameObject.SetActive(true);
    }

    // While we are dragging, update the position and size of the
    // box based on the mouse position.
    if (Input.GetMouseButton(0))
    {
        // Store the current mouse position in screen space.
        Vector2 currentMousePosition =
            new Vector2(Input.mousePosition.x,
                Input.mousePosition.y);

        // Figure out the lower-left corner, and the upper-right
        // corner
        var xMin =
            Mathf.Min(currentMousePosition.x,
                initialClickPosition.x);
        var xMax =
            Mathf.Max(currentMousePosition.x,
                initialClickPosition.x);

        var yMin =
            Mathf.Min(currentMousePosition.y,
                initialClickPosition.y);
        var yMax =
            Mathf.Max(currentMousePosition.y,
                initialClickPosition.y);

        // Build a rectangle from these corners
        var screenSpaceRect = Rect.MinMaxRect(xMin, yMin,
            xMax, yMax);

        // The anchor of the box has been configured to be its
        // lower-left corner, so by setting its anchoredPosition, we
        // set its lower-left corner.
        selectionBox.anchoredPosition = screenSpaceRect.position;

        // The size delta is how far the box extends from its anchor.
        // Because the anchor's minimum and maximum are the same
        // point, changing its size delta directly changes its
        // final size.
        selectionBox.sizeDelta = screenSpaceRect.size;

        // Update our selection box
        SelectionRect = screenSpaceRect;
    }

    // When we release the mouse button, hide the box, and record
    // that we're no longer selecting
    if (Input.GetMouseButtonUp(0))
    {
        SelectionComplete();
    }
    // Hide the box
}
```


