


>>> print(np.dot([10, 15, 30], B))
[150, 205]

我们能否想出一些直观的、现实世界中的例子，来展示点积的用处？这类例子在某些数学和物理领域（例如三维几何）中比比皆是。但也有更简单的应用。假设你拥有一家宠物店，出售三种异国鸟类。表 13.5 列出了价格。

# 表 13.5. 宠物店鸟类价格

| Parrots | Macaws | Peacocks |
|---------|--------|----------|
| $10 | $15 | $30 |

再假设你记录了两个月的销售数据，如表 13.6 所示。

# 表 13.6. 鸟类月度销售数据

| Birds | October sales | November sales |
|-------|---------------|----------------|
| Parrots | 0 | 1 |
| Macaws | 2 | 3 |
| Peacocks | 4 | 5 |

你想做的是计算这两个月的鸟类总销售额。虽然挑选数据并不困难，但使用点积并让函数 `np.dot` 为你完成所有数学运算会更加容易。

图 13.18 展示了如何获得结果中的第一个元素：150。将每个销售数字乘以第一个月对应的销售数字。

![](img/6e45221a8147f2442c653a3d4a866570_838_0.png)

Dot Product for first elem. = (10*0) + (15*2) + (30*4)
= 0 + 30 + 120
= 150

**图 13.18.** 点积的计算方法，第一部分

按照此过程，你得到 0 + 30 + 120，总计 150。
你可以用同样的方法得到另一个数字 205（参见图 13.19）。将每个价格乘以第二个月对应的销售数字，然后求和。按照此过程，你得到 10 + 45 + 150，总计 205。

![](img/6e45221a8147f2442c653a3d4a866570_839_0.png)

Dot Product for 2nd elem. = (10*1) + (15*3) + (30*5)
= 10 + 45 + 150
= 205

**图 13.19.** 点积的计算方法，第二部分

因此，完整的点积为

```
[150, 205]
```

在这种情况下，点积给出了所追踪的两个月（十月和十一月）内所有鸟类的总销售额。

## 13.9.2 外积函数

另一种乘数组的方法是使用 **outer** 函数计算 _外积_。这最常用于两个一维数组之间，以产生一个二维数组。如果此函数用于更高维度的数组，则这些数组中的每个输入都会被展平为一维。

![](img/6e45221a8147f2442c653a3d4a866570_840_0.png)

Click here to view code image

```
numpy.outer(A, B, out=None)
```

该函数的作用是计算数组 A 和 B 的外积并返回它。`out` 参数（如果包含）指定一个目标数组，用于放置结果。它必须已经存在且大小合适。

要获得外积，请依次将 A 的每个元素乘以 B 的每个元素，以产生一个二维数组。就形状而言，我们可以这样表达这种关系：

```
(A) * (B) => (A, B)
```

简单来说，外积包含 A * B 的每种组合，因此如果 C 是结果，那么 C[x, y] 包含 A[x] 乘以 B[y]。

下面是一个相对简单的示例：

Click here to view code image

```
>>> import numpy as np
>>> A = np.array([0, 1, 2])
>>> B = np.array([100, 200, 300, 400])
>>> print(np.outer(A, B))
[[   0    0    0    0]
 [ 100  200  300  400]
 [ 200  400  600  800]]
```

在这个示例中，A 的第一个元素乘以 B 的每个元素，以产生结果的第一行；这就是为什么该行中的每个数字都是 0，因为 0 乘以任何值都是 0。A 的第二个元素（即 1）乘以 B 的每个元素，以产生第二行，以此类推，第三行也是如此。

外积的一个明显用途是我们在第 11 章“随机和数学包”中解决的一个问题：如何创建乘法表。**numpy** 包支持一个更简单的解决方案，而且无论如何都更快。

```
>>> A = np.arange(1,10)
>>> print(np.outer(A, A))
```

哇，这段代码相当简单！结果是

```
[[ 1 2 3 4 5 6 7 8 9]
 [ 2 4 6 8 10 12 14 16 18]
 [ 3 6 9 12 15 18 21 24 27]
 [ 4 8 12 16 20 24 28 32 36]
 [ 5 10 15 20 25 30 35 40 45]
 [ 6 12 18 24 30 36 42 48 54]
 [ 7 14 21 28 35 42 49 56 63]
 [ 8 16 24 32 40 48 56 64 72]
 [ 9 18 27 36 45 54 63 72 81]]
```

如同在第 11 章中一样，我们可以使用一些字符串操作来清理结果，消除方括号。

```
s = str(np.outer(A, A))
s = s.replace('[', '')
s = s.replace(']', '')
```

```
print(' ' + s)
```

如果我们愿意，可以将这四条语句合并为两条更紧凑的语句。

Click here to view code image

```
s = str(np.outer(A, A))
print(' ' + s.replace('[', '').replace(']', ''))
```

最后，这将产生

```
1 2 3 4 5 6 7 8 9
2 4 6 8 10 12 14 16 18
3 6 9 12 15 18 21 24 27
4 8 12 16 20 24 28 32 36
5 10 15 20 25 30 35 40 45
6 12 18 24 30 36 42 48 54
7 14 21 28 35 42 49 56 63
8 16 24 32 40 48 56 64 72
9 18 27 36 45 54 63 72 81
```

## 13.9.3 其他线性代数函数

除了点积和外积，**numpy** 还提供其他线性代数函数。请记住，它们不需要单独的“矩阵”类型。标准 **numpy** 数组类型 **ndarray** 可以与任何这些函数一起使用。

但是，与线性代数相关的函数列表很长，需要一整本书来介绍。有关更多说明，请参见官方在线文档 **https://docs.scipy.org/doc/numpy/reference/routines.linalg.html**

# 表 13.7. 常用线性代数函数

| Syntax | Description |
|---|---|
| `np.dot(A, B [,out])` | Compute the dot product between `A` and `B`. |
| `np.vdot(A, B)` | Compute the vector dot product. |
| `np.outer(A, B [,out])` | Compute the outer product formed by multiplying each element in `A` by each element in `B`. Flatten `A` and `B` into one-dimensional inputs as needed. |
| `np.inner(A, B [,out])` | Compute the inner product of `A` and `B`. |
| `np.tensordot(A, B [,out])` | Compute the tensor dot product of `a` and `B`. |
| `np.kron(A, B)` | Compute the Kronecker product of `a` and `B`. |
| `np.linalg.det(A)` | Compute the linear-algebra determinant of `A`. |

# 13.10 三维绘图

三维绘图的主题比较高级，要在此处完整介绍该主题本身就需要一整本书！然而，通过观察球体表面的绘图，你可以了解你已经见过的 **numpy** 函数如何帮助你创建三维表面。

以下示例需要导入你已经熟悉的包，但它还包括 **mpl_toolkits** 包。幸运的是，如果你已经下载了其他包，则此包已为你下载，你只需导入它即可。

Click here to view code image

```
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

# Make data
uA = np.linspace(0, 2 * np.pi, 100)
vA = np.linspace(0, np.pi, 100)
X = 10 * np.outer(np.cos(uA), np.sin(vA))
Y = 10 * np.outer(np.sin(uA), np.sin(vA))
Z = 10 * np.outer(np.ones(np.size(uA)), np.cos(vA))
```

```
# Plot the surface
ax.plot_surface(X, Y, Z, color='w')
plt.show()
```

此处的大部分计算涉及获取角度从 0 到 2 * np.pi 的正弦和余弦值，然后通过外积将结果相乘。最后，由三个数组 X、Y 和 Z 描述一组三维点，软件根据此绘制球体表面。图 13.20 显示了生成的图表。

![](img/6e45221a8147f2442c653a3d4a866570_845_0.png)

**图 13.20.** 球体的三维投影

# 13.11 “NUMPY” 金融应用

**numpy** 包强大的函数范围延伸到金融领域。例如，给定利率和付款计划的数据，你可以使用 **pmt** 函数来确定房屋或汽车的月供。

![](img/6e45221a8147f2442c653a3d4a866570_846_0.png)

Click here to view code image

```
numpy.pmt(rate, nper, pv [, fv] [, when ])
```

_rate_ 是以浮点数表示的利率，而不是百分比。例如，0.065 的利率相当于 6.5% 的利率。由于这是每个付款周期的利率，你需要除以 12 才能从年利率得到月利率。

_nper_ 参数是付款周期的总数。如果付款是按月而不是按年进行，则需要将年数乘以 12。

_pv_ 参数是现值。这是借入的金额。

可选参数包括 _fv_，即预期的未来价值（假设这笔钱将在期末偿还）。另一个可选参数是 _when_，它可以可选地取


