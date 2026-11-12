# 第 4 章 数学

数学是使用像 Unity 这样的空间开发工具时不可避免的一部分。无论你喜欢还是讨厌它，你都需要使用它、承认它，甚至彻底拥抱它，才能完成任何工作。幸运的是，你可以跳过或假装大部分数学并不存在，但它始终存在，支持着你，潜伏在下方。

本章探讨了使用 Unity 开发时会遇到的最常见的数学相关问题。

> **注意**
>
> 这并不是游戏或 Unity 开发所需的全部数学；远非如此！但这足以帮助你入门。我们尽力围绕你可能想使用数学解决的问题来组织内容，而不是数学本身。

你是否想过向量是什么？或者如何使用矩阵？四元数又如何？我们将在这里介绍它们，同时也会涉及 Unity 开发中的常见数学问题，例如计算你 Unity 项目中到其他物体的角度和距离。

> **提示**
>
> 本章中没有任何内容是 Unity 开发所特有的。当我们讨论代码时，确实会从 Unity 为中心的角度来介绍，但这里的一切都是适用于空间或游戏开发的普通数学。

继续阅读，了解 Unity 开发所需的刚好足够的数学。

我们强烈推荐 Paco Nathan 的 *Just Enough Math* 视频系列，同样来自 O'Reilly。如果你想学习更多数学，请查看。

## 4.1 使用向量存储不同维度的坐标

### 问题

Unity 开发涉及大量不同维度的坐标：

- 一维坐标，例如 1 或 -9
- 二维坐标，例如 8, 3 或 -1, 4
- 三维坐标，例如 3, 1, -3 或 -7, 6, 1

实际上，在 Unity 中构建东西时，只要你跟踪需要跟踪的内容，通常不需要在意具体怎么做。然而，跟踪坐标的一种常见且有用的方法是使用向量。

向量是一组不同维度的坐标。因此，我们这里的问题是：如何在 Unity 中使用向量，以及可以用它们做什么？

### 解决方案

为了在此提供解决方案，我们需要稍微展开一下问题。我们承认，我们故意编写了一个非常宽泛的问题陈述，但这样做是为了向你展示有关向量的所有所需知识，而无需为你可能想对向量进行的每个操作或动作编写一个微小的配方。

> **提示**
>
> 你经常会看到向量被赋予非常具体的含义：在视频游戏中，它通常是几何中的坐标，但绝对没有理由不能在其中存储你想要的任何内容。它们只是一种数据结构。

首先，在 Unity 中，你可以定义一个 Vector2：一个具有两个维度的向量，通常是 x 和 y。Vector2 通常用于表示 Unity 中 2D 空间里的一个点。

你可以定义一个具有两个维度的 Vector2：

```
Vector2 direction = new Vector2(0.0f, 2.0f);
```

或使用 Unity 的内置向量之一：

```
var up = Vector2.up; // ( 0, 1)
var down = Vector2.down; // ( 0, -1)
var left = Vector2.left; // (-1, 0)
var right = Vector2.right; // ( 1, 0)
var one = Vector2.one; // ( 1, 1)
var zero = Vector2.zero; // ( 0, 0)
```

Unity 还有一种称为 Vector3 的类型，它是一个具有三个维度的向量。Vector3 类中有几个预定义的向量可用：

```
Vector3 point = new Vector3(1.0f, 2f, 3.5f);
```

```
var up = Vector3.up; // ( 0, 1, 0)
var down = Vector3.down; // ( 0, -1, 0)
var left = Vector3.left; // (-1, 0, 0)
var right = Vector3.right; // ( 1, 0, 0)
var forward = Vector3.forward; // ( 0, 0, 1)
var back = Vector3.back; // ( 0, 0, -1)
var one = Vector3.one; // ( 1, 1, 1)
var zero = Vector3.zero; // ( 0, 0, 0)
```

> **提示**
>
> 可以在 Unity 的 API 文档中了解有关 Vector2 和 Vector3 的更多信息。

Unity 中的每个 `Transform` 组件都有定义的局部方向向量，这些向量相对于其当前旋转。例如，可以通过以下方式访问对象的局部前向方向：

```
var myForward = transform.forward;
```

当然，你可以对向量进行基本算术运算。向量可以相加：

```
var v1 = new Vector3(1f, 2f, 3f);
var v2 = new Vector3(0f, 1f, 6f);

var v3 = v1 + v2; // (1, 3, 9)
```

也可以相互相减：

```
var v4 = v2 - v1; // (-1, -1, 3)
```

