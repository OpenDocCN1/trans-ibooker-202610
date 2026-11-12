Finally, if you plot the original, noisy input and the denoised output images, you will get the following output:

![](bbox=[0.21, 0.165, 0.789, 0.328])

# See also

Refer to the following articles to learn more about this recipe:

+   * http://homepages.ecs.vuw.ac.nz/~marslast/MLbook.html
+   * https://www.youtube.com/watch?v=YAc0i36drEs

# Image inpainting

Inpainting is the process of restoring the damaged or missing parts of an image. Suppose we have a binary input image, D, that specifies the location of the damaged pixels in the input image, f, as shown in the following equation:

```
D(x, y) = { 0 if pixel (x, y) is damaged in image f
         { 1 if pixel (x, y) is not damaged in image f
```

Once the damaged regions in the image are located with the mask, the lost/damaged pixels have to be reconstructed with some algorithm (for example, total variation inpainting). The reconstruction is supposed to be performed fully automatically by exploiting the information presented in non-damaged regions. In this recipe, you will learn how to use OpenCV-Python library functions to implement image inpainting, using two different algorithms.