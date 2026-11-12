
## There's more...

Again, in the previous example, the affine_transform() function was applied to a grayscale image. The same effect can be obtained with a color image also, such as by applying the mapping function to each of the image channels simultaneously and independently. Also, the scikit-image library provides the AffineTransform and PiecewiseAffineTransform classes; you may want to try them to implement affine transformation as well.

## Applying perspective transformation and homography

The goal of perspective (projective) transform is to estimate homography (a matrix, H) from point correspondences between two images. Since the matrix has a Degrees of Freedom (DOF) of eight, you need at least four pairs of points to compute the homography matrix from two images. The following diagram shows the basic concepts required to compute the homography matrix:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_31_0.png)

Fortunately, we don't need to compute the SVD and the H matrix is computed automatically by the `ProjectiveTransform` function from the `scikit-image.transform` module. In this recipe, we will use this function to implement homography.

### Getting ready

We will use a hummingbird's image and an image of an astronaut on the moon (taken from NASA's public domain images) as input images in this recipe. Again, let's start by importing the required libraries as usual:

```python
from skimage.transform import ProjectiveTransform
from skimage.io import imread
import numpy as np
import matplotlib.pyplot as plt
```

### How to do it...

Perform the following steps to apply a projective transformation to an image using the `transform` module from scikit-image:

- 1. First, read the source image and create a destination image with the `np.zeros()` function:

  ```python
  im_src = (imread('images/humming2.png'))
  height, width, dim = im_src.shape
  im_dst = np.zeros((height, width, dim))
  ```

- 2. Create an instance of the `ProjectiveTransform` class:

  ```python
  pt = ProjectiveTransform()
  ```

- 3. You just need to provide four pairs of matching points between the source and destination images to estimate the homography matrix, H, automatically for you. Here, the four corners of the destination image and the four corners of the input hummingbird are provided as matching points, as shown in the following code block:

  ```python
  src = np.array([[ 295., 174.],
     [ 540., 146. ],
     [ 400., 777.],
     [ 60., 422.]])
  dst = np.array([[ 0., 0.],
     [height-1, 0.],
     [height-1, width-1],
     [ 0., width-1]])
  ```

- 4. Obtain the source pixel index corresponding to each pixel index in the destination:

  ```python
  x, y = np.mgrid[:height, :width]
  dst_indices = np.hstack((x.reshape(-1, 1), y.reshape(-1,1)))
  src_indices = np.round(pt(np.inverse(dst_indices), 0).astype(int))
  valid_idx = np.where((src_indices[:,0] < height) &
                   (src_indices[:,1] < width) &
                   (src_indices[:,0] >= 0) & (src_indices[:,1] >=
                   0))
  dst_indices_valid = dst_indices[valid_idx]
  src_indices_valid = src_indices[valid_idx]
  ```

- 5. Copy pixels from the source to the destination images:

  ```python
  im_dst[dst_indices_valid[:,0],dst_indices_valid[:,1]] = im_src[src_indices_valid[:,0],src_indices_valid[:,1]]
  ```

If you run the preceding code snippets, you will get an output like the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_33_0.png)

The next screenshot shows the source image of an astronaut on the moon and the destination image of the canvas. Again, by providing four pairs of mapping points in between the source (corner points) and destination (corners of the canvas), the task is pretty straightforward:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_34_0.png)

The following screenshot shows the output image after the projective transform:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_34_1.png)

### How it works...

In both of the preceding cases, the input image is projected onto the desired location of the output image.

A ProjectiveTransform object is needed to be created first to apply perspective transform to an image.

A set of 4-pixel positions from the source image and corresponding matching pixel positions in the destination image are needed to be passed to the estimate() function along with the object instance and this computes the homography matrix, H (and returns True if it can be computed).

The inverse() function is to be called on the object and this will give you the source pixel indices corresponding to all destination pixel indices.

### There's more...

You can use the warp() function (instead of the inverse() function) to implement homography/projective transform.

### See also

For more details, refer to the following links:

- https://www.youtube.com/watch?v=YwIB9PbQkEM
- https://www.youtube.com/watch?v=2ggjHjRx2SQ
- https://www.youtube.com/watch?v=vviNh5y71ss

## Creating pencil sketches from images

Producing sketches from images is all about detecting edges in images. In this recipe, you will learn how to use different techniques, including the difference of Gaussian (and its extended version, XDOG), anisotropic diffusion, and dodging (applying Gaussian blur + invert + thresholding), to obtain sketches from images.

### Getting ready

The following libraries need to be imported first:

```python
import numpy as np
from skimage.io import imread
from skimage.color import rgb2gray
from skimage import util
from skimage import img_as_float
import matplotlib.pyplot as plt
from medpy.filter.smoothing import anisotropic_diffusion
from skimage.filters import gaussian, threshold_otsu
```

### How to do it...

The following steps need to be performed:

- 1. Define the normalize() function to implement min-max normalization in an image:

  ```python
  def normalize(img):
      return (img-np.min(img)) / (np.max(img)-np.min(img))
  ```

- 2. Implement the sketch() function that takes an image and the extracted edges as input:

  ```python
  def sketch(img, edges):
      output = np.multiply(img, edges)
      output[output>1]=1
      output[edges==1]=1
      return output
  ```

- 3. Implement a function to extract the edges from an image with anisotropic diffusion:

  ```python
  def edges_with_anisotropic_diffusion(img, niter=100, kappa=10,
  gamma=0.1):
      output = img - anisotropic_diffusion(img, niter=niter, \\
      kappa=kappa, gamma=gamma, voxelspacing=None, \\
      option=1)
      output[output > 0] = 1
      output[output < 0] = 0
      return output
  ```

- 4. Implement a function to extract the edges from an image with the dodge operation (there are two implementations):

  ```python
  def sketch_with_dodge(img):
      orig = img
      blur = gaussian(util.invert(img), sigma=20)
      result = blur / util.invert(orig)
      result[result>1] = 1
      result[orig==1] = 1
      return result

  def edges_with_dodge2(img):
      img_blurred = gaussian(util.invert(img), sigma=5)
      output = np.divide(img, util.invert(img_blurred) + 0.001)
      output = normalize(output)
      thresh = threshold_otsu(output)
      output = output > thresh
      return output
  ```

- 5. Implement a function to extract the edges from an image with a Difference of Gaussian (DOG) operation:

  ```python
  def edges_with_DOG(img, k = 200, gamma = 1):
      sigma = 0.5
      output = gaussian(img, sigma=sigma) - gamma*gaussian(img, \\
      sigma=k*sigma)

      output[output > 0] = 1
      output[output < 0] = 0
      return output
  ```

- 6. Implement a function to produce sketches from an image with an Extended Difference of Gaussian (XDOG) operation:

  ```python
  def sketch_with_XDOG(image, epsilon=0.01):
      phi = 10
      difference = edges_with_DOG(image, 200, 0.98).astype(np.uint8)
      for i in range(0, len(difference)):
          for j in range(0, len(difference[0])):
              if difference[i][j] >= epsilon:
                  difference[i][j] = 1
              else:
                  ht = np.tanh(phi*(difference[i][j] - epsilon))
                  difference[i][j] = 1 + ht
      difference = normalize(difference)
      return difference
  ```

If you run the preceding code and plot all of the input/output images, you will obtain an output like the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_38_0.png)