你还可以获取向量的 *magnitude*（大小）。大小也称为向量的 *length*（长度），向量的大小是从原点 (0, 0, 0) 到向量的直线距离。向量的大小是各分量平方和的平方根。例如，向量 (0, 2, 0) 的大小为 2；向量 (0, 1, 1) 的大小约为 1.41（即 2 的平方根）：

```
var forwardMagnitude = Vector3.forward.magnitude; // = 1

var vectorMagnitude =
    new Vector3(2f, 5f, 3f).magnitude; // ~= 6.16
```

> **注意**
>
> 大小为 1 的向量称为单位向量。

然后可以使用大小进行其他计算。例如，要计算两个向量之间的距离，可以用一个向量减去另一个向量，并计算结果的大小：

```
var point1 = new Vector3(5f, 1f, 0f);
var point2 = new Vector3(7f, 0f, 2f);
var distance = (point2 - point1).magnitude; // = 3
```

内置方法 Distance 会为你执行相同的计算：

```
Vector3.Distance(point1, point2);
```

计算向量的大小需要开平方根。但是，有些情况下你不需要向量大小的实际值，只想比较两个长度。在这种情况下，可以跳过开平方根，直接使用大小的平方。这样做会快一些，而我们非常关注快速计算……尤其是在游戏开发中。

要获取此值，请使用 sqrMagnitude 属性：

```
var distanceSquared = (point2 - point1).sqrMagnitude; // = 9
```

许多操作在大小为 1 的向量上效果最好。大小为 1 的向量也称为单位向量，因为其大小是一个单位（即 1）。你可以取一个向量，通过将其除以自身的大小来生成一个新向量，该新向量方向相同但大小为 1。这称为向量归一化：

```
var bigVector = new Vector3(4, 7, 9); // magnitude = 12.08
var unitVector =
bigVector / bigVector.magnitude; // magnitude = 1
```

这是一个常见操作，因此你可以通过使用 *normalized* 属性直接访问向量的归一化版本：

```
var unitVector2 = bigVector.normalized;
```

向量还可以进行缩放。当你将向量乘以一个单个数字（*标量*）时，结果是一个向量，其中源向量的每个分量都乘以该数字：

```
var v1 = Vector3.one * 4; // = (4, 4, 4)
```

你还可以使用 `Scale` 方法进行按分量缩放。该方法接受两个向量并生成第三个向量，其中第一个向量的每个分量乘以第二个向量对应的分量——即，给定两个向量 A 和 B，A.Scale(B) 的结果为 (A.x * B.x, A.y * B.y, A.z * B.z)：

```
v1.Scale(new Vector3(3f, 1f, 0f)); // = (12f, 4f, 0f)
```

你还可以获取两个向量的 *dot product*（点积），它告诉你它们所指方向之间的差异。

点积定义为两个向量对应分量乘积之和。即，给定两个三维向量 A 和 B，A•B = sum(A.x * B.x, A.y * B.y, A.z * B.z)。

可以使用点积来确定两个向量的相似度。两个方向相同的向量之间的点积为 1：

```
var parallel = Vector3.Dot(Vector3.left, Vector3.left); // 1
```

两个方向相反的向量之间的点积为 -1：

```
var opposite = Vector3.Dot(Vector3.left, Vector3.right); // - 1
```

两个互相垂直的向量之间的点积为 0：

```
var orthogonal = Vector3.Dot(Vector3.up, Vector3.forward); // 0
```

作为一个令人愉快的副作用，两个向量之间的点积也是这两个向量之间夹角的余弦。这意味着，给定两个向量之间的点积，你可以通过取其反余弦来计算这两个向量之间的夹角：

```
var orthoAngle = Mathf.Acos(orthogonal);
var orthoAngleDegrees = orthoAngle * Mathf.Rad2Deg; // = 90
```

> **注意**
>
> Mathf.Acos 方法返回以弧度为单位的值。要将其转换为角度，可以将其乘以 Mathf.Rad2Deg 常量。

点积是判断一个对象在另一个对象前方还是后方的好方法。

要判断一个对象是否在另一个对象前方，我们首先需要确定“前方”是什么意思。在 Unity 中，局部 z 轴表示前向方向，可以通过对象 Transform 的 forward 属性访问它。

我们可以通过用第一个对象的位置减去第二个对象的位置来生成一个表示从第一个对象指向第二个对象的方向向量。然后，我们可以对该向量与第一个对象的前向方向进行点积运算。

