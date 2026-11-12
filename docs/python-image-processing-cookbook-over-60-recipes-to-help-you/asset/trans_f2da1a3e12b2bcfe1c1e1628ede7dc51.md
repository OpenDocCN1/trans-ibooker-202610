

## 如何操作...

使用opencv-python库函数，执行以下步骤，利用Yolo v3预训练模型实现目标检测：

- 1. 首先，使用适当的值初始化所有参数（例如，非极大值抑制阈值和分类置信度阈值）：

```
conf_threshold = 0.5 # Confidence threshold
nms_threshold = 0.4 # Non-maximum suppression threshold
width = 416 # Width of network's input image
height = 416 # Height of network's input image
```

- 2. 从提供的文本文件中加载对象类别名称（共有80个类别，对应MS-COCO数据集，包括人、自行车、汽车、摩托车和飞机）。为每个对象类别创建唯一颜色（以区分检测到的不同对象的边界框）。另外，使用以下代码片段初始化模型配置和预训练权重路径：

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

- 3. 使用Darknet模型配置文件和权重文件加载预训练深度神经网络模型：

```
net = cv2.dnn.readNetFromDarknet(model_configuration,
model_weights)
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_OPENCV)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CPU)
```

- 4. 定义以下函数以获取网络中输出层的名称：

```
def get_output_layers(net):
    layers_names = net.getLayerNames()
    return [layers_names[i[0] - 1] for i in
    net.getUnconnectedOutLayers()]
```

- 5. 要在预测对象周围绘制边界框（以及带有预测置信度的预测类别标签），请定义以下函数：

```python
def draw_box(img, class_id, conf, left, top, right, bottom):
    label = "{}: {:.2f}%".format(classes[class_id], conf * 100)
    color = tuple([int(255*x) for x in colors[class_id]])
    top = top - 15 if top - 15 > 15 else top + 15
    pil_im = Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
    thickness = (img.shape[0] + img.shape[1]) // 300
    font = ImageFont.truetype("arial.ttf", 25)
    draw = ImageDraw.Draw(pil_im)
    label_size = draw.textsize(label, font)
    text_origin = np.array([left, top-label_size[1]] if \
                                   top-label_size[1] >= 0 else [left, top+1])
    for i in range(thickness):
        draw.rectangle([left + i, top + i, right - i, \
                        bottom - i], outline=color)
    draw.rectangle([tuple(text_origin), tuple(text_origin + \
                        label_size)], fill=color)
    draw.text(text_origin, label, fill=(0, 0, 0), font=font)
    del draw
    img = cv2.cvtColor(np.array(pil_im), cv2.COLOR_RGB2BGR)
    return img
```

- 6. 使用非极大值抑制来去除具有相应低置信度分数的边界框，方法是使用以下代码块：

```python
def post_process(img, outs):
    img_height, img_width = img.shape[0], img.shape[1]
    class_ids = []
    confidences = []
    boxes = []
    for out in outs:
        for detection in out:
            scores = detection[5:]
            class_id = np.argmax(scores)
            confidence = scores[class_id]
            if confidence > conf_threshold:
                center_x, center_y = int(detection[0] * \
                    img_width), int(detection[1] * img_height)
                width = int(detection[2] * \
                    img_width)
                height = int(detection[3] * img_height)
                left, top = int(center_x - width / 2), \
                    int(center_y - height / 2)
                class_ids.append(class_id)
                confidences.append(float(confidence))
                boxes.append([left, top, width, height])
    indices = cv2.dnn.NMSBoxes(boxes, confidences, \
conf_threshold, nms_threshold)
for i in indices:
    i = i[0]
    box = boxes[i]
    left, top, width, height = box[0], box[1], box[2], box[3]
    img = draw_box(img, class_ids[i], confidences[i], left, \
    top, left + width, top + height)
return img
```

