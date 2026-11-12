

## 操作方法...

在此示例中，我们将使用 scikit-image 的形态学分水岭实现对两幅图像进行分割（第一幅是二值图像，第二幅是灰度图像），通过两种不同的方式创建标记：

- 在欧氏距离图像中寻找峰值
- 在梯度图像中寻找低梯度区域

## 形态学分水岭

执行以下步骤，以使用 skimage.morphology 模块的形态学分水岭实现进行二值图像分割：

1. 首先，读取输入的二值圆图像，并将其转换为无符号整数类型的灰度图：

```python
image = img_as_ubyte(rgb2gray(imread('images/circles.png')))
```

2. 计算精确的欧氏距离变换，并在其中寻找峰值，随后对其进行标记，以便用作分水岭算法的标记：

```python
distance = ndi.distance_transform_edt(image)
local_maximum = peak_local_max(distance, indices=False, footprint=np.ones((3, 3)), labels=image)
markers = ndi.label(local_maximum)[0]
```

3. 现在，使用创建的标记运行分水岭算法，以获取输出分割标签。如有需要，移除小对象以消除噪声：

```python
labels = watershed(-distance, markers, mask=image)
labels = remove_small_objects(labels, min_size=100)
```

4. 计算输出中分割对象（标签）的数量以及唯一类别（标签）的数量：

```python
props = regionprops(labels)
print(len(np.unique(labels)), len(props))
# 23 22
```

如果你运行前面的代码片段并绘制图像，将得到类似于下面的图示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_165_0.png)

5. 现在，对另一幅图像（一幅莲花图像）运行形态学分水岭算法，但这次，通过对梯度进行阈值处理来创建标记（将连续的低梯度区域作为标记，并使用半径为 5 的圆盘结构元素以获得更平滑的图像）。首先使用 `median` 滤波器对图像进行去噪：

```python
denoised = rank.median(image, disk(2))
markers = rank.gradient(denoised, disk(5)) < 20
markers = ndi.label(markers)[0]
```

6. 像上次一样运行形态学分水岭算法，但这次，使用梯度和标记作为输入（使用半径为 2 的小圆盘结构元素的局部梯度，以保持边缘细薄）：

```python
gradient = rank.gradient(denoised, disk(2))
labels = watershed(im, basins)
labels = remove_small_objects(labels, min_size=100)
props = regionprops(labels)
print (len(np.unique(labels)), len(props))
# 111 110
```

如果你运行前面的代码片段并绘制图像，将得到以下输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_166_0.png)

如果你以 3D 方式绘制梯度图像，将得到以下输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_167_0.png)

## 使用形态学分水岭进行斑点检测

执行以下步骤，以再次使用 `skimage.morphology` 模块的分水岭算法，这次用于斑点检测：

1. 实现 `segment_with_watershed()` 函数，用于检测图像中的斑点：

```python
def segment_with_watershed(im, cell_thresh, bg_thresh):
    if np.max(im) != 1.0:
        im = (im - im.min()) / (im.max() - im.min())
    im_mask = im < cell_thresh
```

2. 生成集水区和流域：

```python
basins = np.zeros_like(im)
basins[im < cell_thresh] = 2
basins[im > bg_thresh] = 1
```

3. 通过淹没流域运行分水岭分割：

```python
flood_seg = watershed(im, basins)
flood_seg = flood_seg > 1.0
```

4. 现在，腐蚀边界并计算距离变换：

```python
selem = square(3)
flood_erode = binary_erosion(flood_seg, selem=selem)
flood_seg = clear_border(flood_seg, buffer_size=10)
```

5. 使用以下代码行计算距离矩阵：

```python
distances = distance_transform_edt(flood_seg)
```

6. 在距离矩阵中寻找局部极大值：

```python
local_max = peak_local_max(distances, indices=False,
footprint=None, labels=flood_seg,
min_distance=1)
max_lab = label(local_max)
```

7. 执行拓扑分水岭。移除任何散落的小对象：

