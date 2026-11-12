## 第 3 章 输入

让玩家对你的游戏施加控制，嗯，非常重要！这是交互式游戏的核心原则。在本章中，我们将探讨游戏开发者的一些最常见的输入需求。幸运的是，Unity 提供了多种可用的输入方法，从键盘和鼠标输入到游戏手柄输入，再到允许鼠标指针控制的更复杂系统。我们将逐一介绍。

> 注意
> 本章中你将同时使用*旧版* Input 类和 Unity 的新输入系统。它们在现代 Unity 项目中各有其用途。

## 3.1 获取简单的键盘输入

### 问题

你想以尽可能少的步骤了解用户何时在键盘上按键。

### 解决方案

使用旧版 Input 类的 GetKeyDown、GetKeyUp 和 GetKey 方法来了解正在按下哪些键：

```
if (Input.GetKeyDown(KeyCode.A))
{
    Debug.Log("The A key was pressed!");
}
```

```
if (Input.GetKey(KeyCode.A))
{
    Debug.Log("The A key is being held down!");
}
```

```
}
if (Input.GetKeyUp(KeyCode.A))
{
    Debug.Log("The A key was released!");
}
if (Input.anyKeyDown) {
    Debug.Log("A key was pressed!");
}
```

### 讨论

这些方法在不同时间响应：

- GetKeyDown 在本帧按键开始按下时返回 true。
- GetKeyUp 在本帧按键释放时返回 true。
- GetKey 如果按键当前正被按住，则返回 true。

你还可以使用 anyKeyDown 属性来了解是否有任何键被按下，这在你想让玩家“按任意键继续”时很有用。

> **注意**
> Input 类是旧版输入管理器系统的一部分，可追溯至 Unity 的早期版本。Input 使用简单，且在新项目中默认可用，但存在多种局限性：它不允许自定义按钮到游戏内动作的映射，设置游戏内动作并不十分容易，并且你最终需要在其之上构建多个系统。
> 更好的解决方案是 Unity 输入系统，它解决了所有这些问题，也是我们在本章剩余部分将使用的系统。

## 3.2 使用 Unity 输入系统

### 问题

你想使用 Unity 输入系统来获取用户输入。

### 解决方案

按以下步骤从包管理器（配方 1.9）安装 Unity 输入系统：

- 1. 通过选择 Window → Package Manager 打开包管理器窗口。
- 2. 将窗口左上角的 Packages 设置从 In Project 更改为 Unity Registry。
- 3. 在包列表中向下滚动到 Input System，或在右上角的搜索字段中搜索 Input System。
- 4. 从列表中选择 Input System，然后单击 Install 按钮。

Unity 会将输入系统包下载并安装到你的项目中。Unity 可能会询问你是否要更新项目设置以使用新的输入系统；选择 Yes，它将重启编辑器。

安装输入系统后，你可以在代码中使用它。

例如，我们可以使用以下代码来重现配方 3.1 中的操作，但使用输入系统：

```
using UnityEngine.InputSystem;

if (Keyboard.current.aKey.wasPressedThisFrame) {
    Debug.Log("The A key was pressed!");
}
if (Keyboard.current.aKey.isPressed) {
    Debug.Log("The A key is being held down!");
}
if (Keyboard.current.aKey.wasReleasedThisFrame) {
    Debug.Log("The A key was released!");
}

if (Keyboard.current.anyKey.wasPressedThisFrame) {
    Debug.Log("Some key was pressed!");
}
```

### 讨论

虽然直接访问特定按钮（例如键盘上的键）当然是可能的，但处理用户输入的更好方法是使用输入操作（*Input Action*），我们将在下一个配方中介绍。

> **注意**
> 你可能希望直接访问按钮的一个地方是用于开发目的。例如，在开发早期阶段或测试游戏版本时，设置特殊的*开发者键*来在游戏中执行特殊任务（例如让玩家免受伤害）可能很方便。你通常不会将这些暴露给玩家，因此没有必要将它们设置为输入操作，而是直接访问按钮即可。

## 3.3 使用输入操作

### 问题

你想设置输入操作，将物理输入映射到游戏内动作。

### 解决方案

在 Unity 输入系统中，*输入操作（*Input Action*）*是玩家在游戏中可以执行的游戏内动作（例如移动、跳跃、转向），由用户可以交互的一个或多个物理控件表示（例如键盘上的 W 键或游戏手柄上的左摇杆）。

输入操作允许你从游戏中发生的高级事件来思考，而不是在按下特定按钮的低层级思考。更重要的是，因为你可以将多个控件绑定到单个操作，这使得构建针对多种控制方案的游戏变得容易得多。

