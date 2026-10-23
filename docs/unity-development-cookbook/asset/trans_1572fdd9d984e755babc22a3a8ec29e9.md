

+   2. 为每个立方体添加一个 TriggerObjectiveOnClick 组件。对于每个立方体，将任务资源拖到其 Quest 字段中，并为目标选择合适的状态（即，将蓝色立方体设置为 Failed，其余设置为 Complete）。

图 9-7 显示了红色立方体的 TriggerObjectiveOnClick 脚本在配置后的示例。

![](img/e54c00769db669e3746fa1e50090241f_264_0.png)

如果不禁用文本作为射线投射目标，画布本身可能会阻挡你的射线投射。

+   3. 运行游戏。任务的状态显示在屏幕上，并在你点击不同立方体时发生变化。

![](img/e54c00769db669e3746fa1e50090241f_264_1.png)

图 9-7. TriggerObjectiveOnClick 的检查器

## Discussion

所以这是一个很长的部分！别担心，讨论不会像解决方案那样长。

![](img/e54c00769db669e3746fa1e50090241f_264_2.png)

你可以在第 12 章了解更多关于 UI 的内容。

在此创建任务系统涉及创建作为任务的对象（立方体）、显示任务状态的 UI，然后是实际的任务。在构建此类系统时，重要的是要考虑对象可能涉及的各种状态组合。想想一个调皮的、恶意的、困惑的或不幸的玩家可能会做什么：他们可能会乱序进行、跳过内容，或找到你的代码未预期的操作方式。

## 9.5 Dragging a Box to Select Objects

### Problem

你想要点击并拖动鼠标在屏幕上创建可见的矩形。松开鼠标时，你想要知道哪些对象在该矩形内。

### Solution

首先，我们将创建绘制框的画布，以及框对象本身：

+   1. 打开 GameObject 菜单，并选择 UI → Canvas。
+   2. 从 GameObject 菜单中选择 UI → Image。
+   3. 将新图像命名为“Selection Box。”

接下来，我们将创建一个在被选中时输出消息的脚本：

+   1. 创建一个新的 C# 脚本名为 BoxSelectable.cs，并添加以下代码：

```
// Handles the input and display of a selection box.
public class BoxSelectable: MonoBehaviour {

    public void Selected() {

        Debug.LogFormat("{0} was selected!", gameObject.name);

    }
}
```

+   2. 创建一些对象——是什么并不重要；立方体即可——并放置它们使摄像机可以看到它们。
+   3. 将 BoxSelectable.cs 脚本添加到每个对象。

现在我们将创建一个脚本来更新选择框的位置和大小，并报告哪些对象被选中：

+   1. 创建一个新的 C# 脚本名为 BoxSelection.cs，并添加以下代码：

```
// Handles the input and display of a selection box.
public class BoxSelection : MonoBehaviour
{
    // Draggable inspector reference to the Image GameObject's
    // RectTransform.
    public RectTransform selectionBox;

```
// This variable will store the location of wherever we first click
// before dragging.
private Vector2 initialClickPosition = Vector2.zero;

// The rectangle that the box has dragged, in screen space.
public Rect SelectionRect { get; private set; }

// If true, the user is actively dragging a box
public bool IsSelecting { get; private set; }

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
```

```
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

    // Hide the box
    selectionBox.gameObject.SetActive(false);

    // We're no longer selecting
    IsSelecting = false;
}
}
```

```
// Called when the user finishes dragging a selection box.
void SelectionComplete()
{
    // Get the component attached to this scene
    Camera mainCamera = GetComponent<Camera>();

    // Get the bottom-left and top-right corners of the screen-space
    // selection view, and convert them to viewport space
    var min = mainCamera.ScreenToViewportPoint(
        new Vector3(SelectionRect.xMin, SelectionRect.yMin));
    var max = mainCamera.ScreenToViewportPoint(
        new Vector3(SelectionRect.xMax, SelectionRect.yMax));

    // We want to create a bounding box in viewport space. We have
    // the x and y coordinates of the bottom-left and top-right
    // corners; now we'll include the z coordinates.
    min.z = mainCamera.nearClipPlane;
    max.z = mainCamera.farClipPlane;

    // Construct our bounding box
    var viewportBounds = new Bounds();
    viewportBounds.SetMinMax(min, max);

    // Check each object that has a Selectable component
    foreach (var selectable in FindObjectsOfType<BoxSelectable>()) {

        // Figure out where this object is in viewport space
        var selectedPosition = selectable.transform.position;

        var viewportPoint =
            mainCamera.WorldToViewportPoint(selectedPosition);

        // Is that point within our viewport bounding box? If it is,
        // they're selected.
        var selected = viewportBounds.Contains(viewportPoint);

        if (selected) {
            // Let them know.
            selectable.Selected();
        }
    }
}
```

+   2. 将 BoxSelection 组件附加到主摄像机。
+   3. 将 Selection Box 图像拖到 Selection Box 字段上。

4. 运行游戏。你可以点击并拖动来创建框。松开鼠标时，所有在矩形内且具有 BoxSelectable 组件的对象都会记录它们被选中。

### Discussion

此解决方案最有趣的部分是使用视口空间，它是归一化的且相对于摄像机。

![](img/e54c00769db669e3746fa1e50090241f_269_0.png)

如果使用切片精灵，你的选择框可以有边框。

## 9.6 Creating a Menu Structure

### Problem

你想要创建一个菜单页面集合，其中一次只有一个菜单可见。

### Solution

要创建菜单页面集合：

+   1. 创建一个名为 Menu.cs 的新脚本：

```
// Contains UnityEvent, which this script uses
using UnityEngine.Events;

public class Menu : MonoBehaviour {

    // Invoked when a menu appears onscreen.
    public UnityEvent menuDidAppear = new UnityEvent();

    // Invoked when a menu is removed from the screen.
    public UnityEvent menuWillDisappear = new UnityEvent();

}
```

+   2. 创建一个名为 MenuManager.cs 的新脚本：

```
public class MenuManager : MonoBehaviour {

    [SerializeField] List<Menu> menus = new List<Menu>();

    private void Start()
```

