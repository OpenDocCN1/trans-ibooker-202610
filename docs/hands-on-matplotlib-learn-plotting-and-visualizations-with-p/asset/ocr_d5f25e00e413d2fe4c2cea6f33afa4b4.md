# CHAPTER 11　3D VISUALIZATIONS IN MATPLOTLIB

```
```python
X = np.linspace(-10, 10, 30)
y = np.linspace(-10, 10, 30)
X, Y = np.meshgrid(X, y)
Z = np.sin(np.sqrt(X ** 2 + Y ** 2))
```
```

You can create a contour as follows:

```
```python
fig = plt.figure()
ax = fig.add_subplot(projection='3d')
ax.contour(X, Y, Z, 50, cmap='coolwarm')
plt.show()
```
```

Figure 11-4 shows the output.

![](bbox=[0.143, 0.388, 0.817, 0.793])

*Figure 11-4.* 3D contour plot