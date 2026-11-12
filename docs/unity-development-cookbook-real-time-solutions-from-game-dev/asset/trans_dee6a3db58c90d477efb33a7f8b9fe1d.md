

## 5.11 管理精灵排序

### 问题

你希望确保一个对象在另一个对象前面或后面绘制。

### 解决方案

要管理精灵排序：

- 1. 选择要控制其绘制顺序的精灵渲染器。
- 2. 更改 Order in Layer 的值。在排序层中，精灵渲染器按此数字的递减顺序绘制；较大的数字绘制在较小的数字前面。

### 讨论

Order in Layer 设置控制精灵在其排序层内的绘制顺序。并且总是至少有一个排序层，称为 Default，无法移除。

要创建排序层，打开 Edit 菜单，选择 Project Settings → Tags & Layers。创建你的排序层，并重新排列它们的顺序（图 5-13）。

图 5-13. 创建和配置排序层

![](img/4dc1688a9a64dca9b820486c73104ca2_143_0.png)

使用排序层来确保大型精灵集合绘制在其他集合的上方或下方。例如，你可以创建一个名为 Background 的排序层和另一个名为 Characters 的排序层；Background 排序层上的对象始终绘制在 Characters 层上的对象后面，无论其 Order in Layer 属性如何。

你可以通过在 Inspector 中更改其 Sorting Layer 属性为精灵渲染器选择排序层（图 5-14）。

排序层的名称与 Layers 相似，但它们是不同的东西。两个对象可以位于不同的排序层上，同时位于同一层。

图 5-14. 为精灵渲染器选择排序层

![](img/4dc1688a9a64dca9b820486c73104ca2_144_0.png)

## 5.12 使用排序组

### 问题

你希望使用排序组来确保一组精灵一起绘制。

### 解决方案

使用排序组来管理精灵的排序顺序：

- 1. 创建一个空游戏对象。将其命名为“Sprite Group”。
- 2. 向其添加 SortingGroup 组件。
- 3. 将任何你希望保持排序顺序的精灵渲染器移动为该对象的子对象。

精灵现在将保持该顺序，无论同一屏幕区域中可能有任何其他对象。

### 讨论

排序组确保该组的所有成员一起绘制，
并且没有其他对象可以在它们之间绘制。

例如，图 5-15 和 5-16 均显示两个角色，每个角色都由许多其他精灵组成。这些精灵的排序顺序已配置，例如，头部绘制在身体上方，一只手臂绘制在身体后面。在图 5-15 中，两个角色的父对象上都有一个排序组，而在图 5-16 中，它们没有。这导致各个身体部件错误地与另一个角色重叠。

图 5-15. 两组精灵，每组都在其父对象上附加了排序组

![](img/4dc1688a9a64dca9b820486c73104ca2_146_0.png)

图 5-16. 相同的两组精灵，排序组已禁用

![](img/4dc1688a9a64dca9b820486c73104ca2_147_0.png)

## 5.13 创建 2.5D 场景

### 问题

你希望创建一个包含 2D 和 3D 元素混合的场景。

### 解决方案

要创建 2.5D 场景：

- 1. 选择你的相机，并将其 Projection 模式更改为 Perspective。
- 2. 选择你的背景对象，并将其移离相机。

当相机从一侧移动到另一侧时，距离相机更远的物体将移动得“更慢”，就像 3D 物体（和现实生活中的物体）一样，这会产生深度感。

请注意，当使用透视相机时，更容易遇到排序问题，尤其是当精灵不直接面向相机时。使用排序组以确保 2D 对象排序正确。

### 讨论

“2.5”有多种不同的风格。例如，你可以使用多个平面并获得视差效果，这意味着你的美术资产仍然是平面的绘制精灵，但玩家会感觉到它们存在于一个更深层的环境中。许多游戏使用这种技术；例如，我们参与协助的游戏 Night in the Woods 几乎完全由平面精灵组成，但其中一些距离相机更远。因为相机的 Projection 模式设置为 Perspective，远处的物体移动得更慢，这增强了距离感。

另一种技术是将 2D 精灵与 3D 背景相结合。例如，在 DuckTales: Remastered (Capcom) 中，所有角色都是 2D 精灵，但它们在 3D 环境中移动。这对平台游戏尤其有效，因为它突出了无法穿越的垂直墙壁，并且当相机被允许上下移动时，会创造出一个更加动态的环境，从而暴露出背景的多个视图。

