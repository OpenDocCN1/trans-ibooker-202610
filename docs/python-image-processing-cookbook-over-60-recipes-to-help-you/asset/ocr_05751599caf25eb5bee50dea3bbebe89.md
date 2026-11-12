# # Multiple object tracking with Python-OpenCV

**Object tracking** (in a video) is an image/video processing task that locates one or multiple moving objects over time. The goal of the task is to find an association between the target object(s) in the successive video frames. The task becomes difficult when the objects move faster relative to the frame rate or when the object to be tracked changes its orientation over time. The object tracking systems use a motion model taking into account how the target object may change for different possible motions of the object.

Object tracking is useful in human-computer interaction, security/surveillance, traffic control, and many more areas. Since it considers the appearance and the location of an object in the past frame, under certain circumstances, we may still be able to track an object despite the object detection fails. Few tracking algorithms that perform local searches are very fast. Hence, it’s generally a good strategy to track an object indefinitely once it is detected for the first time. Most real-world applications implement tracking and detection simultaneously.

In this recipe, you will learn how to track multiple objects in a video using `opencv-python` functions, where the object locations in the very first frame will be provided to you in terms of bounding box coordinates.

# ## Getting started

Let's start by importing the required libraries first. Note that the `opencv` version used for this recipe is 3.4.4:

```
#pip install opencv-python==3.4.4.19
#pip install opencv-contrib-python==3.4.4.19
import time
import cv2
import matplotlib.pyplot as plt
from imutils import resize
print(cv2.__version__)
# 3.4.4
```