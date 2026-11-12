

## 第 4 章 数学

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

### 4.1 使用向量存储不同维度的坐标

#### 问题

Unity 开发涉及大量不同维度的坐标：

- 一维坐标，例如 1 或 -9
- 二维坐标，例如 8, 3 或 -1, 4
- 三维坐标，例如 3, 1, -3 或 -7, 6, 1

实际上，在 Unity 中构建东西时，只要你跟踪需要跟踪的内容，通常不需要在意具体怎么做。然而，跟踪坐标的一种常见且有用的方法是使用向量。

向量是一组不同维度的坐标。因此，我们这里的问题是：如何在 Unity 中使用向量，以及可以用它们做什么？

#### 解决方案

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

