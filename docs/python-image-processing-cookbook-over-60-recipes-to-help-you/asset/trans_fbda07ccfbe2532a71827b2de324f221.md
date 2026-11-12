

## 图像分割

图像分割是指将图像划分为不同的区域或类别，每个区域包含具有相似属性的像素，图像中的每个像素都被分配到其中一个类别。

图像分割通常旨在将图像简化为更有意义、更易于分析的段表示。如果分割做得好，图像分析中的所有其他阶段都会变得更简单，这意味着分割的质量和可靠性决定了图像分析是否会成功。但将图像划分为正确的段往往是一个非常具挑战性的问题。

在本章中，我们将学习以下方法：

- 使用 Otsu 和 Ridder–Calvard 进行阈值分割
- 使用自组织映射进行图像分割
- 使用 scikit-image 进行 RandomWalk 分割
- 使用 GMM–EM 算法进行皮肤颜色分割
- 医学图像分割
- 深度语义分割
- 深度实例分割

## 使用 Otsu 和 Ridder-Calvard 进行阈值分割

阈值分割是指一类使用像素值作为阈值，从灰度图像创建二值图像（仅包含黑白像素的图像）的算法（这是最简单的方法，将图像中的前景对象与背景分离）。阈值可以手动选择（通过查看像素值的直方图），也可以使用算法自动选择。图像分割技术可以是非上下文的（不考虑图像中特征之间的空间关系，仅根据某些全局属性（例如颜色/灰度级）对像素进行分组）或上下文的（此外利用空间关系）。在本方法中，你将学习如何使用 `mahotas` 库函数中的几种流行的基于直方图的阈值分割方法，即 Otsu 方法（假设直方图为双峰分布）和 Ridder–Calvard 方法。使用 Otsu 方法，对于阈值的每个可能值，都会计算由该阈值分隔的两个像素类别之间的加权类内方差。最优阈值是使此方差最小的阈值，如下图所示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_235_0.png)

相比之下，Ridder–Calvard 方法通过一个迭代过程自动选择（最优）阈值，该过程在一系列迭代后提供对对象区域越来越干净的提取。

### 准备工作

我们将使用苏巴斯·钱德拉·鲍斯雕像的灰度图像来演示使用 mahotas 进行二值图像分割。像往常一样，让我们先加载所需的库：

```
%matplotlib inline
import mahotas as mh
import numpy as np
import matplotlib.pyplot as plt
```

### 操作步骤

按照以下步骤使用 mahotas 实现阈值分割算法：

1. 首先，读取图像并使用以下代码片段中的 mahotas 库函数获取输入灰度图像的最优阈值。如你所见，对于给定的输入图像，上述两种算法得到的阈值非常接近：

```
image = mh.imread('images/netaji.png')
thresh_otsu, thresh_rc = mh.otsu(image), mh.rc(image)
print(thresh_otsu, thresh_rc)
# 161 161.5062276206947
```

2. 使用为像素值获得的最优阈值对输入图像执行二值分割。你将得到两个不同的二值输出图像，分别对应上述算法：

```
binary_otsu, binary_rc = image > thresh_otsu, image > thresh_rc
```

3. 最后，使用以下代码绘制输入图像、输入图像的直方图、计算的阈值以及获得的输出图像：

```
fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(20, 15))
axes = axes.ravel()
axes[0].imshow(image, cmap=plt.cm.gray)
axes[0].set_title('Original', size=20), axes[0].axis('off')
axes[1].hist(image.ravel(), bins=256, density=True)
axes[1].set_title('Histogram', size=20)
axes[1].axvline(thresh_otsu, label='otsu', color='green', lw=3)
axes[1].axvline(thresh_rc, label='rc', color='red', lw=2)
axes[1].legend(loc='upper left', prop={'size': 20}), axes[1].grid()
axes[2].imshow(binary_otsu, cmap=plt.cm.gray)
axes[2].set_title('Thresholded (Otsu)', size=20),
axes[2].axis('off')
axes[3].imshow(binary_rc, cmap=plt.cm.gray)
axes[3].set_title('Thresholded (Ridder-Calvard)', size=20),
axes[3].axis('off')
plt.tight_layout()
plt.show()
```

### 工作原理

在 `mahotas` 中，阈值函数具有简单的接口：它们接受一个图像并返回一个值。

使用 `otsu()` 函数计算输入图像的最优 Otsu 阈值。

同样，使用 `rc()` 函数计算给定输入图像的 Ridder–Calvard 阈值。

最后，运行代码块后，你将得到如下图形作为输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_238_0.png)

所有值小于或等于最优阈值的像素都被赋值为零（黑色前景对象），而值高于阈值的像素被赋值为一（白色背景），以便为这两种算法分别获得输出二值图像。

### 更多内容

二值分割有许多阈值分割算法；scikit-image 库提供了其中一些算法的实现。你也可以评估这些算法，并选择为你的图像提供最佳结果的算法。对于每种算法，如果你想使用局部版本，必须指定半径；否则，将默认调用全局版本。下图展示了不同的（全局）阈值分割函数在 MIT 统计学课程的输入灰度黑板图像上的表现。请自行实现；你也可以尝试使用不同半径为不同算法实现局部版本，并观察半径对二值输出图像的影响。如下图所示，对于给定的输入图像，Isodata 和 Otsu 在基于阈值的二值分割方面优于其他方法：

### 原始图像

![](img/7e2c8bbde0ebcd14e54d2862adfee480_239_0.png)

摘自 MITx 18.6501x 统计学基础课程的黑板讲义

### Isodata

![](img/7e2c8bbde0ebcd14e54d2862adfee480_239_1.png)

摘自 MITx 18.6501x 统计学基础课程的黑板讲义

### Li

![](img/7e2c8bbde0ebcd14e54d2862adfee480_239_2.png)

摘自 MITx 18.6501x 统计学基础课程的黑板讲义

### Mean

![](img/7e2c8bbde0ebcd14e54d2862adfee480_239_3.png)

摘自 MITx 18.6501x 统计学基础课程的黑板讲义

### 另请参阅

请参阅以下文章，了解有关此方法的更多信息：

- https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4310039
- https://engineering.purdue.edu/kak/computervision/ECE661.08/OTSU_paper.pdf
- https://scikit-image.org/docs/dev/api/skimage.filters.html#skimage.filters.try_all_threshold
- https://mahotas.readthedocs.io/en/latest/thresholding.html
- 第 8 章，图像分割，出自 Packt 出版社的《使用 Python 进行图像处理实战》一书。

## 使用自组织映射进行图像分割

自组织映射（SOM）是一种竞争性学习网络（一类有趣的无监督机器学习），也是最受欢迎的神经网络模型之一。在这个网络中，在任何给定时间只有一个神经元被激活，因此输出神经元之间相互竞争以被激活。这个被激活的神经元称为**获胜神经元**。当一个神经元放电时，它最近的邻居往往比更远处的邻居更容易兴奋（定义了一个距离衰减的拓扑邻域）。因此，神经元被迫通过自适应或学习过程自行组织，并在输入和输出之间创建一个特征映射。这就是为什么这个网络被称为自组织映射。

SOM 算法的自适应过程分为以下两个步骤：

1. **排序（自组织）阶段**：在此阶段，权重向量发生拓扑排序。通常，这一阶段需要大约 1,000 次迭代。
2. **收敛阶段**：在此阶段，特征映射被精细调整，并对输入空间形成准确的统计量化。通常，这一阶段至少需要神经元数量 500 倍的迭代次数。

