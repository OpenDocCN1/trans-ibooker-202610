In this recipe, you will learn how to detect objects in images with a **Fully Convolutional Network** (FCN) deep learning model, **YOLO** (v3). Given an input image with some objects (for example, animals, and cars), we will aim to detect objects in the image with a pretrained YOLO (v3) model, with bounding boxes.

YOLO v3 has a number of incremental improvements over YOLO v2:

+   - YOLO v3 uses Darknet-53 (a deeper network with shortcut connections) instead of Darknet-19 used by YOLO v2.
+   - It has a more advanced feature extractor/object detector using feature map up sampling/concatenation.
+   - To detect small objects more efficiently, YOLO v3 performs detections at three different scales.
+   - YOLO v3 uses nine anchor boxes (three for each scale, instead of the total of five anchor boxes used by YOLO v2), predicting more bounding boxes than YOLO v2 given the same-sized image as input.
+   - YOLO v3 computes the object confidence scores and class labels with independent logistic regression classifiers instead of softmax (by computing cross-entropy error terms instead of the squared-loss functions).

In this recipe, you will learn how to use a pretrained YOLO v3 model to detect objects in an image using `opencv-python` library functions.

# # Getting started

First, download the pretrained YOLO v3 model files (for example, the .cfg and .weights files for YOLOv3-416, with input image sizes 416 x 416) from https://pjreddie.com/darknet/yolo/ and save the files inside the `models` folder. Let's now import all of the required libraries using the following code snippet:

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image, ImageDraw, ImageFont
import colorsys
from random import shuffle
```