
### There's more...

Here are a few more useful morphological functions that you can try on your own (similar to the ones shown in this recipe). Use the mahotas.bwperim() function in the fractal binary image to compute the border and the mahotas.thin() function for morphological thinning/skeletonization.

Use morphological filters to get rid of the noise and the smaller planets (as shown in the following screenshot) from the binary image obtained by thresholding the image of the colored planet from NASA public domain images (https://www.flickr.com/photos/nasacommons/), as shown here:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_155_0.png)

Also, start with the following MRI grayscale image and apply white/black top hat morphological filters to obtain the output shown:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_156_0.png)

Start with the keyboard binary image and get rid of all of the symbols to get the output image shown here (hint: use binary erosion by reconstruction with an appropriate structuring element):

![](img/7e2c8bbde0ebcd14e54d2862adfee480_157_0.png)

Compare morphological diameter closing with closing by applying them to binary images where the results obtained are different when these two filters are applied.

Apply `mahotas` functions to apply the morphological local minima (and maxima) filters to a binary image. How are regional minima (and maxima) different from local minima (maxima)? Demonstrate with a binary image where these two are different. You can apply morphological filters to grayscale images as well (refer to the first link in the following See also and Grayscale operations sections for a demonstration of such an implementation).

### See also

Refer to the following works to further enrich your knowledge of this recipe:

- Chapter 6 of the book, Hands-on Image Processing with Python
- https://mahotas.readthedocs.io/en/latest/api.html
- https://simpleitk.readthedocs.io/en/master/Documentation/docs/source/filters.html
- http://people.ciirc.cvut.cz/~hlavac/TeachPresEn/11ImageProc/71-3MatMorpholBinEn.pdf
- http://www.cs.uu.nl/docs/vakken/ibv/reader/chapter6.pdf

## Morphological pattern matching

In this recipe, you will learn how to use the morphological compound operation, hit-or-miss-transform, to find patterns from a binary image. **Hit-or-miss transform** is a morphological operation that is used to detect a given pattern in a binary image. It uses a pair of disjointed structuring elements to define the pattern to be matched and the morphological erosion operator to implement the pattern matching. The hit-or-miss transform returns a binary image as output in which only those sets of positions are non-zero where the first SE matches and the second SE completely misses the foreground, respectively, in the input binary image.

### Getting ready

In this recipe, we will use an image of a script of a Bengali poem by Tagore and search the image to find the occurrence of a Bengali character (ঈ) using morphological pattern matching. Let's start by importing the Python libraries, modules, and functions required:

```
import numpy as np
import matplotlib.pyplot as plt
from skimage.io import imread
from skimage.color import rgb2gray, gray2rgb
from scipy import ndimage
```

### How to do it...

Perform the following steps to implement morphological pattern matching with hit-or-miss-transform:

- 1. Define the hit_or_miss_transform() function that accepts the filenames corresponding to the input image and two SEs (the first one for *hit* and the second one for *miss*) as input:

```
def hit_or_miss_transform(im, s1, s2):
```

- 2. Inside the function, read the input image and structuring elements, consequently converting them into grayscale:

```
im = rgb2gray(imread(im))
m, n = im.shape
s1 = rgb2gray(imread(s1))
s2 = rgb2gray(imread(s2))
```

- 3. Apply the hit-or-miss transform using corresponding functions from the scipy.ndimage module, which returns a Boolean NumPy ndarray where the elements corresponding to the match position are set to True. Convert the output type into an unsigned 8-bit integer:

```
hom_transformed = ndimage.binary_hit_or_miss(im, structure1=s1, \
                                      structure2=s2).astype(np.uint8)
```

- 4. Now, highlight the matches (pixels with the value 1) with red squares and plot the highlighted output image using the following code snippet:

