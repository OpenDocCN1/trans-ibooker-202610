
Plot the components of the dictionary learned from the patches. You will obtain a figure like the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_123_0.png)

- 4. Define the show_with_diff() function and invoke it to show the distorted image:

```python
def show_with_diff(image, reference, title):
    plt.figure(figsize=(10, 5))
    plt.subplot(121, plt.title('Image'))
    plt.imshow(image, vmin=0, vmax=1, cmap=plt.cm.gray, \
               interpolation='nearest'), plt.axis('off')

    plt.subplot(122)
    difference = image - reference
    plt.title('Difference (norm: %.2f)' % \
              np.sqrt(np.sum(difference ** 2)))
    plt.imshow(difference, vmin=-0.5, vmax=0.5, cmap=plt.cm.gray_r, \
               interpolation='nearest')
    plt.axis('off'), plt.suptitle(title, size=20)
    plt.subplots_adjust(0.02, 0.02, 0.98, 0.79, 0.02, 0.2)
    plt.show()

show_with_diff(distorted, lena, 'Distorted image')
```

If you run the preceding code snippet, you will get an output as shown in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_124_0.png)

- 5. Extract patches from the noisy bottom half of the image and normalize the extracted noisy patches:

```python
print('Extracting noisy patches... ')
data = extract_patches_2d(distorted[height // 2:, :], patch_size)
data = data.reshape(data.shape[0], -1)
intercept = np.mean(data, axis=0)
data -= intercept
```

- 6. Reconstruct the (noisy) bottom half of the image using the dictionary components (atoms):

```python
print('Orthogonal Matching Pursuit\n2 atoms' + '...')
kwargs = {'transform_n_nonzero_coefs': 2}
reconstruction = lena.copy()
dico.set_params(transform_algorithm='omp', **kwargs)
code = dico.transform(data)
patches = np.dot(code, V)
patches += intercept
patches = patches.reshape(len(data), *patch_size)
reconstruction[height // 2:, :] = reconstruct_from_patches_2d(patches, (height // 2, width))
show_with_diff(reconstruction, lena, 'Orthogonal Matching Pursuit 2 atoms')
```

If you run the preceding code snippet, you will get the following output, which shows the reconstructed image:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_125_0.png)

As you can see from the preceding screenshot, the difference norm has decreased a lot, which indicates that the reconstructed image is a much better approximation of the original image.

### There's more...

You can learn the dictionary from the image patches in an online manner too; instead of providing all 22,256 image patches (from the upper half) at once to the MiniBatchDictionaryLearning object, for example, you can provide 128 patches at a time (as a batch) and learn the dictionary and reconstruct the noisy lower half of the image online, as demonstrated in the following section.

### Online dictionary learning

Here are a few steps for online learning and reconstruction:

- 1. Define the constants:

```python
batch_size = 128
n_epochs = 2
n_batches = len(data) // batch_size
```

- 2. Instantiate the MiniBatchDictionaryLearning object, this time with an additional argument, batch_size, to the constructor:

```python
dico = MiniBatchDictionaryLearning(n_components=256, alpha=1, \
                                   n_iter=1, batch_size=batch_size)
# Now extract noisy patches as before
```

- 3. Iteratively extract the current batch of patches and use `partial_fit()` to fit/update the dictionary from the current batch:

```python
for epoch in range(n_epochs):
    for i in range(n_batches):
        batch = data[i * batch_size: (i + 1) * batch_size]
        dico.partial_fit(batch)
    V = dico.components_
    n_updates += 1
    # Now reconstruct lower noisy half of the image using the current dictionary
```

- 4. If you run a few batches and plot the dictionary atoms, the reconstructed image, and the error in reconstruction, you will get the output shown in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_126_0.png)

### See also

Refer to the following links to learn more about this recipe:

- https://www.cs.technion.ac.il/~ronrubin/Publications/KSVD-OMP-v2.pdf
- https://github.com/scikit-learn/scikit-learn/blob/7813f7efb/sklearn/decomposition/dict_learning.py
- https://scikit-learn.org/stable/modules/linear_model.html#omp
- https://www.youtube.com/watch?v=Efbr3YP9wGw

## Compressing an image using wavelets

In this recipe, you will learn how to use wavelets to transform an image and discard the lower-order bits from the output of the transform, so that most of its values are zero (or very small), but most of the signal (pixels) is preserved. We shall use the mahotas library functions for the demonstration.

### Getting ready

In this recipe, we will use the cameraman grayscale image as input. Let’s get started by importing the required libraries and modules:

```python
import numpy as np
import mahotas
from mahotas.thresholding import soft_threshold
from matplotlib import pyplot as plt
import os
```

