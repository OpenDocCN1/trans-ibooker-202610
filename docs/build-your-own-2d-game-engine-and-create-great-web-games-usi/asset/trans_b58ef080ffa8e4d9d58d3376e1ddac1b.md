
如图[6-15](#334805_2_En_6_Chapter.xhtml#Fig15)所示，逐像素碰撞测试相当简单，涉及三个`GameObject`实例：大型`Collector`爪牙、小型`Portal`爪牙和`DyePack`。`Collector`和`Portal`爪牙分别由`arrow`和`WASD`键控制。`MyGame`的实现细节与之前的项目相似，不再展示。

值得注意的代码片段是`update()`函数中的碰撞测试，如下所示：

```
update() {
    let msg = "No Collision";
    this.mCollector.update(engine.input.keys.W, engine.input.keys.S,
        engine.input.keys.A, engine.input.keys.D);
    this.mPortal.update(engine.input.keys.Up, engine.input.keys.Down,
        engine.input.keys.Left, engine.input.keys.Right);
    let h = [];
    // Portal's resolution is 1/16 x 1/16 that of Collector!
    // VERY EXPENSIVE!!
    // if (this.mCollector.pixelTouches(this.mPortal, h)) {
    if (this.mPortal.pixelTouches(this.mCollector, h)) {
        msg = "Collided!: (" + h[0].toPrecision(4) + " " +
            h[1].toPrecision(4) + ")";
        this.mDyePack.setVisibility(true);
        this.mDyePack.getXform().setXPos(h[0]);
        this.mDyePack.getXform().setYPos(h[1]);
    } else {
        this.mDyePack.setVisibility(false);
    }
    this.mMsg.setText(msg);
}
```

### 观察

现在你可以通过移动两个爪牙并在不同位置使它们相交（例如，顶部与底部碰撞，左侧与右侧碰撞）或移动它们使其有大面积重叠来测试碰撞精度。注意，预测实际报告的相交位置（`DyePack`的位置）相当困难，甚至不可能。重要的是要记住，逐像素碰撞函数主要是一个返回true或false以指示是否发生碰撞的函数。你不能依赖此函数来计算实际的碰撞位置。

最后，尝试切换到调用`Collector.pixelTouches()`函数来检测碰撞。注意其性能低于实时要求！在这种情况下，`Collector.pixelTouches()`函数的计算成本是`Portal.pixelTouches()`函数的16×16=256倍。

# 广义逐像素碰撞

在上一节中，你看到了实现逐像素精确碰撞检测所需的基本操作。然而，正如你可能注意到的，之前的项目仅适用于纹理沿x/y轴对齐的情况。这意味着你的实现不支持旋转对象之间的碰撞。

本节解释了如何在对象旋转时实现逐像素精确的碰撞检测。该项目的基本概念与上一个项目相同；然而，此版本涉及向量分解，快速复习会有帮助。

## 向量回顾：分量与分解

回顾一下，两个垂直方向可用于将向量分解为相应的分量。例如，图[6-18](#334805_2_En_6_Chapter.xhtml#Fig18)包含两个归一化向量（或分量向量），可用于分解向量 ![$$ \overset{\rightharpoonup }{V}=\left(2,3\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq36.png)：归一化分量向量 ![$$ \hat{i}=\left(1,0\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq37.png) 和 ![$$ \hat{j}=\left(1,0\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq38.png) 将向量 ![$$ \overset{\rightharpoonup }{V} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq39.png) 分解为分量 ![$$ 2\hat{i} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq40.png) 和 ![$$ 3\hat{j} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq41.png)。![图6-18 ¶ 向量 ![$$ \overset{\rightharpoonup }{V} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq42.png) 的分解](images/334805_2_En_6_Chapter/334805_2_En_6_Fig18_HTML.png)

图6-18 ¶ 向量 ![$$ \overset{\rightharpoonup }{V} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq42.png) 的分解

一般来说，如图[6-19](#334805_2_En_6_Chapter.xhtml#Fig19)所示，给定归一化的垂直分量向量 ![$$ \hat{L} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq43.png) 和 ![$$ \hat{M} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq44.png) 以及任意向量 ![$$ \overset{\rightharpoonup }{V} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq45.png)，以下公式始终成立： ![$$ \overset{\rightharpoonup }{V}=\left(\overset{\rightharpoonup }{V}\bullet \hat{i}\right)\ \hat{i}+\left(\overset{\rightharpoonup }{V}\bullet \hat{j}\right)\ \hat{j} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_Equa.png)![$$ \overset{\rightharpoonup }{V}=\left(\overset{\rightharpoonup }{V}\bullet \hat{L}\right)\ \hat{L}+\left(\overset{\rightharpoonup }{V}\bullet \hat{M}\right)\ \hat{M} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_Equb.png)![图6-19 ¶ 通过两个归一化分量向量分解向量](images/334805_2_En_6_Chapter/334805_2_En_6_Fig19_HTML.png)

图6-19 ¶ 通过两个归一化分量向量分解向量

向量分解与此项目相关，因为涉及旋转的图像轴。如果没有旋转，图像可以通过沿默认x轴 ![$$ \hat{i} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq46.png) 和y轴 ![$$ \hat{j} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq47.png) 的熟悉的归一化垂直向量集来引用。你在上一个项目中处理了这种情况。你可以在图[6-20](#334805_2_En_6_Chapter.xhtml#Fig20)中看到一个示例。![图6-20 ¶ 轴对齐的纹理](images/334805_2_En_6_Chapter/334805_2_En_6_Fig20_HTML.png)

图6-20 ¶ 轴对齐的纹理

然而，图像旋转后，参考向量集不再位于x/y轴上。因此，碰撞计算必须考虑新旋转的轴 ![$$ \hat{L} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq48.png) 和 ![$$ \hat{M} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq49.png)，如图[6-21](#334805_2_En_6_Chapter.xhtml#Fig21)所示。![图6-21 ¶ 旋转纹理及其分量向量](images/334805_2_En_6_Chapter/334805_2_En_6_Fig21_HTML.png)

图6-21 ¶ 旋转纹理及其分量向量

## 广义像素碰撞项目

该项目演示了如何以逐像素精度检测两个旋转的`TextureRenderable`对象之间的碰撞。与上一个项目类似，一个黄色的`DyePack`对象（作为测试确认）将显示在检测到的碰撞位置。你可以在图[6-22](#334805_2_En_6_Chapter.xhtml#Fig22)中看到该项目运行的示例。该项目的源代码定义在`chapter6/6.5.general_pixel_collisions`文件夹中。![图6-22 ¶ 运行广义像素碰撞项目](images/334805_2_En_6_Chapter/334805_2_En_6_Fig22_HTML.jpg)

图6-22 ¶ 运行广义像素碰撞项目

该项目的控制方式如下：

*   `Arrow keys`：移动小型纹理对象，即`Portal`爪牙
*   `P key`：旋转小型纹理对象，即`Portal`爪牙
*   `WASD keys`：移动大型纹理对象，即`Collector`爪牙
*   `E key`：旋转大型纹理对象，即`Collector`爪牙

该项目的目标如下：

*   通过向量分解访问旋转图像的像素
*   支持两个旋转纹理对象之间的逐像素精确碰撞检测
