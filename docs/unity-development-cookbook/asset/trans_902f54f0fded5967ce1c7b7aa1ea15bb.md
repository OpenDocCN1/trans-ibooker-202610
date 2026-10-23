

最后，我们将创建单例实例，并添加我们希望可用的音频剪辑：

- 1. 通过打开 GameObject 菜单并选择 Create Empty 来创建一个新的游戏对象。将新对象命名为“Audio Manager。”
- 2. 选择新对象。
- 3. 将 AudioManager.cs 脚本拖放到 Inspector 上。
- 4. 将你创建的 SoundEffect 资源添加到 Effects 列表中（图 11-11）。

![](img/e54c00769db669e3746fa1e50090241f_344_0.png)

图 11-11. Audio Manager 的 Inspector，其列表中添加了一个 SoundEffect 资源

你现在可以从代码中触发音效。例如，如果你创建了一个名为“Laser”的 SoundEffect，你可以这样触发它：

// Play a sound called "laser" at the same place as the listener
AudioManager.instance.PlayEffect("laser");

// Play the same sound at the origin
AudioManager.instance.PlayEffect("laser", Vector3.zero);

## Discussion

与任何单例一样，你在初始开发的便捷性和增加的软件复杂性之间做出了权衡。有关更多详细信息，请参阅 Recipe 2.6 的讨论。

## User Interface

Unity 包含的 UI 系统是构建游戏 UI 的强大而灵活的方式。它可用于构建从显式的基于菜单和基于界面的游戏部分，例如实际的菜单和配置屏幕，到游戏内 UI，以及通过有用的额外 UI 元素扩展 Unity 编辑器环境以简化游戏开发流程。

如果你过去使用过 Unity，你会很高兴地知道 UI 系统在近年已被彻底替换！这里涵盖的所有内容都涉及当前的 UI 系统，在 Unity 文档中有时可能会被称为“新 UI 系统”。

旧的 Unity UI 系统专注于基于程序员的工作流程，而不是所涉及的视觉 UI 元素。它是按程序员可能思考的方式设计的，而不是 UI 需要如何表现：视觉上！新系统修复了这一点，我们因此受益匪浅。

你不需要向整体项目添加任何内容即可开始使用 Unity UI 系统。所有操作都通过属于 UI 系统的游戏对象完成。

Unity UI 系统让你可以为玩家创建 UI，以便开始游戏、修改设置等（例如主菜单），以及游戏内 UI，例如你在虚拟世界中电脑屏幕上可能找到的那些（例如第一人称风格游戏中的电脑控制台）。你可以在 Unity 手册中了解 Unity UI 系统的所有组件。

![](img/e54c00769db669e3746fa1e50090241f_345_0.png)

![](img/e54c00769db669e3746fa1e50090241f_345_1.png)

## 12.1 Working with UI Controls

### Problem

你想在 Unity 中构建用户界面。

### Solution

在 Unity 中构建 UI 就像在 Unity 中构建任何东西一样：它是由游戏对象构建而成的。Unity 提供了各种 UI 元素作为游戏对象，如图 12-1 所示。

![](img/e54c00769db669e3746fa1e50090241f_346_0.png)

图 12-1. 显示一系列 UI 元素的游戏对象

要在 Unity 中绘制 UI，你需要一个 Canvas 组件。你可以将 Canvas 添加到游戏对象，也可以直接将其添加到场景中以创建放置 UI 的位置：

- 1. 打开 GameObject 菜单，并选择 UI → Canvas。
所有 UI 元素都作为 Canvas 的子对象添加。我们将向 UI 添加一些文本。

- 2. 选择新添加的画布，然后再次打开 GameObject 菜单并选择 UI → Text。Text 对象将被添加到画布中。

- 3. 选择 Rectangle 工具，并拖动 Text 对象以调整其大小和重新定位。

- 4. 选择 Text 对象，并修改其属性以更改其文本。

仅显示一些文本的 UI 并不是很有用，所以现在我们将编写一个脚本将一些文本记录到控制台，然后将脚本附加到按钮上：

- 1. 创建一个新的 C# 脚本名为 ButtonClicked.cs，删除默认创建的两个方法，并向类中添加以下代码：