> 注意
> 按照本配方中的步骤操作前，请确保已按照配方 3.2 中的步骤在项目中安装了 Unity 输入系统。

为了演示输入操作，我们将从创建一个可以通过用户输入控制的简单场景开始：

- 1. 在新建空场景中，通过打开 GameObject 菜单并选择 3D Object → Cube 来创建一个新立方体。
- 2. 选择新建的立方体，在 Inspector 中单击 Add Component。输入 **CubePlayer**，单击“New script”，然后单击 Create and Add。
- 3. 打开新建的文件 _CubePlayer.cs_，并用以下内容替换其所有内容：

```
using UnityEngine.InputSystem;

public class CubePlayer : MonoBehaviour
{
    [SerializeField] float moveSpeed = 5;
    private Vector2 moveInput = Vector2.zero;

    public void Update() {
        var movement = new Vector3(moveInput.x, moveInput.y, 0)
            * moveSpeed
            * Time.deltaTime;

        transform.Translate(movement);
    }
}
```

```
public void ChangeColour() {
    var color = Color.HSVToRGB(Random.value, 0.8f, 1f);
    GetComponent<Renderer>().material.color = color;
}
```

4. 保存文件，然后返回 Unity。

> 注意
> 如果进入播放模式，立方体不会做任何事情。我们稍后会解决！

既然我们有了一个准备响应用户输入的立方体，我们将创建一个接收用户输入并将其发送到立方体的对象：

- 1. 通过打开 GameObject 菜单并选择 Create Empty 来创建一个新空游戏对象。将新对象命名为 "Player Input"。
- 2. 单击 Add Component 按钮，选择 Input → Player Input。
- 3. 单击 Actions 字段右侧的选择器（圆形图标），然后选择 DefaultInputActions。

DefaultInputActions 是预定义的输入操作集合。其中一个预定义操作是 "Move"，它绑定到常见的运动相关控件，例如键盘上的 WASD 键和游戏手柄上的左摇杆。我们将使立方体对此做出移动响应：

- 1. 将以下方法添加到 CubePlayer.cs：

```
public void OnMove(InputAction.CallbackContext context) {
    // Whenever anything happens with this action, get its current value
    // and store it.
```





```
moveInput = context.ReadValue<Vector2>();
}
```

- 2. 保存文件并返回 Unity。
- 3. 接下来，我们要让输入系统在 Move 动作状态改变时调用此方法。首先，选择 Player Input 对象。
- 4. 在 Player Input 组件的 Inspector 中，将 Behaviour 设置为 Invoke Unity Events。
- 5. 底部将出现一个名为 Events 的新部分。打开 Events → Player。
- 6. 单击 Move 事件旁边的 + 按钮以添加新条目。
- 7. 将 Cube 对象拖放到对象槽中。
- 8. 将函数下拉菜单从 No Function 更改为 CubePlayer → OnMove。
- 9. 按 Play，并使用 W、A、S、D 键移动立方体。

有了这个设置，Player Input 组件将调用立方体 CubePlayer 脚本上的 OnMove 方法。该方法接收一个 InputAction.CallbackContext 参数，让代码能够获取接收到的具体输入信息。Move 动作被定义为包含二维信息的动作（即左右方向和前后方向）。在 OnMove 中，我们调用 ReadValue<Vector2> 来获取该数据并将其存储在 moveInput 变量中。然后在 Update 中使用该变量来实际移动立方体。

**注意**

之所以在 Update 而不是 OnMove 中执行实际移动，是因为 OnMove 仅在输入改变时被调用。如果你按下键盘上的 W 键，OnMove 会被调用一次，当你释放按键时，它会被再次调用。因为我们希望按下 W 意味着“开始移动”而不是“移动一次”，所以我们需要存储在多个帧之间持续存在的信息。

如果你有游戏手柄，请将其连接到电脑并移动左摇杆。立方体将会移动。（你可以使用游戏商店中基本上任何最近的控制器。Xbox、PlayStation 或 Switch 等流行视频游戏主机的控制器通常效果更好，因为玩家更普遍拥有它们。）

### 讨论

默认动作中已预设了一些通常有用的动作，分别用于 _移动_、_观察_ 和 _射击_（视频游戏中的三个动作！）。如果这足以满足你的游戏需求，那就太好了！否则，你可以创建自己的动作。

输入动作存储在项目中的资产文件里，因此如果你想要创建新的自定义输入动作，需要创建一个资产来存储它们：

