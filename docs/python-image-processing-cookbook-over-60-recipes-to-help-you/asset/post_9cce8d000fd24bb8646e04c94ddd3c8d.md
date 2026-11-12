
The `fft2()` function of `scipy.fftpack` was used to transform an image from the spatial to the frequency domain by forcing all the high-frequency basis vectors to be zero (except the lowest thirty basis vectors) and then the image was reconstructed with `ifft2()`.

Finally, the `pywt.Wavelet( 'haar' )` function was used to instantiate a Haar wavelet object. The `pywt.wavdec2()` function was used to get all the wavelet coefficients, and then `pywt.waverec2()` was used to reconstruct the image from the coefficients by discarding the last band.

If you run the preceding code blocks, you will obtain an output along the lines of the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_69_0.png)

As can be seen from the preceding screenshot, PCA seems to do the best job in terms of reconstruction here.

### There's more...

Try to implement PCA yourself using SVD and then project an image on the dominant singular vectors (you may use the `TruncatedSVD()` function from scikit-learn too). Use PSNR values to compare the output (denoised) image quality for different techniques.

### See also

For more information, refer to the following links:

- https://scikit-learn.org/0.19/datasets/olivetti_faces.html
- https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html
- https://www.youtube.com/watch?v=jDnIkyxwX4Y

## Image denoising with anisotropic diffusion

In this recipe, you will learn how to use the anisotropic (heat) diffusion equation to denoise an image preserving the edges by using a `medpy` library function. Isotropic diffusion, on the other hand, is identical to applying a Gaussian filter, which does not preserve the edges in an image, as we have already seen.

### Getting ready

In this recipe, we will use the cameraman grayscale image. As usual, let's start by importing the required libraries:

```
from medpy.filter.smoothing import anisotropic_diffusion
from skimage.util import random_noise
from skimage.io import imread
from skimage import img_as_float
import matplotlib.pyplot as plt
import numpy as np
```

### How to do it...

Perform the following steps to denoise an image with the implementation of Perona-Malik anisotropic diffusion:

- 1. First, read the cameraman grayscale image from disk and add random Gaussian noise (with a variance of 0.025) to it. Clip the noisy image to have values strictly between 0 and 1 (inclusive):

```
img = img_as_float(imread('images/cameraman.png'))
noisy = random_noise(img, var=0.005)
noisy = np.clip(noisy, 0, 1)
```

- 2. Plot the original and noisy images:

```
plt.figure(figsize=(15,18))
plt.gray()
plt.subplots_adjust(0,0,1,1,0.05,0.05)
plt.subplot(221), plt.imshow(img), plt.axis('off'),
plt.title('Original', size=20)
plt.subplot(222), plt.imshow(noisy), plt.axis('off'),
plt.title('Noisy', size=20)
```

- 3. Apply anisotropic diffusion (according to Perona-Malik equation 1) with parameters of kappa=20 and niter=20 to denoise the noisy image. Plot the smoothed image:

```
diff_out = anisotropic_diffusion(noisy, niter=20, kappa=20,
option=1)
plt.subplot(223), plt.imshow(diff_out), plt.axis('off'), \\
plt.title(r'Anisotropic Diffusion (Perona Malik eq 1, \\
iter=20, $\kappa=20$)', size=20)
```

- 4. Again, apply anisotropic diffusion (according to Perona-Malik equation 2 this time), with parameters of kappa=50 and niter=50 to denoise the noisy image. Plot the smoothed image:

```
diff_out = anisotropic_diffusion(noisy, niter=50, kappa=50,
option=2)
plt.subplot(224), plt.imshow(diff_out), plt.axis('off'), \\
plt.title(r'Anisotropic Diffusion (Perona Malik eq 2, \\
iter=50, $\kappa=50$)', size=20)
plt.show()
```

If you run the preceding code snippets, you will get a diagram along the lines of the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_72_0.png)

You can see from the preceding output that, as the \kappa value and the number of iterations are increased, the image becomes more blurry.

### How it works...

