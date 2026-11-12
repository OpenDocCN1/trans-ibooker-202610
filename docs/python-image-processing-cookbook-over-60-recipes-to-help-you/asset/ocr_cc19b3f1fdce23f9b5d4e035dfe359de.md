![](bbox=[0.105, 0.108, 0.895, 0.388])

In this recipe, you will learn how to use the OpenPose deep neural net model for human pose estimation with a pretrained Caffe model using opencv-python library functions (it won the COCO Keypoints Challenge in 2016).

# Getting ready

First, download the pretrained Caffe model from http://posefs1.perception.cs.cmu.edu/OpenPose/models/pose/mpi/pose_iter_160000.caffemodel—this model is trained on the multi-person dataset (MPI). Let's start by importing all the required libraries using the following code snippet:

```
import cv2
import time
import numpy as np
import matplotlib.pyplot as plt
print(cv2.__version__)
# 3.4.4
```