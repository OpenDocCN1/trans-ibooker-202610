## 第 3 章

# NumPy 例程和 Matplotlib 入门

在上一章中，你学习了 NumPy 的基础知识。具体来说，你学习了如何安装它以及如何创建 N-arrays。上一章中学习的所有主题将作为剩余章节的基础，因为 N-array 是我们整本书中将使用的基本数据结构。

在本章中，我们将继续上一章的内容并查看一些 N-array 创建例程。我们还将开始科学计算生态系统中的主要数据可视化库 Matplotlib。我们将使用 NumPy 的 N-array 创建例程来演示 Matplotlib 的可视化。这是一个详细的章节，重点放在编程和可视化上。以下是本章中你将学习的内容：

- 创建 N-arrays 的例程
- Matplotlib
- 使用 NumPy 和 Matplotlib 进行可视化

在本书的剩余章节中，我们将频繁使用 Matplotlib 和 NumPy 来演示数据可视化。




# 创建 Ndarray 的例程

让我们学习创建由 1 和 0 组成的 Ndarray。在本节中，我们将探索许多数组创建例程。使用 Jupyter Notebook 创建一个新笔记本，保存本章的代码。`np.empty()` 返回一个给定形状和类型的新数组，但不会初始化条目。由于对应的元素没有被初始化，它们是任意的（随机的）。让我们看一个小演示。在笔记本的单元格中输入以下代码并运行：

```python
import numpy as np
x = np.empty([3, 3], np.uint8)
print(x)
```

输出如下：

```
[[ 64 244  49]
 [  4   1   0]
 [  0   0 124]]
```

请注意，每次执行时值都会不同，因为它在创建矩阵时不会初始化数值。你可以创建任意大小的矩阵，如下所示：

```python
x = np.empty([3, 3, 3], np.uint8)
print(x)
```

函数 `np.eye()` 返回一个二维矩阵，对角线上的元素为 1，其他元素为 0。以下是一个示例：

```python
y = np.eye(5, dtype=np.uint8)
print(y)
```

输出如下：

```
[[1 0 0 0 0]
 [0 1 0 0 0]
 [0 0 1 0 0]
 [0 0 0 1 0]
 [0 0 0 0 1]]
```

你可以改变对角线索引的位置。默认值为 0，表示主对角线。正值表示上对角线，负值表示下对角线。下面是一些示例。我们先演示上对角线：

```python
y = np.eye(5, dtype=np.uint8, k=1)
print(y)
```

输出如下：

```
[[0 1 0 0 0]
 [0 0 1 0 0]
 [0 0 0 1 0]
 [0 0 0 0 1]
 [0 0 0 0 0]]
```

下面演示下对角线的代码：

```python
y = np.eye(5, dtype=np.uint8, k=-1)
print(y)
```

输出如下：

```
[[0 0 0 0 0]
 [1 0 0 0 0]
 [0 1 0 0 0]
 [0 0 1 0 0]
 [0 0 0 1 0]]
```

*单位矩阵* 是指对角线上所有元素为 1、其余元素为 0 的矩阵。函数 `np.identity()` 返回指定大小的单位矩阵，如下所示：

```python
x = np.identity(5, dtype=np.uint8)
print(x)
```

前面的代码产生的输出与以下代码相同：

```python
y = np.eye(5, dtype=np.uint8)
print(y)
```

两种方法的输出如下：

```
[[1 0 0 0 0]
 [0 1 0 0 0]
 [0 0 1 0 0]
 [0 0 0 1 0]
 [0 0 0 0 1]]
```

函数 `np.ones()` 返回给定大小的矩阵，所有元素均为 1。

```python
x = np.ones((2, 5, 5), dtype=np.int16)
print(x)
```

运行代码，你将看到以下输出：

```
[[[1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]]

 [[1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]]]
```

函数 `np.zeros()` 返回给定大小的矩阵，所有元素均为 0。

```python
x = np.zeros((2, 5, 5, 2), dtype=np.int16)
print(x)
```

运行代码并查看输出。

函数 `np.full()` 返回一个给定形状和类型的新数组，用传入的参数值填充。这里有一个示例：

```python
x = np.full((3, 3, 3), dtype=np.int16, fill_value=5)
print(x)
```

输出如下：

```
[[[5 5 5]
  [5 5 5]
  [5 5 5]]

 [[5 5 5]
  [5 5 5]
  [5 5 5]]

 [[5 5 5]
  [5 5 5]
  [5 5 5]]]
```

下三角矩阵是指对角线和对角线下方的所有元素为 1，其余元素为 0 的矩阵。函数 `np.tri()` 返回给定大小的下三角矩阵，如下所示：

```python
x = np.tri(3, 3, k=0, dtype=np.uint16)
print(x)
```

输出如下：

