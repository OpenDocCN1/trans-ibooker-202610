

= \(\begin{bmatrix} 1 & -1 \\ 1 & 0 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}\)

在 Python 中使用 NumPy 执行此操作时，可以直接使用 `matmul()` 或 `@` 运算符（示例 4-11）来组合这两个矩阵。然后，我们将使用这个合并后的变换，并将其应用于向量 `[1 2]`。

### 示例 4-11.

```python
from numpy import array

# Transformation 1
i_hat1 = array([0, 1])
j_hat1 = array([-1, 0])
transform1 = array([i_hat1, j_hat1]).transpose()

# Transformation 2
i_hat2 = array([1, 0])
j_hat2 = array([1, 1])
transform2 = array([i_hat2, j_hat2]).transpose()

# Combine Transformations
combined = transform2 @ transform1

# Test
print("COMBINED MATRIX:\n {}".format(combined))

v = array([1, 2])    # [-1 1]
print(combined.dot(v)) # [-1 1]
```

> # 使用 DOT() 还是 MATMUL() 和 @
> 
> 一般来说，在 NumPy 中组合矩阵时，应优先使用 `matmul()` 及其简写 `@`，而不是 `dot()` 运算符。前者在处理高维矩阵及元素广播方式方面通常具有更可取的策略。
> 
> 如果你喜欢深入研究这类实现细节，这个 [StackOverflow](https://stackoverflow.com/) 问题是一个很好的起点。

请注意，我们也可以分别对向量 \(\vec{v}\) 应用每个变换，仍然得到相同的结果。如果将最后一行替换为这三行（示例 4-12），对每个变换进行应用，你在该新向量上仍然会得到 [-1 1]。

### 示例 4-12.

```python
rotated = transform1.dot(v)
sheered = transform2.dot(rotated)
print(sheered) # [-1 1]
```

请注意，应用每个变换的顺序很重要！如果我们在 `transformation2` 上应用 `transformation1`，将得到与示例 4-13 中计算的不同结果 `[-2 3]`。因此，矩阵点积不具备交换性，这意味着你不能交换顺序并期望得到相同的结果！

### 示例 4-13.

```python
from numpy import array

# Transformation 1
i_hat1 = array([0, 1])
j_hat1 = array([-1, 0])
transform1 = array([i_hat1, j_hat1]).transpose()

# Transformation 2
i_hat2 = array([1, 0])
j_hat2 = array([1, 1])
transform2 = array([i_hat2, j_hat2]).transpose()

# Combine Transformations, apply shear first and then rotation
combined = transform1 @ transform2

# Test
print("COMBINED MATRIX:\n {}".format(combined))

v = array([1, 2])
print(combined.dot(v)) # [-2 3]
```

将每个变换视为一个函数，我们从内到外应用它们，就像嵌套函数调用一样。

## 实际中的线性变换

你可能会好奇，所有这些线性变换和矩阵与数据科学和机器学习有什么关系。答案是：一切都有关！从导入数据到线性回归、逻辑回归和神经网络中的数值运算，线性变换都是数学处理数据的核心。

不过，在实际操作中，你很少会花时间从几何上将数据可视化为向量空间和线性变换。你将要处理太多维度，无法高效地完成此操作。但了解几何解释仍然很有益，这样你才能明白这些看似巧妙的数值运算到底在做什么！否则，你只是在没有任何上下文的情况下死记硬背数值运算模式。这也能让行列式等新的线性代数概念变得更加直观。

### 行列式

执行线性变换时，我们有时会“扩展”或“压缩”空间，这种发生的程度可能很有帮助。取向量空间中图 4-20 的一个采样区域：在缩放 î 和 ĵ 之后，它会发生什么变化？

![](img/8e0dcc1636d032e7728a07e97751e446_200_0.png)

图 4-20. 行列式衡量线性变换如何缩放面积

面积增加了 6.0 倍，这个因子被称为 **行列式**。行列式描述了向量空间中采样区域在线性变换下的缩放程度，这可以提供关于变换的有用信息。

### 示例 4-14.

```python
from numpy.linalg import det
from numpy import array

i_hat = array([3, 0])
j_hat = array([0, 2])

basis = array([i_hat, j_hat]).transpose()

determinant = det(basis)

print(determinant) # prints 6.0
```

简单的剪切和旋转不应该影响行列式，因为面积不会改变。图 4-21 和示例 4-15 展示了一个简单的剪切，行列式仍保持 1.0 的因子，表明其不变。

![](img/8e0dcc1636d032e7728a07e97751e446_202_0.png)

图 4-21. 简单的剪切不会改变行列式

### 示例 4-15.

```python
from numpy.linalg import det
from numpy import array
i_hat = array([1, 0])
j_hat = array([1, 1])
basis = array([i_hat, j_hat]).transpose()

determinant = det(basis)

print(determinant) # prints 1.0
```

但是缩放会增加或减少行列式，因为这会增加/减少采样面积。当方向翻转（î 和 ĵ 顺时针互换位置）时，行列式将为负数。图 4-22 和示例 4-16 展示了一个行列式，表示的变换不仅缩放了空间，还翻转了向量空间的方向。

![](img/8e0dcc1636d032e7728a07e97751e446_204_0.png)

图 4-22. 翻转空间上的行列式为负数

### 示例 4-16.

```python
from numpy.linalg import det
from numpy import array

i_hat = array([-2, 1])
j_hat = array([1, 2])

basis = array([i_hat, j_hat]).transpose()

determinant = det(basis)

print(determinant) # prints -5.0
```

由于这个行列式为负数，我们可以快速看出方向已经翻转。但行列式告诉你的最关键信息是，该变换是否具有线性相关性。如果行列式为 0，意味着整个空间已被压缩到较低维度。

在图 4-23 中，我们看到两个具有线性相关性的变换：一个将 2D 空间压缩到 1 维，另一个将 3D 空间压缩到 2 维。在这两种情况下，面积和体积分别为 0！

![](img/8e0dcc1636d032e7728a07e97751e446_205_0.png)

![](img/8e0dcc1636d032e7728a07e97751e446_205_1.png)

图 4-23. 2D 和 3D 中的线性相关性

示例 4-17 展示了上述 2D 示例的代码，将整个 2D 空间压缩到一条一维数轴上。

### 示例 4-17.

```python
from numpy.linalg import det
from numpy import array

i_hat = array([-2, 1])
j_hat = array([3, -1.5])

basis = array([i_hat, j_hat]).transpose()

determinant = det(basis)

print(determinant) # prints 0.0
```

因此，检测 0 行列式对于判断变换是否具有线性相关性非常有帮助。当你遇到这种情况时，很可能会面临一个棘手甚至无法解决的问题。

### 特殊类型的矩阵

有些特殊的矩阵案例值得关注，我们应该介绍一下。

#### 方阵

方阵是行数和列数相等的矩阵。

![](img/8e0dcc1636d032e7728a07e97751e446_206_0.png)

它们主要用于表示线性变换，并且是许多运算（如特征值分解）的前提条件。

#### 单位矩阵

单位矩阵是一个方阵，其对角线上的元素为 1，其他元素均为 0。

单位矩阵：

| 1 | 0 | 0 |
|---|---|---|
| 0 | 1 | 0 |
| 0 | 0 | 1 |

单位矩阵有什么重要性呢？其实，当你拥有单位矩阵时，基本上就已经撤销了变换并找到了起始基向量。这在下一节求解方程组时将发挥重要作用。

#### 逆矩阵

逆矩阵是撤销另一个矩阵变换的矩阵。假设我有矩阵 $ A $。

A =

| 4 | 2 | 4 |
|---|---|---|
| 5 | 3 | 7 |
| 9 | 3 | 6 |

矩阵 $ A $ 的逆矩阵称为 $ A^{-1} $。我们将在下一节学习如何使用 Sympy 或 NumPy 计算逆矩阵，以下是矩阵 $ A $ 的逆矩阵。

A^{-1} =

| -1/2 | 0 | 1/3 |
|---|---|---|
| 5.5 | -2 | 4/3 |
| -2 | 1 | 1/3 |

当我们对 $ A^{-1} $ 和 $ A $ 进行矩阵乘法时，最终得到一个单位矩阵。我们将在下一节关于方程组的内容中，通过 NumPy 和 Sympy 看到这一点。

```
| -1/2 0 1/3 || 4 2 4 || 1 0 0 |
| 5.5 -2 4/3 || 5 3 7 | = | 0 1 0 |
| -2 1 1/3 || 9 3 6 |   | 0 0 1 |
```

