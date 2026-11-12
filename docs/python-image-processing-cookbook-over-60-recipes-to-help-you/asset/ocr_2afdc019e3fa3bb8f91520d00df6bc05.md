# How it works...

The `VideoCapture()` function from OpenCV-Python was used to create a `VideoCapture` object with the video file as input. Then, the `read()` method of that object was used to capture frames from the video.

The `imread()` and `imwrite()` functions from OpenCV-Python were used to read/write images from/to disk.

The `cv2.split()` function was used to split an RGB image into individual color channels, while the `cv2.merge()` function was used to combine them back into an RGB image.

# There's more...

**Focus stacking** (also known as extended depth of fields) is a technique (in image processing/computational photography) that takes multiple images (of the same subject but captured at different focus distances) as input and then creates an output image with a higher DOF than any of the individual source images by combining the input images. We can simulate focus stacking in Python. The following is an example of focus stacking grayscale image frames extracted from a video using the `mahotas` library.

# Extended depth of field with mahotas

Perform the following steps to implement focus stacking with the `mahotas` library functions:

+   1. Create the image stack first by extracting grayscale image frames from a highway traffic video at night:

```
import mahotas as mh
def create_image_stack(vid_file, n = 200):
    vidcap = cv2.VideoCapture(vid_file)
    success,image = vidcap.read()
    i = 0
    success = True
    h, w = image.shape[:2]
    imstack = np.zeros((n, h, w))
    while success and i < n:
        imstack[i,...] = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
        success, image = vidcap.read()
        i += 1
    return imstack
```