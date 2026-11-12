

## 清单 5.1 从网络链接下载图像数据

```
import torch #A
import torch.nn.functional as F
import torchvision
from torchvision import datasets, transforms
from torch import nn
import matplotlib.pyplot as plt
import numpy as np
from PIL import Image
import requests
from io import BytesIO
torch.manual_seed(8) #B
url='https://raw.githubusercontent.com/jackliu333/regularization_in_deep_learning/main/ImageNet.png' #C
response = requests.get(url) #C
img = Image.open(BytesIO(response.content)) #D
img = np.array(img) #E
img = img[..., :3] #F
```

- #A 导入支持库
- #B 设置随机种子以确保可重复性
- #C 通过 get 方法发起 HTTP 请求
- #C 通过 get 方法发起 HTTP 请求
- #D 访问响应内容以检索图像
- #E 转换为 NumPy 数组
- #F 忽略第四个通道，因为不需要它

然后我们可以按如下方式绘制图像，其输出如图 5.3 所示。

```
>>> plt.imshow(img, interpolation='nearest')
>>> plt.show()
```

![](img/ee98e91e6845f41077b163bec833ee40_152_0.png)

图 5.3 从网络链接检索的同一图像的可视化。

在 PyTorch 中，常见的数据增强是通过 torchvision 库的 transforms 模块实现的。为了演示随机扰动的效果，我们将包含三种增强，包括通过 RandomHorizontalFlip() 函数进行水平翻转、通过 RandomRotation() 函数进行旋转，以及通过 ColorJitter() 函数进行颜色抖动。每个函数都有自己的参数来控制随机噪声的级别，并使用 Compose() 函数将它们链接在一起到 img_augment 变量中。此外，增强后的图像被转换为张量对象以便与 PyTorch 框架配合使用。完整规范请参见下面的代码清单。

## 清单 5.2 指定数据增强方案

```
img_augment = transforms.Compose([transforms.RandomHorizontalFlip(), #A
              transforms.RandomRotation(10), #B
              transforms.ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2), #C
             ])
transforms.ToTensor() #D
```

- #A 相对于水平轴翻转图像
- #B 将图像旋转最多 10 度
- #C 随机更改图像亮度、对比度和饱和度，最高可达一定程度
- #D 转换为张量对象以便与 torch 配合使用

现在让我们将指定的数据增强方案应用到城市景观图像上，并可视化一些随机生成的图像。在下面的代码清单中，我们首先定义一个函数 `gen_img()` 将图像张量（作为副本引用，以避免更改原始对象）转换为 PIL 图像格式，以便我们可以应用数据增强。然后通过将通道维度从第一个移动到最后一个来重塑增强后的图像，以便可以正确地绘制它。然后我们应用该函数生成 10 个图像，绘制成两行。

## 清单 5.3 可视化增强后的图像

```python
import copy
def gen_img(img):
    img2 = transforms.ToPILImage()(copy.copy(img)) #A
    img2 = img_augment(img2) #B
    img2 = np.transpose(img2, (1, 2, 0)) #C
    return img2

fig = plt.figure(figsize=(10, 4))

for idx in np.arange(10): #D
    ax = fig.add_subplot(2, 5, idx+1, xticks=[], yticks=[])
    plt.imshow(gen_img(img))
    ax.set_title(idx)
```

运行上述代码后，图 5.4 显示了增强后的图像。生成的图像现在在旋转、翻转、亮度、对比度和饱和度的级别上有所不同，所有这些都是预先指定的，并作为随机扰动引入原始图像。这些人工图像随后可以作为额外的训练数据，用于构建更稳健的模型。

![](img/ee98e91e6845f41077b163bec833ee40_153_0.png)

**图 5.4 具有不同级别旋转、翻转和亮度等的增强图像。**

接下来，我们将通过比较使用和不使用数据增强训练的模型来查看性能提升。我们将使用 CIFAR10 数据集，这是一个开源数据集，包含 10 个类别中的 60000 张 32x32 彩色图像，每个类别包含 6000 张图像。

