
## How to do it...

In this recipe, you will learn how to use functions from different libraries (for example, mahotas, skimage, and SimpleITK) to apply a few different morphological filters to an input binary image. Let's first execute the following steps to compute a few basic metrics (for example, the Euler number) for a binary image (using mahotas) and to use filters such as flood_fill and diameter_closing (using the skimage.morphology module).

## Computing the Euler number, eccentricity, and center of mass with mahotas/scikit-image

Perform the following steps to compute different metrics using the mahotas and skimage library functions:

- 1. Read the NASA black hole image (https://www.nasa.gov/sites/default/files/thumbnails/image/blackhole.png) and convert it into a binary image using a manual threshold (t=60). Compute the Euler number for the binary image:

```python
blackhole = mh.imread('images/blackhole.png')
blackhole_gray = mh.colors.rgb2grey(blackhole).astype(np.uint8)
t = 60
bin_blackhole = (blackhole_gray > t).astype(np.uint8)
print(mh.euler(bin_blackhole))
# 0.0
```

- 2. Compute center_of_mass and eccentricity of the binary image:

```python
cms = mh.center_of_mass(bin_blackhole)
print('Eccentricity = ', mh.features.eccentricity(bin_blackhole))
# 0.5449847073316407
```

- 3. Fill the black hole using either of flood_fill (starting from a seed inside the black hole) or diameter_closing (removing all holes of an area less than or equal to 100):

```python
bin_blackhole2 = flood_fill(bin_blackhole, (200, 400), 1)
bin_blackhole3 = diameter_closing(bin_blackhole, 100,
connectivity=2)
```

If you run the preceding code snippets and plot the images, you will get a diagram similar to the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_147_0.png)

The topology of a binary image is measured with the Euler number. It is defined as the difference in the total number of objects in the image and the number of holes present in those objects. For the first binary image, the Euler number is 0 = 1 - 1 (it has a single object that contains a single hole). On the other hand, for the second binary image, the Euler number is 1 - 0 = 1 (since the single object it contains does not have any hole in it).

## Morphological image filters with mahotas

Perform the following steps to see the result of applying a few morphological image filters using mahotas library functions again:

- 1. Read the color mandelbrot (fractal) image, convert it into grayscale, and then use otsu thresholding to convert it into a binary image:

```python
fractal = mh.imread('images/mandelbrot.jpg')
fractal_gray = mh.colors.rgb2grey(fractal).astype(np.uint8)
t = mh.otsu(fractal_gray)
bin_fractal = (fractal_gray > t).astype(np.uint8)
```

- 2. Compute the border between the white and black pixels using the following line of code:

```python
mh.border(bin_fractal, 0, 1)
```

- 3. Compute the regional minimum (with the region defined as a 3 x 3 cross SE around each pixel and taking the whole object into account) using the following line of code:

```python
mh.regmin(bin_fractal)
```

- 4. Close the holes in the image with the SE as a disk of radius 3, using the following line of code:

```python
mh.close_holes(bin_fractal, mh.disk(3))
```

- 5. Apply majority_filter to the binary image using the next line of code:

```python
mh.majority_filter(bin_fractal)
```

Finally, if you plot all of the images along with the center of mass of the original binary image, you will get a diagram similar to the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_148_0.png)

## Binary image filters with SimpleITK

Perform the following steps to demonstrate the application of a few more binary filters to a binary image, this time using SimpleITK library functions:

