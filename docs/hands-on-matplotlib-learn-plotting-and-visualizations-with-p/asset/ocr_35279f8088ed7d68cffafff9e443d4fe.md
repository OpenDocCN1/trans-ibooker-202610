Figure 8-1 shows the output.

![](bbox=[0.35, 0.14, 0.67, 0.35])

# Figure 8-1. Visualizing an image

This is a color image. The Matplotlib library automatically detects that the image has multiple channels and shows it as a color image. However, it goofs up a little bit when we show grayscale images.

```
img2 = plt.imread("D:/Dataset/5.1.11.tiff")
plt.imshow(img2)
plt.show()
```

Figure 8-2 shows the output.

![](bbox=[0.36, 0.61, 0.67, 0.82])

# Figure 8-2. Visualizing a grayscale image