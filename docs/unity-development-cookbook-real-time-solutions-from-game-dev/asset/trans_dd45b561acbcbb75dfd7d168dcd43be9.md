

```
// We now have all the information we need to tell the animator
// what to do. Update its parameters; the animator controller
// will play the right animation. This updates the animation's
// root position as well, which we can optionally use to control
// or influence the object's position.

// We're providing three parameters here.
// - 'Moving' is a bool that unambiguously indicates whether we
//   want to be idle or moving.
// - X and Z speed are intended to control a 2D blend tree.
// - Z speed is forward and backward, while X speed is left and
//   right.

// This can also work with a 1D blend tree, where Z speed is the
// only parameter, but if you do this, you should use this
// script in AgentControlsPosition mode.
//

// This is because if your animator controller doesn't have any
// animations that move the root position sideways, the animator
// will find it difficult to make the kinds of turns that the
// agent may try, and the result will be the visible object
// jumping around onscreen as a result of repeatedly moving too
// far from the agent. Play with your blend trees and
// animations to get good-feeling movement.

animator.SetBool(isMovingParameterName, shouldMove);
animator.SetFloat(sidewaysSpeedParameterName, velocity.x);
animator.SetFloat(forwardSpeedParameterName, velocity.y);

// Is the animator controlling our position, instead of
// the agent?
if (mode == Mode.AnimatorControlsPosition) {

// If the animator is controlling our position, the agent
// will start drifting away from the object. If this happens,
// you'll start seeing visual glitches caused by the
// navigation logic not matching the visible object onscreen.

// To fix this, we'll detect if the object is significantly
// far from the agent. "Significantly" means it's more
// than one agent-radius away from the agent's position
// (that is, the object is outside the agent's cylinder).

// When this happens, we'll start bringing the object closer
// to the agent's position. This reduces animation realism
// slightly, because it's movement that's not reflected in
// the motion of the character, but it prevents larger
// errors from accumulating over time.

// Is the object's position far from where the agent wants
// to be?
if (worldDeltaPosition.magnitude > agent.radius)
{
    // Bring us closer to where the agent wants to be
    transform.position = Vector3.Lerp(transform.position,
        agent.nextPosition,
        Time.deltaTime / 0.15f);
}
}
}
```

```
void OnAnimatorMove()
{
    // Which mode is this script in?
    switch (mode)
    {
        case Mode.AgentControlsPosition:
            // Move the object directly where the agent wants
            // to be.
            transform.position = agent.nextPosition;
            break;
        case Mode.AnimatorControlsPosition:
            // Update our position to where the animation system has
            // placed us, following the animation's root movement.

            // Override the movement in the y-axis to where the
            // agent wants to be (otherwise, we'll just pass through
            // hills, stairs, and other changes in ground height).

            Vector3 position = animator.rootPosition;
            position.y = agent.nextPosition.y;
            transform.position = position;
            break;
    }
}
```

- 4. 将 NavigationAnimator 组件添加到对象。

当导航代理开始移动到另一个位置时，它将更新动画器控制器的参数。

## 讨论

让导航代理在移动时播放动画涉及多个方面。最重要的是，在每一帧都会执行检查，以查看对象的模式是由代理控制其位置，还是由动画器控制位置。然后根据模式触发正确的动画系统。

## 8.7 电影相机跟踪

### 问题

你想要创建一个电影风格的相机，并将其设置为始终保持某个对象在视野中。

### 解决方案

做到这一点的最佳方法是使用 Cinemachine 包。Cinemachine 是一个过程式相机系统，旨在提供电影风格的相机控制系统。

> ### 警告
> 
> 如果你阅读 Unity 的文档，可能会发现有关从 Unity 资源商店获取 Cinemachine 的引用。最新版本不久前已从 Unity 资源商店移至 Unity 包管理器，但一些关于 Cinemachine 的文档包含从资源商店获取它的说明；请不要这样做！资源商店中的版本虽然仍可用，但不是最新版本，不会更新，也不一定包含你期望或需要的功能。请务必始终从包管理器获取 Cinemachine 的副本。要复习包管理器的相关知识，请参阅第 1 章。

首先，安装 Cinemachine 包：

- 1. 打开“窗口”菜单，并选择“包管理器”。
- 2. 在“全部”标签页中，找到 Cinemachine 包，然后点击“安装”。

接下来，我们将设置一个 Cinemachine 虚拟相机：

