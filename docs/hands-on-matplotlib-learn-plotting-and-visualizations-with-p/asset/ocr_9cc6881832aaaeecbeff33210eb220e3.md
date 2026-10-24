Figure 4-13 shows the output.

![](bbox=[0.27, 0.15, 0.72, 0.35])

# Figure 4-13. Demo of colors

You can also write the previous code as follows:

```
plt.plot(x, y+0.4, 'g', x, y+0.2, 'y', x, y, 'r', x, y-0.2, 'c', x, y-0.4, 'k', x, y-0.6, 'm', x, y-0.8, 'w', x, y-1, 'b')
plt.show()
```

The output will be the same as Figure 4-13.

You can customize the line style as follows:

```
plt.plot(x, y, '-', x, y+1, '--', x, y+2, '-.', x, y+3, ':')
plt.show()
```

Figure 4-14 shows the output.