
# Kelvin Sung, Jebediah Pavleas, Matthew Munson, and Jason Pace

**隶属机构：**
1. Bothell, WA, USA
2. Kenmore, WA, USA
3. Lake Forest Park, WA, USA
4. Portland, OR, USA

## 学习目标

完成本章后，您将能够：

*   实现自主行为，如目标锁定追逐和渐进转向
*   准确实现纹理对象碰撞
*   理解像素级精确碰撞的效率问题
*   高效且有效地使用像素级精确碰撞进行编程

## 简介

此时，您的游戏引擎已能在便捷的坐标系统中实现游戏，并呈现和动画化视觉上吸引人的对象。然而，缺乏对对象行为的抽象支持。您可以在所有先前项目中 `MyGame` 对象的 `init()` 和 `update()` 函数中直接看到这一缺陷的后果：`init()` 函数常被繁琐的逐游戏对象设置填满，而 `update()` 函数常被控制对象的条件语句填满，例如检测按键以移动英雄。

一个设计良好的系统应使用适当的面向对象抽象或类来隐藏单个对象的初始化和控制。应引入抽象的 `GameObject` 类来封装并隐藏其初始化和行为的细节。这种方法有两个主要优势。首先，游戏关卡的 `init()` 和 `update()` 函数可以专注于管理单个游戏对象及其交互，而不被不同类型对象的特定细节所困扰。其次，正如您在 `Renderable` 和 `SimpleShader` 类层次结构中所体验的，适当的面向对象抽象创建了标准化接口，并促进了代码共享和重用。

当您从仅绘制对象（即 `Renderable`）转向编程对象行为（即 `GameObject`）时，您会立即注意到，为了使游戏有趣或好玩，对象需要相互作用。对象的有趣行为，如面向或躲避敌人，通常需要了解游戏中其他对象的相对位置。一般来说，解析 2D 世界中所有对象的相对位置并非易事。幸运的是，典型视频游戏只需了解那些相互靠近、即将碰撞或已碰撞的对象。

一种高效但略显粗糙的碰撞检测近似方法是计算对象的边界，并基于碰撞边界框来近似对象碰撞。在最简单的情况下，边界框是边缘与 x/y 轴对齐的矩形框。这些被称为轴对齐边界框，即 AABB。由于轴对齐，检测两个 AABB 何时重叠或即将发生碰撞在计算上很高效。

许多 2D 游戏引擎还可以通过比较两个对象的像素位置，检测至少一个非透明像素重叠的情况，来检测两个纹理对象之间的实际碰撞。这一计算密集型过程被称为逐像素精确碰撞检测、像素级精确碰撞或逐像素碰撞。

本章首先介绍 `GameObject` 类，为抽象游戏对象行为提供平台。然后将 `GameObject` 类泛化，引入包括速度、移动方向和目标锁定追逐在内的常见行为属性。本章其余部分重点在于推导一种高效的逐像素精确碰撞实现，支持纹理和动画精灵对象。

## 游戏对象

如前所述，应引入一种封装典型游戏对象内在行为的抽象，以最小化游戏关卡 `init()` 和 `update()` 函数中的代码聚集，并促进重用。本节介绍简单的 `GameObject` 类，以说明更清晰、整洁的 `init()` 和 `update()` 函数如何清晰地反映游戏内逻辑，并演示用于抽象对象行为的基本平台如何促进设计和代码重用。

### 游戏对象项目

该项目定义简单的 `GameObject` 类，作为构建抽象以表示游戏中具有行为的实际对象的第一步。您可以在图 [6-1](#334805_2_En_6_Chapter.xhtml#Fig1) 中看到该项目运行的示例。注意许多小兵从右向左冲锋，并在到达左边界时环绕。该项目将引导您创建基础设施以支持众多小兵，同时保持 `MyGame` 关卡逻辑简单。该项目的源代码定义在 `chapter6/6.1.game_objects` 文件夹中。

![图 6-1 ¶ 运行游戏对象项目](images/334805_2_En_6_Chapter/334805_2_En_6_Fig1_HTML.jpg)

**图 6-1 ¶ 运行游戏对象项目**

项目控制如下：

*   **WASD 键**：上、左、下、右移动英雄

项目目标如下：

*   开始定义 `GameObject` 类以封装游戏中的对象行为
*   演示创建 `GameObject` 类的子类，以保持 `MyGame` 关卡 `update()` 函数的简洁性
*   引入 `GameObjectSet` 类，演示对具有相同接口的一组同质对象的支持

您可以在 `assets` 文件夹中找到以下外部资源文件：`minion_sprite.png`；您还将找到包含默认系统字体的 `fonts` 文件夹。注意，如图 [6-2](#334805_2_En_6_Chapter.xhtml#Fig2) 所示，`minion_sprite.png` 图像文件已从上一个项目更新，以包含两个额外的精灵元素：DyePack 和 Brain 小兵。

![图 6-2 ¶ minion_sprite.png 图像的新精灵元素](images/334805_2_En_6_Chapter/334805_2_En_6_Fig2_HTML.png)

**图 6-2 ¶ minion_sprite.png 图像的新精灵元素**

#### 定义 GameObject 类

目标是定义一个逻辑抽象，以封装游戏中典型对象的所有相关行为特征，包括控制位置、绘制等能力。正如前一章中的 `Scene` 对象一样，主要结果是提供一个定义良好的接口，以管辖子类实现的函数。更复杂的行为将在下一节介绍。此示例仅演示定义了最小行为的 `GameObject` 类的潜力。

1.  添加新文件夹 `src/engine/game_objects` 用于存储 `GameObject` 相关文件。
2.  在此文件夹中创建一个新文件，命名为 `game_object.js`，并添加以下代码：

```javascript
class GameObject {
    constructor(renderable) {
        this.mRenderComponent = renderable;
    }
    getXform() { return this.mRenderComponent.getXform(); }
    getRenderable() { return this.mRenderComponent; }
    update() { }
    draw(aCamera) {
        this.mRenderComponent.draw(aCamera);
    }
}
export default GameObject;
```

随着对 `Renderable` 和 `Transform` 对象的访问器的定义，所有 `GameObject` 实例都可以被绘制，并具有定义的位置和大小。注意，`update()` 函数设计为供子类用特定于每个对象的行为重写，因此留空。

#### 成套管理游戏对象

因为大多数游戏由许多相互作用的对象组成，定义一个实用程序类来支持处理一组 `GameObject` 实例很有用：
