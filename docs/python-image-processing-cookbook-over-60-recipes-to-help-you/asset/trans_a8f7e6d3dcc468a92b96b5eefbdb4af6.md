

## 准备工作

在本配方中，我们将使用二值长颈鹿图像来演示如何将几种形态学操作应用于二值图像。让我们先导入所需的库：

```
%matplotlib inline
from skimage.io import imread
from skimage.color import rgb2gray
from skimage.filters import threshold_otsu
from scipy.ndimage.morphology import binary_erosion, binary_dilation,
binary_fill_holes
from scipy.ndimage.morphology import morphological_gradient,
distance_transform_edt
from skimage import morphology as morph
import numpy as np
import matplotlib.pyplot as plt
```

## 如何执行...

您需要执行以下步骤，才能使用 scipy.ndimage.morphology 模块中的函数将一些形态学运算符应用于二值图像：

- 1. 读取输入的长颈鹿图像，并使用阈值处理将其转换为二值图像。使用 Otsu 算法获取最优阈值，然后将高于阈值的像素值改为白色和黑色：

```
im = rgb2gray(imread('images/giraffe.jpg'))
thres = threshold_otsu(im)
im = (im > thres).astype(np.uint8)
```

- 2. 使用 2 x 2 方形结构元素（SE/内核）对图像进行腐蚀，并反转图像：

```
eroded = binary_erosion(im, structure=np.ones((2, 2)),
iterations=20)[20:, 20:]
eroded = 1 - eroded
```

- 3. 现在，使用 11 x 11 方形 SE 对腐蚀后的图像进行膨胀，并通过膨胀图像与腐蚀图像之差计算图像的边界。接着，在反转后的边界图像上使用（欧氏）距离变换：

```
dilated = binary_dilation(eroded, structure=np.ones((11, 11)))
boundary = np.clip(dilated.astype(np.int) - eroded.astype(np.int),
0, 1)
dt = distance_transform_edt(np.logical_not(boundary))
```

- 4. 计算二值图像的形态学梯度。反转输出以获取边缘：

```
edges = 1 - morphological_gradient(im, size=3)
```

如果您绘制前面的图像，将得到类似于下图的输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_139_0.png)

再次执行以下步骤，以便能够将更多形态学运算符应用于二值图像，但这次使用 scikit-image.morphology 模块中的函数：

- 1. 读取输入圆形图像并将其转换为二值图像（这次选择阈值为 0）：

```
im = rgb2gray(imread('images/circles.png'))
im = (im > 0).astype(np.uint8)
```

- 2. 使用不同大小的圆盘 SE 对二值图像进行腐蚀。然后，使用连通性为 1 对腐蚀后图像的连通分量进行标记：

```
disk2 = morph.disk(radius=2)
disk8 = morph.disk(radius=8)
eroded2 = morph.binary_erosion(im, selem=disk2)
eroded8 = morph.binary_erosion(im, selem=disk8)
labeled = morph.label(eroded8, connectivity=1)
```

- 3. 膨胀图像并计算图像中的边缘，这次通过膨胀图像与腐蚀图像之差来计算形态学梯度：

```
dilated2 = morph.binary_dilation(im, selem=disk2)
edges = dilated2.astype(np.int) - eroded2.astype(np.int)
```

- 4. 对二值图像进行骨架化：

```
skeleton = morph.skeletonize(im)
```

- 5. 绘制获得的所有图像：

```
plt.figure(figsize=(15,15))
plt.subplot(221), plt.imshow(im), plt.axis('off'),
plt.title('original binary image', size=15)
plt.subplot(222), plt.imshow(labeled, cmap='spectral'),
plt.axis('off')
plt.title('eroded with connected components (radius 8)', size=15)
plt.subplot(223), plt.imshow(edges), plt.axis('off'),
plt.title('edges (radius 2)', size=15)
plt.subplot(224), plt.imshow(skeleton), plt.axis('off')
plt.title('skeleton binary image', size=15)
plt.show()
```

如果运行前面的代码，您将得到类似于以下内容的结果：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_141_0.png)

### 工作原理...

