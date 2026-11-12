# # Getting ready

In this recipe, the input image we will use will be an orange fish in an aquarium and the object of interest will be the fish. You will detect the fish, change its color, and make it transparent using the color range of the fish in HSV space. Let's start by importing the required libraries:

```
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```
```

# # How to do it...

To do the recipe, the following steps need to be performed:

+   1. Read the input and background image. Convert the input image from BGR into the HSV color space:

```
```python
bck = cv2.imread("images/fish_bg.png")
img = cv2.imread("images/fish.png")
hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```
```

+   2. Create a mask for the fish by selecting a possible range of HSV colors that the fish can have:

```
```python
mask = cv2.inRange(hsv, (5, 75, 25), (25, 255, 255))
```
```

+   3. Slice the orange fish using the mask:

```
```python
imask = mask>0
orange = np.zeros_like(img, np.uint8)
orange[imask] = img[imask]
```
```

+   4. Change the color of the orange fish to yellow by changing the hue channel value only (add 20) and converting the image back into the BGR space:

```
```python
yellow = img.copy()
hsv[...,0] = hsv[...,0] + 20
yellow[imask] = cv2.cvtColor(hsv, cv2.COLOR_HSV2BGR)[imask]
yellow = np.clip(yellow, 0, 255)
```
```