```
[[1 0 0]
 [1 1 0]
 [1 1 1]]
```

你甚至可以改变次对角线的位置。次对角线以下的所有元素将为 0。

```python
x = np.tri(5, 5, k=1, dtype=np.uint16)
print(x)
```

输出如下：

```
[[1 1 0 0 0]
 [1 1 1 0 0]
 [1 1 1 1 0]
 [1 1 1 1 1]
 [1 1 1 1 1]]
```

另一个使用负次对角线值的示例如下：

```python
x = np.tri(5, 5, k=-1, dtype=np.uint16)
print(x)
```

输出如下：

```
[[0 0 0 0 0]
 [1 0 0 0 0]
 [1 1 0 0 0]
 [1 1 1 0 0]
 [1 1 1 1 0]]
```

类似地，你可以使用函数 `np.tril()` 来获得下三角矩阵。它接受另一个矩阵作为参数。这里有一个演示：

```python
x = np.ones((5, 5), dtype=np.uint8)
y = np.tril(x, k=-1)
print(y)
```

输出如下：

```
[[0 0 0 0 0]
 [1 0 0 0 0]
 [1 1 0 0 0]
 [1 1 1 0 0]
 [1 1 1 1 0]]
```

上三角矩阵是指对角线和对角线上方的所有元素为 1，其余元素为 0 的矩阵。

```python
x = np.ones((5, 5), dtype=np.uint8)
y = np.triu(x, k=0)
print(y)
```

输出如下：

```
[[1 1 1 1 1]
 [0 1 1 1 1]
 [0 0 1 1 1]
 [0 0 0 1 1]
 [0 0 0 0 1]]
```

你可以使用负的次对角线，如下所示：

```python
x = np.ones((5, 5), dtype=np.uint8)
y = np.triu(x, k=-1)
print(y)
```

输出如下：

```
[[1 1 1 1 1]
 [1 1 1 1 1]
 [0 1 1 1 1]
 [0 0 1 1 1]
 [0 0 0 1 1]]
```

你可以使用正的次对角线，如下所示：

```python
x = np.ones((5, 5), dtype=np.uint8)
y = np.triu(x, k=1)
print(y)
```

输出如下：

```
[[0 1 1 1 1]
 [0 0 1 1 1]
 [0 0 0 1 1]
 [0 0 0 0 1]
 [0 0 0 0 0]]
```

# Matplotlib

Matplotlib 是科学 Python 生态系统中不可或缺的一部分，用于可视化。它是 NumPy 的扩展，提供了类似 Matlab 的绘图和可视化接口。它最初由 John D. Hunter 开发，作为可在 Python 中使用的开源替代方案。

你可以按如下方式使用 Jupyter Notebook 安装它：

```bash
!pip3 install matplotlib
```

注意 `pip3` 命令前面的 ! 符号。这是因为当你想在笔记本中运行操作系统命令时，必须在其前面加上 !。

在安装 Matplotlib 库之前，你可能需要使用以下命令升级 pip：

```python
!python -m pip install --upgrade pip
```

要在笔记本中使用 Matplotlib 库进行基本绘图，你必须按如下方式导入其 pyplot 模块：

```python
import matplotlib.pyplot as plt
```

pyplot 模块提供了类似 Matlab 的接口用于创建可视化。此外，要在笔记本中显示 Matplotlib 可视化，你必须运行以下魔法命令：

```python
%matplotlib inline
```

这会强制 Matplotlib 在生成可视化的代码单元正下方内联显示输出。在需要使用 Matplotlib 时，我们总是会使用它。下面也导入 NumPy：

```python
import numpy as np
```

你可以在 https://matplotlib.org/ 上了解更多关于 Matplotlib 的信息。

# 使用 NumPy 和 Matplotlib 进行可视化

你现在将学习如何使用 Ndarray 创建例程来创建 NumPy Ndarray，然后使用 Matplotlib 将它们可视化。让我们从创建 Ndarray 的例程开始。

第一个例程是 `arange()`。它创建给定间隔的等间距值。停止值参数是必需的。起始值和间隔参数的默认参数分别为 0 和 1。这里有一个示例：

```python
x = np.arange(6)
```

在前面的示例中，停止值为 6（由于范围是半开放的，`np.arange(6)` 产生 0 到 5）。因此，它创建一个从 0 开始到 5 结束的 Ndarray。该函数返回具有半开区间的序列，这意味着输出中不包含停止值。由于我们没有指定间隔，它默认为 1。你可以按如下方式查看输出及其数据类型：

```python
print(x)
type(x)
```

输出如下：

```
[0 1 2 3 4 5]
numpy.ndarray
```

让我们继续绘制这些数字。要在 2D 中绘图，我们需要 x-y 对。让我们保持简单，设 y = f(x) = x+1，运行以下语句：

