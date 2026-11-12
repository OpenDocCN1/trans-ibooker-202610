# How to do it...

Perform the following steps to implement object detection with a Yolo v3 pretrained model, using the opencv-python library functions:

+   1. First, initialize all of the parameters (for example, thresholds for non-maximum suppression and confidence for classification) with appropriate values:

```
conf_threshold = 0.5 # Confidence threshold
nms_threshold = 0.4 # Non-maximum suppression threshold
width = 416 # Width of network's input image
height = 416 # Height of network's input image
```

+   2. Load the names of the object classes (there are 80 classes corresponding to the MS-COCO dataset, including a person, bicycle, car, motorbike, and airplane) from the text file provided. Create unique colors corresponding to each object class (to distinguish the bounding boxes for the different objects detected). Also, initialize the model configuration and pretrained weight paths using the following code snippet:

```
classes_file = "models/yolov3/coco_classes.txt";
classes = None
with open(classes_file, 'rt') as f:
    classes = f.read().rstrip('\n').split('\n')
HSV_tuples = [(x/len(classes), x/len(classes), 0.8) for x in
    range(len(classes))]
shuffle(HSV_tuples)
colors = list(map(lambda x: colorsys.hsv_to_rgb(*x), HSV_tuples))
model_configuration = "models/yolov3/yolov3.cfg"
model_weights = "models/yolov3/yolov3.weights"
```

+   3. Load the pretrained deep neural net model with the Darknet model configuration and weight files:

```
net = cv2.dnn.readNetFromDarknet(model_configuration,
model_weights)
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_OPENCV)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CPU)
```

+   4. Define the following function to obtain the names of the output layers in the network:

```
def get_output_layers(net):
    layers_names = net.getLayerNames()
    return [layers_names[i[0] - 1] for i in
    net.getUnconnectedOutLayers()]
```