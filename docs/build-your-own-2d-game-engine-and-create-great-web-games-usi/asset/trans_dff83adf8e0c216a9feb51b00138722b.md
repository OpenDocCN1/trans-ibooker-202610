

# 碰撞位置修正项目

对于您的引擎，您将结合投影法和冲量法的优势。投影法将用于分离相互渗透的物体，而冲量法将用于计算冲量，以减小导致相互渗透方向上的物体速度。正如所述，简单的投影法可能导致系统不稳定，例如物体堆叠时会相互陷入。您将通过实现一个松弛循环来克服这种不稳定性，在该循环中，在单个更新周期内，通过重复应用投影法，以增量方式分离相互渗透的物体。

有了松弛循环，投影法的每一次应用被称为一次松弛迭代。在每次松弛迭代期间，投影法会按总渗透深度的固定百分比，以增量方式减少相互渗透。例如，默认情况下，引擎将松弛迭代次数设置为 15 次，每次松弛迭代可减少 80% 的相互渗透。这意味着在一次更新函数调用中，在运动积分近似之后，碰撞检测和解析过程将执行 15 次。虽然代价高昂，但重复的增量分离确保了系统的稳定。

该项目将指导您实现松弛迭代，以增量方式解析物体间的相互渗透。您将使用上一个项目计算出的碰撞信息来修正碰撞物体的位置。您可以在图 [9-24](#334805_2_En_9_Chapter.xhtml#Fig24) 中看到该项目运行的示例。该项目的源代码定义在 `chapter9/9.6.collision_position_correction` 中。

![图 9-24 ¶ 运行碰撞位置修正项目](images/334805_2_En_9_Chapter/334805_2_En_9_Fig24_HTML.jpg)

**图 9-24** ¶ 运行碰撞位置修正项目

该项目的控制方式与上一个项目相同，仅在行为控制中增加了 `P` 键命令：

**行为控制**：

*   **`P` 键**：切换所有物体的渗透解析
*   **`V` 键**：切换所有物体的运动
*   **`H` 键**：向所有物体注入随机速度
*   **`G` 键**：随机创建一个新的刚性圆形或矩形

**绘制控制**：

*   **`C` 键**：切换所有 `CollisionInfo` 的绘制
*   **`T` 键**：切换所有物体的纹理
*   **`R` 键**：切换 `RigidShape` 的绘制
*   **`B` 键**：切换每个 `RigidShape` 边界的绘制

**物体控制**：

*   **左/右箭头键**：依次遍历并选择一个物体
*   **`WASD` 键**：移动选中的物体
*   **`Z/X` 键**：旋转选中的物体
*   **`Y/U` 键**：增加/减小选中物体的 `RigidShape` 尺寸；这不会改变对应 `Renderable` 物体的尺寸
*   **上/下箭头键 + `M`**：增加/减小选中物体的质量

项目目标如下：

*   实现带有松弛迭代的位置修正
*   使用计算出的碰撞信息并理解其重要性
*   理解并体验实现相互渗透解析

## 更新物理组件

之前的项目已经建立了所需的模拟基础设施，包括完成 `RigidShape` 的实现。现在您可以专注于位置修正逻辑的细节，该逻辑位于 `src/engine/components` 文件夹下 `physics.js` 文件中物理组件的核心部分，是局部且隐藏的。

### 步骤 1：定义变量和 Getter/Setter

编辑 `physics.js` 以定义变量以及位置修正率、松弛循环计数和切换位置修正计算的相关 getter 和 setter。确保导出新定义的函数。

```javascript
let mPosCorrectionRate = 0.8;  // 投影分离物体的百分比
let mRelaxationCount = 15;     // 松弛迭代次数
let mCorrectPosition = true;

function getPositionalCorrection() {
    return mCorrectPosition;
}

function togglePositionalCorrection() {
    mCorrectPosition = !mCorrectPosition;
}

function getRelaxationCount() {
    return mRelaxationCount;
}

function incRelaxationCount(dc) {
    mRelaxationCount += dc;
}

// ... 与之前代码相同 ...

export {
    // ... 与之前代码相同 ...
    togglePositionalCorrection,
    getPositionalCorrection,
    getRelaxationCount,
    incRelaxationCount
};
```

### 步骤 2：定义 positionalCorrection() 函数

定义 `positionalCorrection()` 函数，按预定义的速率 `mPosCorrectionRate` 移动并减少物体间的重叠。为了在模拟中正确支持物体动量，每个物体移动的量与其质量成反比。也就是说，发生碰撞时，质量较大的物体移动的量将小于质量较小的物体。请注意，移动方向沿着 `collisionInfo` 对象中定义的碰撞法线。

```javascript
function positionalCorrection(s1, s2, collisionInfo) {
    if (!mCorrectPosition)
        return;
    let s1InvMass = s1.getInvMass();
    let s2InvMass = s2.getInvMass();
    let num = collisionInfo.getDepth() /
              (s1InvMass + s2InvMass) * mPosCorrectionRate;
    let correctionAmount = [0, 0];
    vec2.scale(correctionAmount, collisionInfo.getNormal(), num);
    s1.adjustPositionBy(correctionAmount, -s1InvMass);
    s2.adjustPositionBy(correctionAmount, s2InvMass);
}
```

### 步骤 3：修改 collideShape() 函数

修改 `collideShape()` 函数，以便在检测到碰撞时执行位置修正。请注意，仅当至少有一个物体的质量非零时，才执行碰撞检测。

```javascript
function collideShape(s1, s2, infoSet = null) {
    // ... 与之前代码相同 ...
    if ((s1 !== s2) &&
        ((s1.getInvMass() !== 0) || (s2.getInvMass() !== 0))) {
        if (s1.boundTest(s2)) {
            hasCollision = s1.collisionTest(s2, mCInfo);
            if (hasCollision) {
                vec2.subtract(mS1toS2, s2.getCenter(), s1.getCenter());
                if (vec2.dot(mS1toS2, mCInfo.getNormal()) < 0)
                    mCInfo.changeDir();
                positionalCorrection(s1, s2, mCInfo);
                // ... 与之前代码相同 ...
            }
        }
    }
    return hasCollision;
}
```

### 步骤 4：在工具函数中集成松弛循环

在所有三个工具函数 `processObjToSet()`、`processSetToSet()` 和 `processSet()` 中集成一个循环，以便在执行位置修正时运行松弛迭代：

```javascript
function processObjToSet(obj, set, infoSet = null) {
    let j = 0, r = 0;
    let hasCollision = false;
    let s1 = obj.getRigidBody();
    for (r = 0; r < mRelaxationCount; r++) {
        for (j = 0; j < set.size(); j++) {
            let s2 = set.getObjectAt(j).getRigidBody();
            hasCollision = collideShape(s1, s2, infoSet) || hasCollision;
        }
    }
    return hasCollision;
}

function processSetToSet(set1, set2, infoSet = null) {
    let i = 0, j = 0, r = 0;
    let hasCollision = false;
    for (r = 0; r < mRelaxationCount; r++) {
        // ... 与之前代码相同 ...
    }
    return hasCollision;
}

// 碰撞 GameObjectSet 中所有物体与其自身
function processSet(set, infoSet = null) {
    let i = 0, j = 0, r = 0;
    let hasCollision = false;
    for (r = 0; r < mRelaxationCount; r++) {
        // ... 与之前代码相同 ...
    }
    return hasCollision;
}
```

## 在 MyGame 中测试位置修正