## 5.2.1 不使用数据增强进行训练

CIFAR10 数据集是许多研究论文中常用的基准数据集，也将是本章使用的数据集。本节将查看如果不将任何数据增强应用于训练过程时的基准性能。在 CIFAR10 数据集中，图像分为十个类别：飞机、汽车、鸟、猫、鹿、狗、青蛙、马、船和卡车。让我们通过可视化每个类别的随机样本来了解这些图像。

在下面的代码清单中，我们首先下载 CIFAR10 数据集并应用一系列变换：将像素值从原始范围 [0,1] 归一化到 [-1,1]，并将图像对象转换为张量格式。请注意，Normalize() 函数中的均值和标准差参数被设置为所有三个颜色通道的统一值 0.5。然而，更合适的方法是从所有图像数据中推导这些按通道的统计量。由于差异很小，我们只关心将像素值限制在小而相似的范围内，因此出于实现便利性，我们在所有通道中都使用 0.5。其余代码遵循与上一章相同的结构，每批加载 100 张图像。

## 清单 5.4 加载 CIFAR10 数据集

```
transform = transforms.Compose([transforms.ToTensor(),
                                      transforms.Normalize(mean=(0.5, 0.5, 0.5), std=(0.5,
                                                           0.5, 0.5))]) #A
training_dataset = datasets.CIFAR10(root='./data', train=True, download=True,
                                      transform=transform) #B
validation_dataset = datasets.CIFAR10(root='./data', train=False, download=True,
                                       transform=transform) #B
training_loader = torch.utils.data.DataLoader(training_dataset, batch_size=100,
                                              shuffle=True) #C
validation_loader = torch.utils.data.DataLoader(validation_dataset,
                                                batch_size = 100, shuffle=False) #C
```

- #A 加载数据集时用于训练集和测试集的变换步骤
- #B 下载 CIFAR10 数据集
- #C 定义一次读取 100 张图像的数据加载器

运行上述代码将在 data 文件夹下下载 CIFAR10 数据集，并将其分别加载到 training_loader 和 validation_loader 数据加载器中，用于训练和验证。

接下来，我们将定义一个函数，将图像转换为 NumPy 格式以使用 matplotlib 进行可视化。我们还将图像转换回原始范围 [0,1]，并将其重塑为通道在后（channel-last）格式。然后将此函数应用于 training_loader 第一批中的前十张图像，如下面的代码清单所示。

## 清单 5.5 可视化 CIFAR10 中的十张图像

```
def convert_tensor_to_np(tensor_img):
    image = tensor_img.clone().numpy() #B
    image = image.transpose(1, 2, 0) #C
    image = image * np.array((0.5, 0.5, 0.5)) + np.array((0.5, 0.5, 0.5)) #D
    image = image.clip(0, 1)
    return image

dataiter = iter(training_loader)
images, labels = dataiter.next()
classes = ['plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck'] #F
fig = plt.figure(figsize=(10, 4)) #G
for idx in np.arange(10):
    ax = fig.add_subplot(2, 5, idx+1, xticks=[], yticks=[])
    plt.imshow(convert_tensor_to_np(images[idx]))
    ax.set_title(classes[labels[idx].item()])
```

- #A 将图像格式化以供绘图
- #B 转换为 NumPy
- #C 将颜色通道移至最后
- #D 将图像反归一化回 [0,1]
- #E 从第一批中获取一些随机训练图像
- #F 定义类标签以用于可视化
- #G 在 2x5 网格中绘制图像

运行上述代码将输出排列成两行的十张图像，如图 5.5 所示。

![](img/ee98e91e6845f41077b163bec833ee40_155_0.png)

图 5.5 可视化 CIFAR10 中的十张随机图像。

## LENET

LeNet 架构是一种广泛使用的基准神经网络架构，规模较小且易于掌握。我们将构建一个 LeNet 变体，它由三个 CONV->RELU->MAXPOOLING 操作块组成，后面跟随两个全连接层。一个 DROPOUT 层是
