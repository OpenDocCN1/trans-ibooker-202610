4. Plot the approximation image and the horizontal, vertical, and diagonal edges (details) detected:

```
fig = plt.figure(figsize=(15, 20))
for i, a in enumerate([LL, LH, HL, HH]):
    ax = fig.add_subplot(2, 2, i + 1)
    a = abs(a)
    if i > 0:
        th = threshold_otsu(a)
        a[a > th] = 1
        a[a <= th] = 0
    ax.imshow(a, interpolation="nearest", cmap=plt.cm.gray)
    ax.set_title(titles[i], fontsize=20)
    ax.set_xticks([])
    ax.set_yticks([])
fig.tight_layout()
plt.show()
```

The following image shows the original input image:

![](bbox=[0.23, 0.44, 0.76, 0.76])

The following image shows the output you will get if you run the preceding code: