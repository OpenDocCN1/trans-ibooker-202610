# Getting ready

Import the following libraries before you start:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

# How to do it...

Follow the given steps to implement image-inpainting using two different algorithms from
the OpenCV-Python library:

+   1. Read the RGB image and the grayscale mask image:
+      im = cv2.imread('images/cat.png')
+      mask = cv2.imread('images/cat_mask.png',0)
+   
+   2. Binarize the mask image using threshold:
+      _, mask = cv2.threshold(mask, 100, 255, cv2.THRESH_BINARY)
+   
+   3. Remove the masked-out pixels from the input image using bitwise_and with
+   the binary mask image:
+      src = cv2.bitwise_and(im, im, mask=mask)
+      mask = cv2.bitwise_not(mask)
+   
+   4. Obtain the inpainted destination image returned by the cv2.inpaint() method
+   using the cv2.INPAINT_NS algorithm:
+      dst1 = cv2.inpaint(src, mask, 5, cv2.INPAINT_NS)
+   
+   5. Obtain the inpainted destination image returned by the cv2.inpaint() method
+   using the cv2.INPAINT_TELEA algorithm:
+      dst2 = cv2.inpaint(src, mask, 5, cv2.INPAINT_TELEA)

If you run the preceding code, you will get a figure like the following screenshot
as the output image: