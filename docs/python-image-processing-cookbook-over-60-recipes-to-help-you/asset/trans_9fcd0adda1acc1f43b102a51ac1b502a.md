


```python
0, 1)),
    transforms.Lambda(lambda tensor:tensor_round(tensor))
 ])
```

- 4. 下载 lfw 数据集，并创建批大小为 8 的数据加载器：

```python
batch_size = 8 # 16
dataset = fetch_lfw_people(min_faces_per_person=70,
   resize=0.4).images / 255
dataloader = DataLoader(dataset, batch_size=batch_size,
   shuffle=True)
```

- 5. 现在，实现自动编码器类、编码器和解码器成员以及该类的 forward() 方法：

```python
class autoencoder(nn.Module):
    def __init__(self):
        super(autoencoder, self).__init__()
        self.encoder = nn.Sequential(
            nn.Linear(50 * 37, 512),
            nn.ReLU(True),
            nn.Linear(512, 128),
            nn.ReLU(True))
        self.decoder = nn.Sequential(
            nn.Linear(128, 512),
            nn.ReLU(True),
            nn.Linear(512, 50 * 37),
            nn.Sigmoid())
    def forward(self, x):
        x = self.encoder(x)
        x = self.decoder(x)
        return x
```

- 6. 实例化自动编码器类，然后使用以下代码片段将损失函数定义为二元交叉熵（BCE）损失，并将优化器定义为 Adam 优化器：

```python
learning_rate = 1e-3
cuda = False #True
model = autoencoder()
if cuda:
    model = model.cuda()
criterion = nn.BCELoss()
optimizer = torch.optim.Adam(model.parameters(),
    lr=learning_rate, weight_decay=1e-5)
```

- 7. 训练自动编码器 100 个时代。在每个时代中，使用前向传播进行自动编码器预测（以及损失函数计算）和反向传播来更新不同层中的权重：

```python
num_epochs = 100
for epoch in range(1, num_epochs+1):
    for data in dataloader:
        img = data
        #...
        if cuda: noisy_img = noisy_img.cuda()
        img = Variable(img)
        if cuda: img = img.cuda()
        output = model(noisy_img)     # forward-prop
        loss = criterion(output, img)
        MSE_loss = nn.MSELoss()(output, img)
        optimizer.zero_grad()
        loss.backward()   # back-prop
        optimizer.step()
        print('epoch [{}/{}] , loss::{:.4f}, MSE_loss::{:.4f}'
              .format(epoch, num_epochs, loss.data.item(), MSE_loss.data.item()))
        if epoch % 10 == 0:
            x = to_img(img.cpu().data)
            x_hat = to_img(output.cpu().data)
            x_noisy = to_img(noisy_img.cpu().data)
            weights = to_img(model.encoder[0].weight.cpu().data)
```

- 8. 打印模型以查看网络架构：

```python
print (model)
# autoencoder(
#   (encoder): Sequential(
#     (0): Linear(in_features=1850, out_features=512, bias=True)
#     (1): ReLU(inplace)
#     (2): Linear(in_features=512, out_features=128, bias=True)
#     (3): ReLU(inplace)
#   )
#   (decoder): Sequential(
#     (0): Linear(in_features=128, out_features=512, bias=True)
#     (1): ReLU(inplace)
#     (2): Linear(in_features=512, out_features=1850, bias=True)
#     (3): Sigmoid()
#   )
# )
```

下图显示了网络结构，以及先前的去噪自动编码器如何与 lfw 面部数据集一起工作：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_63_0.png)

## 工作原理...

我们从 PyTorch 使用 `nn.Sequential()`，通过依次指定构建块（`nn.Module`）来构建自动编码器。来自噪声图像的每个像素都形成一个输入节点，因此输入有 50*37 个节点。输出也具有相同数量的节点。

使用 `nn.Linear(50*37, 512)` 实例化了大小为 1,850 x 512 的全连接层。`nn.ReLU(True)` 用于在输入/隐藏层之间应用原地 ReLU 非线性，而 `nn.Sigmoid()` 用于在最终层之前对输出层应用 Sigmoid 函数。使用 `nn.BCELoss()` 采用二元交叉熵损失。`backward()` 函数用于反向传播并重新计算神经网络中的权重。

可以通过设置 cuda = True 在 GPU 上运行代码，以运行得更快。

下图显示了原始图像、噪声图像、自动编码器重建的输出图像，以及编码器第一个隐藏层在两个不同时代结束时学到的几个特征：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_64_0.png)

如下图所示，随着时代数量的增加，自动编码器输出生成的去噪图像与相应的原始图像越来越相似：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_65_0.png)

## 更多内容...