- 7. 读取输入图像，从输入图像中获取blob（进行所需的预处理），并以图像blob作为输入对YOLO v3模型运行前向传递。最后，调用post_process()函数，为模型检测到的所有对象绘制边界框及其对象类：

```
img = cv2.imread('images/mytable.png')
blob = cv2.dnn.blobFromImage(img, 1/255, (width, height), [0,0,0],
1, crop=False)
net.setInput(blob)
outs = net.forward(get_output_layers(net))
img = post_process(img, outs)
```

运行上述代码块，在图像中绘制检测到的对象周围的边界框，以及与预测类别相对应的置信度。您将获得类似于以下的输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_344_0.png)

## 工作原理...

在`post_process()`函数中，会扫描所有边界框（由模型作为输出返回），随后丢弃置信度分数较低的边界框。检测到对象的类别标签是对应最高概率分数的标签。运行非极大值抑制算法以剪除重叠/冗余的边界框。

使用`opencv-python`函数`cv2.dnn.readNetFromDarknet()`读取预训练Darknet模型，将提供的包含网络架构文本描述的`.cfg`文件路径和包含预训练网络的`.weights`文件路径作为参数。

使用`cv2.dnn.blobFromImage()`函数从输入图像创建4D blob（深度学习模型期望的输入格式）。使用`net.setInput()`函数设置网络的输入。

使用`net.forward()`函数运行前向传递并获取输出层的输出。

最后，使用`post_process()`函数去除重叠的边界框以及置信度较低（低于提供的置信度阈值）的边界框。

## 更多内容...

预训练权重是通过使用MS-COCO图像数据集（从此处获取：http://cocodataset.org/）作为输入来训练模型获得的。尝试自行训练模型，然后使用该模型在一些未见图像中检测对象。

## 另请参阅

请参阅以下链接以了解有关此配方的更多信息：

- https://pjreddie.com/darknet/yolo/
- https://arxiv.org/abs/1506.02640
- https://arxiv.org/abs/1612.08242
- https://arxiv.org/abs/1804.02767
- http://cocodataset.org/
- https://sandipanweb.wordpress.com/2018/03/11/autonomous-driving-car-detection-with-yolo-in-python/
- https://www.youtube.com/watch?v=nDPWywWRlRo
- https://www.youtube.com/watch?v=9s_FpFpdYW8
- https://www.youtube.com/watch?v=iSB_xbYA0wE
- https://www.youtube.com/watch?v=iSB_xbYA0wE
- 《使用Python进行图像处理实战》一书的第10章

## 使用Faster R-CNN进行目标检测

如前一章第7章“图像分割”中的深度实例分割配方所讨论的，基于区域的目标检测方法（例如，R-CNN和Fast R-CNN）依赖于区域建议算法（选择性搜索）来猜测对象位置。Faster R-CNN是另一种基于区域的目标检测模型，由Girshick等人再次提出，作为对R-CNN（2013）和Fast R-CNN（2015）的改进。Fast R-CNN通过引入ROI Pooling降低了检测执行时间（例如，对于较慢的R-CNN模型），但区域建议计算仍然成为瓶颈。Faster R-CNN引入了区域建议网络（RPN）。它通过与检测网络共享卷积特征，实现了几乎零成本的区域建议。

区域建议网络（RPN）是一种全卷积网络（FCN），它在每个位置预测潜在包含对象的区域，并给出对象边界框以及对象性分数（即，区域包含对象的概率）。RPN的端到端训练使其能够预测高质量的区域建议（使用锚点和最近的注意力机制）。随后，Fast R-CNN使用这些区域进行可能的检测。RPN和Fast R-CNN被连接以形成单个网络。该网络使用四个损失进行联合训练：

- RPN提供对象/非对象的分类（带有对象性分数）。
- RPN使用回归计算边界框坐标。
- 最终分类器（来自Fast R-CNN）对对象进行分类（带有分类分数）。
- 通过回归计算输出的对应对象的边界框。

