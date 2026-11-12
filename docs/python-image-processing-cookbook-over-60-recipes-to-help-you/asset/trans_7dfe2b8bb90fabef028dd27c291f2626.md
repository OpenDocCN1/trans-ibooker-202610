

## 使用约束最小二乘滤波器恢复图像

在此示例中，我们将展示名为 **约束最小二乘（CLS）** 滤波器的又一个频域滤波器。正如滤波器名称所暗示的，它是一种带有额外平滑性约束的逆（最小二乘）滤波器，该约束通过施加平滑性约束，不允许恢复图像中出现任意高频波动。您将学习如何实现 CLS 滤波器，以及如何通过在图像上应用该滤波器来恢复退化图像。此外，您将使用反滤波、维纳滤波和 CLS 等不同频域滤波器实现来比较恢复图像的质量。

## 准备工作

让我们先导入所有所需的库，使用以下代码：

```
import numpy as np
import scipy.fftpack as fp
from skimage.io import imread
from skimage.color import rgb2gray
from skimage.restoration import wiener, unsupervised_wiener
from skimage.measure import compare_psnr
import matplotlib.pylab as plt
```

## 操作步骤

执行以下步骤以实现 CLS 滤波器，并比较不同频域滤波器下的恢复图像：

- 1. 首先，定义 `cls_filter()` 函数以实现约束最小二乘滤波器：

```
def cls_filter(y,h,c,lambd):
    Hf = fp.fft2(fp.ifftshift(h))
    Cf = fp.fft2(fp.ifftshift(c))
    Hf = np.conj(Hf) / (Hf*np.conj(Hf) + lambd*Cf*np.conj(Cf))
    Yf = fp.fft2(y)
    I = Yf*Hf
    im = np.abs(fp.ifft2(I))
    return (im, Hf)
```

- 2. 读取输入图像并将其转换为灰度图像。使用方框核（在频域中完成）对输入灰度图像进行模糊处理，并使用给定标准差（σ）的白高斯噪声（WGN）对图像进行退化：

```
x = rgb2gray(imread('images/building.png'))
M, N = x.shape
h = np.ones((4,4))/16 # blur filter
h = np.pad(h, [(M//2-2, M//2-2), (N//2-2, N//2-2)], \
mode='constant')
sigma = 0.05
Xf = fp.fft2(x)
Hf = fp.fft2(fp.ifftshift(h))
Y = Hf*Xf
y = fp.ifft2(Y).real + sigma*np.random.normal(size=(M,N))
```

- 3. 对退化图像应用伪逆滤波器，以获得恢复图像及伪逆滤波器核的频率响应：

```
epsilon = 0.25
pix, F_pseudo = pseudo_inverse_filter(y, h, epsilon)
```

- 4. 将平衡参数设置为 0.25 应用维纳滤波器，以获得恢复图像：

```
wx = wiener(y, h, balance=0.25)
```

- 5. 接下来，应用无监督维纳滤波器以获得恢复图像：

```
uwx, _ = unsupervised_wiener(y, h)
```

- 6. 构建高通滤波器（二维拉普拉斯算子）并将其转换为频域，以将其用作约束核。使用所创建的约束核以及正则化参数（λ）的值，对退化图像应用 CLS 滤波器：

```
c = np.array([[0,1/4,0],[1/4,-1,1/4],[0,1/4,0]])
c = np.pad(c, [(M//2-1, M//2-2), (N//2-2, N//2-1)], \
mode='constant')
Cf = fp.fft2(fp.ifftshift(c))
lambd = 20
clx, F_restored = cls_filter(y, h, c, lambd)
print(r'Restored (CLS, $\lambda$=${}) PSNR: {}'.format(lambd, \
      np.round(compare_psnr(x, clx),3))
# Restored (CLS, $\lambda$=20) PSNR: 28.924
```

如果运行上述代码片段并绘制所有输入/输出图像及滤波器的频率响应，您将得到类似于以下截图的输出（这只是实际输出的一部分）：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_103_0.png)

从前面的输出中可以看出，CLS 滤波器（$\lambda$ 值较高）和 $wiener$ 滤波器在恢复图像的质量方面表现相当出色。

- 7. 使用以下代码以 3D 形式绘制核的频率响应：

