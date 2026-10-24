```
out = plt.contour(X, Y, Z)
plt.clabel(out, inline=True,
          fontsize=10)
plt.show()
```

Figure 7-6 shows the output.

![](bbox=[0.24, 0.24, 0.71, 0.452])

*Simple labeled contour*

You can also add a color bar to the output as follows:

```
out = plt.contour(X, Y, Z)
plt.clabel(out, inline=True,
          fontsize=10)
plt.colorbar(out)
plt.show()
```