```python
y = x + 1
```

现在，让我们使用 `plot()` 函数将其可视化。它需要 x 和 y 的值以及绘图选项。稍后你将在本章中学到更多关于绘图选项的内容。

```python
plt.plot(x, y, 'o--')
plt.show()
```

函数 `show()` 用于显示绘图。如你所见，我们使用绘图选项 `o--` 进行可视化。这意味着数据点用实心圆表示，线条用虚线表示，如图 3-1 所示。

![](img/124f805c8155c7226d129ebbcc2842f0_60_0.png)

图 3-1. 可视化 y=f(x)=x+1




以下是函数 arange() 使用 start 和 stop 参数的函数调用示例：

```
np.arange(2, 6)
```

它返回以下输出（它直接打印且我们没有将其存储在变量中）：

```
array([2, 3, 4, 5])
```

我们甚至可以按如下方式添加间隔参数：

```
np.arange(2, 6, 2)
```

输出如下：

```
array([2, 4])
```

我们可以按如下方式绘制多个图形：

```
plt.plot(x, y, 'o--')
plt.plot(x, -y, 'o-')
plt.show()
```

输出将有一条线和另一条虚线，如图 3-2 所示。

![](img/124f805c8155c7226d129ebbcc2842f0_61_0.png)

## Figure 3-2. Visualizing multiple lines

你甚至可以按如下方式为图形添加标题：

```
plt.plot(x, y, 'o--')
plt.plot(x, -y, 'o-')
plt.title('y=x and y=-x')
plt.show()
```

输出将具有如图 3-3 所示的标题。

![](img/124f805c8155c7226d129ebbcc2842f0_62_0.png)

Figure 3-3. Visualizing multiple lines and title

函数 linspace(start, stop, number) 返回指定区间上均匀间隔的数字数组。你必须按如下方式传递起始值、结束值和值的数量：

```
N = 16
x = np.linspace(0, 15, N)
print(x)
```

之前的代码创建了 11 个数字（0 到 10，包含两端）如下：

```
[ 0.  1.  2.  3.  4.  5.  6.  7.  8.  9. 10. 11. 12. 13. 14. 15.]
```

让我们按如下方式可视化它：

```
y = x
plt.plot(x, y, 'o--')
plt.axis('off')
plt.show()
```

Figure 3-4 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_63_0.png)

## Figure 3-4. Output of y = x with linspace()

正如你所见，我们使用行 plt.axis('off') 关闭了坐标轴。同样，你可以按如下方式计算并可视化对数空间中的值：

```
y = np.logspace(0.1, 2, N)
print(y)
plt.plot(x, y, 'o--')
plt.show()
```

print 函数的输出如下：

```
[ 1.25892541  1.68525904  2.25597007  3.01995172  4.04265487
  5.41169527  7.2443596   9.69765359 12.98175275 17.37800829
 23.26305067 31.14105584 41.68693835 55.80417175 74.70218989
 100.        ]
```

Figure 3-5 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_64_0.png)

## Figure 3-5. Output of logspace()

你甚至可以按如下方式计算等比数列：

```
y = np.geomspace(0.1, 2000, N)
print(y)
plt.plot(x, y, 'o--')
plt.show()
```

print 语句的输出如下：

```
[1.00000000e-01 1.93524223e-01 3.74516250e-01 7.24779664e-01
1.40262421e+00 2.71441762e+00 5.25305561e+00 1.01659351e+01
1.96735469e+01 3.80730788e+01 7.36806300e+01 1.42589867e+02
2.75945932e+02 5.34022222e+02 1.03346236e+03 2.00000000e+03]
```

Figure 3-6 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_64_1.png)

## Figure 3-6. Output of geomspace()

## Running the Matplotlib Program as a Script

你可以使用 Python 的脚本模式运行 Matplotlib 程序。将 Listing 3-1 中所示的程序保存为 prog01.py。

### Listing 3-1. prog01.py

```
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
print(x)
type(x)

y=x+1

plt.plot(x, y, 'o--')
plt.show()
```

当你运行此程序时，输出将显示在单独的窗口中，如图 3-7 所示。

![](img/124f805c8155c7226d129ebbcc2842f0_65_0.png)

Figure 3-7. Output in a separate window

## Summary

我们将主要使用 Jupyter Notebook 在浏览器窗口中显示可视化。

本章重点介绍了创建 Ndarrays 的例程。你还学习了 Matplotlib 的基础知识。除了基础知识之外，你还学习了如何使用简单图形可视化 Ndarrays。NumPy 和 Matplotlib 的内容远不止本章所学的内容。还有更多 NumPy 和数据可视化例程。

在下一章中，你将探索更多操作 NumPy Ndarrays 的 NumPy 例程。
