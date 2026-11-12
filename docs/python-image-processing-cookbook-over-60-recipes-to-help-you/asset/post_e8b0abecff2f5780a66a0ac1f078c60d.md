
The following screenshot shows the fixed and the moving images and their initial alignment (as you can see they are not properly aligned):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_182_0.png)

- 3. Create an instance of the `ImageRegistrationMethod` class, which implements a configurable multi-resolution registration framework in SimpleITK:

```
registration_method = sitk.ImageRegistrationMethod()
```

- 4. Use the CenteredTransformInitializer() method to align the centers of the input images and define the center of the fixed image as the center of rotation:

```
initial_transform = sitk.CenteredTransformInitializer(fixed_image, \
                    moving_image, sitk.Similarity2DTransform())
```

- 5. Set the similarity metric as the MattesMutualInformation metric and the interpolator as a simple linear interpolator using the following code lines of code:

```
registration_method.SetMetricAsMattesMutualInformation(numberOfHistogramBins=50)
registration_method.SetInterpolator(sitk.sitkLinear)
```

- 6. Set the optimizer as a gradient descent optimizer with the learning rate parameter initialized to 1.0 and the number of iterations set to 100:

```
registration_method.SetOptimizerAsGradientDescent(learningRate=1.0,
                     numberOfIterations=100, convergenceMinimumValue=1e-6,
                     convergenceWindowSize=10)
```

- 7. Apply the initial transformation and then perform registration with the settings from the preceding steps to obtain the estimated final transform:

```
registration_method.SetInitialTransform(initial_transform,
inPlace=False)
final_transform =
registration_method.Execute(sitk.Cast (fixed_image,
sitk.sitkFloat32),
sitk.Cast (moving_image, sitk.sitkFloat32))
```

- 8. Resample the moving (source) image according to the final transformation obtained in the last step to obtain an image aligned to the fixed (target) image:

```
resampler = sitk.ResampleImageFilter ()
resampler.SetReferenceImage (fixed_image)
resampler.SetInterpolator (sitk.sitkLinear)
resampler.SetDefaultPixelValue(100)
resampler.SetTransform(final_transform)
out = resampler.Execute (moving_image)
```

- 9. Finally, combine the target image with the transformed (aligned) source image (to visualize the alignment):

```
simg1 = sitk.Cast (sitk.RescaleIntensity(fixed_image), \
sitk.sitkUInt8)
simg2 = sitk.Cast(sitk.RescaleIntensity(out), sitk.sitkUInt8)
cimg = sitk.Compose(simg1, simg2, simg1//2.+simg2//2.)
```

If you run the preceding code snippets and plot the target (CT-scan) image, aligned source (MRI-T1) image, and combined image, you will get an output like the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_183_0.png)

## How it works...

The ImageRegistrationMethod class provides an interface method to the modular SimpleITK (v4) registration framework. This class captures all of the necessary elements required to execute a simple image registration between two images.

Registration will done using optimization; the SetOptimizerAsGradientDescent() method implements a simple gradient descent optimizer. At each iteration, the current parameter is updated according to the following equation (where f is the objective function for the optimization and $p_n$ represents the value of a parameter, p, at iteration n, with a parameter that can be initialized with a small random value):

$p_{n+1} = p_n - \text{learningRate} \frac{\delta f(p_n)}{\delta p_n}$

The parameters passed to the gradient descent function are learningRate (defaults to 1.0), numberOfIterations (the number of iterations to run the gradient descent algorithm), convergenceMinimumValue (when the convergence value reaches this value, the algorithm is treated as converged), and convergenceWindowSize.

The SetMetricAsMattesMutualInformation() method is used to register two images with the mutual information between them using the method of Mattes et al.

## There's more

Plot the metric value during the gradient descent iterations (using the SimpleITK observers); it should look like the following curve (the metric value should increase with iterations in general since the mutual information is being maximized and the local maxima obtained need to be highlighted):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_184_0.png)

