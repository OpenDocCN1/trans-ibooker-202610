

```
y_values = [x**2 - 3 for x in x_values]
# -----------------------------------------------------------------------------
```

the_plot = axes.plot(x_values, y_values)

<Figure size 432x288 with 0 Axes>
<Figure size 432x288 with 1 Axes>

图 13.2 显示了我们调用 plt.show() 时 **matplotlib** 生成的绘图。

![](img/4ffeafa3e3f1373f02971e43b0755ea6_514_0.png)

图 13.2：抛物线 y = x² - 3 的绘图

为了使标题的格式更好，我在多项式方程的两侧使用了美元符号之间的 LaTeX 标记。LaTeX \mathregular 表达式告诉 **matplotlib** 使用与 surrounding 文本相同的字体。

为了使这张图更平滑，我们需要计算更多点。我们可以使用列表，但使用一维 NumPy 数组会更高效。

## 13.1.7 NumPy 数组

如果我们以字节为单位测量，Python 对象的大小是多少？让我们看看一个空字符串的大小。

```
from sys import getsizeof

getsizeof("")
```

`getsizeof` 是 **sys** 模块中的一个函数，它返回对象使用的内存字节数。对于字符串的情况，它包括长度、字符本身，以及垃圾回收等过程的系统开销。如果我们添加一些文本，大小会变大，正如我们所期望的。

```
```python
a = "Better a witty fool, than a foolish wit."
len(a)
```
```

40

```
```python
getsizeof(a)
```
```

89

一个包含字符数是 100 倍的字符串对象的大小大约是 100 倍大，考虑到开销。

```
```python
b = 100 * a
len(b)
```
```

4000

```
```python
getsizeof(b)
```
```

4049

如果包含这两个字符串的列表呢？

```
```python
getsizeof([a, b])
```
```

72

这很奇怪，而且远小于字符串大小的总和。如果我们有一个包含较长字符串两个副本的列表，大小也是相同的。

```
```python
getsizeof([b, b])
```
```

72

如果我们包含第三个副本，字节数会适度增加。

```
```python
getsizeof([b, b, b])
```
```

80

getsiz​​eof 测量对象的大小，**但不测量它所引用的对象的大小。** 在这个例子中，`getsizeof` 没有包含列表中字符串的大小。我们可以用以下类似的函数来更好地近似实际使用的总内存量：

```
```python
def getsizeof_list(the_list):
    size = getsizeof(list)
    for item in the_list:
        size += getsizeof(item)
    return size
```
```

```
```python
x_values = list(range(1000))
getsizeof_list(x_values)
```
```

28404

### 练习 13.2

> 如果列表包含集合、字典或另一个列表，`getsizeof_list` 的结果是否正确？编写一个新函数，递归地测量对象及其包含内容的大小。

通过将数据紧密地连续打包在内存中，可以更高效地存储数值数据。这种结构称为 *array*，NumPy 是一个广泛使用的 Python 包，包含强大的多维数组支持。NumPy 自称“Python 科学计算的基础包”，我同意。[ANN] [NPY] [NUM] 按照传统，我们将 NumPy 包导入为 np。

```
```python
import numpy as np
```
```

*arange* 函数类似于 *range*，但它比使用迭代器、生成器或列表更高效地构建数组。

```
```python
x_values_array = np.arange(1000)

getsizeof(x_values_array)
```
```

4104

看看数组从 0 到 999 的整数使用的内存比上面的列表少多少？size 函数返回数组中的项目数。

```
x_values_array.size
```

1000

正如我们在第 5.2 节看到的那样，Python 可以计算任意大的整数。当我们知道整数的值是有界的时，我们可以使用固定数量的字节来容纳数字。items size 属性获取每个项目使用的大小。

```
x_values_array.itemsize
```

4

这个结果说明每个整数值使用 4 个字节。将它乘以项目数，你就得到 nbytes 属性的值，即所有数组数据的总字节数。

```
x_values_array.nbytes
```

4000

### 练习 13.3

为什么 x_values_array.nbytes 与 getsizeof(x_values_array) 不同？

如果您有一个数字元组的列表，请使用 array 创建一个数组。NumPy 强制数组中的所有数字具有相同的类型，它称之为 dtype。

```
np.array([2, 3.4])
```

array([2., 3.4])

```
np.array((-1, 3.1415926j))
```

