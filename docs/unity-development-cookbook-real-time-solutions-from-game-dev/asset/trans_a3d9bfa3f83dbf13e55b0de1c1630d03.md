

- 1. 创建带有独立碰撞体的游戏对象。这些可以是任何类型的 2D 碰撞体，例如盒子、多边形或圆形。
- 2. 创建一个空游戏对象，并将其设为带有碰撞体的对象的父对象。
- 3. 接下来，为父对象添加 CompositeCollider2D 组件。
- 4. 最后，选择子游戏对象并勾选“Used By Composite”复选框（图 5-6）。当您选择父对象时，其碰撞体形状将通过组合其子对象的形状来定义。

![](img/4dc1688a9a64dca9b820486c73104ca2_133_0.png)

图 5-6. 在碰撞体上启用 Used By Composite 选项

### 讨论

复合碰撞体可以更高效，并防止由两个相互紧邻的碰撞体引起的问题。当两个平面碰撞体相邻时，滑过它们的对象可能会因为大多数物理引擎受到的精度问题而在它们相遇的点处“绊倒”。通过完全移除该角点来解决此问题。

图 5-7 展示了此示例。顶部对象不使用复合碰撞体，且包含一排三个盒形碰撞体，它们之间的线条可见。底部对象同样包含一排三个盒形碰撞体，但配置为复合碰撞体的一部分。它们之间的线条消失了，最终碰撞体是一个没有角点的大矩形形状。

![](img/4dc1688a9a64dca9b820486c73104ca2_134_0.png)

图 5-7. 两组碰撞体：使用复合碰撞体与不使用

## 5.7 使用 Sprite Packer

### 问题

您希望将多个精灵打包到单个纹理中，以节省浪费的空间。

### 解决方案

要使用 Sprite Packer：

- 1. 打开 Assets 菜单并选择 Create → Sprite Atlas，创建一个新的 Sprite Atlas。
- 2. 将要分组的精灵拖入 Objects for Packing 列表。也可以拖入文件夹。

### 讨论

_纹理图集_ 是一张包含多个图像的单个纹理。

构建游戏时，Unity 会收集图集中的所有精灵，并将它们全部打包到一张纹理图像中；在游戏过程中，当精灵渲染器需要显示其精灵时，将使用图集中包含所需数据的子区域。

纹理图集可以带来更好的性能。如果多个精灵渲染器使用存储在同一图集中的精灵，它们可以同时绘制。此外，打包过程会裁剪精灵的边缘，从而减少存储最终图像所需的内存量。

除了创建图集本身之外，您无需做任何其他操作即可使用图集。如果需要绘制的精灵存储在图集中，精灵渲染器将自动使用图集纹理。

## 5.8 对 2D 对象施加力

### 问题

您希望向 2D 对象施加物理力。

### 解决方案

您可以通过脚本使用 Rigidbody2D 类的 AddForce 方法向 2D 对象施加力。

以下脚本演示了如何使用玩家输入向对象施加力：

```
public class AlienForce : MonoBehaviour {

    // The amount of force to apply.
    [SerializeField] float verticalForce = 1000;
    [SerializeField] float sidewaysForce = 1000;

    // A cached reference to the Rigidbody2D on this object.
    Rigidbody2D body;

    // On game start, get the reference to the rigid body and cache it.
    void Start() {
        body = GetComponent<Rigidbody2D>();
    }

    // You will get smoother movement if you apply physical forces in
// FixedUpdate, because it's called a fixed number of times per second,
// with a fixed simulation timestep, which means more stable simulation
void FixedUpdate ()
{
    // Get user input, and scale it by the amount of force we want to
    // apply
    var vertical = Input.GetAxis("Vertical") * verticalForce;
    var horizontal = Input.GetAxis("Horizontal") * sidewaysForce;

    // Generate a force vector from these inputs, scaled by time
    var force =
        new Vector2(horizontal, vertical) * Time.deltaTime;

    // Add the force to the sprite
    body.AddForce(force);
}
}
```