我们现在可以利用对点积的了解来判断第二个对象是否在第一个对象前方。请回顾，两个方向相同的向量的点积为 1。如果第二个对象正位于第一个对象前方，那么指向该对象的方向将相同，这意味着两个向量的点积将为 1。如果是 0，那么该



物体与前进方向呈直角。如果是 -1，则表示该物体正位于对象正后方，因为它与前进方向完全相反：

```
var directionToOtherObject =
   someOtherObjectPosition - transform.position;
var differenceFromMyForwardDirection =
   Vector3.Dot(transform.forward, directionToOtherObject);

if (differenceFromMyForwardDirection > 0) {
  // The object is in front of us
} else if (differenceFromMyForwardDirection < 0) {
  // The object is behind us
} else {
  // The object neither before or behind us; it's at a perfect right
  // angle to our forward direction.
}
```

> **提示**
>
> 叉积仅定义于三维向量。

你还可以从两个向量中得到一个新向量，沿某个大小从一个向量移动到另一个。这对于防止过冲 特别有用。这里我们从 (0, 0, 0) 移动到 (1, 1, 1)，且移动距离不超过 0.5 个单位：

```
var moved =
   Vector3.MoveTowards(Vector3.zero, Vector3.one, 0.5f);
// = (0.3, 0.3, 0.3) (a vector that has a magnitude of 0.5)
```

或者沿法线定义的平面进行反射：

```
var v = Vector3.Reflect(new Vector3(0.5f, -1f, 0f), Vector3.up); // = (0.5, 1, 0)
```

你还可以对两个输入向量进行 *线性插值*，或 *lerp*，前提是给定一个 0 到 1 之间的数字。如果给定 0，你将得到第一个向量；如果给定 1，你将得到第二个向量；如果给定 0.5，你将得到两者中间的某个位置：

```
var lerped = Vector3.Lerp(Vector3.zero, Vector3.one, 0.65f); // = (0.65, 0.65, 0.65)
```

如果指定的数字不在 0 到 1 的范围内，lerp 会将其限制在 0 到 1 之间。你可以通过使用 LerpUnclamped 来防止这种情况：

```
var unclamped = Vector3.LerpUnclamped(Vector3.zero, Vector3.right, 2.0f); // = (2, 0, 0)
```

### 讨论

这只是在 Unity 中使用向量的一个小小示例。Unity 为你对向量执行的数学运算可以大大简化很多事情。例如，你可以使用点积来判断一个点是在角色前方还是后方，或者创建一个雷达来确定敌人的位置。

向量还能让缩放或旋转等复杂操作变得非常简单直接。你不需要手动计算每个对象及其与其他对象的关系，只需使用向量数学即可。

总的来说，向量让你能够用比原本所需的更简洁的代码来处理几何相关问题。它们是你游戏开发工具箱中绝佳的数学工具！

## 4.2 在 3D 空间中旋转

### 问题

你想在 3D 空间中旋转物体。

### 解决方案

要在 3D 空间中旋转，你需要使用 *四元数*，这是一种数学结构，对于表示 3D 空间中的旋转非常有用。四元数可以表示绕任意轴旋转任意角度。

> 四元数可能是一个棘手的东西，因为从纯数学角度来说，它们是四维数字。
> 但就游戏开发而言，它们本质上只是一个旋转，即使你不完全理解四元数 *为什么* 能起作用，也没关系。

```
var rotation = Quaternion.Euler(90, 0, 0);
```

然后使用它来围绕原点旋转一个点：

```
var input = new Vector3(0, 0, 1);

var result = rotation * input;
// = (0, -1, 0)
```

存在一个单位四元数，它不表示任何旋转：

```
var identity = Quaternion.identity;
```

你可以使用 Slerp 方法对两个旋转进行 *插值*（即混合），它能够平滑地在旋转之间移动，使得每一步的角度变化保持恒定。这比线性角度插值更好，因为在线性插值中，角度变化率不恒定：

```
var rotationX = Quaternion.Euler(90, 0, 0);
var halfwayRotated = Quaternion.Slerp(identity, rotationX, 0.5f);
```

> **提示**
>
> Slerp 是 *球面线性插值* 的缩写。

你还可以组合四元数。例如，要先绕 y 轴旋转，再绕 x 轴旋转，需要将它们相乘（应用顺序是相反的）：

```
var combinedRotation =
Quaternion.Euler(90, 0, 0) * // rotate around x
Quaternion.Euler(0, 90, 0); // rotate around y
```