- 1. First, read the NASA supernova image (https://www.nasa.gov/image-feature/goddard/2019/hubble-sets-sights-on-an-explosive-galaxy) and convert the image into a binary one. Negate the binary image:

```python
supernova_gray = sitk.ReadImage('images/supernova.jpg',
sitk.sitkFloat32)
supernova_bin =
sitk.BinaryNotImageFilter().Execute(sitk.OtsuThresholdImageFilter()
.Execute(supernova_gray))
```

- 2. Instantiate the SimpleITK filter objects corresponding to the opening and closing morphological filters. Set the SE as a disk of radius 3. First, apply the opening filter followed by the closing filter to the binary supernova image:

```python
open_f = sitk.BinaryMorphologicalOpeningImageFilter()
close_f = sitk.BinaryMorphologicalClosingImageFilter()
open_f.SetKernelRadius(3)
close_f.SetKernelRadius(3)
supernova_bin1 = close_f.Execute(open_f.Execute(supernova_bin))
```

- 3. Apply the opening filter followed by the closing filter again to the input binary image, but this time with the SE as a circle with a radius of 7:

```python
open_f.SetKernelRadius(7)
close_f.SetKernelRadius(7)
supernova_bin2 = close_f.Execute(open_f.Execute(supernova_bin))
```

- 4. Next, apply the median filter with SEM as a disk with a radius of 3 to the input binary image:

```python
med_f = sitk.BinaryMedianImageFilter()
med_f.SetRadius(3)
supernova_bin3 = med_f.Execute(supernova_bin)
```

- 5. Apply BinaryMinMaxCurvatureFlowImageFilter() to the binary image using the following code snippet (with a stencil radius of 2 and a threshold parameter value of 60):

```python
curv_f = sitk.BinaryMinMaxCurvatureFlowImageFilter()
curv_f.SetStencilRadius(2)
curv_f.SetThreshold(60)
supernova_bin4 = curv_f.Execute(sitk.Cast(supernova_bin,
sitk.sitkFloat32))
```

- 6. Apply BinaryContourImageFilter() to the image after instantiating the corresponding object:

```python
cont_f = sitk.BinaryContourImageFilter()
supernova_bin5 = cont_f.Execute(supernova_bin)
```

- 7. Finally, instantiate VotingBinaryImageFilter() on the binary image:

```python
vote_f = sitk.VotingBinaryImageFilter()
vote_f.SetRadius(5)
supernova_bin6 = vote_f.Execute(sitk.Cast(supernova_bin2,
sitk.sitkInt32))
```

If you run the preceding code snippets and plot all of the images, you will get a diagram similar to the following:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_151_0.png)

## Dilation by reconstruction with skimage

Perform the following steps to separate drawing from text using dilation by reconstruction using the skimage library functions:

- 1. Read the input image (Tagore’s drawing-ridden Bengali manuscript of one of his songs in his own handwriting) and convert it into a binary image with thresholding followed by binary inversion (you want to have the alphabets as foreground objects, that is, in white):

```python
img = rgb2gray(imread('images/tagore_manuscript.jpg'))
th = 0.6 #threshold_otsu(img)
img[img <= th] = 0
img[img > th] = 1
img = 1 - img
```

- 2. The mask image required for reconstruction by dilation is simply the binary image obtained previously. Create the seed image (reconstruction by dilation) using the binary_erosion operation with a vertical line filter. Finally, perform the reconstruction by dilation using the mask and seed images created:

```python
mask = img
seed = binary_erosion(img, rectangle(1, 50))
words = reconstruction(seed, mask, 'dilation')
```

If you run the preceding code snippets and plot all of the images, you will get the drawing separated from the writing (words), as shown in the following diagram:

### Tagore's drawing-ridden manuscript

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_0.png)

### Seed for the reconstruction

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_1.png)

### Tagore's drawing

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_2.png)

### Tagore's manuscript without drawing

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_3.png)

## How it works...

The `euler()` function from the `mahotas` library was used to compute the Euler number (or characteristics) of the input binary image. By default, it uses 8-connectivity.

The `flood_fill()` function from the `skimage.morphology` module was used to perform flood filling on the black hole binary image, starting at a specific seed point (200,400) on the dark region inside the black hole (passed as the second argument to the function); the connected points equal to the `seed pixel value` (here, 0 or black) are found and then set to `new_value` (here, 1 or white), which is the third input parameter of the function.

The `diameter_closing()` function from the `skimage.morphology` module was used to remove all `holes` of the black hole image with `maximum extension` (a side of the `bounding box` enclosing a `hole`) smaller than the `diameter_threshold` parameter (that is, 100). This operator is also known as **bounding box closing**.

The `reconstruction()` function from the `skimage.morphology` module was used to implement dilation by reconstruction.

The dilation by reconstruction operation uses a `seed` and a `mask` image. The seed image specifies the values to start with (that `spreads` iteratively using the SE for `dilation`), whereas the mask image (the original image was used as the mask image) specifies the maximum value allowed at each pixel. The mask image puts a constraint on the `spread` of the `foreground` values.

The `BinaryMedianImageFilter()` constructor was used to instantiate the binary median filter (for denoising the supernova image) and the radius of the disk SE for this morphological filter was set using the `SetRadius()` method. The filter is applied to the input binary image using the `Execute()` method, and likewise for other SimpleITK filters.
