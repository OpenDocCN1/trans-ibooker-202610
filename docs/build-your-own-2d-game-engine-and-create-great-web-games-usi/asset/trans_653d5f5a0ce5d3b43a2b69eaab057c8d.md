
# 相机插值

## 人体运动与指数插值

人体运动和移动通常遵循指数插值函数。例如，尝试将头从面朝前转向面朝右，或者移动手去拿桌上的物体。注意在这两种情况下，你开始时动作相对较快，当接近目的地时会显著减速。也就是说，你可能开始时快速转头，随着视线接近右侧而迅速减速，同样你的手可能开始时快速朝物体移动，当手快要触及物体时显著减速。在这两个例子中，你的位移都遵循图 [7-4](#334805_2_En_7_Chapter.xhtml#Fig4) 所示的指数插值函数，即快速变化后随着接近目的地而迅速减速。这就是你将在游戏引擎中实现的函数，因为它模仿人类运动，对人类玩家来说显得很自然。

> **注意**
>
> 线性插值通常称为 `LERP` 或 `lerp`。lerp 的结果是初始值和最终值的线性组合。在本章中，以及在几乎所有情况下，图 [7-4](#334805_2_En_7_Chapter.xhtml#Fig4) 所示的指数插值都是通过反复应用 `lerp` 函数来近似的，其中每次调用的初始值都是前一次 `lerp` 调用的结果。这样，指数函数就用分段线性函数来近似了。

本节介绍 `Lerp` 和 `LerpVec2` 实用工具类，以支持因相机操作而产生的平滑渐进的相机移动。

## 相机插值项目

该项目演示了来自相机操作的更平滑、视觉上更吸引人的插值结果。你可以在图 [7-5](#334805_2_En_7_Chapter.xhtml#Fig5) 中看到该项目运行的示例。该项目的源代码定义在 `chapter7/7.2.camera_interpolations` 文件夹中。

![Figure 7-5 ¶ Running the Camera Interpolations project](images/334805_2_En_7_Chapter/334805_2_En_7_Fig5_HTML.jpg)

**图 7-5 ¶ 运行相机插值项目**

### 项目控制

该项目的控制与前一个项目相同：

*   **`WASD` 键**：移动 Dye 角色（`Hero` 对象）。注意当 `Hero` 对象试图移出 WC 边界 90% 范围时，相机 WC 窗口会更新以跟随 `Hero` 对象。
*   **方向键**：移动 `Portal` 对象。注意 `Portal` 对象无法移出 WC 边界 80% 范围。
*   **`L`/`R`/`P`/`H` 键**：选择左小兵、右小兵、`Portal` 对象或 `Hero` 对象作为焦点对象。`L`/`R` 键也会设置相机聚焦于左或右小兵。
*   **`N`/`M` 键**：以相机中心为基准放大或缩小。
*   **`J`/`K` 键**：在确保当前焦点对象相对位置不变的情况下放大或缩小。换句话说，随着相机缩放，所有对象的位置都会改变，除了焦点对象。

### 项目目标

项目目标如下：

*   理解给定值之间插值的概念
*   实现支持相机参数渐进变化的插值
*   体验相机参数的插值变化

与前几个项目一样，你可以在 `assets` 文件夹中找到外部资源文件。

### 插值作为实用工具

类似于支持变换功能的 `Transform` 类和支持碰撞检测的 `BoundingBox` 类，可以定义一个 `Lerp` 类来支持值的插值。为了保持源代码有序，应定义一个新文件夹来存放这些实用工具。

创建 `src/engine/utils` 文件夹，并将 `transform.js` 和 `bounding_box.js` 文件移入该文件夹。

#### Lerp 类

定义 `Lerp` 类以计算两个值之间的插值：

1.  **在 `src/engine/utils` 文件夹中创建一个新文件**，命名为 `lerp.js`，并定义构造函数。该类旨在在 `mCycles` 持续时间内将值从 `mCurrentValue` 插值到 `mFinalValue`。在每次更新期间，根据 `mCurrentValue` 和 `mFinalValue` 之差的 `mRate` 增量计算中间结果，如下所示。

```javascript
class Lerp {
    constructor(value, cycles, rate) {
        this.mCurrentValue = value;   // 插值的起始值
        this.mFinalValue = value;     // 插值的最终值
        this.mCycles = cycles;
        this.mRate = rate;
        // 插值剩余的周期数
        this.mCyclesLeft = 0;
    }
    ... 后续实现 ...
}
```

2.  **定义计算中间结果的函数：**

```javascript
// 子类应重写此函数以处理非标量值
_interpolateValue() {
    this.mCurrentValue = this.mCurrentValue + this.mRate *
                         (this.mFinalValue - this.mCurrentValue);
}
```

注意 `_interpolateValue()` 函数计算的结果是在 `mCurrentValue` 和 `mFinalValue` 之间进行线性插值。这样，在每次迭代中 `mCurrentValue` 都会被设置为中间值，随着它接近 `mFinalValue` 的值，从而近似一条指数曲线。

3.  **定义一个配置插值的函数。** `mRate` 变量定义插值结果接近最终值的速度。`mRate` 为 `0.0` 将导致完全没有变化，而 `1.0` 会导致瞬间变化。`mCycle` 变量定义插值过程的持续时间。

```javascript
config(stiffness, duration) {
    this.mRate = stiffness;
    this.mCycles = duration;
}
```

4.  **定义相关的 getter 和 setter 函数。** 注意 `setFinal()` 函数既设置最终值，又触发新一轮插值计算。

```javascript
get() { return this.mCurrentValue; }
setFinal(v) {
    this.mFinalValue = v;
    this.mCyclesLeft = this.mCycles;   // 将触发插值
}
```

5.  **定义触发计算每个中间结果的函数：**

```javascript
update() {
    if (this.mCyclesLeft <= 0) { return; }
    this.mCyclesLeft--;
    if (this.mCyclesLeft === 0) {
        this.mCurrentValue = this.mFinalValue;
    } else {
        this._interpolateValue();
    }
}
```

6.  **最后，确保导出定义的类：**

```javascript
export default Lerp;
```

#### LerpVec2 类

由于许多相机参数都是 `vec2` 对象（例如 WC 中心位置），因此将 `Lerp` 类泛化以支持 `vec2` 对象的插值非常重要：

1.  **在 `src/engine/utils` 文件夹中创建一个新文件**，命名为 `lerp_vec2.js`，并定义其构造函数：

```javascript
class LerpVec2 extends Lerp {
    constructor(value, cycle, rate) {
        super(value, cycle, rate);
    }
    ... 后续实现 ...
}
```

2.  **重写 `_interpolateValue()` 函数** 以计算 `vec2` 的中间结果：

```javascript
_interpolateValue() {
    vec2.lerp(this.mCurrentValue, this.mCurrentValue,
              this.mFinalValue, this.mRate);
}
```

`gl-matrix.js` 文件中定义的 `vec2.lerp()` 函数计算 `vec2` 的 `x` 和 `y` 分量。所涉及的计算与 `Lerp` 类中的 `_interpolateValue()` 函数相同。

最后，记得更新引擎访问文件 `index.js`，将新定义的 `Lerp` 和 `LerpVec2` 功能转发给客户端。

### 使用 CameraState 表示插值中间结果

`Camera` 对象的状态必须泛化以支持插值中间结果的渐进变化。引入 `CameraState` 类来实现这一目的。

1.  **在 `src/engine/cameras` 文件夹中创建一个新文件**，命名为 `camera_state.js`，导入定义的 `Lerp` 功能，并定义构造函数：