```
plot_freq_spec_3d(F_restored) # frequency response of CLS filter \ kernel
plot_freq_spec_3d(fp.fft2(x))
plot_freq_spec_3d(fp.fft2(y))
plot_freq_spec_3d(fp.fft2(clx))
plot_freq_spec_3d(fp.fft2(uxx))
```

您将作为输出的一部分获得以下截图：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_104_0.png)

## 工作原理

以下图示展示了如何在频域中计算 CLS 滤波器并使用它恢复退化图像：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_105_0.png)

如我们所见，当 $\lambda=0$ 时，CLS 滤波器变为逆滤波器。参数 $\lambda$（正则化器）控制平滑度——$\lambda$ 值越高，恢复的图像越平滑。

二维拉普拉斯核 [[0, 1/4, 0], [1/4, -1, 1/4], [0, 1/4, 0]] 用作 CLS 滤波器的高通滤波器约束核（C）。恢复图像的质量使用 `compare_psnr()` 函数从 skimage.measurement 模块中进行比较，即通过将使用不同滤波器获得的恢复（估计）图像与原始图像进行比较。

`cls_filter()` 函数用于实际实现 CLS 滤波器。它接受退化图像、模糊核、约束高通滤波器（拉普拉斯核）和正则化参数 $\lambda$ 作为输入，并返回恢复的图像以及 CLS 滤波器的频率响应。

## 更多内容

为 CLS 滤波器使用不同的正则化参数值（从 0 等低值到 100 等高值），并观察恢复图像质量如何变化。比较维纳滤波器和 CLS 滤波器，用于恢复带有运动模糊的退化图像（例如，以下书籍图像）。您将得到类似于以下截图的输出（还要使用 MSE 指标来测试图像质量）：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_106_0.png)

## 另请参阅

Refer to the following article to learn more about this recipe:
http://www.cs.uoi.gr/~cnikou/Courses/Digital_Image_Processing/Chapter_05b_Image_Restoration_(Linear_Restoration).pdf

## 使用马尔可夫随机场进行图像恢复

在此示例中，我们将讨论如何使用 **马尔可夫随机场（MRF）** 来对图像进行去噪。假设我们有一个噪声二值图像 X，其像素值为 \(X_{ij} \in \{-1, +1\}\)，我们希望恢复无噪声图像 Y。如果假设噪声量较小，则 X 中的一个像素与 Y 中对应的像素之间存在良好的相关性，并且在 4 连通邻域中，X 的像素之间相关性也很好。这可以建模为如下图所示的 MRF，包含我们希望最小化的总能量函数：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_107_0.png)

## 准备工作

在此示例中，我们将使用 cameraman 灰度图像，并对图像添加噪声。接下来，我们将使用 MRF 通过最小化能量函数来对图像进行去噪。让我们先导入所需的库：

```
import numpy as np
from skimage.color import rgb2gray
import matplotlib.pyplot as plt
```

## 操作步骤

执行以下步骤以使用 MRF 对图像进行去噪：

- 1. 首先，让我们实现 `read_image_and_binarize()` 函数以读取图像并将其二值化（作为预处理步骤）。请注意，输出的二值图像的像素值将来自 {-1, 1}：

```
def read_image_and_binarize(image, threshold=128):
    im = (rgb2gray(plt.imread(image))).astype(int)
    im[im < threshold] = -1
    im[im >= threshold] = 1
    return im
```

- 2. 让我们实现 `compute_energy_helper()` 函数，以根据噪声输入图像 X 和要生成的输出去噪图像 Y 来计算能量：

```
def compute_energy_helper(Y, i, j):
    try:
        return Y[i][j]
    except IndexError:
        return 0

def compute_energy(X, Y, i, j, zeta, eta, Y_i_j):
    energy = -eta * X[i][j] * Y_i_j
    for (k, l) in [(-1,0), (1,0), (0,-1), (0,1)]: # 4-connected nbd
        energy -= zeta * Y_i_j * compute_energy_helper(Y, i+k, j+l)
    return energy
```

- 3. 现在让我们实现一个向图像添加噪声的函数。以下函数通常向图像添加 10% 的噪声：

```
def add_noise(im):
    im_noisy = im.copy()
    for i in range(im_noisy.shape[0]):
        for j in range(im_noisy.shape[1]):
            r = np.random.rand()
            if r < 0.1:
                im_noisy[i][j] = -im_noisy[i][j]
    return im_noisy
```

