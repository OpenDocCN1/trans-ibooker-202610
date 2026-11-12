

## 如何执行...

在本示例中，您将学习如何使用不同库（例如，mahotas、skimage 和 SimpleITK）中的函数，对输入的二值图像应用几种不同的形态学滤波器。首先执行以下步骤，使用 mahotas 计算二值图像的几个基本指标（例如，欧拉数），并使用 flood_fill 和 diameter_closing 等滤波器（使用 skimage.morphology 模块）。

## 使用 mahotas/scikit-image 计算欧拉数、离心率和质心

执行以下步骤，使用 mahotas 和 skimage 库函数计算不同的指标：

- 1. 读取 NASA 黑洞图像（https://www.nasa.gov/sites/default/files/thumbnails/image/blackhole.png），并使用手动阈值（t=60）将其转换为二值图像。计算二值图像的欧拉数：

```python
blackhole = mh.imread('images/blackhole.png')
blackhole_gray = mh.colors.rgb2grey(blackhole).astype(np.uint8)
t = 60
bin_blackhole = (blackhole_gray > t).astype(np.uint8)
print(mh.euler(bin_blackhole))
# 0.0
```

- 2. 计算二值图像的 center_of_mass 和 eccentricity：

```python
cms = mh.center_of_mass(bin_blackhole)
print('Eccentricity = ', mh.features.eccentricity(bin_blackhole))
# 0.5449847073316407
```

- 3. 使用 flood_fill（从黑洞内部的种子点开始）或 diameter_closing（移除面积小于或等于 100 的所有空洞）之一来填充黑洞：

```python
bin_blackhole2 = flood_fill(bin_blackhole, (200, 400), 1)
bin_blackhole3 = diameter_closing(bin_blackhole, 100,
connectivity=2)
```

如果运行上述代码片段并绘制图像，您将得到类似于下图的图表：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_147_0.png)

二值图像的拓扑通过欧拉数来衡量。它定义为图像中对象总数与这些对象中存在的空洞数量之差。对于第一个二值图像，欧拉数为 0 = 1 - 1（它包含一个对象，该对象包含一个空洞）。另一方面，对于第二个二值图像，欧拉数为 1 - 0 = 1（因为它包含的单个对象中没有任何空洞）。

## 使用 mahotas 的形态学图像滤波器

执行以下步骤，再次使用 mahotas 库函数查看应用几种形态学图像滤波器的结果：

- 1. 读取彩色分形（mandelbrot）图像，将其转换为灰度图像，然后使用 Otsu 阈值法将其转换为二值图像：

```python
fractal = mh.imread('images/mandelbrot.jpg')
fractal_gray = mh.colors.rgb2grey(fractal).astype(np.uint8)
t = mh.otsu(fractal_gray)
bin_fractal = (fractal_gray > t).astype(np.uint8)
```

- 2. 使用以下代码行计算白色和黑色像素之间的边界：

```python
mh.border(bin_fractal, 0, 1)
```

- 3. 使用以下代码行计算区域最小值（区域定义为围绕每个像素的 3 x 3 十字形结构元素，并考虑整个对象）：

```python
mh.regmin(bin_fractal)
```

- 4. 使用半径为 3 的圆盘结构元素闭合图像中的空洞，使用以下代码行：

```python
mh.close_holes(bin_fractal, mh.disk(3))
```

- 5. 使用以下代码行将 majority_filter 应用于二值图像：

```python
mh.majority_filter(bin_fractal)
```

最后，如果绘制所有图像以及原始二值图像的质心，您将得到类似于下图的图表：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_148_0.png)

## 使用 SimpleITK 的二值图像滤波器

执行以下步骤，演示使用 SimpleITK 库函数对二值图像应用几个额外的二值滤波器：

- 1. 首先，读取 NASA 超新星图像（https://www.nasa.gov/image-feature/goddard/2019/hubble-sets-sights-on-an-explosive-galaxy）并将图像转换为二值图像。对二值图像取反：

```python
supernova_gray = sitk.ReadImage('images/supernova.jpg',
sitk.sitkFloat32)
supernova_bin =
sitk.BinaryNotImageFilter().Execute(sitk.OtsuThresholdImageFilter()
.Execute(supernova_gray))
```

- 2. 实例化对应于开运算和闭运算形态学滤波器的 SimpleITK 滤波器对象。将结构元素设置为半径为 3 的圆盘。首先对二值超新星图像应用开运算滤波器，然后应用闭运算滤波器：

```python
open_f = sitk.BinaryMorphologicalOpeningImageFilter()
close_f = sitk.BinaryMorphologicalClosingImageFilter()
open_f.SetKernelRadius(3)
close_f.SetKernelRadius(3)
supernova_bin1 = close_f.Execute(open_f.Execute(supernova_bin))
```