- 1. 选择 Player Input 游戏对象，并进入附加到其上的 Player Input 组件的 Inspector。
- 2. 通过点击并按下 Backspace 键来清空 Actions 槽。
- 3. 点击 Create Actions 按钮。
- 4. Unity 会询问你想在哪里保存新资产。将其保存在 Assets 文件夹中的某个位置。

Unity 将打开新创建的资产，你现在正在查看动作列表。

> ### 注意

你也可以使用 Assets → Create → Input Actions 菜单创建新的输入动作资产，但这会创建一个空白集合。通过 Player Input 组件进行操作可以省去重新设置一切的麻烦，如果你只想向默认集合添加一个新动作的话。

> ### 注意

输入映射代表玩家可以处于的不同模式。例如，默认集合有两个：Player 用于一般游戏玩法，UI 用于与用户界面交互。拥有多个输入映射允许游戏将不同控制解释为不同动作；例如，当输入映射设置为 UI 时，左摇杆表示“导航 UI”，而不是“移动”。

让我们来看看 Player 输入映射。其中包含三个动作：Move、Look 和 Shoot。每个动作都有若干绑定，即与玩家可交互的物理按钮的连接。例如，Move 绑定到游戏手柄的左摇杆；键盘上的 W、A、S、D 键；VR 控制器的主 2D 轴；以及操纵杆的摇杆移动。

让我们创建一个新动作——它将是一个按钮（即，我们可以从中获取是否被按下的信息），我们将使其改变立方体的颜色：

- 1. 在 Actions 中，点击 + 按钮来创建新动作。将新动作命名为“Change Colour”。

请注意，Action Type 已设置为 Button。保持此设置不变。还有其他类型的动作；例如，Move 动作是一个 Value，并被配置为输出一个 2D 向量。（这就是为什么 CubePlayer.cs 中的 OnMove 使用 ReadValue<Vector2>。）如果 Change Colour 是一个按钮，它将在按钮按下和释放时触发事件。

- 2. 新创建的动作最初带有一个未配置的绑定。让我们来设置它。首先，选择 Change Colour 内部的空绑定。

- 3. 打开 Path 下拉菜单，并选择 Keyboard → By Location of Key (US Layout) → C。

Path 下拉菜单允许你配置应绑定到该动作的具体物理控制。你可以添加任意数量的绑定；例如，如果你打开 Move 动作，将看到大量绑定，每个绑定对应不同类型的输入设备。

By Location of Key 路径允许你在键盘上指定一个*位置*。在此示例中，我们将把它设置为美国布局键盘上 C 键所在位置的任意按键。

## 键盘与按键的含义

虽然键盘最初设计的目的是为了能够输入文本，但游戏并不会以这种方式使用键盘作为控制设备。为 PC 游戏设计控制方案的游戏设计师通常会先确定玩家手指应放在键盘的哪个位置，然后反过来进行设计。

毕竟，字母 W 本身并没有什么与之特殊之处意味着“前进”，但 W、A、S、D 键的巧妙布局使得使用美国 QWERTY 布局键盘的右撇子玩家能舒适地将左手放在键盘上，并轻松触及一组正好适合控制前进、后退和左右移动的键。然而，如果将这些动作绑定到特定字母，任何不使用该特定键盘布局的人都将处于严重劣势。与此同时，按“键盘上第二行的第三个键（不包括功能键）”这种方式来操作也相当麻烦。

By Location of Key 通过指定美国布局中的一个键，但将其视为一个位置来解决这个问题。当你将 Change Colour 动作绑定到 C 键的位置时，该动作将绑定到其他键盘上该位置的任意按键。

值得注意的是，By Location of Key 通常最适合于你希望玩家非常频繁使用并形成肌肉记忆的按钮。如果某个动作使用频率较低，那么将动作绑定到特定命名的按键，而不是其位置，可能更合理，因为这可以让玩家更容易通过名称找到不常用动作的相应按键。

- 4. 将 Use In Control Scheme 设置为 Keyboard & Mouse。这将在当前控制方案设置为使用这些设备时使绑定生效。

- 5. 接下来，我们还将该动作绑定到游戏手柄控制器上的一个按钮。在动作列表中，点击 Change Colour 的 + 按钮，并选择 Add Binding。将出现一个新的空绑定。

- 6. 选择新绑定，并将其 Path 设置为 Gamepad → Button South。

**注意**

