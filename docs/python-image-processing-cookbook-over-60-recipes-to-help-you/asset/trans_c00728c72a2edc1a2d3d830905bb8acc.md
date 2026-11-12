

## 准备工作

让我们导入开始所需的包和模块：

```
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

## 操作步骤...

首先，我们将检测并分离斑块，然后开始对象计数。

## 使用腐蚀进行斑块分离与检测

使用形态学腐蚀检测和分离斑块，这次使用 OpenCV-Python 函数，请执行以下步骤：

- 1. 读取灰度圆图像并使用阈值处理将其转换为二值图像：

```
image = cv2.imread('images/circles.png', 0)
image = cv2.threshold(image, 100, 255, cv2.THRESH_BINARY)[1]
```

- 2. 创建二值腐蚀的 SE（内核，使用不同内核大小）并腐蚀图像：

```
kernel = np.ones((21,21),np.uint8)
eroded = cv2.morphologyEx(image, cv2.MORPH_ERODE, kernel)
kernel = np.ones((11,11),np.uint8)
eroded1 = cv2.morphologyEx(image, cv2.MORPH_ERODE, kernel)
```

- 3. 现在，使用以下代码行查找腐蚀图像的轮廓：

```
cnts, _ = cv2.findContours(eroded, cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
```

- 4. 最后，使用不同颜色在输入图像上绘制轮廓：

```
output = cv2.cvtColor(image.copy(), cv2.COLOR_GRAY2RGB)
count = 0
for c in cnts:
    cv2.drawContours(output, [c], -1, (np.random.randint(0,255), \
np.random.randint(0,255), np.random.randint(0,255)), 2)
    count += 1
```

如果你运行前面的代码片段并绘制图像，你将得到以下输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_173_0.png)

如前面的输出所示，连接的圆圈已被分离，所有 22 个圆圈都已检测到。

## 使用闭运算和开运算进行对象计数

使用形态学闭运算和开运算计数对象，再次使用 OpenCV-Python 函数，请执行以下步骤：

- 1. 读取 rasagolla 输入（一种典型的孟加拉甜点）图像，将其转换为灰度，并反转：

```
image = cv2.imread('images/rasagolla.jpg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
gray = cv2.bitwise_not(gray) #255 - gray
```

- 2. 使用 Canny 边缘检测器查找图像中的边缘，对边缘图像进行阈值处理，然后，使用 4 x 4 方形 SE，对阈值化的边缘图像应用二值闭运算，接着是二值开运算（以分离对象）：

```
edged = cv2.Canny(gray, 50, 150)
thresh = cv2.threshold(gray, 100, 255, cv2.THRESH_BINARY_INV)[1]
kernel = np.ones((4, 4), np.uint8)
thresh = cv2.morphologyEx(thresh, cv2.MORPH_CLOSE, kernel) #Close
thresh = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel) #Open
```

- 3. 现在，查找前面代码获得的图像中前景对象的轮廓（即轮廓线）：

```
_, cnts, _ = cv2.findContours(thresh.copy(), cv2.RETR_EXTERNAL,
cv2.CHAIN_APPROX_SIMPLE)
```

- 4. 最后，遍历找到的轮廓，在输出图像上绘制它们（再次使用不同颜色），忽略小对象：

```
output = image.copy()
count = 0
for c in cnts:
    if cv2.contourArea(c) > 5: # ignore small objects
        cv2.drawContours(output, [c], -1, \
            (np.random.randint(0, 255), \
            np.random.randint(0, 255), \
            np.random.randint(0, 255)), 2)
        count += 1
```

如果你运行前面的代码片段并绘制图像，你将得到以下输出，检测到所有八个对象：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_175_0.png)

## 工作原理...

opencv-python 的 morphologyEx() 函数用于对图像应用形态学操作。该函数接受源图像（即二值图像）、形态学操作（例如 cv2.MORPH_CLOSE 和 cv2.MORPH_OPEN）以及 SE 内核（例如，用于 4 x 4 方形内核的 np.ones((4, 4), np.uint8)）来执行形态学操作。

cv2.findContours() 和 cv2.drawContours() 函数分别用于查找和绘制轮廓。输出图像上的每个轮廓都绘制了随机颜色的轮廓线，然后通过循环找到的轮廓逐个显示。

(np.random.randint(0, 255), np.random.randint(0, 255), np.random.randint(0, 255)) 被用于生成随机 BGR 元组。cv2.contourArea() 函数被用于丢弃面积小于或等于 5 的小对象。

## 更多内容...

使用形态学操作从以下二值图像中检测并分离对象：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_176_0.png)

你也能从二值输入图像中检测出既不是垂直也不是水平的线条吗？

现在，进行一些地理空间图像处理：从以下地图图像（印度名为 Sundarban 的红树林森林）中计算岛屿数量，你将得到类似以下的图表：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_177_0.png)

## 另请参阅

请参阅以下文章以了解有关此配方的更多信息：

- https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_morphological_ops/py_morphological_ops.html
- https://docs.opencv.org/trunk/d4/d86/group__imgproc__filter.html#ga67493776e3ad1a3df63883829375201f
- https://www.pyimagesearch.com/2015/11/02/watershed-opencv/

## 图像配准

图像配准是指一种图像处理任务，其目标是将目标图像与源图像对齐。从更广义的角度来说，它旨在计算空间变换（函数），将一张图像中的（某些）点映射到另一张图像中的对应点。通常，从一张图像到另一张图像查找变换被称为对齐，然后使用估计的变换实际执行图像变形过程被称为**配准**。有三种一般哲学用于比较以确定对齐：

- 基于强度（比较一张图像与另一张图像的实际像素值，例如，使用互信息）
- 基于分割（注册二值分割）
- 基于地标（或特征）（在两张图像中标记关键点，并推导出使每对地标匹配的变换）

我们将估计（用于注册图像）的变换可能是以下任何一种类型：

- 刚性（旋转、平移）
- 仿射（刚性 + 缩放和剪切/倾斜）
- 可变形（自由形式 = 仿射 + 向量场）

也有许多其他类型的变换。

在本章中，我们将专注于基于强度和特征的图像配准技术及其应用，并将介绍以下配方：

- 使用 SimpleITK 进行医学图像配准
- 使用 ECC 算法和变形进行图像对齐
- 使用 dlib 进行人脸对齐
- 使用 RANSAC 算法进行鲁棒匹配和单应性
- 图像拼接（全景）
- 人脸变形
- 实现图像搜索引擎

## 使用 SimpleITK 进行医学图像配准

如前所述，配准的目标是估计关联给定输入图像中点的变换。通过配准估计的变换据说将点从**固定**图像坐标系映射到**移动**图像坐标系。SimpleITK 提供了一个可配置的多分辨率配准框架，在 `ImageRegistrationMethod` 类中实现。要使用 `ImageRegistrationMethod` 创建特定的配准实例，你需要选择几个共同定义配准实例的组件：

- 变换
- 相似度度量
- 优化器
- 插值器

以下图显示了 SimpleITK 中配准框架的组件：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_180_0.png)

图像配准是在参数空间中使用优化完成的（要应用于移动图像以与固定图像对齐的变换由参数定义）；这些参数使用优化算法进行调整，以优化图像之间计算的相似度度量。

## 准备工作

我们将使用 CT 扫描图像和 MRI-T1 图像来演示使用 SimpleITK 进行图像配准。这些图像从从 https://www.insight-journal.org/rire/download.php 下载的原始 (.mhd) 文件中提取（同意许可后下载 ZIP 并提取文件）。让我们开始加载所需的库，一如既往：

```
%matplotlib inline
import SimpleITK as sitk
import numpy as np
import matplotlib.pyplot as plt
```

## 操作步骤...

执行以下步骤以使用 SimpleITK 实现图像配准：

- 1. 首先，读取要对齐的图像，将 CT 扫描图像作为固定（目标）图像，将 MRI-T1 图像作为移动（源）图像：

```
fixed_image = sitk.ReadImage("images/ct_scan_11.jpg", \
sitk.sitkFloat32)
moving_image = sitk.ReadImage("images/mr_T1_06.jpg", \
sitk.sitkFloat32)
```

- 2. 将 SimpleITK Image 对象转换为 NumPy 数组，以便使用 matplotlib 显示图像：

```
fixed_image_array = sitk.GetArrayFromImage(fixed_image)
moving_image_array = sitk.GetArrayFromImage(moving_image)
print(fixed_image_array.shape, moving_image_array.shape)
# (512, 512) (512, 512)
```
