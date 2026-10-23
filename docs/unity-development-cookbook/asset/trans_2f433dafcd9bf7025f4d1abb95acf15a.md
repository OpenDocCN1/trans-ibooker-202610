

## 理解 Canvas

由于使用 Unity UI 系统构建的所有内容都位于 canvas 内，了解其工作原理也很有必要。首先，最重要的是，canvas 有一个渲染模式设置，它决定了 canvas 如何被渲染。可能的模式有：屏幕空间覆盖、屏幕空间摄像机和世界空间。

在屏幕空间方面，覆盖意味着 canvas 会渲染在场景之上。如果屏幕尺寸/分辨率发生变化，canvas 会相应调整大小。屏幕空间摄像机的行为与覆盖类似，但在这种情况下，canvas 会放置在由你指定的摄像机前方，并且所有内容都基于摄像机的设置进行渲染。关于摄像机的回顾，请参考第 8 章。

将 canvas 渲染为世界空间会使 canvas 像场景中的普通对象一样表现：你会使用此模式来显示旨在成为游戏中世界一部分的 UI，例如飞船控制面板，或类似内容。在这种情况下，canvas 的大小取自其 RectTransform 的大小。

## 12.2 主题化控件

### 问题

你使用自己的美术资源作为控件，而不是 Unity 内置的美术资源。你希望这些美术资源能够随着控件大小变化而缩放，而不会拉伸。

### 解决方案

在构建游戏美术资源时，尤其是 UI，你需要确保界面元素在游戏最终运行的任何分辨率下都看起来最佳。为此，你需要以特定方式设置用于绘制 UI 的精灵，并告诉 Unity 引擎哪些部分可以在不影响外观的情况下进行缩放：

- 1. 首先导入你想要使用的纹理。

![](img/e54c00769db669e3746fa1e50090241f_350_0.png)

在处理 UI 时，必须确保纹理配置为导入为“Sprite / UI”。关于如何设置资源导入的提醒，请参阅第 1 章。

在本示例中，我们将使用 Kenney 的开源 UI 资源包中的美术资源来自定义一个面板。我们将使用 PNG 文件夹，并将纹理在导入器中设置为精灵。

![](img/e54c00769db669e3746fa1e50090241f_351_0.png)

互联网上有大量免费游戏美术资源。Kenney 制作了一些我们最喜欢的材料，我们在本书中经常引用它！查看它的所有资源包。

- 2. 打开 GameObject 菜单，并选择 UI → Panel 在场景中添加一个面板。
- 3. 选择 Panel 游戏对象，并将其 Source Image 更改为来自 Kenney UI 包的 blue_panel 图像。
- 4. 调整面板大小，你会看到图像被拉伸（图 12-4），而且看起来不太好。我们可以修复它！

![](img/e54c00769db669e3746fa1e50090241f_351_1.png)

图 12-4. 一个拉伸的面板，角部失真

要修复它，我们需要设置面板的精灵，使只有特定区域被拉伸。在此纹理中，除了角部外，其他所有部分都可以缩放而不影响外观，因此我们将据此进行设置：

- 1. 选择 blue_panel 纹理。
- 2. 点击 Sprite Editor 按钮。
- 3. 在 Sprite Editor 中，将出现在屏幕顶部的绿色点向下拖动，直到绿色线位于图像的圆角下方。你也可以手动输入更改，将边框的 T 值设置为 5。
- 4. 对其他三个边缘重复此过程。完成后，Sprite Editor 窗口应如下图 12-5 所示。
- 5. 点击 Apply，并关闭 Sprite Editor。
- 6. 选择 Panel 对象。
- 7. 将 Image 组件的 Image Type 从 Simple 更改为 Sliced。
- 8. 拉伸外观将消失，因为只有精灵的中间区域被允许拉伸，角部的形状将被保留（图 12-6）。

图 12-5. Sprite Editor 窗口，精灵的边框已配置

图 12-6. 来自图 12-4 的同一个拉伸面板，但精灵已配置为仅在中间拉伸

### 讨论

我们在这里使用的技术有时被称为 9 切片，因为切片过程定义了图像的九个区域。它并不比使用单个图像资源与九个子部分更复杂，每个子部分的行为不同：角部不缩放，而图像的其余部分根据需要拉伸。

你可能会注意到，Image 组件的 Image Type 还有其他选项：Simple、Tiled 和 Filled。

