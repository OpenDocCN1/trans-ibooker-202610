
# 相机抖动与物体振荡项目

## 项目概述

本项目演示如何实现阻尼简谐运动以模拟物体振荡，并注入伪随机性以创建相机抖动效果。您可以在图 [7-7](#334805_2_En_7_Chapter.xhtml#Fig7) 中看到该项目运行的示例。除了增加了一个用于创建物体振荡和相机抖动效果的命令外，该项目与前一个项目相同。该项目的源代码定义在 `chapter7/7.3.camera_shake_and_object_oscillate` 文件夹中。

![图 7-7 ¶ 运行相机抖动与物体振荡项目](images/334805_2_En_7_Chapter/334805_2_En_7_Fig7_HTML.jpg)

## 控制

### 新增控制

*   **`Q 键`**: 启动 Dye 角色的位置振荡和相机抖动效果。

### 现有控制（与前一个项目相同）

*   **`WASD 键`**: 移动 Dye 角色（即 `Hero` 对象）。请注意，当 `Hero` 对象试图移出 `WC` 边界的 90% 范围时，相机 `WC` 窗口会更新以跟随 `Hero` 对象。
*   **`方向键`**: 移动 `Portal` 对象。请注意，`Portal` 对象无法移出 `WC` 边界的 80% 范围。
*   **`L/R/P/H 键`**: 选择左侧小兵、右侧小兵、`Portal` 对象或 `Hero` 对象作为焦点对象。`L/R` 键还会将相机焦点设置为左侧或右侧小兵。
*   **`N/M 键`**: 以相机中心为基准进行放大或缩小。
*   **`J/K 键`**: 在确保当前焦点对象相对位置不变的情况下进行放大或缩小。换句话说，随着相机缩放，所有物体的位置都会改变，除了焦点对象的位置。

## 目标

项目目标如下：

*   了解如何使用简单的数学函数来建模位移
*   体验物体上的振荡效果
*   体验相机上的抖动效果
*   将振荡实现为阻尼简谐运动，并引入伪随机性以创建相机抖动效果

与前几个项目一样，您可以在 `assets` 文件夹中找到外部资源文件。

## 抽象抖动行为

相机抖动能力是许多游戏中常见且动态的行为。然而，重要的是要认识到抖动行为不仅可以应用于相机。也就是说，抖动效果可以抽象为对数值（如大小、点或位置）的扰动（抖动）。在相机抖动的情况下，被抖动的数值恰好代表相机的 x 和 y 位置。因此，抖动及其相关支持应该是游戏引擎的通用工具函数，以便游戏开发者可以将其应用于任何数值。以下是将要定义的新工具：

*   **`Oscillate`**: 实现数值随时间进行简谐振荡的基类
*   **`Shake`**: `Oscillate` 类的扩展，向振荡幅度引入随机性，以模拟抖动效果在数值上的轻微混乱
*   **`ShakeVec2`**: `Shake` 类的扩展，将 `Shake` 行为扩展到两个数值，如位置

### 创建 Oscillate 类以模拟简谐运动

因为所有描述的行为都依赖于简单振荡，所以应首先实现它。

#### 步骤 1.1：创建 `oscillate.js`

在 `src/engine/utils` 文件夹中创建一个新文件，命名为 `oscillate.js`。定义一个名为 `Oscillate` 的类，并添加以下代码来构造对象：

```javascript
class Oscillate {
    constructor(delta, frequency, duration) {
        this.mMag = delta;
        this.mCycles = duration; // cycles to complete the transition
        this.mOmega = frequency * 2 * Math.PI; // Converts to radians
        this.mNumCyclesLeft = duration;
    }
    ... implementation to follow ...
}
export default Oscillate;
```

`delta` 变量表示阻尼前的初始位移，单位为 `WC` 空间。`frequency` 参数指定振荡程度，值为 1 代表余弦函数的一个完整周期。`duration` 参数定义振荡持续时间，单位为游戏循环更新次数。

#### 步骤 1.2：定义阻尼简谐运动

定义阻尼简谐运动：

```javascript
_nextDampedHarmonic() {
    // computes (Cycles) * cos(Omega * t)
    let frac = this.mNumCyclesLeft / this.mCycles;
    return frac * frac * Math.cos((1 - frac) * this.mOmega);
}
```

请参阅图 [7-8](#334805_2_En_7_Chapter.xhtml#Fig8)。`mNumCyclesLeft` 是振荡剩余的周期数，即 `k-t`，而 `frac` 变量，![$$ \frac{k-t}{k} $$](images/334805_2_En_7_Chapter/334805_2_En_7_Chapter_TeX_IEq1.png)，是阻尼因子。该函数返回 -1 到 1 之间的值，可根据需要进行缩放。

![图 7-8 ¶ 指定数值振荡的阻尼简谐运动](images/334805_2_En_7_Chapter/334805_2_En_7_Fig8_HTML.png)

#### 步骤 1.3：定义受保护函数 `_nextValue()`

定义一个受保护函数以获取下一个阻尼谐波运动的值。此函数可能看起来微不足道且没有必要。然而，正如您在下一小节中将看到的，此函数允许 shake 子类覆盖并注入随机性。

```javascript
// local/protected methods
_nextValue() {
    return (this._nextDampedHarmonic());
}
```

#### 步骤 1.4：定义结束检查和重启函数

定义函数以检查振荡是否结束以及重启振荡：

```javascript
done() { return (this.mNumCyclesLeft <= 0); }
reStart() { this.mNumCyclesLeft = this.mCycles; }
```

#### 步骤 1.5：定义公共函数 `getNext()`

最后，定义一个公共函数来触发振荡计算。请注意，计算出的振荡结果必须按所需幅度 `mMag` 进行缩放：

```javascript
getNext() {
    this.mNumCyclesLeft--;
    let v = 0;
    if (!this.done()) {
        v = this._nextValue();
    }
    return (v * this.mMag);
}
```

### 创建 Shake 类以随机化振荡

现在，您可以通过向效果中引入伪随机性来扩展振荡行为，以传达抖动感。

#### 步骤 1.1：创建 `shake.js`

在 `src/engine/utils` 文件夹中创建一个新文件 `shake.js`。定义 `Shake` 类以扩展 `Oscillate`，并添加以下代码来构造对象：

```javascript
import Oscillate from "./oscillate.js";
class Shake extends Oscillate {
    constructor(delta, frequency, duration) {
        super(delta, frequency, duration);
    }
    ... implementation to follow ...
}
export default Shake;
```

#### 步骤 1.2：覆盖 `_nextValue()` 以随机化符号

按如下方式覆盖 `_nextValue()` 以随机化振荡结果的符号。回想一下，`_nextValue()` 函数是从公共 `getNext()` 函数调用的，用于获取振荡值。虽然阻尼简谐振荡的结果在幅度上持续且可预测地减小，但相关值的符号被随机化，导致突然且意外的不连续性，传达出抖动结果的混乱感。

```javascript
_nextValue() {
    let v = this._nextDampedHarmonic();
    let fx = (Math.random() > 0.5) ? -v : v;
    return fx;
}
```

### 创建 ShakeVec2 类以模拟 vec2（即位置）的抖动

现在，您可以将抖动效果推广以支持同时抖动两个值。这是一个有用的工具，因为 2D 游戏中的位置是双值实体，且位置是抖动效果的便利目标。例如，在本项目中，对相机位置（一个双值实体）进行抖动，模拟了相机抖动效果。

`ShakeVec2` 类扩展了 `Shake` 类，以支持对 `vec2` 对象进行抖动，同时抖动 x 和 y 两个维度的值。x 维度的抖动通过 `Shake` 对象的实例支持，而 y 维度则通过超类中定义的 `Shake` 类功能支持。

#### 步骤 1.1
