
N = 20
theta = np.linspace(0.0, 2 * np.pi, N)
r = 10 * np.random.rand(N)

The set of points can be visualized as follows:

```
plt.subplot(projection='polar')
plt.bar(theta, r, bottom=0.0,
    color=['r', 'g', 'b'], alpha=0.2)
plt.show()
```

Figure 9-7 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_139_0.png)

# Figure 9-7. A simple polar graph

You can see that this creates a bar visualization, as shown in Figure 9-7. There are a few useful video tutorials on YouTube where you can learn more about creating visualizations in the polar coordinate system.

```
https://www.youtube.com/watch?v=mDT_DG_A0JA
https://www.youtube.com/watch?v=GMcRqtm4mNo
https://www.youtube.com/watch?v=VmQ1isayjJI
```

Let’s create a simple graph. Let’s create the dataset for it as shown here:

```
r = np.arange(0, 5, 0.2)
theta = 2 * np.pi * r
plt.subplot(projection='polar')
plt.plot(theta, r)
plt.show()
```

This creates a simple linear visualization on a polar graph. As this is a polar graph, you will see a spiral-like structure, as shown in Figure 9-8.

![](img/124f805c8155c7226d129ebbcc2842f0_140_0.png)

Figure 9-8. A simple linear visualization on a polar graph

This is not a perfect spiral as the distance between the consecutive points is 0.2. If you reduce the distance, then you will get a perfect spiral. Let’s tweak the data as follows:

```
r = np.arange(0, 5, 0.01)
theta = 2 * np.pi * r
plt.subplot(projection='polar')
plt.plot(theta, r)
plt.show()
```

This creates a perfect spiral, as shown in Figure 9-9.

![](img/124f805c8155c7226d129ebbcc2842f0_141_0.png)

Figure 9-9. A spiral visualization

Let’s see a couple of examples of scatter plots on a polar graph. To start, prepare the data as shown here:

```
N = 150
r = np.random.rand(N)
theta = 2 * np.pi * np.random.rand(N)
size = r * 100
```

You can visualize this as follows:

```
plt.subplot(projection='polar')
plt.scatter(theta, r, c=theta,
    s=size, cmap='hsv',
    alpha=0.5)

plt.show()
```

The output shows only part of the entire polar graph, as shown in Figure 9-11.

![](img/124f805c8155c7226d129ebbcc2842f0_143_0.png)

Figure 9-11. A partial scatter plot

As an exercise, you may want to create partial spirals and bar graphs.

# Summary

In this chapter, learned how to create pie charts and polar charts in detail.

In the next chapter, you will learn how to create a few more visualizations, namely, using the routines pColor(), pColormesh(), and colorbar().

# CHAPTER 10

# Working with Colors

In the previous chapter, you learned how to visualize pie charts and polar charts with Matplotlib.

In this chapter, you will learn how to work with colors. The following are the routines you will learn to use in this chapter:

- pcolor()
- pcolormesh()
- colorbar()

After reading this chapter, we will be able to work with colors in Matplotlib.

## pcolor()

The routine pcolor() creates a pseudocolor plot with a rectangular (nonsquare) grid. Pseudocolor means the object or image is rendered in colors different than those in which it was recorded. Let’s create a new notebook for this chapter, as shown here:

```
%matplotlib inline
import matplotlib.pyplot as plt
import numpy as np
```

Let’s create a nonsquare matrix and use the routine pcolor() to visualize it, as shown here:

```
data = np.random.rand(5, 6)
plt.pcolor(data)
plt.show()
```

Figure 10-1 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_145_0.png)

Figure 10-1. A simple demonstration of pcolor()

You can also use custom color maps as follows:

```
plt.pcolor(data, cmap='YlGnBu_r')
plt.show()
```

Figure 10-2 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_145_1.png)

Figure 10-2. A simple demonstration of pcolor() with color maps

Figure 10-4 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_147_0.png)

Figure 10-4. Shading with nearest

