Now, if you plot the original image, you will get an output like the following screenshot:

![](bbox=[0.23, 0.165, 0.77, 0.765])

+   3. Detect faces in the grayscale image using the following lines of code.

```
rects = detector(gray, 2)
print ('Number of faces detected:', len(rects))
# 17
```