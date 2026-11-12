If you run the preceding code snippets and plot the images, you will get a diagram along the lines of the following:

![](bbox=[0.185, 0.16, 0.815, 0.65])

5. Now, run the morphological watershed algorithm on another image (of a lotus), but this time, create the markers by thresholding on the gradient (find continuous low gradient regions as markers and use a disk SE with a radius of 5 to get a smoother image). Denoise the image with the `median` filter to start with:

```
denoised = rank.median(image, disk(2))
markers = rank.gradient(denoised, disk(5)) < 20
markers = ndi.label(markers)[0]
```