- 1. 打开 Cinemachine 菜单，并选择 Create Virtual Camera（图 8-15）。这将创建并选中一个新的虚拟相机。
- 2. 将虚拟相机定位到场景中的某个位置。
- 3. 将你希望相机查看的对象拖放到虚拟相机的 Look At 插槽中（图 8-16）。
- 4. 运行游戏。相机将看向目标！

- Cinemachine 窗口帮助
- 创建虚拟相机
- 创建自由观察相机
- 创建混合列表相机
- 创建状态驱动相机
- 创建 ClearShot 相机
- 创建带轨道的平移相机
- 创建目标组相机
- 创建混合相机
- 创建 2D 相机
- 创建带平台的轨道
- 导入示例资源包
- 导入后处理 V1 适配器资源包
- 导入后处理 V2 适配器资源包
- 关于

图 8-15. 创建虚拟相机

### Cinemachine 虚拟相机 (脚本)

| 属性 | 值 |
| --- | --- |
| 状态 | 启用 |
| 游戏窗口引导线 | [x] |
| 播放期间保存 | [ ] |
| 优先级 | 10 |
| 跟随 | 无 (Transform) |
| 看向 | Cinemachine 角色 (Transform) |
| 位置混合 | 线性 |

### 镜头

| 属性 | 值 |
| --- | --- |
| 视野 | 40 |
| 近裁剪平面 | 0.1 |
| 远裁剪平面 | 5000 |
| 荷兰角 | 0 |

### 身体

| 属性 | 值 |
| --- | --- |
| — | 不执行任何操作 |

### 瞄准

| 属性 | 值 |
| --- | --- |
| 构图器 | |
| 跟踪对象偏移 | X 0 Y 1.19 Z 0 |
| 前瞻时间 | 0 |
| 前瞻平滑 | 10 |
| 前瞻忽略 Y | [ ] |
| 水平阻尼 | 0.5 |
| 垂直阻尼 | 0.5 |
| 屏幕 X | 0.45131 |
| 屏幕 Y | 0.50125 |
| 死区宽度 | 0.17368 |
| 死区高度 | 0.1 |
| 软区宽度 | 0.8 |
| 软区高度 | 0.8 |
| 偏移 X | 0 |
| 偏移 Y | 0 |

### 噪声

| 属性 | 值 |
| --- | --- |
| — | 无 |

### 扩展

| 属性 | 值 |
| --- | --- |
| 添加扩展 | (选择) |

图 8-16. 配置虚拟相机以查看目标

## 讨论

Cinemachine 使用虚拟相机，它们并非真正的相机：它们的行为更像现实世界中的摄影师，控制真实相机的位置、方向、镜头设置等。虚拟相机本身不执行渲染。Cinemachine 虚拟相机由——听起来有些不祥的——Cinemachine 大脑控制。

Cinemachine 大脑将真实 Unity 相机与虚拟 Cinemachine 相机连接起来。由于大多数电影场景中可能存在多个虚拟相机，大脑将根据你分配给它的优先级监控情况，并在必要时在相机之间进行混合，将虚拟相机的状态应用到真实相机上，如图 8-17 所示。

![](img/4dc1688a9a64dca9b820486c73104ca2_289_0.png)

图 8-17. 配置相机大脑

> ### 注释
> 
> 你可以通过阅读 Cinemachine 文档和 Unity 的 Cinemachine 教程系列来了解更多有关 Cinemachine 的信息。

## 8.8 自动切换相机

### 问题

场景中有多个相机，你希望 Cinemachine（请参阅配方 8.7）在它们之间自动切换，以便目标对象始终保持在视野中。

### 解决方案

这里的解决方案是使用 Cinemachine ClearShot 相机：

- 1. 打开 Cinemachine 菜单，并选择 Create ClearShot Camera。
- 2. 选择 ClearShot 对象，并将你希望保持视野的目标拖入 Look At 字段。
- 3. 将所需数量的虚拟相机添加为 ClearShot 组的子项。

## 讨论

ClearShot 是一个具有一个或多个虚拟相机作为子项的对象，并配置为查看某个目标。它将从其不同的子项中选择哪个相机对目标的拍摄效果最佳。

> **提示**
> 
> 在 Unity 脚本 API 参考中了解有关 ClearShot 相机的更多信息。

## 8.9 保持多个对象在视野中

### 问题

你的场景中有多个对象，你希望相机尝试同时将它们全部保持在视野中。

### 解决方案