在步骤 1 中，使用了 `scikit-image.filters` 模块的 `threshold_otsu()` 函数来寻找创建二值图像的最优阈值（通过对灰度图像进行阈值处理）。

在步骤 2 中，分别使用了 scipy.ndimage 的 binary_erosion() 和 binary_dilation() 函数将腐蚀和膨胀操作应用于二值图像。这些函数接受要腐蚀/膨胀的二值图像、要使用的结构元素（默认 SE 为连通性为 1 的方形）以及操作重复的次数（默认 1）。非零元素被视为 True。

如您所见，np.ones((2, 2)) 用于创建大小为 2 的方形 SE，而 morph.disk(radius=2) 用于生成圆盘形状的结构元素。该函数来自 scikit-image 形态学模块。在这里，如果两个像素之间的欧氏距离不超过半径值，则认为一个像素是另一个像素的邻居。复合运算符形态学梯度用于计算输入二值图像中的边缘，其计算方法是使用相同的 SE（半径为 2 的圆盘）求膨胀图像与腐蚀图像之差，如下所示：

> $g_B(A) = A \oplus B - A \ominus B$

使用了 scikit-image.morphology 模块的 skeletonize() 函数来计算二值图像的骨架。它使用形态学细化操作将每个连通分量收缩为单像素宽的骨架。从数学上讲，形态学算子骨架定义如下：

> nB = \underbrace{B \oplus \cdots \oplus B}_{n \text{ times}},
> 0B = \{o\}, \quad \text{origin } o
> S - n(A) = (A \ominus nB) - (A \ominus B) \circ B,
> n = 0, 1, \dots, N, \text{size of the structuring element } B
> S(X) = \bigcup_n S_n(X)
> skeleton

使用了 scikit-image.morphology 模块的 label() 函数为二值图像中的每个连通区域分配一个唯一值（作为整数数组）。该函数的 connectivity 参数表示可以认为一个像素是另一个像素邻居的最大正交跳跃次数——对于二值图像，默认值为 2。

### 更多内容...

使用形态学运算符检测链环栅栏中的孔洞，并找出以下链环图像中孔洞的位置（共有两个）（提示：首先将其转换为二值图像）：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_143_0.png)

同样，对提供的 Tetris 图像使用形态学操作以检测模式并生成输出，如下截图所示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_143_1.png)

使用形态学操作在以下卡片图像中查找菱形，如下所示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_144_0.png)

使用以下彩色骰子图像，并使用形态学操作在其中查找点：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_144_1.png)

比较不同库中 *膨胀* 和 *腐蚀* 的实现（例如，scikit-image 的 morphology 模块或 scipy.ndimage 的实现与 Mahotas、OpenCV-Python 和 SimpleITK 的实现），就速度、提供的功能、输出质量等方面进行比较。

形态学运算符也可以扩展到处理灰度输入图像；有关示例，请参阅以下 *另请参阅* 部分中的第二个链接。

## 另请参阅

请参阅以下链接以了解有关此配方的更多信息：

- https://web.stanford.edu/class/ee368/Handouts/Lectures/2019_Winter/7-Morphological.pdf
- https://www.cs.auckland.ac.nz/courses/compsci773s1c/lectures/ImageProcessing-html/topic4.htm
- 《Hands-on Image Processing with Python》一书，第 6 章

## 应用形态学滤波器

从上一个配方中可以看到，形态学运算符（例如腐蚀、膨胀、腐蚀、开运算和闭运算）可以通过二值图像过滤来生长/缩小图像区域，以及移除或填充图像区域边界像素。在本配方中，您将学习如何将更多形态学滤波器应用于二值图像，以增强图像或获得所需结果。更多形态学滤波器包括顶帽变换、形态学梯度和形态学拉普拉斯。

## 准备工作

要开始，请使用以下代码块导入所需的库、模块和函数：

```
#!pip install itk
from skimage.morphology import flood_fill, diameter_closing,
binary_erosion, rectangle, reconstruction
from skimage.filters import threshold_otsu
import mahotas as mh
import itk
import SimpleITK as sitk
import matplotlib.pyplot as plt
import numpy as np
```

