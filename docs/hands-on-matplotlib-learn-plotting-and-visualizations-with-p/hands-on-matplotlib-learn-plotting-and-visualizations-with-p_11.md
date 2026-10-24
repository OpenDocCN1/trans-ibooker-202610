# 第 11 章

# Matplotlib 中的 3D 可视化

在上一章中，你学习了如何在 Matplotlib 中处理颜色。

在本章中，你将学习如何使用 3D 可视化。以下是本章将要学习的主题：

- 绘制 3D 线图、散点图和等高线图
- 使用线框图、曲面和示例数据
- 绘制柱状图
- 绘制箭头图（quiver）和茎叶图（stem）
- 使用 3D 体积

线框图、曲面和 3D 等高线图用于显示体积数据。柱状图用于显示分类数据。箭头图用于可视化向量。阅读完本章后，你将能够在 Matplotlib 中使用所有这些 3D 可视化。

## 准备工作

我建议你为本章的所有示例创建一个新的 notebook。在开始之前，你需要安装一个额外的库，如下所示：

```
!pip3 install PyQt5
```

Qt 是一个用于 GUI 的跨平台库。PyQt5 是 Qt 的 Python 绑定。安装该库后，你可以使用以下魔法命令强制 Jupyter Notebook 在单独的 QT 窗口中显示可视化结果：

```
%matplotlib qt
```

# 第 11 章 Matplotlib 中的 3D 可视化

因此，当你创建可视化时，还可以与它们进行交互。让我们来学习基础知识。首先，我们导入所有必需的库，如下所示：

```
```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits import mplot3d
```
```

然后我们创建一个 figure 对象，如下所示：

```
```python
fig = plt.figure()
```
```

然后按如下方式创建 3D 坐标轴：

```
```python
ax = plt.axes(projection='3d')
```
```

之后，你必须添加用于可视化的代码。不过，对于这个示例，你将使用以下代码行为一个空的图形和坐标轴创建可视化：

```
```python
plt.show()
```
```

图 11-1 显示了输出结果。





## CHAPTER 11 Matplotlib 中的 3D 可视化

![](img/124f805c8155c7226d129ebbcc2842f0_154_0.png)

**Figure 11-1.** *一个空的且可交互的 3D 可视化*

你可以使用鼠标与此可视化进行交互并改变其方向。在继续之前，请花时间探索所有交互可能性。

## 绘制 3D 线条

让我们绘制一条 3D 线条。让我们按如下所示创建图形和坐标轴：

```python
fig = plt.figure()
ax = plt.axes(projection='3d')
```

让我们按如下方式创建 3D 数据：

```python
z = np.linspace(0, 30, 1000)
x = np.sin(z)
y = np.cos(z)
```

你可以按如下方式创建 3D 绘图：

```python
ax.plot3D(x, y, z, 'red')
plt.show()
```

图 11-2 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_155_0.png)

**Figure 11-2.** 3D 线性图

## 3D 散点图

你可以创建随机点并使用 3D 散点显示它们，如下所示。让我们首先按如下所示创建图形和坐标轴：

```python
fig = plt.figure()
ax = plt.axes(projection='3d')
```

```python
X = np.linspace(-10, 10, 30)
y = np.linspace(-10, 10, 30)
X, Y = np.meshgrid(X, y)
Z = np.sin(np.sqrt(X ** 2 + Y ** 2))
```

你可以按如下方式创建等高线：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
ax.contour(X, Y, Z, 50, cmap='coolwarm')
plt.show()
```

图 11-4 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_157_0.png)

**Figure 11-4.** 3D 等高线图

你可以使用以下代码获得与图 11-4 中可视化相似的输出：

```python
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.contour3D(X, Y, Z, 40,
    cmap='coolwarm')
plt.show()
```

你也可以使用 contourf() 函数按如下方式创建填充等高线：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
ax.contourf(X, Y, Z, 50, cmap='coolwarm')
plt.show()
```

## 线框、曲面和示例数据

你可以按如下方式绘制相同数据集的线框：

