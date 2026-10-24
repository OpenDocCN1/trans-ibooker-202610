![](bbox=[0.14, 0.1, 0.82, 0.48])

# Figure 11-5. 3D wireframe

The same data can be visualized as a 3D surface as follows:

```
fig = plt.figure()
ax = plt.axes(projection='3d')
ax.plot_surface(X, Y, Z, color='Blue')
ax.set_title('Surface Plot')
plt.show()
```