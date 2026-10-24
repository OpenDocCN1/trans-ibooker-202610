Figure 10-5 shows the output.

![](bbox=[0.27, 0.15, 0.7, 0.37])

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