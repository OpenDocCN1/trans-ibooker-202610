
# gl-matrix 中的向量运算

## 概述

在 `gl-matrix` 库中，`vec2` 对象实现了二维向量的功能。方便的是，你也可以使用 `vec2` 对象来表示空间中的二维点或位置。在前面的示例中，*P*[*a*]、*P*[*b*] 和 ![$$ {\overset{\rightharpoonup }{V}}_{ab} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq6.png) 都可以作为 `vec2` 对象的实例来实现。然而，![$$ {\overset{\rightharpoonup }{V}}_{ab} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq7.png) 才是数学上定义的向量。*P*[*a*] 和 *P*[*b*] 代表用于创建向量的位置或点。

回顾一下，向量也可以被归一化。*归一化* 向量（也称为*单位向量*）的大小始终为 1。你可以通过以下函数看到归一化向量，如图 [6-4](#334805_2_En_6_Chapter.xhtml#Fig4) 所示。注意，普通向量的数学符号是 ![$$ {\overset{\rightharpoonup }{V}}_a $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq8.png)，而归一化向量的符号是 ![$$ {\hat{V}}_a $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq9.png)：

*   ![$$ vec2. normalized\left({\overset{\rightharpoonup }{V}}_a\ \right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq10.png): 归一化向量 ![$$ {\overset{\rightharpoonup }{V}}_a $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq11.png) 并将结果存储到 `vec2` 对象中

![Figure 6-4 ¶ A vector being normalized](images/334805_2_En_6_Chapter/334805_2_En_6_Fig4_HTML.png)

图 6-4 ¶ 向量归一化

指向位置的向量也可以被旋转。例如，如果向量 ![$$ \overset{\rightharpoonup }{V}=\left({x}_v,{y}_v\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq12.png) 表示从原点指向位置 (*x*[*v*], *y*[*v*]) 的方向，并且你想将其旋转 θ 角度，则如图 [6-5](#334805_2_En_6_Chapter.xhtml#Fig5) 所示，你可以使用以下公式来求出 *x*[*r*] 和 *y*[*r*]：

*   *x*[*r*] = *x*[*v*] cos *θ* − *y*[*v*] sin *θ*
*   *y*[*r*] = *x*[*v*] sin *θ* + *y*[*v*] cos *θ*

![Figure 6-5 ¶ A vector from the origin to the position (xv, yv) being rotated by the angle theta](images/334805_2_En_6_Chapter/334805_2_En_6_Fig5_HTML.png)

图 6-5 ¶ 从原点指向位置 (*x*[*v*], *y*[*v*]) 的向量被角度 theta 旋转

> **注意**
>
> JavaScript 三角函数，包括 `Math.sin()` 和 `Math.cos()` 函数，假设输入为弧度而非角度。回顾一下，1 度等于 ![$$ \frac{\uppi}{180} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq13.png) 弧度。

始终重要的是要记住，向量由其方向和大小定义。换句话说，两个向量可以相等，而与向量的位置无关。图 [6-6](#334805_2_En_6_Chapter.xhtml#Fig6) 显示了两个向量 ![$$ {\overset{\rightharpoonup }{V}}_a $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq14.png) 和 ![$$ {\overset{\rightharpoonup }{V}}_{bc} $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq15.png)，它们位于不同的位置，但具有相同的方向和大小，因此它们相等。相比之下，向量 ![$$ {\overrightarrow{V}}_d $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq16.png) 不同，因为它的方向和大小与其他向量不同。

![Figure 6-6 ¶ Three vectors represented in 2D space with two vectors equal to each other](images/334805_2_En_6_Chapter/334805_2_En_6_Fig6_HTML.png)

图 6-6 ¶ 二维空间中表示的三个向量，其中两个向量相等

## 点积

两个归一化向量的点积提供了求这两个向量夹角的方法。例如，给定以下内容：

*   ![$$ {\overset{\rightharpoonup }{V}}_1=\left({x}_1,{y}_1\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq17.png)
*   ![$$ {\overset{\rightharpoonup }{V}}_2=\left({x}_2,{y}_2\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq18.png)

则以下成立：

*   ![$$ {\overset{\rightharpoonup }{V}}_1\bullet {\overset{\rightharpoonup }{V}}_2={\overset{\rightharpoonup }{V}}_2\bullet {\overset{\rightharpoonup }{V}}_1={x}_1{x}_2+{y}_1{y}_2 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq19.png)。

此外，如果向量 ![$$ {\overset{\rightharpoonup }{V}}_1 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq20.png) 和 ![$$ {\overset{\rightharpoonup }{V}}_2 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq21.png) 都是归一化的，则

*   ![$$ {\hat{V}}_1\bullet {\hat{V}}_2=\cos \theta $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq22.png)

图 [6-7](#334805_2_En_6_Chapter.xhtml#Fig7) 描绘了 ![$$ {\overset{\rightharpoonup }{V}}_1 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq23.png) 和 ![$$ {\overset{\rightharpoonup }{V}}_2 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq24.png) 向量及其夹角 *θ* 的示例。同样重要的是要认识到，如果 ![$$ {\overset{\rightharpoonup }{V}}_1\bullet {\overset{\rightharpoonup }{V}}_2=0 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq25.png)，则这两个向量垂直。

![Figure 6-7 ¶ The angle between two vectors, which can be found through the dot product](images/334805_2_En_6_Chapter/334805_2_En_6_Fig7_HTML.png)

图 6-7 ¶ 两个向量之间的夹角，可通过点积求得

> **注意**
>
> 如果你需要复习或温习点积的概念，请参考 [www.mathsisfun.com/algebra/vectors-dot-product.html](http://www.mathsisfun.com/algebra/vectors-dot-product.html)。

## 叉积

两个向量的叉积产生一个与原始两个向量都 *正交*（即垂直）的向量。在二维游戏中，二维维度平铺在屏幕上，叉积的结果是一个指向内部（朝向屏幕）或外部（远离屏幕）的向量。这可能看起来很奇怪，因为在二维或 x/y 平面上叉乘两个向量会产生一个位于第三维度或 z 轴上的向量，这不符合直觉。然而，第三维度中的结果向量包含关键信息。例如，该向量在第三维度中的方向可用于确定游戏对象需要顺时针还是逆时针旋转。仔细看看以下内容：

*   ![$$ {\overset{\rightharpoonup }{V}}_1=\left({x}_1,{y}_1\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq26.png)
*   ![$$ {\overset{\rightharpoonup }{V}}_2=\left({x}_2,{y}_2\right) $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq27.png)

鉴于上述内容，以下成立：

*   ![$$ {\overset{\rightharpoonup }{V}}_3={\overset{\rightharpoonup }{V}}_1\times {\overset{\rightharpoonup }{V}}_2 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq28.png) 是一个同时垂直于 ![$$ {\overset{\rightharpoonup }{V}}_1 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq29.png) 和 ![$$ {\overset{\rightharpoonup }{V}}_2 $$](images/334805_2_En_6_Chapter/334805_2_En_6_Chapter_TeX_IEq30.png) 的向量。