Anisotropic diffusion was used to smooth (denoise) an image by keeping the edges mostly unchanged (even sharpened). In the anisotropic diffusion process (which is an iterative process), the Gaussian kernel is used as a conductivity function (c), according to Perona-Malik equation 1, as shown in the following screenshot:

Anisotropic Heat Diffusion Equation

$I_t = div(c(x, y, t)\nabla I) = c(x, y, t).\Delta I + \nabla c \cdot \nabla I$
Image at time t divergence Gradient conductivity function Laplacian

$= \nabla \cdot (c\nabla I) = \frac{\partial}{\partial x}(cI_x) + \frac{\partial}{\partial y}(cI_y)$

calculate the conductivity function c every iteration based on the current image I

$c(x, y, t) = g(\| \nabla I(x, y, t) \|)$

Perona-Malik edge-stopping function

$g(\nabla I) = e^{( - (\| \nabla I \| / K)^2 )} \quad (\text{equation 1})$

$g(\nabla I) = \frac{1}{1 + \left( \frac{\| \nabla I \|}{K} \right)^2} \quad (\text{equation 2})$

Taken from http://image.diku.dk/imagecanon/material/PeronaMalik1990.pdf

The `anisotropic_diffusion()` function from the filter.smoothing module in the medpy library was used to implement the diffusion process. The niter parameter to this function represents the number of iterations on which the diffusion process is to be run. The kappa (K) parameter is an integer representing the conduction coefficient (for example, with values between 20 – 100). K controls the conduction, which is a function of the gradient. When K is small, the conduction and the diffusion across steep edges will be blocked. A higher K value decreases the impact of intensity gradients on conduction. The gamma parameter controls the speed of diffusion, and a gamma value of ≤ 0.25 should be used for stability.

### There's more...

Compare the denoised image obtained using anisotropic diffusion with the one obtained by a convolution with a Gaussian kernel (also known as **isotropic diffusion**). Use PSNR to compare the quality. Try to implement the anisotropic diffusion on your own (refer to the second link given in the following *See also* section).

### See also

For more information, refer to the following links:

- http://image.diku.dk/imagecanon/material/PeronaMalik1990.pdf
- https://sandipanweb.wordpress.com/2017/10/08/some-more-variational-image-processing-diffusiontv-denoising-image-inpainting/

## Improving image contrast with histogram equalization

In *Chapter 1, Image Manipulation and Transformation*, we saw how the contrast stretching operation can be used to increase the contrast of an image. However, it is just a linear scaling function that is applied to image pixel values, and hence the image enhancement is less drastic than its more sophisticated counterpart, histogram equalization. This recipe will show how to implement contrast stretching using the histogram equalization. It is also a point transform that uses a non-linear mapping that reassigns the pixel intensity values in the input image in such a way that the output image has a uniform distribution of intensities (a flat histogram), and thereby enhances the contrast of the image.

### Getting ready

In this recipe, we will implement histogram equalization with our own function, and also use scikit-image's global and local (adaptive) histogram equalization functions, starting with an RGB image. As usual, let's start by importing the required libraries:

```
import numpy as np
import matplotlib.pyplot as plt
from skimage.io import imread
from skimage.exposure import equalize_hist, equalize_adapthist
```

### How to do it...

To execute this recipe, perform the following steps:

- 1. Define the `plot_image()` and `plot_hist()` functions to display an image and return its cumulative distribution function (cdf), respectively:

```
def plot_image(image, title):
    plt.imshow(image)
    plt.title(title, size=20)
    plt.axis('off')

def plot_hist(img):
    colors = ['r', 'g', 'b']
    cdf = np.zeros((256, 3))
    for i in range(3):
        hist, bins = np.histogram(img[..., i].flatten(), 256, [0, 256], \
            normed=True)
        cdf[..., i] = hist.cumsum()
        cdf_normalized = cdf[..., i] * hist.max() / cdf.max()
        plt.plot(cdf_normalized, color = colors[i], \
            label='cdf ({})'.format(colors[i]))
        binWidth = bins[1] - bins[0]
        plt.bar(bins[:-1], hist*binWidth, binWidth,
            label='hist ({})'.format(colors[i]))
    plt.xlim([0, 256])
    plt.legend(loc = 'upper left')
    return cdf
```
