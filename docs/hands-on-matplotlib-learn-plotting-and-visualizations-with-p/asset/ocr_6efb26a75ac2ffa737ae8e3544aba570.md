You can create a simple stream plot as follows:

```
plt.streamplot(X, Y, U, V)
plt.show()
```

Figure 7-10 shows the output.

![](bbox=[0.28, 0.27, 0.73, 0.48])

Figure 7-10. Simple stream plot

You can also have stream plots of variable densities as follows:

```
plt.streamplot(X, Y, U, V,
    density=[0.5, 0.75])
plt.show()
```