> **注意**
>
> 此组合不满足交换律：乘法顺序很重要！先按 *x* 旋转再按 *y* 旋转，与先按 *y* 旋转再按 *x* 旋转并不相同。

### 讨论

表示 3D 空间中旋转的另一种方法是使用 *欧拉角* ——即分别存储绕 x 轴、y 轴和 z 轴的旋转。欧拉角易于理解，在代码中表达旋转时通常使用它们。

然而，这种方法容易出现一个称为 *万向锁* 的问题，当物体旋转导致两个旋转轴平行时就会发生这种情况。此时，物体将失去一个自由度。四元数不存在这个问题，它们可以从任意方向朝任何方向旋转。

避免万向锁的替代方法是使用表示旋转的矩阵（参见配方 4.3）。但是，一个 4×4 旋转矩阵包含 16 个数字，而四元数只有 4 个，这意味着为了获得相同的结果，四元数所占空间比矩阵小。

## 4.3 使用矩阵在 3D 空间中执行变换

### 问题

你想在一个结构中表示一整套变换——即移动、旋转和缩放。

### 解决方案

你可以使用 _矩阵_ 来表示一个完整的变换。矩阵只是一个数字网格（公式 4-1）：

```
var matrix = new Matrix4x4();
```

_公式 4-1. 一个 4×4 矩阵_

| 1 | 0 | 0 | 0 |
|---|---|---|---|
| 0 | 1 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 5 | 0 | 0 | 1 |

你可以设置和获取网格中每个位置的值：

```
var m00 = matrix[0, 0];
```

```
matrix[0, 1] = 2f;
```

你可以将矩阵与向量相乘以生成新向量。根据矩阵内部的值，这会导致原始向量被移动、缩放和旋转。你还可以执行更复杂的操作，例如剪切或应用透视投影。

你可以将两个矩阵相乘以生成第三个矩阵。当你用这个新矩阵乘以向量时，结果与分别用每个原始矩阵按顺序乘以向量的结果相同。

> **提示**
>
> 计算机图形学，因此也是游戏开发，通常使用 4×4 矩阵，因为它们可以用于执行广泛的常见变换。

现在我们将创建一个沿 x 轴将向量移动（平移）5 个单位的矩阵。首先，我们将使用四个 Vector4（四维向量）定义一个新矩阵：

```
var translationMatrix = new Matrix4x4(
    new Vector4(1, 0, 0, 5),
    new Vector4(0, 1, 0, 0),
    new Vector4(0, 0, 1, 0),
    new Vector4(1, 0, 0, 1)
);
```

> **注意**
>
> 我们用来创建矩阵的每个 Vector4 代表一列，而不是一行。

这段代码创建的矩阵如公式 4-1 所示。

> **注意**
>
> 当我们将三维向量与矩阵相乘时，会在向量末尾添加 1，从而形成一个四维向量。这个额外的分量通常称为 w 分量。

将此矩阵与一个四维向量 V 相乘，得到以下结果：

```
1*Vx + 0*Vy + 0*Vz + 5*Vw = resultX
0*Vx + 1*Vy + 0*Vz + 0*Vw = resultY
0*Vx + 0*Vy + 1*Vz + 0*Vw = resultZ
0*Vx + 0*Vy + 0*Vz + 1*Vw = resultW
```

例如，要将点 (0, 1, 2)（一个 Vector3）与此矩阵相乘：

1. 我们首先添加 w 分量：

```
Vx = 0, Vy = 1, Vz = 2, Vw = 1

1*0 + 0*1 + 0*2 + 5*1 = 5
0*0 + 1*1 + 0*2 + 0*1 = 1
0*0 + 0*1 + 1*2 + 0*1 = 2
0*0 + 0*1 + 0*2 + 1*1 = 1
```

2. 然后我们丢弃第四个分量，得到结果。因此，我们的最终结果是向量 (5, 1, 2)。

不过，Unity 不会让我们亲自完成所有这些工作，它在 Matrix4x4 类型中提供了 MultiplyPoint 方法：

```
var input = new Vector3(0, 1, 2);

var result = translationMatrix.MultiplyPoint(input);
// = (5, 1, 2)
```





## 注意

你可能会好奇，矩阵为什么还要有第四行，因为这只意味着我们需要给向量添加并移除一个没用的第四个分量。它的存在是为了支持透视投影等操作。如果你只做平移、旋转和缩放等变换，只使用矩阵的一部分即可，可以改用 Matrix4x4 的 MultiplyPoint4x3 函数。这会快一些，但只能用于平移、旋转和缩放，不能用于其他任何任务。

