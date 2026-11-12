
RPN 在 CNN 特征之上实现了一个滑动窗口。对于窗口的每个位置，它会计算一个得分和一个（每个锚点的）边界框（如果 k 是锚点的数量，则需要计算 4k 个边界框坐标）。由此，Faster R-CNN 使得在测试图像上进行目标检测可以实时完成。下图展示了 Faster R-CNN 网络的架构：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_347_0.png)

在本示例中，你将学习如何在 TensorFlow 中使用预训练的 Faster-RCNN 模型来检测图像中的对象。

## 开始使用

首先，从 TensorFlow 目标检测模型动物园（https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md）下载一个 Faster R-CNN 的预训练模型（例如 faster_rcnn_resnet101_coco，以 ResNet101 为骨干网络并在 MS-COCO 数据集上训练），并将模型的 .pb 文件（冻结的推理图）提取/保存到 models 文件夹的相应位置。现在，让我们使用以下代码片段导入所有必需的包：

```python
import numpy as np
import cv2
from PIL import Image, ImageFont, ImageDraw
```

```python
import json
import colorsys
import matplotlib.pyplot as plt
import tensorflow as tf
print(tf.__version__)
# 2.0.0
```

## 操作步骤...

执行以下步骤，在 TensorFlow 中使用预训练的 Faster R-CNN 网络实现目标检测：

- 1. 初始化 MobileNet SSD 模型用于检测的类别标签列表：

```python
with open('models/image_info_test2017.json','r') as R: js = json.loads(R.read())
labels = {i['id']:i['name'] for i in js['categories']}
print(len(labels))
# 80
```

- 2. 为每个类别的边界框生成一组唯一的颜色：

```python
HSV_tuples = [(x/len(labels), 0.8, 0.8) for x in range(len(labels))]
colors = list(map(lambda x: colorsys.hsv_to_rgb(*x), HSV_tuples))
```

- 3. 读取输入图像和预训练的 TensorFlow 模型的冻结推理图：

```python
img = cv2.imread('images/bus.jpg')
with tf.io.gfile.GFile('models/faster_rcnn/frozen_inference_graph.pb', 'rb') as f:
    graph_def = tf.compat.v1.GraphDef()
    graph_def.ParseFromString(f.read())
```

- 4. 预处理输入图像以作为输入馈送到模型中，恢复会话，并使用以下代码对模型运行前向传播：

```python
rows, cols = img.shape[:2]
conf = 0.2
with tf.compat.v1.Session() as sess:
    inp = cv2.resize(img, (300, 300))
    inp = inp[:, :, [2, 1, 0]] # BGR2RGB
    sess.graph.as_default()
    tf.import_graph_def(graph_def, name='')
    out = sess.run([sess.graph.get_tensor_by_name('num_detections:0'),
                    sess.graph.get_tensor_by_name('detection_scores:0'),
                    sess.graph.get_tensor_by_name('detection_boxes:0'),
                    sess.graph.get_tensor_by_name('detection_classes:0')],
                   feed_dict={'image_tensor:0': inp.reshape(1, inp.shape[0], inp.shape[1], 3)})
```

- 5. 使用以下代码块绘制 Faster R-CNN 预训练模型检测到的对象的边界框（置信度超过阈值 confidence 时）：

```python
for i in range(num_detections):
    idx = int(out[3][0][i])
    score = float(out[1][0][i])
    bbox = [float(v) for v in out[2][0][i]]
    if score > conf:
        x, y, right, bottom = bbox[1] * cols, bbox[0] * rows, bbox[3] * cols, bbox[2] * rows
        label = "{}: {:.2f}%".format(labels[idx], score * 100)
        color = tuple([int(255 * x) for x in colors[idx]])
        y = y - 15 if y - 15 > 15 else y + 15
        thickness, font = (img.shape[0] + img.shape[1]) // 300, ImageFont.truetype("arial.ttf", 15)
        draw = ImageDraw.Draw(Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB)))
        label_size = draw.textsize(label, font)
        text_origin = np.array([x, y - label_size[1]] if y - label_size[1] > 0 else [x, y + 1])
        for i in range(thickness):
            draw.rectangle([x + i, y + i, right - i, bottom - i], outline=color)
        draw.rectangle([tuple(text_origin), tuple(text_origin + label_size)], fill=color)
        draw.text(text_origin, label, fill=(0, 0, 0), font=font)
```

如果运行前面的代码，你将得到类似以下的截图：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_350_0.png)

## 工作原理...

对于图像中检测到的每个对象，通过运行模型的前向传播返回的输出（out）包含以下内容：

