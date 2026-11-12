
If you run the preceding code block, then you will get the following heatmap as output:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_329_0.png)

6. For each keypoint, check the presence of the keypoint in the image and retrieve the keypoint coordinates corresponding to the maximum of the confidence map from that keypoint (along with a threshold to be used to decrease false positives):

```
threshold = 0.1
image1 = image.copy()
points = []
for i in range(n_points):
    prob_map = output[0, i, :, :] # confidence map of \
        corresponding body's part.
    min_val, prob, min_loc, point = cv2.minMaxLoc(prob_map)
    # scale the point to fit on the original image
    x = (width * point[0]) / w
    y = (height * point[1]) / h
    if prob > threshold:
        cv2.circle(image1, (int(x), int(y)), 8, (255, 0, 255), \
            thickness=-1, lineType=cv2.FILLED)
        cv2.putText(image1, "{}".format(i), (int(x), int(y)), \
            cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 0), 2, \
            lineType=cv2.LINE_AA)
        cv2.circle(image, (int(x), int(y)), 8, (255, 0, 255), \
thickness=-1, lineType=cv2.FILLED)
points.append( (int(x), int(y)) )
else:
    points.append(None)
```

7. Finally, since the indices of the keypoints are known beforehand, just join the pairs to draw the skeleton on the image:

```
for pair in pose_pairs:
    part_from = body_parts[pair[0]]
    part_to = body_parts[pair[1]]
    if points[part_from] and points[part_to]:
        cv2.line(image, points[part_from], points[part_to], \
                 (0, 255, 0), 3)

plt.figure(figsize=[20,12])
plt.subplot(121), plt.imshow(cv2.cvtColor(image1,
cv2.COLOR_BGR2RGB)), plt.axis('off'), plt.title('Keypoints',
size=20)
plt.subplot(122), plt.imshow(cv2.cvtColor(image,
cv2.COLOR_BGR2RGB)), plt.axis('off'), plt.title('Pose', size=20)
plt.show()
```

If you run the preceding code block, you will get the following output:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_330_0.png)

## How it works...

The cv2.dnn.blobFromImage() function was used to convert the image from the OpenCV format to Caffe blob format. First, the pixel values were normalized so that they were within the (0,1) range. Then the size of the resizing target image was provided. Finally, the mean value was subtracted. No color-channel swapping was required since both OpenCV and Caffe use the same color format of BGR.

The cv2.minMaxLoc() function was used to compute the local maxima of the confidence map.

## See also

- https://www.learnopencv.com/deep-learning-based-human-pose-estimation-using-opencv-cpp-python/
- https://cv-tricks.com/pose-estimation/using-deep-learning-in-opencv/
- https://medium.com/beyondminds/an-overview-of-human-pose-estimation-with-deep-learning-d49eb656739b
- https://arxiv.org/pdf/1812.08008.pdf
- https://arxiv.org/pdf/1611.08050.pdf
- https://github.com/CMU-Perceptual-Computing-Lab/openpose

## Object Detection in Images

Object detection is an image processing/computer vision task that detects (semantic) objects' instances corresponding to a given type (for example, faces, humans, vehicles, and buildings) in images. In this chapter, you will learn to implement a few state-of-the-art object detection techniques. The following image processing tasks are related to each other (although they refer to different tasks): image classification, object localization, and object detection. Image classification (discussed in the last chapter) aims to predict a class label for an image, whereas object localization deals with identifying the locations and drawing bounding boxes around an object in an image. Object detection combines these two tasks (classification + localization)—it draws a bounding box around each object (there can be multiple objects) of interest in the image and assigns each of them a class label. All of these problems are collectively known as object recognition. The following screenshot explains the difference between these techniques:

| Classification | Classification + Localization | Object Detection |
|---|---|---|
| CAR | CAR | TRUCK CAR |
| Single object | | Multiple objects |

In this chapter, we are going to cover the following recipes:

- Object detection with HOG/SVM
- Object detection with Yolo V3
- Object detection with Faster R-CNN
- Object detection with Mask R-CNN
- Multiple object tracking with `opencv-python`
- Text detection/recognition in images with EAST/Tesseract
- Face detection with Viola-Jones/Haar-like features

## Object detection with HOG/SVM

A popular feature descriptor for object detection is the **Histogram of Oriented Gradients (HOG)**. HOG descriptors can be computed from an image by first computing the horizontal and vertical gradient images, then computing the gradient histograms and normalizing across blocks, and finally flattening into a feature descriptor vector. These normalized block descriptors finally obtained are called HOG descriptors, a feature descriptor used in a variety of computer vision and image processing applications for object detection.

The use of HOG descriptors has been particularly successful for detecting humans, animals, faces, and text. We already described how to compute a HOG descriptor from an image. At first, a (linear) **Support Vector Machine (SVM)** binary classifier model is trained with several positive and negative training example images. Positive images are examples that contain the object we want to detect. The negative training set can be any images that do not contain the object we want to detect. Positive and negative source images are converted into HOG block descriptors. The SVM trainer selects the best hyperplane/decision boundary (defined using a set of support vectors) to separate the positive and negative examples in the training image dataset. The support vectors are used later by the SVM model to classify HOG-descriptor blocks from a test image to detect the presence/absence of an object.

Classification is traditionally performed by repeatedly stepping a sliding window of, say, 64 pixels wide by 128 pixels high across a test image and computing the HOG descriptors. As the HOG descriptor calculation does not have an intrinsic sense of scale and since the objects can occur at multiple scales within an image, the HOG description computation is repeated across each level of a scale pyramid. The scaling factor between each level in the scale pyramid is commonly between 1.05 and 1.2 and the image is repeatedly scaled down until the scaled source image can no longer accommodate a complete HOG window. If the SVM classifier predicts the detection of an object at any scale, the corresponding bounding box is returned. The following diagram shows a typical HOG object (human) detection workflow:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_334_0.png)

In this recipe, you will learn how to use OpenCV-Python library functions to detect human objects in an image.

## Getting started

Let's start by importing the required libraries, using the following code snippet:

```
import numpy as np
import cv2
import matplotlib.pyplot as plt
```

## How to do it...

Perform the following steps to detect people in an image using opencv-python library functions:

1. First, read the image and initialize the HOG detector using the following lines of code:

```
img = cv2.imread("images/walk.jpg")
hog = cv2.HOGDescriptor()
```

2. Initialize the SVM detector to be the default people detector:

```
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())
```

3. Run the detection algorithm, with a spatial stride of 4 pixels (horizontal and vertical), a scale stride of 1.1, and no grouping of the rectangles (to notice that HOG detects the same object at multiple scales in the scale pyramid). As you can see, 314 potential bounding boxes were detected (with multiple detections of the same object):

```
(found_bounding_boxes, weights) = hog.detectMultiScale(img, \
                       winStride=(4, 4), padding=(8, 8), \
                       scale=1.1, finalThreshold=0)
print(len(found_bounding_boxes)) # number of bounding boxes
# 314
```

4. Define the following function to implement non-maximum suppression to ignore redundant, overlapping bounding boxes:

```
def non_max_suppression(boxes, scores, threshold):
    assert boxes.shape[0] == scores.shape[0]
    ys1, xs1, ys2, xs2 = boxes[:, 0], boxes[:, 1], boxes[:, 2], \
        boxes[:, 3]
    areas = (ys2 - ys1) * (xs2 - xs1)
    scores_indexes = scores.argsort().tolist()
    boxes_keep_index = []
```
