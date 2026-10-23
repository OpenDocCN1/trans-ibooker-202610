

让玩家对你的游戏拥有控制权，嗯，这很重要！这是互动游戏的核心原则。在本章中，我们将探讨游戏开发者最常见的一些输入需求。幸运的是，Unity 提供了多种输入方法，从键盘和鼠标输入到手柄输入，再到更复杂的鼠标指针控制系统。我们将在此介绍它们。

![](img/e54c00769db669e3746fa1e50090241f_69_0.png)

在本章中，你将同时使用传统的 Input 类，以及 Unity 的新输入系统。它们在现代 Unity 项目中各有其用武之地。

## 3.1 获取简单的键盘输入

### Problem

你希望以最少的步骤知道用户何时按下键盘上的按键。

### Solution

使用传统 Input 类的 GetKeyDown、GetKeyUp 和 GetKey 方法来查找正在按下的按键：

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

if (Input.GetKeyUp(KeyCode.A))
{
    Debug.Log("The A key was released!");
}

if (Input.anyKeyDown) {
    Debug.Log("A key was pressed!");
}
```

### Discussion

这些方法在不同时间做出响应：

- GetKeyDown 在本帧按键开始被按下时返回 true。
- GetKeyUp 在本帧按键被释放时返回 true。
- GetKey 如果按键当前被按住则返回 true。

你也可以使用 anyKeyDown 属性来判断是否任意按键被按下，这在你想提示玩家“按任意键继续”时很有用。

![](img/e54c00769db669e3746fa1e50090241f_70_0.png)

Input 类是传统 Input Manager 系统的一部分，该系统可追溯至 Unity 最早的版本。输入操作简单易用，并且在新项目中默认可用，但它存在诸多限制：它不允许自定义按钮到游戏内操作的映射，设置游戏内操作也不算容易，而且你最终需要在其之上构建许多系统。

更好的解决方案是 Unity Input System，它解决了所有这些问题，并且是我们将在本章剩余部分使用的系统。

## 3.2 使用 Unity Input System

### Problem

你想使用 Unity Input System 来获取用户输入。

### Solution

从 Package Manager 安装 Unity Input System（Recipe 1.9），按以下步骤操作：

- 1. 通过选择 Window → Package Manager 打开 Package Manager 窗口。
- 2. 将窗口左上角的 Packages 设置从 In Project 改为 Unity Registry。
- 3. 在软件包列表中向下滚动到 Input System，或在右上角的搜索字段中搜索 Input System。
- 4. 从列表中选择 Input System，然后点击 Install 按钮。

Unity 将 Input System 软件包下载并安装到你的项目中。Unity 可能会询问你是否要更新项目设置以使用新的 Input System；选择 Yes，它将重新启动编辑器。

安装 Input System 后，你就可以在代码中使用它了。

例如，我们可以用以下代码重新实现 Recipe 3.1 中的功能，但使用 Input System：

```
using UnityEngine.InputSystem;

if (Keyboard.current.aKey.wasPressedThisFrame) {
    Debug.Log($"The A key was pressed!");
}
if (Keyboard.current.aKey.isPressed) {
    Debug.Log($"The A key is being held down!");
}
if (Keyboard.current.aKey.wasReleasedThisFrame) {
    Debug.Log($"The A key was released!");
}

if (Keyboard.current.anyKey.wasPressedThisFrame) {
    Debug.Log("Some key was pressed!");
}
```

### Discussion

虽然完全可以直接访问特定的按钮，比如键盘上的按键，但使用用户输入的更好方式是：Input Actions，我们将在下一个配方中介绍。

![](img/e54c00769db669e3746fa1e50090241f_71_0.png)

你可能想要直接访问按钮的地方是为了开发目的。例如，在开发早期阶段，或测试游戏版本时，让特殊开发者键执行游戏中的特殊任务，比如使玩家免疫伤害，会很方便。你通常不会向玩家公开这些功能，因此没有必要将其作为 Input Action 设置，而是直接访问按钮即可。

## 3.3 使用 Input Actions

### Problem

你想设置 Input Actions，将物理输入映射到游戏内操作。

### Solution

在 Unity Input System 中，Input Action 是玩家可在游戏中执行的操作（例如移动、跳跃、转向），它由用户可以交互的一个或多个物理控件表示（例如键盘上的 W 键，或手柄上的左摇杆）。

Input Actions 让你能够以游戏中的高层级事物来思考，而不是必须在低层级思考按下特定按钮。更重要的是，因为你可以将多个控件绑定到单个操作上，所以更易于构建针对多种控制方案的游戏。

![](img/e54c00769db669e3746fa1e50090241f_72_0.png)

在按照本配方中的步骤操作之前，请确保已通过 Recipe 3.2 的步骤在项目中安装了 Unity Input System。

为了演示 Input Actions，我们将从创建一个可由用户输入控制的简单场景开始：

- 1. 在一个新空场景中，通过打开 GameObject 菜单并选择 3D Object → Cube 创建一个新立方体。
- 2. 选择新创建的立方体，在 Inspector 中点击 Add Component。输入 CubePlayer，点击“New script”，然后点击 Create and Add。
- 3. 打开新创建的文件 CubePlayer.cs，并用以下内容替换其全部内容：

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
```

