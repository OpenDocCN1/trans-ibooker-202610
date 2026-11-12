
```
top + base_line), (255, 255, 255), cv2.FILLED)
cv2.putText(img, label, (left, top), cv2.FONT_HERSHEY_SIMPLEX, \
0.75, (0, 0, 0), 1)
class_mask = cv2.resize(class_mask, (right - left + 1, \
bottom - top + 1))
mask = (class_mask > mask_threshold)
roi = img[top:bottom+1, left:right+1][mask]
# color = colors[class_id%len(colors)]
# comment the above line and uncomment below two lines to generate
different instance colors
color_index = random.randint(0, len(colors)-1)
color = colors[color_index]
img[top:bottom+1, left:right+1][mask] = [(0.3*color[0], \
0.3*color[1], 0.3*color[2])] + \
0.7 * roi).astype(np.uint8)
mask = mask.astype(np.uint8)
contours, hierarchy = cv2.findContours(mask, \
cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
cv2.drawContours(img[top:bottom+1, left:right+1], contours, \
-1, color, 3, cv2.LINE_8, hierarchy, 100)
```

3. Define the following function to post-process the output obtained from the network to extract the bounding boxes and masks corresponding to the objects detected:

```
def post_process(boxes, masks):
    num_classes, num_detections = masks.shape[1], boxes.shape[2]
    img_height, img_width = img.shape[0], img.shape[1]
    for i in range(num_detections):
        box = boxes[0, 0, i]
        mask = masks[i]
        score = box[2]
        if score > conf_threshold:
            class_id = int(box[1])
            left, top, right, bottom = int(img_width*box[3]), \
            int(img_height*box[4]), \
            int(img_width*box[5]), \
            int(img_height*box[6])
            left, top = max(0, min(left, img_width - 1)), \
            max(0, min(top, img_height - 1))
            right, bottom = max(0, min(right, img_width - 1)), \
            max(0, min(bottom, img_height - 1))
            class_mask = mask[class_id]
            draw_box(img, class_id, score, left, top, right, \
            bottom, class_mask)
```

4. Load the names of classes using the following code snippet from the label names file:

```
classes_file = "models/mask_rcnn/mscoco_labels.names";
classes = None
with open(classes_file, 'rt') as f:
    classes = f.read().rstrip('\n').split('\n')
```

5. Load the pretrained Mask R-CNN model by loading the tensorflow graph and the weight file for the model:

```
text_graph = "models/mask_rcnn/mask_rcnn_inception_v2_coco_2018_01_28.pbtxt"
model_weights = "models/mask_rcnn/frozen_inference_graph.pb"
net = cv2.dnn.readNetFromTensorflow(model_weights , text_graph)
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_OPENCV)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CPU)
```

6. Prepare the colors to be used to draw the bounding boxes for the objects corresponding to the different classes:

```
colors_file = "models/mask_rcnn_inception_v2_coco_2018_01_28/colors.txt"
with open(colors_file, 'rt') as f:
    colors_str = f.read().rstrip('\n').split('\n')
colors = []
for i in range(len(colors_str)):
    rgb = colors_str[i].split(' ')
    color = np.array([float(rgb[0]), float(rgb[1]), float(rgb[2])])
    colors.append(color)
```

7. Read the input image, preprocess the image to create a blob as expected by the model, and set the blob as the input to the pretrained model using the next code snippet:

```
img = cv2.imread('images/road.png')
blob = cv2.dnn.blobFromImage(img, swapRB=True, crop=False)
net.setInput(blob)
```

8. Run a forward pass on the pretrained network with the input blob and then post-process the output obtained to draw the bounding boxes and masks around the objects detected using the following code block:

```
boxes, masks = net.forward(['detection_out_final',
'detection_masks'])
post_process(boxes, masks)
```

If you run the code and plot the input image along with the output post-processed image, you will get the following output:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_356_0.png)

As you can see, Mask R-CNN correctly detects partially occluded objects (cars) for the input (test) image.

## How it works...

The `draw_box()` function does the following:

- 1. It draws a bounding box around a detected object.
- 2. It prints a label of the (most probable) class the object is assigned to.
- 3. It displays the label to the top-left corner of the bounding box.
- 4. Then, it resizes the mask, threshold, and color and applies it to the image.
- 5. Finally, it draws the contours on the image corresponding to the mask.

The `post_process()` function extracts the bounding box along with the mask for every object detected for each image. Then, it chooses the right color (corresponding to the class label of the object) to draw the object bounding box and overlay the mask on the image.

## There's more...

Train the Mask R-CNN network on the MS-COCO dataset (download the dataset from here: `http://cocodataset.org/`) and save the model trained (a GPU is highly recommended); use it to predict objects in your image.

## See also

- `https://arxiv.org/pdf/1703.06870.pdf`
- `https://www.pyimagesearch.com/2018/11/19/mask-r-cnn-with-opencv/`
- `https://github.com/facebookresearch/Detectron`
- `https://arxiv.org/pdf/1405.0312.pdf`
- `https://www.youtube.com/watch?v=g7z4mkfRjI4`
- `https://www.youtube.com/watch?v=FR25P1lMBY8`
- `https://www.youtube.com/watch?v=FR25P1lMBY8`

## Multiple object tracking with Python-OpenCV

**Object tracking** (in a video) is an image/video processing task that locates one or multiple moving objects over time. The goal of the task is to find an association between the target object(s) in the successive video frames. The task becomes difficult when the objects move faster relative to the frame rate or when the object to be tracked changes its orientation over time. The object tracking systems use a motion model taking into account how the target object may change for different possible motions of the object.

Object tracking is useful in human-computer interaction, security/surveillance, traffic control, and many more areas. Since it considers the appearance and the location of an object in the past frame, under certain circumstances, we may still be able to track an object despite the object detection fails. Few tracking algorithms that perform local searches are very fast. Hence, it’s generally a good strategy to track an object indefinitely once it is detected for the first time. Most real-world applications implement tracking and detection simultaneously.

In this recipe, you will learn how to track multiple objects in a video using `opencv-python` functions, where the object locations in the very first frame will be provided to you in terms of bounding box coordinates.

### Getting started

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

### How to do it...

Perform the following steps to implement multiple object tracking with opencv-python functions:

- 1. First, create a `MultiTracker` object using the following line of code:

  ```
  multi_tracker = cv2.MultiTracker_create()
  ```

- 2. You will track a couple of moving cars in a video. The location of the cars is provided to you for the very first frame in the video. You may try to extract the frames from the video and use your favorite object detection algorithm to obtain the bounding boxes of the cars in the very first frame:

  ```
  car_bbox = (141,175,45,29)
  car2_bbox = (295,170,55,39)
  bboxes = [car_bbox, car2_bbox]
  colors = [(0, 255, 255), (255, 255, 0)]
  ```

- 3. Read the first frame of the input video and start an OpenCV multi-object tracker by initializing it with the frame and the supplied bounding box coordinates (to locate the objects in the very first frame, to start with) using the following code snippet:

  ```
  vs = cv2.VideoCapture('images/road.mp4')
  _, frame = vs.read()
  frame = resize(frame, width=500)
  for bbox in bboxes:
      multi_tracker.add(cv2.TrackerCSRT_create(), frame, bbox)
  ```

- 4. Iteratively read frames from the video stream (read 3 frames in a second, for example, to reduce the number of frames to be read), until all of the frames are read, by using the following code block:

  ```
  j = 0
  while True:
      vs.set(cv2.CAP_PROP_POS_MSEC, (j*300)) # 1 sec read 3 frames
      _, frame = vs.read()
      if frame is None:
          break
  ```