Start with an MRI-T1 image and its shifted (translated) version, as shown in the following image, and register the images using SimpleITK. Use two different similarity metrics (for example, mean-squared and mutual information metrics) and the gradient descent optimizer. You should obtain an output like the following screenshot (notice that mutual information can be used to output better alignment):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_185_0.png)

Use checkerboard visualization (hint: use a SimpleITK filter) to display the images before and after registration (by combining the pixels from the two input/output images in a checkerboard pattern):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_186_0.png)

Use SimpleITK's Registration framework to register two images using deformable (non-affine) transformation (for example, elastic transform).

## See also

- https://www.cs.cmu.edu/~galeotti/methods_course/ITK_Registration.pdf
- https://buildmedia.readthedocs.org/media/pdf/simpleitk/master/simpleitk.pdf
- http://insightsoftwareconsortium.github.io/SimpleITK-Notebooks/Python_html/60_Registration_Introduction.html
- https://simpleitk.readthedocs.io/en/master/Documentation/docs/source/registrationOverview.html
- https://stackoverflow.com/questions/41692063/what-is-the-difference-between-image-registration-and-image-alignment
- https://www.youtube.com/watch?v=MaFiP6RerpM

## Image alignment with ECC algorithm and warping

The parametric image alignment problem involves finding a transformation that aligns two images. In this recipe, you will learn how to estimate the geometric transform (in terms of a warp matrix) between two images using the ECC criterion with OpenCV-Python library's implementation. Given a pair of image profiles (intensities), \(I_i(x)\) (the reference image) and \(I_w(y)\) (the warped image), and a set of coordinates \(T=\{x_k, k=1,\ldots,K\}\) (known as the target area), the alignment problem is to find the corresponding coordinate set in the warped image. Assuming \(\varphi\) is the given transformation model, the alignment problem can be extrapolated to the problem of estimating the parameters, \(p\), as shown in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_187_0.png)

\(\rho(p)\) is maximized with gradient-based approaches. The ECC criterion does not depend on the changes in contrast/brightness. An approximation to the non-linear objective function is iteratively optimized instead to make the optimizer computationally simple.

## Getting ready

In this recipe, we will use a colored (RGB) image whose color channels are not properly aligned (resulting in an improper visualization of the colored image). You will align the color channels using the ECC algorithm and then render the colored image to improve its visualization. Let's start by importing the required libraries as usual:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

## How to do it...

Follow these steps to implement the ECC algorithm using OpenCV-Python:

- 1. Since the color channels of an image are correlated more strongly in the gradient than in the intensity domain, we will run the ECC algorithm in the gradient domain. Define the `get_gradient ()` function, which you will use shortly:

```
def get_gradient(im) :
 grad_x = cv2.Sobel(im,cv2.CV_32F,1,0,ksize=3)
 grad_y = cv2.Sobel(im,cv2.CV_32F,0,1,ksize=3)
 grad = cv2.addWeighted(np.absolute(grad_x), 0.5, \
         np.absolute(grad_y), 0.5, 0)
 return grad
```

- 2. Read the input RGB image and plot the original image and its unaligned color channels:

```
im_unaligned = cv2.imread("images/me_unaligned.jpg")
height, width = im_unaligned.shape[:2]
print(height, width)
# 992 610
channels = ['B', 'G', 'R']
plt.figure(figsize=(30,12))
plt.gray()
plt.subplot(1,4,1), plt.imshow(cv2.cvtColor(im_unaligned, \
 cv2.COLOR_BGR2RGB)), plt.axis('off'), \
 plt.title('Unaligned Image (ECC)', size=20)
for i in range(3):
 plt.subplot(1,4,i+2), plt.imshow(im_unaligned[...,i]), \
 plt.axis('off')
plt.title(channels[i], size=20)
plt.suptitle('Unaligned Image and Color Channels', size=30)
plt.show()
```

If you run the preceding code snippet, you will get an output like the following screenshot:
