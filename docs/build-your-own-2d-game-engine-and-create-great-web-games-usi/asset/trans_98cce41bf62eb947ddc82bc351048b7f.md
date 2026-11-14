

```javascript
function collideShape(s1, s2, infoSet = null) {
    let hasCollision = false;
    if ((s1 !== s2) &&
        ((s1.getInvMass() !== 0) || (s2.getInvMass() !== 0))) {
        if (s1.boundTest(s2)) {
            hasCollision = s1.collisionTest(s2, mCInfo);
            if (hasCollision) {
                ... identical to previous code ...
                positionalCorrection(s1, s2, mCInfo);
                **resolveCollision(s1, s2, mCInfo);**
                ... identical to previous code ... };
```

### 更新 MyGame 以测试碰撞解析

对 `MyGame` 类的修改微不足道，主要是将运动和位置修正默认设为激活状态。此外，创建的 `RigidShape` 对象的初始随机旋转被禁用，因为此时碰撞响应不支持旋转。一如既往，您可以参考 `src/my_game` 文件夹中的源代码文件以获取实现细节。

### 观察

您应该通过三种方式测试您的实现。首先，确保移动的形状发生碰撞并表现自然。其次，尝试更改对象的物理属性。第三，观察运动形状与具有无限质量的静止形状（周围的墙壁和静止平台）之间的碰撞解析。请记住，仅考虑线性速度，碰撞不会产生旋转。

现在，运行项目，注意到形状逐渐落向平台和地板，在轻微反弹后运动停止。这清楚地表明 `Euler Integration`（欧拉积分）、碰撞检测、位置修正和解析的基本情况都在按预期运行。按 `H` 键激发所有形状，按 `C` 键显示碰撞信息。注意游荡的形状与墙壁/平台通过柔和的弹跳正常交互，且没有明显的相互渗透。

使用左/右箭头键选择一个对象，并使用 `N`/`F` 和上/下箭头键调整其恢复系数/摩擦系数。例如，将恢复系数调整为 1，摩擦系数调整为 0。现在按 `H` 键注入速度。注意对象看起来弹性十足，且由于摩擦系数为 0，似乎在平台/地板上滑行。您可以尝试不同的系数设置，观察相应的弹性和滑腻程度。

可以通过按 `G` 键增加场景中的形状数量来测试系统的稳定性。15 次松弛循环在每次迭代中持续且逐步地将相互渗透的形状分开。例如，您可以使用 `V` 和 `P` 键关闭运动和位置修正，并创建多个（例如 10 到 20 个）重叠的形状。现在开启运动和位置修正，观察一个正常运行的系统。

在下一个项目中，您将改进解析方案，以考虑碰撞产生的角速度变化。

## 碰撞响应的角度分量

现在您已经有了具体的理解，并成功实现了用于线性速度碰撞响应的 `Impulse Method`（冲量法），是时候集成对更一般旋转情况的支持了。在讨论细节之前，将牛顿线性力学与旋转力学的对应关系联系起来会很有帮助。即，线性位移对应旋转，速度对应角速度，力对应扭矩，质量对应转动惯量或角质量。转动惯量决定了绕旋转轴产生所需角加速度所需的扭矩。

以下讨论侧重于在 `Impulse Method`（冲量法）公式中集成旋转，并不试图对旋转的牛顿力学进行回顾。方便的是，将正确的旋转集成到 `Impulse Method`（冲量法）中不涉及任何新算法的推导。所需的只是在适当考虑旋转属性的情况下制定冲量响应。

### 考虑旋转的碰撞

将旋转集成到 `Impulse Method`（冲量法）公式中的关键在于认识到这样一个事实：您一直在使用的线性速度，例如物体 A 的速度 ![$$ {\overrightarrow{\mathrm{V}}}_{\mathrm{A}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq69.png)，实际上是形状在其中心位置的速度。在没有旋转的情况下，该速度在整个物体中是恒定的，可应用于任何位置。然而，如图 [9-28](#334805_2_En_9_Chapter.xhtml#Fig28) 所示，当物体的运动包含角速度 ![$$ {\overrightarrow{\upomega}}_{\mathrm{A}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq70.png) 时，其在位置 P 处的线性速度 ![$$ {\overrightarrow{\mathrm{V}}}_{AP1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq71.png) 实际上是该点与形状旋转中心之间的相对位置或位置矢量 ![$$ {\overrightarrow{\mathrm{R}}}_{AP} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq72.png) 的函数。

*   ![$$ {\overrightarrow{\mathrm{V}}}_{AP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+\left({\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq73.png)

![图 9-28 ¶ 存在旋转时某位置的线性速度](images/334805_2_En_9_Chapter/334805_2_En_9_Fig28_HTML.png)

**图 9-28 ¶ 存在旋转时某位置的线性速度**

> **注意**
>
> 角速度是一个与线性速度垂直的矢量。在这种情况下，由于线性速度定义在 X/Y 平面上，![$$ \overrightarrow{\upomega} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq74.png) 是 z 方向的矢量。回顾本章“简介”部分的讨论，最初的假设是刚体形状对象是具有均匀分布质量的连续几何体，其质心位于几何形状的中心。这个质心是旋转轴的位置。为简单起见，在您的实现中，![$$ \overrightarrow{\upomega} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq75.png) 将作为一个简单的标量存储，表示矢量的 z 分量大小。

图 [9-29](#334805_2_En_9_Chapter.xhtml#Fig29) 展示了一个具有线速度 ![$$ {\overrightarrow{\mathrm{V}}}_{\mathrm{B}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq76.png) 和角速度 ![$$ {\overrightarrow{\upomega}}_{\mathrm{B}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq77.png) 的物体 B 在位置 P 处与物体 A 发生碰撞。到目前为止，您知道碰撞前两个物体在点 P 处的线速度如下：

*   ![$$ {\overrightarrow{\mathrm{V}}}_{AP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+\left({\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq78.png) **(9)**
*   ![$$ {\overrightarrow{\mathrm{V}}}_{BP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{B}1}+\left({\overrightarrow{\upomega}}_{\mathrm{B}1}\times {\overrightarrow{\mathrm{R}}}_{BP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq79.png) **(10)**

![图 9-29 ¶ 具有角速度的碰撞形状](images/334805_2_En_9_Chapter/334805_2_En_9_Fig29_HTML.png)

**图 9-29 ¶ 具有角速度的碰撞形状**

碰撞后，碰撞位置的线速度可以表示如下：