使用彩色图像（向 fetch_lfw_people() 函数传递 color=True）重复上述去噪过程。使用噪声图像训练一个过完备自动编码器（隐藏层节点数大于输入节点数，且只有一个隐藏层）。如果同时将原始图像用作输入和输出进行训练，会发生什么？现在，使用原始图像同时作为输入和输出，但在隐藏层使用 L1 正则化器，训练一个稀疏过完备自动编码器。

## 另请参见

如需更多信息，请参阅以下链接：

- http://vis-www.cs.umass.edu/lfw/
- https://scikit-learn.org/0.15/datasets/labeled_faces.html
- https://pytorch.org/docs/stable/_modules/torch/nn/modules/linear.html
- https://www.youtube.com/watch?v=B6YO5_RLA1A

## 使用 PCA/DFT/DWT 进行图像去噪

主成分分析（PCA）、离散傅里叶变换（DFT）和离散小波变换（DWT）也是可用于去噪图像的传统机器学习技术。这些技术中的每一种都将学习图像空间的表示（近似），并主要保留图像中的信息内容，同时去除噪声。

## 准备工作

我们将为本示例使用 Olivetti 面部数据集。该数据集包含总共 400 张灰度人脸图像（每张大小为 64 x 64），每个对象 10 张，共 40 个对象。和往常一样，让我们先导入所需的库：

```python
import numpy as np
from numpy.random import RandomState
import matplotlib.pyplot as plt
from sklearn.datasets import fetch_olivetti_faces
from sklearn import decomposition
from skimage.util import random_noise
from skimage import img_as_float
```

```python
from time import time
import scipy.fftpack as fp
import pywt
```

## 操作步骤

执行以下步骤实现滤波器：

- 1. 首先下载人脸数据：

```python
dataset = fetch_olivetti_faces(shuffle=True, random_state=rng)
original = img_as_float(dataset.data)
faces = original.copy()
print(faces.shape)
# (400, 4096)
```

- 2. 向图像添加方差为 0.005 的随机高斯噪声：

```python
image_shape = (64, 64)
rng = RandomState(0)
n_samples, n_features = faces.shape
faces = random_noise(faces, var=0.005)
```

- 3. 使用 PCA 且仅使用 50 个主成分重建图像：

```python
n_components = 50 # 256
estimator = decomposition.PCA(n_components=n_components,
                              svd_solver='randomized', whiten=True)
print("Extracting the top %d PCs..." % (n_components))
t0 = time()
faces_recons = estimator.inverse_transform(estimator.fit_transform(faces))
train_time = (time() - t0)
print("done in %0.3fs" % train_time)
```

- 4. 随机选择五个图像索引，显示原始图像、噪声图像和 PCA 重建图像：

```python
indices = np.random.choice(n_samples, 5, replace=False)
plt.figure(figsize=(20, 4))
for i in range(len(indices)):
    plt.subplot(1,5,i+1),
    plt.imshow(np.reshape(faces_recons[indices[i],:], image_shape)),
    plt.axis('off')
plt.suptitle('PCA reconstruction with {} components (eigenfaces)'.format(n_components), size=25)
plt.show()
```

- 5. 使用 FFT 低通滤波器（LPF），丢弃除三十个最低频率分量外的所有分量，并使用这些基向量重建噪声图像。显示重建的图像：

```python
n_components = 30
plt.figure(figsize=(20, 4))
for i in range(len(indices)):
    freq = fftshift(fft2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float)))
    freq[:freq.shape[0]//2 - n_components//2, :] = 0
    freq[freq.shape[0]//2 + n_components//2:, :] = 0
    freq[:, :freq.shape[1]//2 - n_components//2] = 0
    freq[:, freq.shape[1]//2 + n_components//2:] = 0
    plt.subplot(1, 5, i+1),
    plt.imshow(ifft2(ifftshift(freq)).real), plt.axis('off')
    plt.suptitle('FFT LPF reconstruction with {} basis vectors'.format(n_components), size=25)
plt.show()
```

- 6. 使用 DWT 并丢弃第七个频带来重建图像。显示重建的图像：

```python
plt.figure(figsize=(20, 4))
wavelet = pywt.Wavelet('haar')
for i in range(len(indices)):
    wavelet_coeffs = pywt.waverec2((np.reshape(faces[indices[i], :],
    image_shape)).astype(float), wavelet)
    plt.subplot(1, 5, i+1),
    plt.imshow(pywt.waverec2(wavelet_coeffs[:-1], wavelet)),
    plt.axis('off')
    plt.suptitle('Wavelet reconstruction with {} subbands'.format(len(wavelet_coeffs)-1), size=25)
plt.show()
```

## 工作原理...

scikit-learn 的 `decomposition.PCA()` 函数用于实例化 PCA 对象。首先使用 `fit_transform()` 函数将图像投影到更低维度（从 4096 维到 50 维），然后使用 `inverse_transform()` 函数从较低维度的表示中重建图像。

