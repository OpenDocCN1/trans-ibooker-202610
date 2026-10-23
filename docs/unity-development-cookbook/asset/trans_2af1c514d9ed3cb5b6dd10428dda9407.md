

![](img/e54c00769db669e3746fa1e50090241f_111_0.png)

图 5-6. 在碰撞体上启用 Used By Composite 选项

## Discussion

复合碰撞体可以更高效，并且还能防止两个紧挨着的碰撞体所引起的问题。当两个平面碰撞体彼此相邻时，一个在上面滑动的物体可能会因为大多数物理引擎都存在的精度问题而在它们相接的点处“绊倒”。通过完全移除这个角落即可解决此问题。

图 5-7 显示了此问题的示例。上方的物体未使用复合碰撞体，包含三個按行排列的盒状碰撞体，它们之间的线条可见。下方的物体也有三个按行排列的盒状碰撞体，但配置为复合碰撞体的一部分。它们之间的线条消失了，最终的碰撞体变成了一个没有角落的大矩形形状。

![](img/e54c00769db669e3746fa1e50090241f_111_1.png)

图 5-7. 两组碰撞体：有复合碰撞体和没有复合碰撞体

## 5.7 Using the Sprite Packer

### Problem

你想将多个精灵打包到单一纹理中以节省浪费的空间。

### Solution

使用精灵打包器的方法如下：

- 1. 通过打开 Assets 菜单并选择 Create → Sprite Atlas 创建一个新的 Sprite Atlas。
- 2. 将你想要分组的精灵拖到 Objects for Packing 列表中。你也可以拖动文件夹。

### Discussion

*纹理图集* 是一张包含多个图像的单一纹理。在游戏构建时，Unity 会收集图集中所有的精灵并构建一张将它们全部打包在一起的单一纹理图像；在游戏过程中，当精灵渲染器需要显示其精灵时，就会使用图集中包含所需数据的那部分区域。

纹理图集可以带来更好的性能。如果多个精灵渲染器使用存储在同一图集中的精灵，它们可以同时绘制。此外，打包过程会裁剪精灵的边缘，从而减少存储最终图像所需的内存量。

除了创建图集本身之外，你无需做任何事情来使用图集。精灵渲染器会自动使用图集纹理，如果它们需要绘制的精灵存储在图集中。

## 5.8 Applying Forces to 2D Objects

### Problem

你想对 2D 物体施加物理力。

### Solution

你可以通过脚本使用 Rigidbody2D 类的 AddForce 方法对 2D 物体施加力。

下面的脚本演示了如何使用玩家输入对物体施加力：

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
    void FixedUpdate() {
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

![](img/e54c00769db669e3746fa1e50090241f_113_0.png)

对于如此小的示例来说并不特别实用，但输入通常应该在 Update 中收集，而不是像我们这里这样在 FixedUpdate 中收集。FixedUpdate 可能每帧渲染运行多次，这可能导致你得到额外的输入！

### Discussion

与在三维中操作的 Rigidbody 组件不同，Rigidbody2D 组件仅在二维中操作。这意味着你施加的所有力都是 Vector2，而不是 Vector3。此外，由于只有一个旋转自由度，你只能在一个轴上施加旋转。

除了用代码施加力之外，你还可以使用 ConstantForce2D 组件（图 5-8）。顾名思义，这些组件会持续对物体施加力。它们适用于你知道某物需要移动但又不需要真正控制它的物体和效果。

![](img/e54c00769db669e3746fa1e50090241f_114_0.png)

图 5-8. 一个 ConstantForce2D 组件

## 5.9 Creating a Conveyor Belt

### Problem

你想创建一个推动接触到它的任何物体的物体。例如，你想创建一个传送带。

### Solution

表面效应器会沿着效应器边缘推动物体施加力。要创建传送带：

- 1. 创建一个新的游戏对象，并为其添加一个 BoxCollider2D 组件。将其命名为“Conveyor Belt”。
- 2. 打开 Used By Effector 设置（图 5-9）。
- 3. 通过点击 Add Component 按钮并选择 Physics 2D → Surface Effector 2D 添加一个 SurfaceEffector2D 组件（图 5-10）。
- 4. 在其上方放置任何带有 RigidBody2D 和 Collider2D 的物体。
- 5. 运行游戏。物体将落到效应器上并被其推动。

![](img/e54c00769db669e3746fa1e50090241f_114_1.png)

如果 Speed 值为正数，物体将被推向右侧；如果 Speed 值为负数，物体将被推向左侧。Speed Variation 值可让你为施加给物体的速度添加随机变化。

图 5-9. 在碰撞体上启用 Used By Effector 选项

图 5-10. SurfaceEffector2D 组件的设置

### Discussion

效应器与碰撞体协同工作以对物体施加力。当带有 Rigidbody2D 的物体与带有效应器的 Collider2D 碰撞时，效应器会根据效应器的类型和设置对其施加力。 （如果效应器的 Collider2D 组件被标记为触发器，则会对边界内的物体施加力。）

Unity 的 2D 物理引擎中有多个效应器。例如，点效应器可以从空间中的单点吸引和排斥物体，浮力效应器施加向上力以模拟浮力，而平台效应器创建使物体能够单向穿过的力。

## 5.10 Using a Custom Material for Sprites

### Problem

你想使用自定义材质来绘制精灵。

### Solution

使用自定义材质为精灵绘制的方法如下：

- 1. 通过打开 Assets 菜单并选择 Create → Material 创建一个新的材质资源。将新材质命名为“Sprite Diffuse”。
- 2. 选择新的精灵，并在 Inspector 顶部将着色器更改为 Sprites → Diffuse（图 5-11）。
- 3. 选择场景中任何带有精灵渲染器的游戏对象，并将你的 Sprite Diffuse 材质拖到其 Material 槽中。它现在将响应光照（图 5-12）。

![](img/e54c00769db669e3746fa1e50090241f_116_0.png)

图 5-11. 在材质上选择 Sprites → Diffuse 着色器

![](img/e54c00769db669e3746fa1e50090241f_117_0.png)

图 5-12. 使用 Diffuse 着色器的精灵

### Discussion

默认着色器忽略光线——纹理的颜色就是屏幕上使用的颜色（而不是由环境着色）。

![](img/e54c00769db669e3746fa1e50090241f_117_1.png)

如果你计划为精灵添加光照，你应该设计精灵使其非常明亮。除非精灵正好被明亮的光线直射，否则它会比纹理中看起来更暗。

## 5.11 Managing Sprite Sorting

### Problem

你想确保一个物体在另一个物体前面或后面绘制。

### Solution

管理精灵排序的方法如下：

- 1. 选择你想要控制绘制顺序的精灵渲染器。
- 2. 更改 Order in Layer 值。精灵渲染器在排序层内按此数字的递减顺序绘制；较大的数字绘制在较小的数字前面。

