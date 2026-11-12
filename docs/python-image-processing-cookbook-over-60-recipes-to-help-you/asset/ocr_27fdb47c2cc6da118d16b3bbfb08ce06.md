3. **Computing the mid-way (morphed) face**: Before computing the whole morph sequence, compute the mid-way face of your source and destination images. This involves computing the average shape (that is, the average of each keypoint location in the two faces): compute the location of feature points in the morphed image, M. For each triangle in the source image, compute the affine transform that maps three corners of the triangle to the corresponding corner points of a triangle in the morphed image, and then transform all pixels inside the triangle to the morphed image using the affine transform just computed. Finally, alpha-blend these two images and this is your final morphed image.

In this recipe, you will learn how to implement face morphing between a human and a lion face, where the facial landmarks for the human face are obtained automatically using the dlib library's facial landmark extraction, and the corresponding feature points in the lion's face are manually created.

# Getting ready

Let's first start by importing all of the required libraries that you will need to implement face morphing:

```
from scipy.spatial import Delaunay
from skimage.io import imread
import scipy.misc
import cv2
import dlib
import numpy as np
from matplotlib import pyplot as plt
```