```
xs, ys = np.where(hom_transformed == 1)
hom_transformed = gray2rgb(hom_transformed)
w, h = 10, 12
for i in range(len(xs)):
    x, y = (xs[i], ys[i])
    for j in range(max(0, x-h), min(m-1, x+h)):
        for k in range(max(0, y-w), min(n, y+w)):
            hom_transformed[j, k, 0] = 1.
    for j in range(max(0, x-h), min(m, x+h)):
        for k in range(max(0, y-w), min(n-1, y+w)):
            hom_transformed[j, k, 0] = 1.
# plot the highlighted output image hom_transformed here with
matplotlib
```

- 5. Finally, call the hit_or_miss_transform() function to match patterns such as the Bengali letter ড only, but not similar to letters such as ঢ. The first SE matches both the patterns ড and ঢ, but the second SE discards the pattern ড by ensuring the corresponding mismatch:

```
hit_or_miss_transform('images/poem.png', 'images/bo.png',
 'images/bo_inv_1.png')
```

The structuring elements you need to specify (corresponding to hit and miss, respectively) are shown in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_160_0.png)

If you run the preceding code snippets with the given structuring elements and plot the input and output images, you will get a diagram along the lines of the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_160_1.png)

As you can see from the preceding screenshot, all of the র patterns are matched but not the ণ pattern.

- 6. Again, call the hit_or_miss_transform() function to match patterns such as the Bengali letter র, except for the ones with vowel signs, such as রি and রে:

```
hit_or_miss_transform('images/poem.png', 'images/bo.png', 'images/bo_inv_2.png')
```

The structuring elements you need to specify (corresponding to hit and miss, respectively) this time are shown in the following screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_161_0.png)

If you run the code snippets with the new given structuring elements and plot the input and output images, you will get a diagram along the lines of the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_161_1.png)

### How it works...

As you can see in the preceding section, the `binary_hit_or_miss()` function from the `scipy.ndimage.morphology` module was used to apply the morphological hit-or-miss transform operation (shown in the following screenshot) to a binary image:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_162_0.png)

The input binary image, along with a couple of disjointed structuring elements, are passed to the function. The first SE argument represents part of the SE that must match (hit) the foreground. The second SE argument represents part of the SE that must miss the foreground completely.

This `binary_hit_or_miss()` function finds the locations inside the input image that match the given pattern (defined by the SEs).

The `np.where()` function was used to find the match coordinates and those matched pixels were then highlighted in red (using the `gray2rgb()` function from the `skimage.color` module to convert the 2D binary image into a 3D color image).

### There's more...

Let's now apply the morphological pattern extraction to organic chemistry: find -O- and =O bonding from the following structure of the triglyceride molecule and highlight them, as shown in the next screenshot:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_162_1.png)

### See also

Refer to the following link to learn more about this recipe:

- https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.ndimage.morphology.binary_hit_or_miss.html

## Segmenting images with morphology

Image segmentation is the partitioning of an image into distinct regions or categories that correspond to different objects or parts of objects. In this recipe, you will learn how to use a region-based segmentation method to a binary image using the morphological watershed algorithm. We can think of a grayscale image as a topographic surface. If we flood this surface from its minimum values and prevent the merging of waters from different sources, then the image gets partitioned into two different sets, namely, the _catchment basins_ (segments) and the _watershed lines_. To prevent over-segmentation, a pre-defined set of markers is used and the flooding of the surface starts from these markers. The following are the steps involved in the segmentation of an image by the watershed transformation:

- 1. Find the markers and the segmentation criterion (the function that is to be used to split the regions; often, this is the contrast/gradient).
- 2. Run a marker-controlled watershed algorithm with these two elements.

### Getting ready

Let's import all of the packages, modules, and functions required to demonstrate morphological segmentation with the watershed algorithm:

```
from scipy import ndimage as ndi
from skimage.morphology import watershed, binary_dilation, binary_erosion,
remove_small_objects
from skimage.morphology import disk, square
from scipy.ndimage import distance_transform_edt
from skimage.measure import label, regionprops
from skimage.segmentation import clear_border
from skimage.filters import rank, threshold_otsu
from skimage.feature import peak_local_max, blob_log
from skimage.util import img_as_ubyte
from skimage.io import imread
from skimage.color import rgb2gray
import numpy as np
import matplotlib.pyplot as plt
```
