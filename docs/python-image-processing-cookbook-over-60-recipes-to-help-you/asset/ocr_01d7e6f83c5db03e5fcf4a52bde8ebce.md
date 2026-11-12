If you plot the gradient image in 3D, you will get the following output:

![](bbox=[0.245, 0.145, 0.755, 0.615])

# Blob detection with morphological watershed

Perform the following steps to use the `skimage.morphology` module's watershed again, this time for blob detection:

+   1. Implement the `segment_with_watershed()` function, which will detect the blobs in an image:

```
def segment_with_watershed(im, cell_thresh, bg_thresh):
    if np.max(im) != 1.0:
        im = (im - im.min()) / (im.max() - im.min())
    im_mask = im < cell_thresh
```