Unity 还提供了使用矩阵转换点的辅助方法：

```
var input = new Vector3(0, 1, 2);

var translationMatrix = Matrix4x4.Translate(new Vector3(5, 1, -2));

var result = translationMatrix.MultiplyPoint(input);
// = (5, 2, 0)
```

你还可以使用矩阵和四元数绕原点旋转点：

```
var rotate90DegreesAroundX = Quaternion.Euler(90, 0, 0);

var rotationMatrix = Matrix4x4.Rotate(rotate90DegreesAroundX);

var input = new Vector3(0, 0, 1);

var result = rotationMatrix.MultiplyPoint(input);
```

在此例中，点已从原点前方移至下方，得到的点为 (0, -1, 0)。

如果你的向量表示一个方向，并且想使用矩阵旋转该向量，可以使用 MultiplyVector。此方法仅使用执行旋转所需的矩阵部分，因此速度会快一些：

```
result = rotationMatrix.MultiplyVector(input);
// = (0, -1, 0) - the same result.
```

你还可以使用矩阵将点从原点处缩放：

```
var scale2x2x2 = Matrix4x4.Scale(new Vector3(2f, 2f, 2f));

var input = new Vector3(1f, 2f, 3f);

var result = scale2x2x2.MultiplyPoint3x4(input);
// = (2, 4, 6)
```

将矩阵相乘会得到一个新矩阵，当它与向量相乘时，产生的结果与将向量按顺序分别乘以每个原始矩阵的结果相同。换句话说，如果你把矩阵视为修改点的指令，就可以将多个矩阵合并为一个步骤。

提示

当我们像这样组合矩阵时，称之为串联矩阵。

在此示例中，我们串联矩阵：

```
var translation = Matrix4x4.Translate(new Vector3(5, 0, 0));
var rotation = Matrix4x4.Rotate(Quaternion.Euler(90, 0, 0));
var scale = Matrix4x4.Scale(new Vector3(1, 5, 1));

var combined = translation * rotation * scale;

var input = new Vector3(1, 1, 1);
var result = combined.MultiplyPoint(input);
Debug.Log(result);
// = (6, 1, 5)
```

与四元数一样，乘法顺序很重要！矩阵乘法不具备*交换性*，而常规数字乘法则具备。例如，2 * 5 与 5 * 2 是同一个计算：两个计算的结果都是数字 10。

但是，先移动对象再旋转它，与先旋转对象再移动它，产生的结果不同。同样，如果将平移点的矩阵与旋转点的矩阵按相反顺序组合，结果也不会相同。

使用乘法组合矩阵时，会按照乘法顺序的逆序应用它们。给定点 P 和矩阵 A、B、C：

P * (A * B * C) == (A * (B * (C * P)))

你可以使用 Matrix4x4.TRS 方法创建组合的平移-旋转-缩放矩阵：

```
var transformMatrix = Matrix4x4.TRS(
new Vector3(5, 0, 0),
Quaternion.Euler(90, 0, 0),
new Vector3(1, 5, 1)
);
```

这个新矩阵会对应用到的任何点先进行缩放，再旋转，最后平移。

你还可以获取一个矩阵，用于将组件在局部空间中的位置转换为世界空间，这意味着获取局部位置，并应用此对象及其所有父对象的局部平移、旋转和缩放：

```
var localToWorld = this.transform.localToWorldMatrix;
```

你还可以获取执行逆运算的矩阵，即从世界空间转换为局部空间：

```
var worldToLocal = this.transform.worldToLocalMatrix;
```

呼。用矩阵可以做很多事情！

### 讨论

Unity 内部使用矩阵表示变换。为简单起见，你大部分时间都直接访问位置、旋转和缩放。不过，了解底层工作原理始终是件好事。

## 4.4 使用角度

### 问题

你想处理向量之间的角度。

### 解决方案

在 Unity 中，大多数以欧拉角表示的旋转都是以角度给出的。

我们可以使用角度旋转物体，使用 Transform 类的 Rotate 方法：

```
// Rotate 90 degrees - one quarter circle - around the X axis
transform.Rotate(90, 0, 0);
```

> 提示
> 
> 一个圆有 360 度，一个圆有 2π 弧度。它们只是角度的不同计量单位。

角度对大多数人来说更熟悉，但弧度通常更易于计算。这就是 Unity 的某些部分（尤其是与数学相关的部分）期望使用弧度的原因。一个圆有 2π 弧度：

