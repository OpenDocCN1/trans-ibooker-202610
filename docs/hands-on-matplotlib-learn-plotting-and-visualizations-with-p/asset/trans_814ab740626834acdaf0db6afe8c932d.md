

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

## CHAPTER 4

## Revisiting Matplotlib Visualizations

在上一章中，你学习了 NumPy 库中创建和操作 Ndarrays 的许多例程。你将在本书中多次需要这些例程。

本章专门用于探索使用 Matplotlib 准备的可视化的美学方面。你将学习如何自定义 Matplotlib 可视化的外观。具体来说，我们将详细探讨以下主题：

- Single-line plots
- Multiline plots
- Grid, axes, and labels
- Colors, lines, and markers
- Subplots
- Object-oriented style
- Working with the text

阅读本章后，你将能够以编程方式自定义可视化的美学方面，使其更具表现力。

## Single-Line Plots

当图形中只有一个使用函数 plot() 的可视化时，它被称为单线图。在本节中，你将看到绘制单线图的几种方法。我们已经使用函数 plot() 绘制了单线图。让我们通过更多具体的示例进一步详细探讨这一概念。

## CHAPTER 4 REVISITING MATPLOTLIB VISUALIZATIONS

为本章的演示创建一个新的笔记本。你也可以按如下方式使用 Python 列表来可视化绘图：

```
```python
%matplotlib inline
import matplotlib.pyplot as plt
x = [4, 5, 3, 1, 6, 7]
plt.plot(x)
plt.show()
```
```

Figure 4-1 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_68_0.png)

***Figure 4-1.*** *Demonstrating a simple single-line graph*

在这种情况下，y 轴的值被假定。

以下是另一个使用 Ndarray 的单线图的示例：

```
```python
import numpy as np
x = np.arange(25)
plt.plot(x)
plt.show()
```
```

Figure 4-2 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_69_0.png)

Figure 4-2. A simple single-line graph with arange()

让我们可视化二次图 y = f(x) = x³+1。代码如下：

```
plt.plot(x, [(y**3 + 1) for y in x])
plt.show()
```

Figure 4-3 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_69_1.png)

Figure 4-3. y = f(x) = x³+1

## CHAPTER 4   REVISITING MATPLOTLIB VISUALIZATIONS

你可以按如下方式以更简单的方式编写相同的代码：

```
```python
plt.plot(x, x**3 + 1)
plt.show()
```
```

## Multiline Plots

可以在同一输出中可视化多个绘图。让我们看看如何在同一可视化中显示多条曲线。以下是简单示例：

```
```python
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt
x = np.arange(7)
plt.plot(x, -x**2)
plt.plot(x, -x**3)
plt.plot(x, -2*x)
plt.plot(x, -2**x)
plt.show()
```
```

Figure 4-4 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_70_0.png)

**Figure 4-4.** *Multiline graph*

正如你所见，Matplotlib 自动为曲线分别分配颜色。

你可以按如下方式以更简单的方式编写相同的代码：

```
```python
plt.plot(x, -x**2, x, -x**3,
         x, -2*x, x, -2**x)
plt.show()
```
```

输出将与 Figure 4-4 相同。

让我们看另一个示例：

```
```python
x = np.array([[3, 2, 5, 6], [7, 4, 1, 5]])
plt.plot(x)
plt.show()
```
```

Figure 4-5 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_71_0.png)

**Figure 4-5.** *Multiline graph, another example*

你也可以按如下方式使用随机数据创建多线图：

```
```python
data = np.random.randn(2, 10)
print(data)
plt.plot([data[0], data[1]])
plt.show()
```
```

Figure 4-6 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_72_0.png)

Figure 4-6. Multiline graph, random data

在这个示例中，我们使用例程 np.random.randn() 以随机方式生成数据。由于此例程会生成随机数据，因此每次执行时的输出都会不同。所以，你每次执行代码时看到的输出都会不同。

## Grid, Axes, and Labels

现在你将学习如何在可视化中启用网格。这可以通过语句 plt.grid(True) 来完成。你还将学习如何操作坐标轴的限制。但在此之前，你将快速学习如何将可视化保存为硬盘上的图像。查看以下代码：

```
x = np.arange(3)
plt.plot(x, -x**2, x, -x**3, x, -2*x, x, -2**x)
plt.grid(True)
plt.savefig('test.png')
plt.show()
```

语句 plt.savefig('test.png') 将图像保存到 Jupyter Notebook 文件的当前目录。Figure 4-7 显示了输出。