```python
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_wireframe(X, Y, Z, color='Green')
ax.set_title('wireframe')
plt.show()
```

图 11-5 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_159_0.png)

**Figure 11-5.** 3D 线框

相同的数据可以按如下方式可视化为 3D 曲面：

```python
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_surface(X, Y, Z, color='Blue')
ax.set_title('Surface Plot')
plt.show()
```

图 11-6 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_160_0.png)

**Figure 11-6.** *3D 曲面*

你也可以使用随 Matplotlib 库提供的示例数据来演示可视化。`get_test_data()` 函数可以按如下方式获取该示例数据：

```python
from mpl_toolkits.mplot3d import axes3d
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
X, Y, Z = axes3d.get_test_data(0.02)
ax.plot_wireframe(X, Y, Z,
                  rstride=10,
                  cstride=10)
plt.show()
```

图 11-7 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_161_0.png)

**Figure 11-7.** 可视化测试数据

作为练习，尝试使用测试数据创建曲面和等高线。

## 条形图

你可以在 3D 坐标轴中显示 2D 条形。让我们按如下所示创建图形和坐标轴：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
```

让我们为条形定义颜色。

```python
colors = ['r', 'g', 'b', 'c', 'm', 'y','k']
yticks = [0, 1, 2, 3, 4, 5, 6]
```

现在，让我们使用以下循环以定义的颜色创建条形图：

```python
for c, k in zip(colors, yticks):
    x = np.arange(25)
    y = np.random.rand(25)
    ax.bar(x, y, zs=k, zdir='y',
           color=c, alpha=0.8)
plt.show()
```

图 11-8 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_162_0.png)

**Figure 11-8.** 在 3D 坐标中可视化 2D 条形

你也可以使用 Matplotlib 创建 3D 条形图。让我们首先按如下所示创建数据：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')

x = np.arange(10) * np.arange(10)
y = np.arange(10) * np.arange(10)
x, y = np.meshgrid(x, y)
x, y = x.ravel(), y.ravel()
top = x + y
bottom = np.zeros_like(top)
width = depth = 5
```

然后你可以按如下方式将其显示为 3D 条形：

```python
ax.bar3d(x, y, bottom, width,
         depth, top,
         shade=True,
         color='g')
plt.show()
```

图 11-9 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_164_0.png)

**Figure 11-9.** 可视化 3D 条形

## 箭羽图和茎图

箭羽图用于表示方向性实体（例如，矢量）。让我们按如下所示定义数据：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
x = y = z = np.arange(-0.1, 1, 0.2)
X, Y, Z = np.meshgrid(x, y, z)
u = np.cos(np.pi * X) * np.sin(np.pi * Y) * np.sin(np.pi * Z)
v = -np.sin(np.pi * X) * np.cos(np.pi * Y) * np.sin(np.pi * Z)
w = np.sin(np.pi * X) * np.sin(np.pi * Y) * np.cos(np.pi * Z)
```

最后，你可以按如下方式可视化数据：

```python
ax.quiver(X, Y, Z, u, v, w,
          length=0.1,
          normalize=True)
plt.show()
```

图 11-10 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_165_0.png)

**Figure 11-10.** *箭羽图*

你也可以创建茎图，其中在可视化中绘制垂直线。让我们使用三角函数按如下所示定义数据：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
theta = np.linspace(0, 2 * np.pi)

x = np.sin(theta)
y = np.cos(theta)
z = np.cos(theta)
```

你可以按如下方式可视化茎图：

```python
ax.stem(x, y, z)
plt.show()
```

图 11-11 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_166_0.png)

**Figure 11-11.** 茎图

## 3D 体积

你可以将 3D 体积数据显示为封闭曲面。让我们按如下方式创建此类数据：

```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
u = np.linspace(0, 2 * np.pi, 100)
v = np.linspace(0, np.pi, 100)
x = 10 * np.outer(np.cos(u), np.sin(v))
y = 10 * np.outer(np.sin(u), np.sin(v))
z = 10 * np.outer(np.ones(np.size(u)), np.cos(v))
```