> **注意**
>
> 对于这么小的示例来说尤其不实用，但输入通常会在 Update 中收集，而不是像我们在这里所做的那样在 FixedUpdate 中收集。FixedUpdate 每个渲染帧可以运行多次，这可能意味着最终会得到额外的输入！

### 讨论

与在三维空间中运行的 Rigidbody 组件不同，Rigidbody2D 组件仅在二维空间中运行。这意味着您施加的所有力都是 Vector2，而不是 Vector3。此外，由于只有一个旋转自由度，您只能在一个轴上施加旋转。

除了通过代码施加力之外，您还可以使用 ConstantForce2D 组件（图 5-8）。顾名思义，这些组件会持续向对象施加力。它们适用于知道某物需要移动，但不需要真正控制它的对象和效果。

![](img/4dc1688a9a64dca9b820486c73104ca2_137_0.png)

*图 5-8. ConstantForce2D 组件*

## 5.9 创建传送带

### 问题

您希望创建一个可以推动任何接触它的对象的对象。例如，您希望创建一个传送带。

### 解决方案

表面效果器会施加力，将物体沿着效果器边缘推动。要创建传送带：

- 1. 创建一个新游戏对象，并向其添加 `BoxCollider2D` 组件。将其命名为“Conveyor Belt”。
- 2. 打开“Used By Effector”设置（图 5-9）。
- 3. 单击 Add Component 按钮并选择 Physics 2D → Surface Effector 2D，添加 `SurfaceEffector2D` 组件（图 5-10）。
- 4. 将任何带有 `Rigidbody2D` 和 `Collider2D` 的对象放置在其上方。
- 5. 运行游戏。对象会掉落到效果器上并被其推动。

### 注意

如果 Speed 值为正数，对象将被推向右侧；如果为负数，对象将被推向左侧。Speed Variation 值可让您为应用于对象的速度添加随机变化。

![](img/4dc1688a9a64dca9b820486c73104ca2_138_0.png)

图 5-9. 在碰撞体上启用 Used By Effector 选项

![](img/4dc1688a9a64dca9b820486c73104ca2_138_1.png)

图 5-10. SurfaceEffector2D 组件的设置

### 讨论

效果器与碰撞体协作向对象施加力。当带有 Rigidbody2D 的对象与带有效果器的 Collider2D 碰撞时，效果器会根据其类型和设置向其施加力。（如果效果器的 Collider2D 组件被标记为触发器，则会对边界内的对象施加力。）

Unity 的 2D 物理引擎中有多种效果器。例如，点效果器可以从空间中的某一点吸引和排斥物体，浮力效果器会施加模拟浮力的向上力，平台效果器会创建允许物体单向通过的力。

## 5.10 为精灵使用自定义材质

### 问题

您希望使用自定义材质绘制精灵。

### 解决方案

要为精灵使用自定义材质：

- 1. 打开 Assets 菜单并选择 Create → Material，创建一个新材质资产。将新材质命名为“Sprite Diffuse”。
- 2. 选择新的精灵，并在 Inspector 顶部将着色器更改为 Sprites → Diffuse（图 5-11）。
- 3. 选择场景中任何带有精灵渲染器的游戏对象，并将您的 Sprite Diffuse 材质拖入其 Material 插槽。它现在将响应光照（图 5-12）。

![](img/4dc1688a9a64dca9b820486c73104ca2_140_0.png)

图 5-11. 在材质上选择 Sprites → Diffuse 着色器

![](img/4dc1688a9a64dca9b820486c73104ca2_141_0.png)

图 5-12. 使用 Diffuse 着色器的精灵

### 讨论

默认着色器忽略光照——纹理的颜色就是屏幕上显示的颜色（而不是受环境遮光）。

> **注意**
>
> 如果您计划对精灵进行光照处理，应该将精灵设计得非常明亮。除非精灵被明亮的光照直接照射，否则它会比在纹理中显示的更暗。

