
The following photo shows one of the extracted input frames:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_46_0.png)

If you run the preceding code block, you will obtain a long exposure-like image like the one shown here:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_46_1.png)

Notice the continuous effects in the clouds and the waterfall.

## How it works...

The `VideoCapture()` function from OpenCV-Python was used to create a `VideoCapture` object with the video file as input. Then, the `read()` method of that object was used to capture frames from the video.

The `imread()` and `imwrite()` functions from OpenCV-Python were used to read/write images from/to disk.

The `cv2.split()` function was used to split an RGB image into individual color channels, while the `cv2.merge()` function was used to combine them back into an RGB image.

## There's more...

**Focus stacking** (also known as extended depth of fields) is a technique (in image processing/computational photography) that takes multiple images (of the same subject but captured at different focus distances) as input and then creates an output image with a higher DOF than any of the individual source images by combining the input images. We can simulate focus stacking in Python. The following is an example of focus stacking grayscale image frames extracted from a video using the `mahotas` library.

## Extended depth of field with mahotas

Perform the following steps to implement focus stacking with the `mahotas` library functions:

- 1. Create the image stack first by extracting grayscale image frames from a highway traffic video at night:

```python
import mahotas as mh

def create_image_stack(vid_file, n=200):
    vidcap = cv2.VideoCapture(vid_file)
    success, image = vidcap.read()
    i = 0
    success = True
    h, w = image.shape[:2]
    imstack = np.zeros((n, h, w))
    while success and i < n:
        imstack[i, ...] = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
        success, image = vidcap.read()
        i += 1
    return imstack
```

```python
image = create_image_stack('images/highway.mp4')  # cloud.mp4
stack, h, w = image.shape
```

- 2. Use the sobel() function from mahotas as the pixel-level measure of infocusness:

```python
focus = np.array([mh.sobel(t, just_filter=True) for t in image])
```

- 3. At each pixel location, select the best slice (with maximum infocusness) and create the final image:

```python
best = np.argmax(focus, 0)
image = image.reshape((stack, -1))  # image is now (stack, nr_pixels)
image = image.transpose()  # image is now (nr_pixels, stack)
final = image[np.arange(len(image)), best.ravel()]  # Select the right pixel at each location
final = final.reshape((h, w))  # reshape to get final result
```

The following photo is an input image used in the image stack:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_48_0.png)

The following screenshot is the final output image produced by the algorithm implementation:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_49_0.png)

## See also

For more details, refer to the following links:

- https://mahotas.readthedocs.io/en/latest/edf.html
- https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_gui/py_video_display/py_video_display.html
- https://www.youtube.com/watch?v=5CTSc9GX3X8

## Object detection using color in HSV

In this recipe, you will learn how to detect objects using colors in the HSV color space using OpenCV-Python. You need to specify a range of color values by means of which the object you are interested in will be identified and extracted. You can change the color of the object detected and even make the detected object transparent.

### Getting ready

In this recipe, the input image we will use will be an orange fish in an aquarium and the object of interest will be the fish. You will detect the fish, change its color, and make it transparent using the color range of the fish in HSV space. Let's start by importing the required libraries:

```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

### How to do it...

To do the recipe, the following steps need to be performed:

- 1. Read the input and background image. Convert the input image from BGR into the HSV color space:

```python
bck = cv2.imread("images/fish_bg.png")
img = cv2.imread("images/fish.png")
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

- 2. Create a mask for the fish by selecting a possible range of HSV colors that the fish can have:

```python
mask = cv2.inRange(hsv, (5, 75, 25), (25, 255, 255))
```

- 3. Slice the orange fish using the mask:

```python
imask = mask > 0
orange = np.zeros_like(img, np.uint8)
orange[imask] = img[imask]
```

- 4. Change the color of the orange fish to yellow by changing the hue channel value only (add 20) and converting the image back into the BGR space:

```python
yellow = img.copy()
hsv[..., 0] = hsv[..., 0] + 20
yellow[imask] = cv2.cvtColor(hsv, cv2.COLOR_HSV2BGR)[imask]
yellow = np.clip(yellow, 0, 255)
```

- 5. Finally, create the transparent fish image by first extracting the background without the input image with the fish, and then extracting the area corresponding to the foreground object (fish) from the background image and adding these two images:

```python
bckfish = cv2.bitwise_and(bck, bck, mask=imask.astype(np.uint8))
nofish = img.copy()
nofish = cv2.bitwise_and(nofish, nofish, mask=(np.bitwise_not(imask)).astype(np.uint8))
nofish = nofish + bckfish
```

### How it works...

The following screenshot shows an HSV colormap for fast color lookup. The x axis denotes hue, with values in (0,180), the y axis (1) denotes saturation with values in (0,255), and the y axis (2) corresponds to the hue values corresponding to S = 255 and V = 255. To locate a particular color in the colormap, just look up the corresponding H and S range, and then set the range of V as (25, 255). For example, the orange color of the fish we are interested in can be searched in the HSV range from (5, 75, 25) to (25, 255, 255), as observed here:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_51_0.png)

The inRange() function from OpenCV-Python was used for color detection. It accepts the HSV input image along with the color range (defined previously) as parameters. cv2.inRange() accepts three parameters—the input image, and the lower and upper limits of the color to be detected, respectively. It returns a binary mask, where white pixels represent the pixels within the range and black pixels represent the one outside the range specified.

To change the color of the fish detected, it is sufficient to change the hue (color) channel value only; we don’t need to touch the saturation and value channels.

The bitwise arithmetic with OpenCV-Python was used to extract the foreground/background.

Notice that the background image has slightly different colors from the fish image’s background; otherwise, transparent fish would have literally disappeared (invisible cloaking!).

If you run the preceding code snippets and plot all of the images, you will get the following output:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_52_0.png)

Note that, in OpenCV-Python, an image in the RGB color space is stored in BGR format. If we want to display the image in proper colors, before using imshow() from Matplotlib (which expects the image in RGB format instead), we must convert the image colors with cv2.cvtColor(image, cv2.COLOR_BGR2RGB).

## See also

For more details, refer to the following links:

- https://opencv-python-tutorials.readthedocs.io/en/latest/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html
- https://i.stack.imgur.com/gyuw4.png
- https://stackoverflow.com/questions/10948589/choosing-the-correct-upper-and-lower-hsv-boundaries-for-color-detection-with-cv
- https://www.youtube.com/watch?v=1F0aOM3WJ74

## Image Enhancement

The objective of image enhancement is to improve the quality of an image or make particular features appear more prominent. The techniques used are often more general-purpose techniques and a strong model of the degradation process is not assumed (unlike image restoration, which we will see in the next chapter). Some examples of image enhancement techniques are denoising/smoothing (using different classical image processing, unsupervised machine learning, and deep learning techniques), contrast improvement, and sharpening.

In this chapter, we will cover the following recipes for image enhancement (and their implementations using Python libraries):

- Applying filters to denoise different types of noise in an image
- Image denoising with a denoising autoencoder
- Image denoising with PCA/DFT/DWT
- Image denoising with anisotropic diffusion
- Improving image contrast with histogram equalization
- Implementing histogram matching
- Performing gradient blending
- Edge detection with Canny, LoG/zero-crossing, and wavelets