几乎每个现代游戏控制器右侧都有四个排列成菱形的按钮。这些按钮通常被称为*面板按钮*。*南*按钮代表这个菱形的底部。不同控制器对这些按钮有不同的名称：南按钮在 PlayStation 控制器上是 X 按钮，在 Xbox 控制器上是 A 按钮，在 Nintendo Switch 控制器上是 B 按钮。这很类似于 By Location of Key 用于指定控制的位置而不是控制的名称，指定“南按钮”让你可以引用控制器上按钮的位置。

- 7. 将绑定的 Use In Control Scheme 设置为 Gamepad。

**提示**

如果你没有可用的游戏手柄，尝试将其绑定到其他内容上（例如，左鼠标按钮）。

- 8. 点击 Save Asset，然后关闭窗口。

现在我们已经创建了新动作并将其绑定到一些控制上，我们将让立方体能够响应该动作：

- 1. 将以下方法添加到 *CubePlayer.cs*：

```
public void OnChangeColour(InputAction.CallbackContext context) {
    // When this action is performed, change color.
    if (context.performed) {
        ChangeColour();
    }
}
```




```
}
}
```

2. 保存文件，然后返回到 Unity。

3. 选择 Player Input 对象，并转到其 Player Input 组件的检查器。

> 注意
>
> 当你先前创建新的动作资产时，输入动作字段已被设置为该新资产。你可以通过将替换资产拖放到输入动作字段中，来更改组件所使用的动作资产。

4. 打开事件部分，并选择 Player → 更改颜色。

5. 单击 + 按钮以添加新接收器。

6. 将 Cube 对象拖放到对象字段中，并将函数更改为 CubePlayer → OnChangeColour。

7. 就在此处，执行你先前将移动动作连接到 CubePlayer 的 OnMove 方法时所执行的相同步骤；当你更改组件所使用的动作资产时，连接将会丢失。

进入播放模式，并按下键盘上的 C 键或手柄的南侧按钮（或任何你绑定到更改颜色动作的其他控件）。立方体将更改颜色。

## 3.4 锁定和隐藏鼠标光标

### 问题

你想在游戏中使用鼠标移动，但不希望屏幕上显示光标。

### 解决方案

将 Cursor.lockState 属性设置为 CursorLockMode.Locked 或 CursorLockMode.Confined，以防止鼠标光标离开游戏，并将 Cursor.visible 设置为 false 以使光标不可见：

```
// Lock the cursor to the middle of the screen or window.
Cursor.lockState = CursorLockMode.Locked;

// Prevent the cursor from leaving the window.
Cursor.lockState = CursorLockMode.Confined;

// Don't restrict the mouse cursor at all.
Cursor.lockState = CursorLockMode.None;

// Hide the mouse cursor.
Cursor.visible = false;
```

### 讨论

在编辑器中，你可以按下 Esc 键以重新启用鼠标光标并将其从光标禁闭中释放。

## 3.5 响应鼠标悬停和点击事件

### 问题

你想检测用户何时将鼠标移到对象上，以及何时点击它。

### 解决方案

为此，你可以使用事件系统，它能够跟踪屏幕上光标的位置，并检测何时光标位于场景中的对象之上。要开始使用事件系统，请通过 GameObject 菜单创建一个附有 EventSystem 组件的对象：

1. 打开 GameObject 菜单，并选择 UI → EventSystem。将创建一个新的 EventSystem 对象。

> 提示
>
> 如果你创建一个画布，你也会同时创建一个事件系统，因为 UI 系统依赖于我们在此配方中所使用的相同技术。

2. 接下来，创建一个事件系统将用于确定光标下方有哪些对象的射线投射器组件。射线投射器会从相机位置通过光标位置向场景中追踪射线，以检测射线击中了什么。如果射线击中了碰撞体，则事件系统会将事件分派到目标对象。

3. 选择主相机对象。

4. 打开组件菜单，并选择 事件 → 物理射线投射器。（你也可以通过单击检查器底部的添加组件按钮，并选择 事件 → 物理射线投射器来添加它。）

> 注意
>
> 在此配方中，我们通过利用 3D 物理系统来使用 3D 对象。对于 2D 游戏，相同的技术也适用，但你会向相机添加 2D 物理射线投射器，而不是物理射线投射器。物理射线投射器仅检测 3D 碰撞体，而 2D 物理射线投射器仅检测 2D 碰撞体。如果你在场景中同时使用两种碰撞体类型，两者都可以同时位于你的相机上。

5. 接下来，我们将编写一个脚本，用于检测光标何时移动到脚本所附着的对象上，并更改其渲染器的颜色。创建一个名为 ObjectMouseInteraction.cs 的新 C# 脚本。

6. 将以下 using 指令添加到类的顶部：

