Fortunately, we don't need to compute the SVD and the H matrix is computed automatically by the `ProjectiveTransform` function from the `scikit-image.transform` module. In this recipe, we will use this function to implement homography.

# # Getting ready

We will use a humming bird's image and an image of an astronaut on the moon (taken from NASA's public domain images) as input images in this recipe. Again, let's start by importing the required libraries as usual:

```
from skimage.transform import ProjectiveTransform
from skimage.io import imread
import numpy as np
import matplotlib.pyplot as plt
```

# # How to do it...

Perform the following steps to apply a projective transformation to an image using the `transform` module from scikit-image:

+   1. First, read the source image and create a destination image with the `np.zeros ()` function:
+   
+      ```python
+      im_src = (imread('images/humming2.png'))
+      height, width, dim = im_src.shape
+      im_dst = np.zeros((height, width, dim))
+      ```

+   2. Create an instance of the `ProjectiveTransform` class:
+   
+      ```python
+      pt = ProjectiveTransform()
+      ```