- 3. 再次对输入二值图像应用开运算滤波器，然后应用闭运算滤波器，但这次将结构元素设置为半径为 7 的圆：

```python
open_f.SetKernelRadius(7)
close_f.SetKernelRadius(7)
supernova_bin2 = close_f.Execute(open_f.Execute(supernova_bin))
```

- 4. 接下来，将结构元素为半径为 3 的圆盘的中值滤波器应用于输入二值图像：

```python
med_f = sitk.BinaryMedianImageFilter()
med_f.SetRadius(3)
supernova_bin3 = med_f.Execute(supernova_bin)
```

- 5. 使用以下代码片段将 BinaryMinMaxCurvatureFlowImageFilter() 应用于二值图像（模板半径为 2，阈值参数值为 60）：

```python
curv_f = sitk.BinaryMinMaxCurvatureFlowImageFilter()
curv_f.SetStencilRadius(2)
curv_f.SetThreshold(60)
supernova_bin4 = curv_f.Execute(sitk.Cast(supernova_bin,
sitk.sitkFloat32))
```

- 6. 实例化相应对象后，将 BinaryContourImageFilter() 应用于图像：

```python
cont_f = sitk.BinaryContourImageFilter()
supernova_bin5 = cont_f.Execute(supernova_bin)
```

- 7. 最后，在二值图像上实例化 VotingBinaryImageFilter()：

```python
vote_f = sitk.VotingBinaryImageFilter()
vote_f.SetRadius(5)
supernova_bin6 = vote_f.Execute(sitk.Cast(supernova_bin2,
sitk.sitkInt32))
```

如果运行上述代码片段并绘制所有图像，您将得到类似于下图的图表：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_151_0.png)

## 使用 skimage 的重建膨胀

执行以下步骤，使用 skimage 库函数通过重建膨胀将绘图与文本分开：

- 1. 读取输入图像（泰戈尔手写的一首歌曲的充满插图的孟加拉语手稿），并使用阈值处理然后进行二值反转将其转换为二值图像（您希望将字母作为前景对象，即白色）：

```python
img = rgb2gray(imread('images/tagore_manuscript.jpg'))
th = 0.6 #threshold_otsu(img)
img[img <= th] = 0
img[img > th] = 1
img = 1 - img
```

- 2. 重建膨胀所需的掩码图像就是先前获得的二值图像。使用带有垂直线滤波器的 binary_erosion 操作创建种子图像（重建膨胀）。最后，使用创建的掩码图像和种子图像执行重建膨胀：

```python
mask = img
seed = binary_erosion(img, rectangle(1, 50))
words = reconstruction(seed, mask, 'dilation')
```

如果运行上述代码片段并绘制所有图像，您将看到如下图所示，绘图与书写（单词）已分开：

### 泰戈尔充满插图的手稿

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_0.png)

### 重建的种子

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_1.png)

### 泰戈尔的绘图

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_2.png)

### 泰戈尔的无插图手稿

![](img/7e2c8bbde0ebcd14e54d2862adfee480_153_3.png)

## 工作原理

使用 `mahotas` 库的 `euler()` 函数来计算输入二值图像的欧拉数（或特征）。默认情况下，它使用 8 连通性。

使用 `skimage.morphology` 模块的 `flood_fill()` 函数对黑洞二值图像执行泛洪填充，起始于黑洞内部暗区域的特定种子点 (200,400)（作为第二个参数传递给函数）；找到等于 `seed pixel value`（此处为 0 或黑色）的连接点，然后将其设置为 `new_value`（此处为 1 或白色），这是函数的第三个输入参数。

使用 `skimage.morphology` 模块的 `diameter_closing()` 函数移除黑洞图像中所有最大延伸（围绕 `hole` 的 `bounding box` 的一边）小于 `diameter_threshold` 参数（即 100）的所有 `holes`。该操作也称为 **边界框闭运算**。

使用 `skimage.morphology` 模块的 `reconstruction()` 函数实现了重建膨胀。

重建膨胀操作使用一个 `seed` 和一个 `mask` 图像。种子图像指定起始值（该值使用结构元素用于 `dilation` 迭代 `spread`），而掩码图像（原始图像用作掩码图像）指定每个像素允许的最大值。掩码图像对 `foreground` 值的 `spread` 施加约束。

使用 `BinaryMedianImageFilter()` 构造函数实例化了二值中值滤波器（用于去噪超新星图像），并使用 `SetRadius()` 方法设置了该形态学滤波器的圆盘结构元素半径。该滤波器使用 `Execute()` 方法应用于输入二值图像，其他 SimpleITK 滤波器也是如此。