- 对象的边界框矩形坐标（`out[2][0][i]`，即第 $i^{th}$ 个对象）
- 分配给对象的带置信度的类别标签（`out[3][0][i]`，第 $i^{th}$ 个对象最可能的类别）
- 对象每个类别标签的概率（置信度）（`out[1][0][i]`，对应第 $i^{th}$ 个对象最可能类别的置信度）

图（计算图）是 TensorFlow 用于表示计算的核心概念。GraphDef 是图的序列化版本，预训练模型的 GraphDef 对象可以使用 ParseFromString() 函数进行解析。

可以使用 import_graph_def() 函数导入序列化的 TensorFlow GraphDef 协议缓冲区。它将单独的 GraphDef 对象提取为 tf.Tensor/tf.Operation 对象。提取后，这些对象会被放置在当前默认图中。

## 更多内容...

使用 opencv-python 加载 TensorFlow 预训练模型并运行推理，以检测图像中的对象。使用 GPU 在 Pascal-VOC 图像上训练你自己的 Faster R-CNN 模型（从此处下载标注的图像数据集：http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar）。

若要使用 Keras 进行训练，请参阅 https://github.com/kbarboo/keras-frcnn。使用原始 R-CNN 和 Fast R-CNN 模型实现目标检测，并在测试图像上与 Faster-RCNN 进行速度和精度对比。

## 另请参阅

请参阅以下链接以了解更多关于本示例的信息：

- https://arxiv.org/pdf/1506.01497.pdf
- https://papers.nips.cc/paper/5638-faster-r-cnn-towards-real-time-object-detection-with-region-proposal-networks.pdf
- https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md
- https://stackoverflow.com/questions/47059848/difference-between-tensorflows-graph-and-graphdef
- https://www.tensorflow.org/api_docs/python/tf/graph_util/import_graph_def
- http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
- https://www.youtube.com/watch?v=Z-CmHOoOJJA

## 使用 Mask R-CNN 进行目标检测

由 Girshick 等人提出的 **Mask R-CNN 算法**（2017 年）在用于基于区域的目标检测的 Faster R-CNN 算法基础上包含了多项改进，主要有以下两点贡献：

- ROI Pooling 被 **ROI Align 模块** 取代（更准确）。
- 在 ROI Align 模块的输出处插入一个额外的分支（该分支接收来自 ROI Align 的输出，随后将其馈送到两个连续的卷积层中。最后一个卷积层的输出构成对象掩码）。

`RoIAlign` 模块在所选特征映射区域和输入图像区域之间提供了更精确的对应关系。像素级分割需要更精细的对齐，而不仅仅是计算边界框。下图展示了 **Mask R-CNN** 的架构：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_352_0.png)

在本示例中，你将学习如何使用预训练的 Mask R-CNN (`tensorflow`) 模型检测图像中的对象，这次使用 `opencv-python` 库函数。

## 开始使用

首先，从 http://download.tensorflow.org/models/object_detection/mask_rcnn_inception_v2_coco_2018_01_28.tar.gz 下载预训练的 Mask R-CNN 模型（该模型再次在 MS-COCO 数据集上用 tensorflow 训练，并以 Inception v2 作为骨干网络），并在 models 文件夹内的相应路径中解压缩该模型。使用以下命令导入所有必要的包：

```python
import cv2
print(cv2.__version__)
# 4.1.1
import numpy as np
import os.path
import sys
import random
import matplotlib.pylab as plt
```

## 操作步骤...

执行以下步骤，使用 opencv-python 函数和 Mask R-CNN 预训练模型实现目标检测：

- 1. 初始化参数（例如，Confidence 和 Mask 的阈值），使用以下代码片段：

```python
conf_threshold = 0.5 # Confidence threshold
mask_threshold = 0.3 # Mask threshold
```

- 2. 定义以下函数，用于绘制检测对象的预测边界框，根据预测的对象类为其着色，并将计算出的掩码叠加在输入图像上：

```python
def draw_box(img, class_id, conf, left, top, right, bottom,
             class_mask):
    cv2.rectangle(img, (left, top), (right, bottom), \
                  (255, 178, 50), 3)
    label = '%.2f' % conf
    classes:
    assert(class_id < len(classes))
    label = '%s:%s' % (classes[class_id], label)
    label_size, base_line = cv2.getTextSize(label, \
                                           cv2.FONT_HERSHEY_SIMPLEX, 0.5, 1)
    top = max(top, label_size[1])
    cv2.rectangle(img, (left, top - round(1.5*label_size[1])), \
                  (left + round(1.5*label_size[0])), \
```