```python
final_seg = watershed(-distances, max_lab, mask=flood_seg)
final_seg = remove_small_objects(final_seg, min_size=100)
```

8. 提取区域属性并确定细胞数量。返回结果：

```python
props = regionprops(final_seg)
num_cells = len(props)
return final_seg, distances, basins, num_cells
```

9. 现在，读取输入图像，将其转换为灰度图，进行去噪，并通过调用 `img_as_ubyte()` 函数在其中查找斑点。打印返回的分段数量：

```python
image = img_as_ubyte(rgb2gray(imread('images/sunflowers.jpg')))
image = rank.median(image, disk(2))
labels, distances, markers, nseg = segment_with_watershed(image,
0.25, 0.28)
```

```python
print(nseg)
# 44
```

如果你运行前面的代码片段，将得到如下输出，其中已检测到斑点，如所示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_169_0.png)

## 工作原理

来自 skimage.morphology 模块的 `watershed()` 函数被用于查找输入图像中的分水岭流域，淹没过程从给定的标记开始。该函数接受图像和标记（可选）作为输入参数。`watershed()` 函数接受一个 ndarray 标记作为第二个参数，其中标记了流域的位置（像素值用于为标签矩阵中的像素分配标签），0 表示不是标记。

来自 `scipy.ndimage.morphology` 的 `distance_transform_edt()` 函数用于计算由以下方程给出的精确欧氏变换：

$d_p = \sqrt{(p_x - b_x)^2 + (p_y - b_y)^2}$

在这里，$b$ 是输入像素 $p$ 最近的背景像素（具有最小的欧氏距离）；返回 $p$ 和 $b$ 之间的距离 $d_p$。

来自 `skimage` 模块的 `label()` 函数用于对从欧氏距离变换的局部极大值峰值中获得的标记，或通过对梯度图像进行阈值处理所获得的标记中的特征进行标记。该函数为输入图像中的每个唯一特征分配一个唯一标签。来自 `skimage.measure` 模块的 `regionprops()` 函数用于获取区域属性列表，其中每个项描述一个标记区域。

## 更多内容

你也可以使用 LOG 尺度空间检测斑点。下面是使用此方法进行斑点检测的演示；请将其与使用形态学分水岭获得的结果进行比较。

## 使用 LOG 尺度空间进行斑点检测

执行以下步骤，在 LOG 空间中进行斑点检测：

1. 使用 `blob_log()` 函数在图像中查找斑点：

```python
blobs_log = blob_log(np.invert(image), max_sigma=40, num_sigma=10, threshold=.2)
```

2. 计算第三列中的半径：

```python
blobs_log[:, 2] = blobs_log[:, 2] * np.sqrt(2)
```

3. 遍历找到的斑点并在输出图像上绘制它们，忽略半径较小的斑点：

```python
for blob in blobs_log:
    y, x, r = blob
    if np.pi*r**2 > 150:
        c = plt.Circle((x, y), r, color=[0.75] +
        np.random.rand(2).tolist(), linewidth=2,
        fill=True, alpha=.7)
# add the blobs to the plot
```

如果你运行前面的代码片段并绘制图像，将得到类似于下面的图示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_171_0.png)

## 另请参阅

请参阅以下链接以了解更多有关此示例的信息：

- https://www.doc.ic.ac.uk/~dfg/vision/v02.html
- https://scikit-image.org/docs/dev/api/skimage.morphology.html#skimage.morphology.watershed
- https://docs.scipy.org/doc/scipy/reference/generated/scipy.ndimage.label.html

## 对象计数

在此示例中，你将学习如何使用形态学滤波器在二值图像中计数对象。通常，二值图像中的对象（斑点）是重叠的，在计数之前，我们需要进行一些必要的预处理，例如斑点分离和检测。形态学腐蚀在这些情况下非常有用。然后，可以使用轮廓查找来计数分离后的对象。你也可以使用形态学分水岭分割来分离斑点，然后对它们进行计数。