```
public void ButtonWasClicked() {
    Debug.Log("The button was clicked!");
}
```

这个公共方法 ButtonWasClicked 将连接到我们将添加到 UI 的按钮。它向调试日志发送一条消息，说明按钮已被点击。

- 2. 我们还可以添加一个接受参数的公共方法，允许我们在记录的输出中添加消息。将以下方法添加到类中：

```
public void ButtonWasClickedWithParameter(string parameter) {
    string message =
        string.Format("The button was clicked: {0}", parameter);

    Debug.Log(message);
}
```

我们定义了两个公共方法：ButtonWasClickedWithParameter，它只是记录按钮已被点击，以及 Format，它做同样的事情但包含一个字符串参数，我们可以使用它。

- 3. 顺便说一下，我们还可以添加私有方法，这些方法无法连接到 UI 元素。将以下方法添加到类中：

```
void PrivateButtonWasClicked() {
    Debug.Log(
        "This won’t run as the direct result of a button click!"
    );
}
```

![](img/e54c00769db669e3746fa1e50090241f_347_0.png)

C# 中所有类方法默认都是私有的，因此如果你想创建一个私有方法，就不需要指定保护级别。如果你愿意的话，阻止你包含 private 关键字也没有什么。

现在我们需要将新脚本实际附加到我们一直在构建的 UI 上：

- 1. 将 ButtonClicked 组件（我们的新脚本）附加到 Canvas 对象，或附加到任何其他游戏对象——就本示例而言，哪个对象并不重要。

- 2. 再次打开 GameObject 菜单，并选择 UI → Button。

- 3. 选择你添加的按钮，并点击其 On Click 组件下的 + 按钮。
- 4. 将你在步骤 1 中附加了 ButtonClicked 组件的对象拖到出现的新的对象字段中。
- 5. 在现在出现的函数下拉菜单中，选择 ButtonClicked → ButtonWasClicked。
- 6. 你的 UI 应该类似于图 12-2。运行游戏。当你点击按钮时，文本将出现在控制台中。
- 7. 停止游戏，再次点击 + 按钮。拖入同一个对象，并选择 ButtonClicked → ButtonWasClickedWithParameter。注意此方法如何让你指定在按钮被点击时发送的字符串。

![](img/e54c00769db669e3746fa1e50090241f_348_0.png)

图 12-2. 按钮的 On Click 事件，已配置为调用 ButtonClicked 组件的 ButtonWasClicked 方法

![](img/e54c00769db669e3746fa1e50090241f_348_1.png)

由于它不是公共的，PrivateButtonWasClicked 方法永远不会出现在你可以选择的方法列表中。

## Discussion

除了明显的输出——我们添加了一个按钮使其发生某些事情——这里还有一些有用的移动部件需要注意。

![](img/e54c00769db669e3746fa1e50090241f_349_0.png)

每个画布都附加了一个 Canvas Scaler 组件，它允许你控制画布内 UI 元素的缩放和像素密度。这对于确保你的 UI 在构建可在多个屏幕分辨率上运行的游戏时按预期行为很有用。有关此组件的更多信息，请参阅 Unity 的文档。

每个 UI 元素都存在于一个画布上，每个 UI 元素都附加了一个 RectTransform 组件。如果你熟悉 Transform 组件（如果需要复习，请参阅第 1 章），那么解释 RectTransform 很简单：它是 UI 元素的 Transform 等价物，它为画布内的 UI 元素提供位置、旋转和缩放。

每个 RectTransform 还有四个小的三角形手柄，称为锚点。这些锚点定义了组件如何锚定到其父对象。你可以移动锚点（或在 Inspector 中操作它们），例如，允许按钮相对于其父容器更改其宽度，同时确保其高度始终保持不变，并且它与容器的底部保持相同的距离。

有一个类似于目标的按钮，显示在图 12-3 左上角，它允许你从预设锚点配置中进行选择。

![](img/e54c00769db669e3746fa1e50090241f_349_1.png)

图 12-3. 看起来像目标的按钮允许你从预设锚点配置中进行选择