```
// The sine of pi radians (one half-circle) is zero
Mathf.Sin(Mathf.PI); // = 0
```

你可以这样将弧度转换为角度，或反之：

```
// Converting 90 degrees to radians
var radians = 90 * Mathf.Deg2Rad; // ~= 1.57 (π / 2)

// Converting 2π radians to degrees
var degrees = 2 * Mathf.PI * Mathf.Rad2Deg; // = 360
```

### 讨论

两个单位向量的点积等于它们之间夹角的余弦值。如果你拥有一个角度的余弦值，可以通过取其反余弦来得到原始角度。这意味着你可以这样求两个向量之间的角度：

```
var angle = Mathf.Acos(Vector3.Dot(Vector3.up, Vector3.left));
```

其结果为 π 弧度；如果要向用户显示，应先将其转换为角度。一个圆有 2π 弧度，而一个圆有 360 度；因此，将数值从弧度转换为角度时，需要乘以 180/π。例如，π/2 弧度转换为角度等于 (π/2) * (180/π) = 90。从角度转换为弧度则是相反的过程：乘以 π/180。例如，45 度转换为弧度等于 45 * (π/180) = π/4。

你可以在代码中使用 Mathf.Deg2Rad 和 Mathf.Rad2Deg 常量来简化此操作。如果将以弧度表示的角度乘以 Mathf.Rad2Deg，将得到角度结果；如果将以角度表示的角度乘以 Mathf.Deg2Rad，将得到弧度结果。

## 4.5 查找到目标的距离

### 问题

你想检查一个对象是否在另一个对象的一定范围内。

### 解决方案

你需要为需要知道另一个对象何时进入其范围的对象创建并添加脚本：

- 1. 创建一个名为 RangeChecker.cs 的新 C# 脚本，并向其中添加以下代码：

```
public class RangeChecker : MonoBehaviour {
    // The object we want to check the distance to
    [SerializeField] Transform target;

    // If the target is within this many units of us, it's in range
    [SerializeField] float range = 5;

    // Remembers if the target was in range on the previous frame.
    private bool targetWasInRange = false;

    void Update() {
        // Calculate the distance between the objects
        var distance = (target.position - transform.position).magnitude;

        if (distance <= range && targetWasInRange == false) {
            // If the object is now in range, and wasn't before, log it
            Debug.LogFormat("Target {0} entered range!", target.name);

            // Remember that it's in range for next frame
            targetWasInRange = true;
        } else if (distance > range && targetWasInRange == true) {
            // If the object is not in range, but was before, log it
            Debug.LogFormat("Target {0} exited range!", target.name);

            // Remember that it's no longer in range for next frame
            targetWasInRange = false;
        }
    }
}
```

- 2. 将此脚本附加到任何对象，并将任何其他对象附加到脚本的 Target 字段，脚本将检测目标进入和退出指定范围的时刻。

### 讨论

如果将此配方与配方 4.6 结合，你可以很容易地构建一种行为，让对象只能“看到”其前方的附近对象。此脚本的更高级版本可见于配方 10.1。

## 4.6 查找到目标的角度

### 问题

你想求两个对象之间的角度。

### 解决方案

你需要为需要知道自身与另一个对象之间夹角的对象创建并添加脚本：

- 1. 创建一个名为 AngleChecker.cs 的新 C# 脚本，并向其中添加以下代码：

```
public class AngleChecker : MonoBehaviour {

// The object we want to find the angle to
[SerializeField] Transform target;
```




```csharp
void Update () {
    // Get the normalized direction to the target
    var directionToTarget =
        (target.position - transform.position).normalized;

    // Take the dot product between that direction and our forward
    // direction
    var dotProduct = Vector3.Dot(transform.forward,
        directionToTarget);

    // Get the angle
    var angle = Mathf.Acos(dotProduct);

    // Log the angle, limiting it to 1 decimal place
    Debug.LogFormat(
        "The angle between my forward direction and {0} is {1:F1}°",
        target.name, angle * Mathf.Rad2Deg
    );
}
```

- 2. 将此脚本附加到任何对象，并将任何其他对象附加到脚本的 Target 字段，该脚本将记录对象的前向方向与目标对象之间的角度（以度为单位）。

## 讨论

“两个对象之间的角度”这一概念取决于你选择至少一个方向。你无法获取空间中两个点之间的角度，因为它们之间存在无限个可能的角度。相反，你需要选择相对于第一个对象的方向，并将其与指向第二个对象的方向进行比较。
