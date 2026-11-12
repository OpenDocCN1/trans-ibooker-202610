# Restoring an image with the Wiener filter

The Wiener filter is **Mean Squared Error (MSE)** filtering that incorporates both the degradation function and the statistical characteristics of noise. The underlying assumption is that the noise and image are uncorrelated. It optimizes the filter so that MSE is minimized. In this recipe, you will learn how to implement the Wiener filter using functions from scikit-image restoration module and how to apply the filter to restore a degraded image, both in a supervised and unsupervised manner.

# Getting ready

In this recipe, we shall use a cactus image as input and corrupt it with noise/blur. As usual, let's first import all of the required libraries using the following lines of code:

```
from skimage.io import imread
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.ticker import LinearLocator, FormatStrFormatter
```

# How to do it...

Follow the steps listed to implement the Wiener filter and apply it to restore images corrupted with blur/noise:

+   1. Read the image and convert it into grayscale:
+   
+   ```python
+   im = color.rgb2gray(imread('images/cactus.png'))
+   ```

+   2. Define `convolve2d()` to implement the convolution in the frequency domain (by the convolution theorem, this implementation works much faster than the one in the spatial domain). Construct the 7x7 average (box-blur) point-spread function and use it as convolution-kernel to blur the image, with the following code:

```
def convolve2d(im, psf, k):
    M, N = im.shape
    freq = fp.fft2(im)
    # assumption: min(M,N) > k > 0, k odd
    psf = np.pad(psf, (((M-k) // 2, (M-k) // 2 + 1), \
                     ((N-k) // 2, (N-k) // 2 + 1)), mode='constant ')
    freq_kernel = fp.fft2(fp.ifftshift(psf))
    return np.abs(fp.ifft2(freq*freq_kernel))
```