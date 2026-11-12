
## How to do it...

Go through the following steps to implement deep instance segmentation using opencv-python functions:

- 1. Define the following function to generate N random (bright) colors using the HSV colorspace:

```
def random_colors(N, bright=True):
    brightness = 1.0 if bright else 0.7
    hsv = [(i / N, 1, brightness) for i in range(N)]
    colors = list(map(lambda c: colorsys.hsv_to_rgb(*c), hsv))
    random.shuffle(colors)
    return 255*np.array(colors)
```

- 2. Initialize the model constants and the path that you have used to download the model:

```
model_path = 'models\\'
conf = 0.5
thresh = 0.3
```

- 3. Read the COCO class labels on which the mask R-CNN (pretrained) model was trained:

```
labels_path = os.path.sep.join([model_path,
"object_detection_classes_coco.txt"])
labels = open(labels_path).read().strip().split("\\n")
```

- 4. Initialize the paths to the pretrained mask R-CNN model weights and configuration files:

```
weights_path = os.path.sep.join([model_path,
"frozen_inference_graph.pb"])
config_path = os.path.sep.join([model_path,
"mask_rcnn_inception_v2_coco_2018_01_28.pbtxt"])
```

- 5. Load the pretrained mask R-CNN model from the disk:

```
net = cv2.dnn.readNetFromTensorflow(weights_path, config_path)
```

- 6. Now, read the input image to be segmented, set it as the loaded model input, and run a forward pass on the model with it to get the object masks and the bounding boxes:

```
image = cv2.imread('images/pets.jpg')
blob = cv2.dnn.blobFromImage(image, swapRB=True, crop=False)
net.setInput(blob)
(boxes, masks) = net.forward(["detection_out_final",
"detection_masks"])
num_classes = masks.shape[1]
num_detections = boxes.shape[2]
print('# instances: {}'.format(num_detections))
# instances: 4
colors = random_colors(num_detections)
print("# classes: {}".format(num_classes))
# classes: 90
```

- 7. For each of the objects detected, if the score is higher than the confidence-threshold value, compute the bounding box corners:

```
h = image.shape[0]
w = image.shape[1]
for i in range(num_detections) :
    box = boxes[0, 0, i]
    mask = masks[i]
    score = box[2]
    if score > conf:
        class_id = int(box[1])
        left, top, right, bottom = int(w * box[3]), int(h * box[4]), \
        int(w * box[5]), int(h * box[6])
        left, top = max(0, min(left, w - 1)), max(0, min(top, h - 1))
        right, bottom = max(0, min(right, w - 1)), \
        max(0, min(bottom, h - 1))
        class_mask = masks[class_id]
```

- 8. Extract the mask for each object detected by the model, colorize the mask with the corresponding class label's color, and blend the mask with the input image (to create an overlay):

```
    label = labels[class_id]
    class_mask = cv2.resize(class_mask, (right - left + 1, \
    bottom - top + 1))
    mask = (class_mask > thresh)
    roi = image[top:bottom+1, left:right+1][mask]
    color_index = np.random.randint(0, len(colors)-1)
    color = np.array(colors[color_index])
```

```
    image[top:bottom+1, left:right+1][mask] = (0.4*color + \
    0.6 * roi).astype(np.uint8)
```

- 9. Draw the object boundaries (with contours) on the segmentation overlay image using the following code block:

```
    mask = mask.astype(np.uint8)
    contours, hierarchy = cv2.findContours(mask, cv2.RETR_TREE, \
    cv2.CHAIN_APPROX_SIMPLE)

    cv2.drawContours(image[top:bottom+1, left:right+1], contours, \
    -1, color, 3, cv2.LINE_8, hierarchy, 100)
    label_size, _ = cv2.getTextSize(label, \
    cv2.FONT_HERSHEY_SIMPLEX, 0.5, 1)

    top = max(top, label_size[1])
    cv2.putText(image, label, ((left + right)//2, top), \
    cv2.FONT_HERSHEY_SIMPLEX, 0.75, (0,0,0), 2)
    cv2.imwrite('images/instance_seg_out.png', image)
```

## How it works...

If you run the code blocks and plot the input and output the segmented images, you will obtain the following figure:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_280_0.png)

Mask R-CNN can detect 90 classes, including people, animals, vehicles, signs, food, and so on (refer to the object_detection_classes_coco.txt file to see the supported classes).

The cv2.dnn.blobFromImage() function was used to construct a blob from the input image and then pass it to the deep-neural-net model.

The input image needs to be preprocessed before it can be fed to the deep learning model (this was also done by the same cv2.dnn.blobFromImage() function). Preprocessing steps normally involve mean subtraction, scaling, and optional channel swapping.

The class label and confidence of each detected object was extracted from the model output. The weak predictions were filtered out (the ones with low confidence, that is with a confidence less than the confidence threshold).

The mask image was extracted from the model output. It was thresholded to obtain a binary image, and for each object detected, the ROI was extracted.

Finally, the masked region was blended with the ROI to create the segmentation overlay image—the output of deep instance segmentation.

## See also

Refer to the following articles to learn more about this recipe:

- https://arxiv.org/pdf/1311.2524.pdf
- https://arxiv.org/pdf/1504.08083.pdf
- https://arxiv.org/pdf/1506.01497.pdf
- https://arxiv.org/pdf/1703.06870.pdf
- https://www.pyimagesearch.com/2018/11/19/mask-r-cnn-with-opencv/
- http://cocodataset.org/

## Image Classification

In this chapter, we will learn about the image classification problem, which is a supervised machine learning task of assigning (the most likely) label to an input image from a fixed set of labels (categories). We will also learn how to classify images using different Python libraries. This is one of the core problems in image processing that has a large variety of practical applications. Moreover, many other seemingly different image-processing tasks (such as object detection and segmentation) can be reduced to image classification. Image classification refers to the process of assigning a label to (that is, classifying) an image based on its visual content. For example, a binary image classification algorithm (model) may be developed to predict whether a human is in an image.

In this chapter, you will primarily learn how to implement two types of image classifiers. The first one is feature-based, where a feature generation (preprocessing) algorithm extracts a set of image features that are used to represent an image. An algorithm for image classification (a classical supervised machine learning algorithm) is then used to classify the image using the features. The following diagram shows a feature-based image classification:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_282_0.png)

The second type of classifier is a deep learning classifier where no preprocessing step (such as feature extraction) is implemented, and an end-to-end deep neural network is trained to perform the classification task. The following diagram shows deep learning-based (end-to-end) image classification:

![](img/7e2c8bbde0ebcd14e54d2862adfee480_283_0.png)

Image classification is a supervised machine learning task that consists of two phases: training and testing. In the training phase, a set of labeled input images (with known class labels) is provided to the classification algorithm, which trains a classification model using the input (typically, the model parameters are learned). Often, a set of validation (held-out) images is used to evaluate the current performance of the model on unseen images. Once the training is over, the learned model can be used to classify or predict the label of new (test) images—this is known as the testing phase. The predicted label can be compared with the ground-truth label to compare the accuracy of a classifier.

In this chapter, we are going to look at the following recipes:

- Classifying images with scikit-learn (HOG and logistic regression)
- Classifying textures with Gabor filter banks
- Classifying images with VGG19/Inception V3/MobileNet/ResNet101 (with PyTorch)
- Fine-tuning (with transfer learning) for image classification
- Classifying traffic signs using a deep learning model (with PyTorch)
- Estimating a human pose using a deep learning model