array([-1.+0.j , 0.+3.1415926j])

假设您想要 0 到 1 之间包含端点的十一个浮点数。linspace(n, m, count) 给您一个从 n 开始以 m 结束的 count 个数的数组。

```
np.linspace(0, 1, 11)

array([0. , 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1. ])
```

我们可以通过以下方式创建我们在图 13.2 中绘制抛物线时使用的 x 坐标的数组：

```
x_values = np.linspace(-4, 4, 9)
```

NumPy 最强大的功能之一是您可以直接对数组使用代数运算。当我们定义 p 来计算抛物线的公式时，我们可以直接将其应用于 x_values。

```
def p(x): return x*x - 3

p(x_values)

array([13., 6., 1., -2., -3., -2., 1., 6., 13.])
```

我们现在准备好用 1,000 个点绘制我们的抛物线。

```
import matplotlib.pyplot as plt
from mpl_helper_2 import initialize_plot
import numpy as np

figure, axes = initialize_plot('x', 'y',
    r"Smooth plot of the parabola $\mathregular{y=x^2-3}$")

# Generate 1000 equally spaced points between -4 and 4
x_values = np.linspace(-4, 4, 1000)

def p(x): return x*x - 3

the_plot = plt.plot(x_values, p(x_values))
```

<Figure size 432x288 with 0 Axes>
<Figure size 432x288 with 1 Axes>

```
plt.show()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_519_0.png)

## 13.1.8 绘制正弦函数

将函数应用于数组的每个元素的技术适用于许多数学运算，不过您应该使用 numpy 模块中的函数而不是 math 模块。这里我从 x = −2π 到 2π 绘制正弦函数：

```
import matplotlib.pyplot as plt
from mpl_helper_2 import initialize_plot
import numpy as np

figure, axes = initialize_plot('x', 'y', "y = sin(x)")

# set the aspect ratio of vertical to horizontal
axes.set_aspect('equal')

# --------------------------------------------------------------------------
# Generate 1000 equally spaced points between -2 pi and 2 pi
x_values = np.linspace(-2*np.pi, 2*np.pi, 1000)
# --------------------------------------------------------------------------

the_plot = plt.plot(x_values, np.sin(x_values))
```

<Figure size 432x288 with 0 Axes>
<Figure size 432x288 with 1 Axes>

## 创建绘图和图表

```
```python
plt.show()
```
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_520_0.png)

这张图在高度上宽但短。我现在使用 `set_xlim` 和 `set_ylim` 方法来设置 x 和 y 坐标显示值的范围。

```
```python
import matplotlib.pyplot as plt
from mpl_helper_2 import initialize_plot
import numpy as np

figure, axes = initialize_plot('x', 'y', "y = sin(x)")

# set the aspect ratio of vertical to horizontal
axes.set_aspect('equal')

# -----------------------------------------------------------------
# show x-coordinates from -7 to 7
axes.set_xlim([-7, 7])

# show y-coordinates from -7 to 7
axes.set_ylim([-7, 7])
# -----------------------------------------------------------------

# Generate 1000 equally spaced points between -2 pi and 2 pi
x_values = np.linspace(-2 * np.pi, 2 * np.pi, 1000)
the_plot = plt.plot(x_values, np.sin(x_values))
```
```

`<Figure size 432x288 with 0 Axes>`
`<Figure size 432x288 with 1 Axes>`

```
```python
plt.show()
```
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_521_0.png)

最后，我使用 *axvline* 和 *text* 方法在 $x = -\pi$ 和 $x = \pi$ 处绘制并标记灰色虚线垂直线。

```
```python
import matplotlib.pyplot as plt
from mpl_helper_2 import initialize_plot
import numpy as np

figure, axes = initialize_plot('x', 'y',
                               "y = sin(x)")

# set the aspect ratio of vertical to horizontal
axes.set_aspect('equal')

# show x-coordinates from -7 to 7
axes.set_xlim([-7, 7])

# show y-coordinates from -7 to 7
axes.set_ylim([-7, 7])

# ----------------------------------------------------------------------
# draw and label a dashed vertical line at x = -pi
axes.axvline(x=-np.pi, color="#808080",
             linestyle="dashed", linewidth=1.5)

axes.text(-4.5, -3.2,
          r"$\mathregular{x = -\pi}$",
          horizontalalignment="center")
```