## 第 6 章 3D 图形

Unity 因其渲染引擎的视觉质量而越来越出名，很大程度上归功于它为艺术家提供的灵活性。在本章中，我们将深入探讨材质、着色器和照明，以及如何构建适合游戏独特外观的自定义效果。

> 提示
> 我们强烈建议也复习第 5 章中的配方，以更全面地了解图形主题。

## 6.1 创建简单材质

### 问题

你希望通过创建材质来自定义 3D 对象表面的外观。

### 解决方案

在本配方中，我们将自定义一个简单的形状，尽管这些步骤适用于你在游戏中最终使用的大多数其他网格：

- 1. 打开 GameObject 菜单并选择 3D Object → Sphere 来创建一个新球体。
- 2. 打开 Assets 菜单，选择 Create → Material。随意命名。
- 3. 选择新材质，并配置其颜色（图 6-1）。
- 4. 将材质拖放到球体上。

检查器

新材质

| 属性 | 值 |
|------|-----|
| 着色器 | Standard |
| 渲染模式 | Opaque |
| 主贴图 | |
| 反照率 | |
| 金属度 | 0.37999 |
| 光滑度 | 0.5 |
| 来源 | Metallic Alpha |
| 法线贴图 | |
| 高度贴图 | |
| 遮挡 | |
| 细节蒙版 | |
| 发射 | |
| 平铺 | X 1 Y 1 |
| 偏移 | X 0 Y 0 |
| 次要贴图 | |
| 细节反照率 x2 | |
| 法线贴图 | 1 |
| 平铺 | X 1 Y 1 |
| 偏移 | X 0 Y 0 |
| UV 集 | UV0 |
| 前向渲染选项 | |
| 镜面高光 | |
| 反射 | |
| 新材质 | |

![](img/4dc1688a9a64dca9b820486c73104ca2_150_0.png)

图 6-1. 显示默认 Standard 着色器的可配置属性的材质检查器

### 讨论

材质对于控制场景中对象的外观至关重要。材质通过将*着色器*（计算构成对象的每个像素最终颜色的程序）与*属性*（传递给着色器并与每像素信息结合的输入）相结合来工作。

大多数情况下，你可能最终会使用 Standard 着色器，这是一种极其可配置的基于物理的渲染（PBR）着色器。你可以指定各种属性，包括反照率（基础）颜色、法线贴图、发射率、光滑度和金属度。尝试调整各种属性，看看你对象的材质会发生什么变化。

Standard 着色器不是唯一的着色器；我们将在本章中探讨其中几个，还会制作自己的着色器。

## 6.2 通过脚本控制材质属性

### 问题

你希望在游戏过程中更改材质的属性，例如其颜色。

### 解决方案

当你可以访问 `MeshRenderer` 或 `SpriteRenderer` 或任何其他类型的渲染器时，可以从中访问材质并修改其属性。例如：

```
public class ChangeMaterialColor : MonoBehaviour {

    // The colors we're fading between
    [SerializeField] Color fromColor = Color.white;
    [SerializeField] Color toColor = Color.green;

    // The speed with which we're fading

[SerializeField] float speed = 1f;

// A cached reference to the renderer (The 'new' keyword makes the
// compiler not warn us about the fact that we're overriding an
// existing property that we inherit from MonoBehaviour; this property
// is deprecated, so we aren't using it and it's okay to override it)
new Renderer renderer;

private void Start()
{
    renderer = GetComponent<Renderer>();
}

private void Update()
{
    // Convert time to a number that smoothly moves between -1 and 1
    float t = Mathf.Sin(Time.time * speed);

    // Convert this to one that moves from 0 to 2
    t += 1;

    // Divide it by 2 to make it move from 0 to 1;
    t /= 2;

    // Interpolate between the two colors
    var newColor = Color.Lerp(fromColor, toColor, t);

    // Apply the new color
    renderer.material.color = newColor;
}
}
```

### 讨论

你可以通过两种方式访问渲染器使用的材质：material 属性和 sharedMaterial 属性。

当你第一次访问渲染器的 material 属性时，Unity 会创建渲染器当前使用的材质的副本，告诉渲染器使用这个新副本，并返回它。这是因为如果多个渲染器都使用同一材质文件，你可能不希望在更改其中一个时更改所有渲染器。

