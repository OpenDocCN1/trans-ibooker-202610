

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

3. 定义以下函数，以后处理从网络获得的输出，提取检测到的对象对应的边界框和掩码：

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

4. 使用以下代码片段从标签名称文件加载类名称：

```
classes_file = "models/mask_rcnn/mscoco_labels.names";
classes = None
with open(classes_file, 'rt') as f:
    classes = f.read().rstrip('\n').split('\n')
```

5. 通过加载 TensorFlow 图和模型的权重文件来加载预训练的 Mask R-CNN 模型：

```
text_graph = "models/mask_rcnn/mask_rcnn_inception_v2_coco_2018_01_28.pbtxt"
model_weights = "models/mask_rcnn/frozen_inference_graph.pb"
net = cv2.dnn.readNetFromTensorflow(model_weights , text_graph)
net.setPreferableBackend(cv2.dnn.DNN_BACKEND_OPENCV)
net.setPreferableTarget(cv2.dnn.DNN_TARGET_CPU)
```

6. 准备用于绘制不同类对象边界框的颜色：

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

7. 读取输入图像，预处理图像以创建模型所需的 blob，并使用以下代码片段将 blob 设置为预训练模型的输入：

```
img = cv2.imread('images/road.png')
blob = cv2.dnn.blobFromImage(img, swapRB=True, crop=False)
net.setInput(blob)
```

8. 在预训练网络上使用输入 blob 运行前向传递，然后对获得的输出进行后处理，使用以下代码块绘制检测到的对象周围的边界框和掩码：

```
boxes, masks = net.forward(['detection_out_final',
'detection_masks'])
post_process(boxes, masks)
```

如果运行代码并绘制输入图像与后处理的输出图像，将获得以下输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_356_0.png)

可以看出，Mask R-CNN 正确检测了输入（测试）图像中被部分遮挡的对象（汽车）。

## 工作原理...

`draw_box()` 函数执行以下操作：

- 1. 它在检测到的对象周围绘制边界框。
- 2. 它打印对象分配的（最可能的）类标签。
- 3. 它在边界框的左上角显示标签。
- 4. 然后，它调整掩码、阈值和颜色的大小，并将其应用到图像上。
- 5. 最后，它在图像上绘制与掩码对应的轮廓。

`post_process()` 函数提取每个图像中每个检测对象的边界框及其掩码。然后，它选择合适的颜色（对应于对象的类标签）来绘制对象边界框，并在图像上叠加掩码。

## 更多内容...

在 MS-COCO 数据集上训练 Mask R-CNN 网络（从这里下载数据集：`http://cocodataset.org/`）并保存训练好的模型（强烈推荐使用 GPU）；使用它来预测图像中的对象。

## 另请参阅

- `https://arxiv.org/pdf/1703.06870.pdf`
- `https://www.pyimagesearch.com/2018/11/19/mask-r-cnn-with-opencv/`
- `https://github.com/facebookresearch/Detectron`
- `https://arxiv.org/pdf/1405.0312.pdf`
- `https://www.youtube.com/watch?v=g7z4mkfRjI4`
- `https://www.youtube.com/watch?v=FR25P1lMBY8`
- `https://www.youtube.com/watch?v=FR25P1lMBY8`

## 使用 Python-OpenCV 进行多对象跟踪

**对象跟踪**（在视频中）是一项图像/视频处理任务，用于随时间定位一个或多个移动对象。该任务的目标是在连续视频帧中找到目标对象之间的关联。当对象相对于帧率移动得更快，或要跟踪的对象随时间改变其方向时，任务会变得困难。对象跟踪系统使用运动模型，考虑目标对象在不同可能运动下的变化方式。

对象跟踪在人机交互、安全/监控、交通控制以及更多领域都很有用。由于它考虑了过去帧中对象的外观和位置，在某些情况下，即使对象检测失败，我们仍然可能能够跟踪对象。执行本地搜索的少数跟踪算法非常快。因此，一旦对象首次被检测到，无限期跟踪它通常是一个好的策略。大多数现实世界的应用程序同时实现跟踪和检测。

在本示例中，您将学习如何使用 `opencv-python` 函数在视频中跟踪多个对象，其中视频第一帧中的对象位置将以边界框坐标的形式提供给您。

### 入门

首先导入所需的库。请注意，本示例使用的 `opencv` 版本为 3.4.4：

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

### 如何执行...

执行以下步骤，使用 opencv-python 函数实现多对象跟踪：

- 1. 首先，使用以下代码行创建 `MultiTracker` 对象：

  ```
  multi_tracker = cv2.MultiTracker_create()
  ```

- 2. 您将在视频中跟踪几辆移动的汽车。汽车的位置已为视频的第一帧提供给您。您可以尝试从视频中提取帧，并使用您喜欢的对象检测算法来获取第一帧中汽车的边界框：

  ```
  car_bbox = (141,175,45,29)
  car2_bbox = (295,170,55,39)
  bboxes = [car_bbox, car2_bbox]
  colors = [(0, 255, 255), (255, 255, 0)]
  ```

- 3. 读取输入视频的第一帧，并使用以下代码片段使用帧和提供的边界框坐标（首先定位第一帧中的对象）初始化 OpenCV 多对象跟踪器来启动它：

  ```
  vs = cv2.VideoCapture('images/road.mp4')
  _, frame = vs.read()
  frame = resize(frame, width=500)
  for bbox in bboxes:
      multi_tracker.add(cv2.TrackerCSRT_create(), frame, bbox)
  ```

- 4. 使用以下代码块迭代地从视频流中读取帧（例如，每秒读取 3 帧以减少要读取的帧数），直到读取所有帧：

  ```
  j = 0
  while True:
      vs.set(cv2.CAP_PROP_POS_MSEC, (j*300)) # 1 sec read 3 frames
      _, frame = vs.read()
      if frame is None:
          break
  ```