```
transform.Translate(movement);
}

public void ChangeColour() {
    var color = Color.HSVToRGB(Random.value, 0.8f, 1f);
    GetComponent<Renderer>().material.color = color;
}
}
```

- 4. 保存文件并返回 Unity。

![](img/e54c00769db669e3746fa1e50090241f_73_0.png)

如果你进入 Play 模式，立方体不会做任何事情。我们下一步会解决这个问题！

现在我们有一个准备好响应用户输入的立方体，我们将创建一个接收用户输入并将其发送给立方体的对象：

- 1. 通过打开 GameObject 菜单并选择 Create Empty 创建一个新空游戏对象。将新对象命名为“Player Input”。
- 2. 点击 Add Component 按钮，并选择 Input → Player Input。
- 3. 点击 Actions 字段右侧的选取器（圆形图标），并选择 DefaultInputActions。

DefaultInputActions 是一个预定义的 Input Actions 集合。其中一个预定义操作是“Move”，它绑定到常见的移动相关控件，如键盘上的 WASD 键，以及手柄上的左摇杆。我们将使立方体响应该操作而移动：

- 1. 向 CubePlayer.cs 添加以下方法：

```
public void OnMove(InputAction.CallbackContext context) {
    // Whenever anything happens with this action, get its current value
    // and store it.
    moveInput = context.ReadValue<Vector2>();
}
```

- 2. 保存文件并返回 Unity。
- 3. 接下来，我们将使此方法在 Move 操作状态发生变化时由输入系统调用。首先，选择 Player Input 对象。
- 4. 在 Player Input 组件的 Inspector 中，将 Behaviour 设置为 Invoke Unity Events。
- 5. 底部会出现一个新区域，标记为 Events。打开 Events → Player。
- 6. 点击 Move 事件附带的 + 按钮以添加新条目。
- 7. 将 Cube 对象拖放到对象槽中。
- 8. 将函数下拉菜单从 No Function 改为 CubePlayer → OnMove。
- 9. 按下 Play，并使用 W、A、S 和 D 键移动立方体。

通过此设置，Player Input 组件将调用立方体 CubePlayer 脚本上的 OnMove 方法。此方法接收一个 InputAction.CallbackContext 方法，让代码获取有关接收到的特定输入的信息。Move 操作被定义为包含二维信息的操作（即左右和前后）。在 OnMove 中，我们调用 ReadValue<Vector2> 来获取该数据并存储在 moveInput 变量中。然后在 Update 中使用该变量实际移动立方体。

![](img/e54c00769db669e3746fa1e50090241f_74_0.png)

我们之所以在 Update 中执行实际移动而不是在 OnMove 中，是因为 OnMove 只会在输入发生变化时被调用。如果你按下键盘上的 W 键，OnMove 将被调用一次，当你释放按键时，它会被再次调用。因为我们希望按下 W 意味着开始移动而不是“移动一次”，所以我们需要存储跨越多帧的信息。

如果你有手柄，将其连接到电脑并移动左摇杆。立方体会移动。（你可以使用几乎任何近期游戏商店中可用的控制器。来自热门视频游戏主机如 Xbox、PlayStation 或 Switch 的控制器通常效果更好，因为它们更常见于玩家。）

### Discussion

默认操作已为 move、look 和 shoot（三个视频游戏动词！）设置了一些通用操作。如果这就是你的游戏所需，太好了！否则，你可以创建自己的。

Input Actions 存储在项目中的资源中，因此如果你想创建新的自定义 Input Actions，你需要创建一个资源来存储它们：

- 1. 选择 Player Input 游戏对象，并转到其上附加的 Player Input 组件的 Inspector。
- 2. 通过点击它并按 Backspace 键清除 Actions 槽。