你可以按如下方式将此数据显示为球体：

```python
ax.plot_surface(x, y, z)
plt.show()
```

图 11-12 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_167_0.png)

**Figure 11-12.** 作为体积的曲面图

你也可以使用 voxels() 函数按如下方式可视化体积：

```python
ma = np.random.randint(1, 3, size=(3, 3, 3))
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.voxels(ma, edgecolor='k')
plt.show()
```

图 11-13 显示了输出。

![](img/124f805c8155c7226d129ebbcc2842f0_168_0.png)

**Figure 11-13.** 3D 体积图

## 总结

在本章中，你学习了如何使用 3D 可视化。线框、曲面和 3D 等高线用于显示体积数据。条形图用于显示分类数据。箭羽图用于可视化矢量。

在下一章中，你将学习如何创建动画。

## Matplotlib 中的动画

在上一章中，你学习了如何在 Matplotlib 中使用 3D 可视化。在本章中，你将学习如何使用动画。本章将学习以下主题：

- Animation basics
- Celluloid library

阅读本章后，你将能够在 Matplotlib 和另一个有用的库中使用动画。

## 动画基础

在本节中，你将学习如何使用 Matplotlib 创建动画。首先让我们为本章创建一个新的笔记本。然后导入以下库：

```python
%matplotlib qt
import numpy as np
from matplotlib import pyplot as plt
from matplotlib.animation import FuncAnimation
```

让我们按如下方式定义对象，即图形、坐标轴和绘图：

```python
fig = plt.figure()
ax = plt.axes(xlim=(0, 4), ylim=(-2, 2))
line, = ax.plot([], [], lw=3)
```





Figure **12-1** shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_171_0.png)

You can interact with animation and change the orientation with your mouse.

Explore all the interactive possibilities before proceeding further.

You can create a progressive spiral, as shown here:

```
```python
fig = plt.figure()
ax = plt.axes(xlim=(-50, 50), ylim=(-50, 50))
line, = ax.plot([], [], lw=2)
def init():
    line.set_data([], [])
    return line,
xdata, ydata = [], []
def animate(i):
    t = 0.2*i
```
```

```
x = t*np.cos(t)
y = t*np.sin(t)
xdata.append(x)
ydata.append(y)
line.set_data(xdata, ydata)
return line,
anim = FuncAnimation(fig, animate,
    init_func=init,
    frames=3000,
    interval=5,
    blit=True)
```

Figure 12-2 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_172_0.png)

Figure 12-2. Visualizing a spiral animation

## Celluloid Library

You can use another simple library called Celluloid for animation. Let’s install it as follows:

```
!pip3 install celluloid
```

You can import it as follows:

```
from celluloid import Camera
```

Let’s create a figure and camera object as follows:

```
fig = plt.figure()
camera = Camera(fig)
```

Let’s create the frames of an animation and save them in memory with the function called camera.snap(), as follows:

```
for i in range(10):
    plt.plot([i] * 10)
    camera.snap()
```

Let’s create the animation as follows:

```
animation = camera.animate()
```

Figure 12-3 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_174_0.png)

Figure 12-3. Animation with the Celluloid library

You can also create a sine wave as follows:

```
fig, axes = plt.subplots()
camera = Camera(fig)
t = np.linspace(0, 2 * np.pi, 128, endpoint=False)
for i in t:
    plt.plot(t, np.sin(t + i), color='green')
    camera.snap()
animation = camera.animate()
```

Figure 12-4 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_175_0.png)

Figure 12-4. Sine wave animation with the Celluloid library

Another example with a bar graph is as follows:

```
fig, axes = plt.subplots()
camera = Camera(fig)
y = np.arange(5)
for i in y:
    plt.bar( np.random.rand(5)*10 , y)
    camera.snap()
animation = camera.animate()
```

Figure 12-5 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_176_0.png)

**Figure 12-5.** *Bar graph animation with the Celluloid library*

## Summary

In this chapter, you learned how to work with animations.

In the next chapter, you will learn how to do even more with Matplotlib.
