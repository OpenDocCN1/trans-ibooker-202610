```
image = create_image_stack('images/highway.mp4') #cloud.mp4
stack,h,w = image.shape
```

+   2. Use the sobel() function from mahotas as the pixel-level measure of infocusness:

```
focus = np.array([mh.sobel(t, just_filter=True) for t in image])
```

+   3. At each pixel location, select the best slice (with maximum infocusness) and create the final image:

```
best = np.argmax(focus, 0)
image = image.reshape((stack,-1)) # image is now (stack, nr_pixels)
image = image.transpose() # image is now (nr_pixels, stack)
final = image[np.arange(len(image)), best.ravel()] # Select the
right pixel at each location
final = final.reshape((h,w)) # reshape to get final result
```

The following photo is an input image used in the image stack:

![](bbox=[0.1, 0.43, 0.9, 0.81])