### How to do it...

Execute the following steps to implement image compression using *wavelets*:

- 1. First, read the input image and convert it into grayscale:

```python
im = mahotas.imread('images/cameraman.png', as_grey=True)
im = im.astype(np.uint8)
print(im.shape)
# (256, 256)
f = np.mean(im==0)
print("Fraction of zeros in original image: {}".format(f))
# Fraction of zeros in original image: 0.0021514892578125
```

- 2. Try a baseline compression method using simple down-sampling (by saving every other pixel) and saving only the high-order bits:

```python
im1 = im[::2, ::2].copy()
im1 = im1 / 8
im1 = im1.astype(np.uint8)
f1 = np.mean(im1==0)
print("Fraction of zeros in original image (after division \
   by 8): {}".format(f1))
# Fraction of zeros in original image (after division by 8):
# 0.01788330078125
```

- 3. Even though you have thrown away 75% of the values in the image in the previous step, still there are only a few zeros. Let's now transform the image using a Daubechies wavelet (D8) and then discard the low-order bits. Check whether you got a better image with the same number of values used:

```python
imw = mahotas.daubechies(mahotas.wavelet_center(im), 'D8')
imw /= 8 # discard lower order bits
imw = imw.astype(np.int8)
f2 = np.mean(imw==0)
print("Fraction of zeros in wavelet transform (after division \
   by 8): {}".format(f2))
# Fraction of zeros in wavelet transform (after division by 8):
# 0.8892402648925781
im2 = mahotas.wavelet_decenter(mahotas.idaubechies(imw, 'D8'), \
   im.shape)
# min-max normalization to have pixel values in between 0-255
im2 = (255 * (im2 - np.min(im2)) / (np.max(im2) - \
   np.min(im2))).astype(np.uint8)
```

- 4. Apply soft-thresholding to further increase the percentage of zeros in the transformed image:

```python
imw = soft_threshold(imw, 12)
f3 = np.mean(imw==0)
print ("Fraction of zeros in wavelet transform (after division \
 by 8 & soft thresholding): {:}".format(f3))
# Fraction of zeros in wavelet transform (after division by 8 & soft thresholding): 0.945472172851562
im3 = mahotas.wavelet_decenter(mahotas.idaubechies(imw, 'D8'), \
 im.shape)
im3 = (255 * (im3 - np.min(im3)) / (np.max(im3) - \
 np.min(im3))).astype(np.uint8)
```

A much higher compression ratio can be achieved with this image, enabling faster transmission over communication channel and less bandwidth consumption, and after transmission, you will be able to reconstruct the image with a quality higher than the one reconstructed from the compressed image using baseline compression, created by just keeping every fourth pixel and low-order bits.

### How it works...

If you execute the preceding code and plot the images, you will get a screenshot like the following one:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_129_0.png)

The im[::2, ::2] code selects every other pixel from the im image and copy() function from NumPy creates a clone of ndarray. The wavelet_center() and wavelet_decenter() functions from mahotas create a centered version of the image and undo the effect of the wavelet_center() function if applied on the transformed image, respectively. These functions correctly handle borders and removed artifacts at the border of the image that are created otherwise. The mahotas.daubechies() and mahotas.idaubechies() functions perform Daubechies wavelet transform and Daubechies inverse wavelet transform respectively and accept a code (D8) for the wavelet to obtain transformed/inverse-transformed image.

### See also

Refer to the following links to find out more about this recipe:

- https://mahotas.readthedocs.io/en/latest/api.html?highlight=mahotas%20daubechies#mahotas.daubechies
- https://mahotas.readthedocs.io/en/latest/wavelets.html
- https://mahotas.readthedocs.io/en/latest/thresholding.html?highlight=soft_threshold

## Using steganography and steganalysis

Steganography aims to hide a message (called stego text) imperceptibly in an input image (known as cover image) resulting in an output stego image (without visible distortion) so that the existence of the message isn't suspected. It provides security through obscurity and is used to convey messages secretly between a sender and a recipient. Steganography hides the message so that it is hard to be observed, unlike cryptography, which encrypts the message so that it is (computationally) hard to be decrypted or understood. On the contrary, steganalysis refers to the process of detecting secret messages that were hidden using steganography; cryptanalysis has a similar relationship with cryptography.

In this recipe, we shall demonstrate a very popular steganography technique known as LSB data hiding (secret messages can be hidden by storing information in the least significant bits of an image; this way, the change in pixel values is at most one and hence the stego image remains indistinguishable from the cover image by the human eye) in an RGB color image and steganalysis techniques to detect secret messages.