```
using UnityEngine.EventSystems;
```

> 注意
>
> 创建事件系统后，如果你已将项目配置为使用 Unity 输入系统（参见配方 3.2），它会警告你需要将组件更新为与输入系统配合使用的组件。要执行此操作，请单击 使用输入系统 UI 模块替换。

要访问处理指针所需的类和接口，我们需要访问 UnityEngine.EventSystems 命名空间。完成此操作后，我们使类符合某些接口，这些接口将 MonoBehaviour 标记为需要知道输入事件的内容。

7. 从代码中删除 ObjectMouseInteraction 类，并用以下代码替换它：

```
public class ObjectMouseInteraction :
    MonoBehaviour,
    IPointerEnterHandler, // Handles mouse cursor entering object
    IPointerExitHandler, // Handles mouse cursor exiting object
    IPointerUpHandler, // Handles mouse button lifting up on this
    // object
    IPointerDownHandler, // Handles mouse button being pressed on this
    // object
    IPointerClickHandler // Handles when mouse is pressed and
    // released on this object
{
}
```

通过实现这些接口，Unity 将在附着对象上发生输入事件时知道要联系此脚本。

> 注意
>
> 在此配方中，我们使用的接口比你在实际情况中可能使用的要多（例如，我们同时处理指针按下和抬起 *和* 点击），但这作为一个很好的演示。这些不是你可以注册以接收方法调用的唯一事件；查看 Unity 文档以了解更多信息。

8. 接下来，我们将添加代码，以便更简单地修改对象的颜色。将以下变量和方法添加到 ObjectMouseInteraction 类中：

```
Material material;

void Start() {
    material = GetComponent<Renderer>().material;
}
```

9. 最后，我们将添加在光标移动到对象上或点击对象时将被调用的方法。将以下方法添加到 ObjectMouseInteraction 类中：

```
public void OnPointerClick(PointerEventData eventData)
{
    Debug.LogFormat("{0} clicked!", gameObject.name);
}

public void OnPointerDown(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer down!", gameObject.name);
    material.color = Color.green;
}
```

```
public void OnPointerEnter(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer enter!", gameObject.name);

    material.color = Color.yellow;
}
```

```
public void OnPointerExit(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer exit!", gameObject.name);

    material.color = Color.white;
}
```

```
public void OnPointerUp(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer up!", gameObject.name);

    material.color = Color.yellow;
}
```

代码现在已完成，可以投入使用。我们将创建一个附带此组件的立方体；当鼠标与其交互时，它将更改颜色：

1. 打开 GameObject 菜单，并选择 3D 对象 → 立方体。

2. 将新立方体放置在相机前方的某个位置。

3. 将 ObjectMouseInteraction.cs 脚本拖放到立方体上。

运行游戏。当鼠标移动到其上或点击它时，立方体将更改颜色。

### 讨论

请记住，相机的物理射线投射器组件只能“看到”碰撞体。没有碰撞体的对象不会接收这些事件。你也可以通过查看事件系统对象的检查器来调试事件系统。在运行期间，它会显示关于光标位置以及当前哪个对象接收射线检测的信息（图 3-1）。

检查器
事件系统
静态
标签 未标记 图层 默认
变换
位置 X 0 Y 0 Z 0
旋转 X 0 Y 0 Z 0
缩放 X 1 Y 1 Z 1
事件系统 (脚本)
脚本 事件系统
首先选择的 无 (游戏对象)
发送导航事件 [x]
拖拽阈值 10
独立输入模块 (脚本)
脚本 独立输入模块
水平轴 水平
垂直轴 垂直
提交按钮 提交
取消按钮 取消
每秒输入操作次数 10
重复延迟 0.5
强制模块激活 [ ]
基础输入 (脚本)
脚本 基础输入
添加组件

```
EventSystem
Selected:
Pointer Input Module of type: UnityEngine.EventSystems.StandaloneInputModule
Pointer: -1
Position: (292.2, 194.7)
delta: (0.0, 0.0)
eligibleForClick: False
pointerEnter: Cube (UnityEngine.GameObject)
pointerPress:
lastPointerPress:
pointerDrag:
Use Drag Threshold: True
Current Raycast:
Name: Cube (UnityEngine.GameObject)
module: Name: Main Camera (UnityEngine.GameObject)
eventCamera: Main Camera (UnityEngine.GameObject)
sortOrderPriority: -2147483648
renderOrderPriority: -2147483648
distance: 9.222726
index: 0
depth: 0
```

图 3-1. 事件系统对象检查器中的调试信息