Simple 会将整个精灵等比缩放。Tiled 的行为与我们在这里使用的 Sliced 设置类似，但会重复中心部分而不是拉伸它。Filled 的行为与 Simple 相同，但会从定义的原点以定义的朝向、方法和数量填充精灵。

![](img/e54c00769db669e3746fa1e50090241f_353_0.png)

你可以在 Unity 手册中了解更多关于使用图像来设置 UI 组件样式的信息。

## 12.3 动画化 UI

### 问题

你想要为用户界面的部分内容做动画。

### 解决方案

UI 动画在视频游戏中很常见。界面通常需要吸引注意力，或用于让玩家感觉自己完成了特别的事情，或理解他们在游戏中做出的某个行为的影响。Unity 使动画化 UI 元素变得与动画化 Unity 中构建的任何其他内容一样简单：非常简单！

![](img/e54c00769db669e3746fa1e50090241f_353_1.png)

这与我们在第 8 章中采用的动画方法不同。你也可以将我们在那里使用的系统连接到 UI，如果你愿意的话。但我们试图向你展示解决问题的多样化方法。

为了演示，我们将让一个按钮在被点击时旋转。首先，我们需要创建一个新按钮：

- 1. 打开 GameObject 菜单，并选择 UI → Button 在场景中添加一个按钮。

![](img/e54c00769db669e3746fa1e50090241f_354_0.png)

你可能会注意到，创建按钮还会创建一个作为按钮父级的 canvas。由于所有 UI 元素都必须作为 canvas 的子级，如果在添加新 UI 元素时场景中没有 canvas，就会为你创建一个。如果场景中已有 canvas，新 UI 组件将被添加为其子级。有关 canvas 的更多信息，请参阅配方 12.1 和 Unity 手册。

- 2. 接下来，我们需要创建一个脚本来实际为我们执行动画。创建一个名为 SpinButton.cs 的新 C# 脚本，删除类中的内容，并在其中添加以下代码：

```
private IEnumerator StartSpinning()
{
    // Don't do any spinning if spin time is zero or less (
    if (spinTime <= 0) {
        yield break;
    }

    // Keep track of how long we've been spinning.
    float elapsed = 0f;

    while (elapsed < spinTime) {
        elapsed += Time.deltaTime;

        // Calculate how far along the animation we are, measured
        // between 0 and 1.
        var t = elapsed / spinTime;

        // Use this value to figure out how many degrees we should be
        // rotated at on this frame.
        var angle = curve.Evaluate(t) * 360f;

        // Calculate the rotation by rotating this many angles around
        // the x-axis.
        transform.localRotation =
            Quaternion.AngleAxis(angle, Vector3.right);

        // Wait a new frame.
        yield return null;
    }

    // The animation is now complete. Reset the rotation to normal.
```

```
transform.localRotation = Quaternion.identity;
}
```

此代码定义了一个方法 StartSpinning，我们将作为协程调用，它会更新附加了它的对象的旋转。

- 3. 我们还需要为旋转的参数定义一些变量。将以下内容添加到我们刚刚创建的方法中：

```
// The amount of time needed to perform a full spin
[SerializeField] float spinTime = 0.5f;

// Controls the pacing of the animation.
[SerializeField] AnimationCurve curve =
    AnimationCurve.EaseInOut(0, 0, 1, 1);
```

![](img/e54c00769db669e3746fa1e50090241f_355_0.png)

当你使用 SerializeField 属性时，你是在告诉 Unity 这些变量的内容应保存到磁盘，并在 Inspector 中显示为可编辑的值。

- 4. 我们需要添加一个公共方法来启动协程，调用我们刚刚创建的 StartSpinning 方法：

```
public void Spin() {
    // Start a spin.
    StartCoroutine(StartSpinning());
}
```

![](img/e54c00769db669e3746fa1e50090241f_355_1.png)

当协程启动时，它会在每次遇到 yield return 语句时暂停执行并将控制权返回给 Unity。如果你需要关于协程的复习，请查看第 2 章。

- 5. 将 SpinButton 组件附加到我们之前创建的按钮上。
- 6. 点击 On Click 字段中的 + 按钮。
- 7. 将按钮本身拖入该字段。
- 8. 在弹出菜单中，选择 SpinButton → Spin。
- 9. 运行游戏。当你点击按钮时，它会旋转！一件杰作。

你也可以通过我们暴露给编辑器的变量来调整曲线的动画节奏，以及动画的持续时间，如图 12-7 所示。