## pcolormesh()

The routine pcolormesh() behaves in the same way as pcolor(); however, it renders large datasets much faster. Let’s create a visualization of the same dataset used for Figure 10-4 but with pcolormesh(). The code is as follows:

```
plt.pcolormesh(X, Y, Z,
    cmap='YlGnBu_r',
    shading='auto')
plt.show()
```

Let’s see an example with shading and a color map, as shown here:

```
nrows = ncols = 5

x = np.arange(ncols + 1)
y = np.arange(nrows + 1)
z = np.arange(nrows * ncols).reshape(nrows, ncols)
plt.pcolormesh(x, y, z,
    shading='flat',
    cmap='coolwarm')

plt.show()
```

Figure 10-5 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_148_0.png)

Figure 10-5. Using pcolormesh() with shading and a color map

Run the following example with different arguments:

```
plt.pcolormesh(x, y, z,
    shading='auto',
    cmap='cool')
plt.show()
```

You can also apply a simple geometric transformation to the dataset as follows:

```
z = np.random.rand(6, 10)
x = np.arange(0, 10, 1)
y = np.arange(4, 10, 1)
T = 0.5
X, Y = np.meshgrid(x, y)
X = X + T * Y
Y = Y + T * X
plt.pcolormesh(X, Y, Z,
    shading='auto')
plt.show()
```

Figure 10-6 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_149_0.png)

**Figure 10-6.** *Using pcolormesh() with a simple transformation*

## colorbar()

You can also add a color bar that correlates with the magnitude of data points in the visualization. The routine colorbar() does the trick. The following is the code:

```
N = 100
X, Y = np.meshgrid(np.linspace(-5, 5, N),
                   np.linspace(-5, 5, N))
Z = (X**2 + Y**2)
img = plt.imshow(Z, cmap='YlGnBu_r')
plt.colorbar(img)
plt.show()
```

You can also extend the color bar as follows:

```
img = plt.imshow(Z, cmap='coolwarm')
plt.colorbar(img, extend='both')
plt.show()
```

Figure 10-9 shows the output.

![](img/124f805c8155c7226d129ebbcc2842f0_151_0.png)

Figure 10-9. Extended color bar

# Summary

In this chapter, you learned how to work with colors. In the next chapter, you will learn how to create 3D visualizations.

# CHAPTER 11

# 3D Visualizations in Matplotlib

In the previous chapter, you learned how to work with colors in Matplotlib.
In this chapter, you will learn how to work with 3D visualizations. The following are the topics you will learn about in this chapter:

- Plotting 3D lines, scatter plots, and contours
- Working with wireframes, surfaces, and sample data
- Plotting bar graphs
- Plotting quiver and stem plots
- Working with 3D volumes

Wireframes, surfaces, and 3D contours are used to show volumetric data. Bar graphs are used to show categorical data. Quiver plots are used for visualizing vectors. After reading this chapter, you will be able to work with all these 3D visualizations in Matplotlib.

## Getting Ready

I recommend that you create a new notebook for all the examples in this chapter. To get ready, you need to install one additional library as follows:

```
!pip3 install PyQt5
```

Qt is a cross-platform library for GUI. PyQt5 is the Python binding for Qt. Once the library is installed, you can use the following magical command to force Jupyter Notebook to show the visualizations in a separate QT window:

```
%matplotlib qt
```

# CHAPTER 11 3D VISUALIZATIONS IN MATPLOTLIB

So, when you create visualizations, you are also able to interact with them. Let’s learn the basics. First, we import all the required libraries, as shown here:

```
```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits import mplot3d
```
```

Then we create a figure object, as shown here:

```
```python
fig = plt.figure()
```
```

Then we create a 3D axis as follows:

```
```python
ax = plt.axes(projection='3d')
```
```

You have to add the code for the visualization after this. However, for this example, you will create the visualization for an empty figure and axes with the following line:

```
```python
plt.show()
```
```

Figure 11-1 shows the output.
