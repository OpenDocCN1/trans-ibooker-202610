# How to do it...

Go through the following steps to implement deep instance segmentation using opencv-python functions:

+   1. Define the following function to generate N random (bright) colors using the HSV colorspace:

```
def random_colors(N, bright=True):
    brightness = 1.0 if bright else 0.7
    hsv = [(i / N, 1, brightness) for i in range(N)]
    colors = list(map(lambda c: colorsys.hsv_to_rgb(*c), hsv))
    random.shuffle(colors)
    return 255*np.array(colors)
```

+   2. Initialize the model constants and the path that you have used to download the model:

```
model_path = 'models\\'
conf = 0.5
thresh = 0.3
```

+   3. Read the COCO class labels on which the mask R-CNN (pretrained) model was trained:

```
labels_path = os.path.sep.join([model_path,
"object_detection_classes_coco.txt"])
labels = open(labels_path).read().strip().split("\\n")
```

+   4. Initialize the paths to the pretrained mask R-CNN model weights and configuration files:

```
weights_path = os.path.sep.join([model_path,
"frozen_inference_graph.pb"])
config_path = os.path.sep.join([model_path,
"mask_rcnn_inception_v2_coco_2018_01_28.pbtxt"])
```

+   5. Load the pretrained mask R-CNN model from the disk:

```
net = cv2.dnn.readNetFromTensorflow(weights_path, config_path)
```