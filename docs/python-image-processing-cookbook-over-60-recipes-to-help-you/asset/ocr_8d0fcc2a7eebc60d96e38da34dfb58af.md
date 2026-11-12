6. Run the morphological watershed as you did last time, but this time, with gradient and marker as input (use the local gradient with SE as a disk of a small radius, 2, to keep the edges thin):

```
gradient = rank.gradient(denoised, disk(2))
labels = watershed(gradient, markers)
labels = remove_small_objects(labels, min_size=100)
props = regionprops(labels)
print (len(np.unique(labels)), len(props))
# 111 110
```

If you run the preceding code snippets and plot the images, you will get the following output:

![](bbox=[0.11, 0.34, 0.89, 0.82])