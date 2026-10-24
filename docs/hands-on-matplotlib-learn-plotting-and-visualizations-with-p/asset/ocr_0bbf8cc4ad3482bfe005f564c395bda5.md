```
alpha = 0.75, cmap='hot'
C = plt.contour(X, Y, f(X, Y), 8,
    colors='black')
plt.clabel(C, inline=1, fontsize=10)
plt.axis('off')
plt.show()
```

Figure 13-2 shows the output.

![](bbox=[0.28, 0.32, 0.72, 0.52])

Figure 13-2. Visualizing a function as a contour

# 3D Vignettes

You can create a 3D vignette visualization as follows:

```
%matplotlib qt
fig = plt.figure()
ax = plt.axes(projection='3d')
X = np.arange(-4, 4, 0.25)
Y = np.arange(-4, 4, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X ** 2 + Y ** 2)
Z = np.sin(R)
ax.plot_surface(X, Y, Z, rstride=1,
    cstride=1, cmap='hot')
ax.contourf(X, Y, Z, zdir='z',
    offset=-2, cmap='hot')
ax.set_zlim(-2, 2)
```