## 人脸识别、图像字幕生成及其他

本章将讨论将一些高级机器学习和深度学习技术应用于解决一些高级图像处理问题。我们将从一个人脸识别问题开始，该问题尝试使用深度人脸嵌入表示，将图像中检测到的一组面孔与一组固定的已知面孔进行匹配。然后，您将学习如何使用一些深度学习模型来解决诸如人脸年龄或性别识别以及自动为灰度图像上色等问题。我们将探讨的另一个有趣的问题是使用名为im2txt的深度学习模型自动为图像生成字幕。最后，我们将专注于图像生成的一些技术。具体来说，我们将关注图像处理中的生成模型（例如，GAN、VAE和RBM），这是图像处理中的一个热门话题。生成模型一词（通常与判别模型（如SVM/逻辑回归）相对）是指一类机器学习/深度学习模型，它试图通过学习概率模型来对输入数据（例如，图像）的生成或分布进行建模。目标是通过从学习到的模型中采样来生成新数据（图像）。

在本章中，您将学习以下配方：

- 使用FaceNet（一种深度学习模型）进行人脸识别
- 使用深度学习模型进行年龄、性别和情感识别
- 使用深度学习进行图像着色
- 使用CNN和LSTM自动为图像生成字幕
- 使用GAN进行图像生成
- 使用变分自编码器重建和生成图像
- 使用受限玻尔兹曼机重建Bangla MNIST图像

### 使用FaceNet进行人脸识别

人脸识别是一项基于人脸图像来识别和验证个人身份的图像处理/计算机视觉任务。人脸识别问题可分为两种不同的类型：

- 人脸验证（这是声称的人吗？）：这是一个1:1匹配问题（例如，使用特定人脸解锁的手机使用人脸验证）。
- 人脸识别（这是谁？）：这是一个1:K匹配问题（例如，进入办公室的员工可以通过人脸识别来识别）。

FaceNet是一个用于人脸识别（验证和识别）的统一系统。它有时被称为暹罗网络。它基于使用深度卷积网络为每个图像学习欧几里得嵌入，该网络将人脸图像编码为128个数字的向量。网络通过三元损失函数进行训练，使得嵌入空间中L2距离的平方与面部相似性直接相关。在本配方中，我们将使用六位数学家（Bayes、Erdos、Euler、Gauss、Markov和Turing）的人脸图像集（训练数据集中每人约12张图像，测试数据集中每人约6张图像）。虽然我们不会教您如何使用FaceNet，但下图向您展示了系统的工作原理（如果从头开始使用三元损失函数进行训练）。FaceNet学习一个嵌入f(x)，其中x是输入图像。模型以一种方式学习参数，使得当x(i)和x(j)是同一人（正样本）的人脸时，||f(x(i))- f(x(j))||2 L2范数较小，当面孔对应不同的人（负样本）时较大，其中f(.)表示深度CNN所呈现的编码（嵌入）函数，如下图所示：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_373_0.png)

在本配方中，您将学习如何使用预训练的FaceNet模型（在Keras中）进行人脸识别，以便将图像中给定的人脸识别为六位数学家之一的人脸。您将把人脸识别问题转换为嵌入空间中的多类分类问题。

### 准备工作

首先，从 https://drive.google.com/drive/folders/12aMYASGCKvDdkygSv1yQq8ns03AStDO_ 下载预训练的FaceNet模型。然后，将模型解压到models文件夹。从互联网下载六位数学家的图像（您可以使用来自 https://github.com/hardikvasa/google-images-download 的自动化脚本）。为每位数学家下载20张图像，对于每位数学家，将12张图像放入train文件夹，其余8张图像放入test文件夹，如下图所示（test文件夹还将包含带有数学家名称的子文件夹）：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_374_0.png)

使用以下代码块导入所需的Python库：

```python
from tensorflow.keras.models import load_model
#!pip install mtcnn
import mtcnn
print(mtcnn.__version__)
# 0.1.0
from mtcnn.mtcnn import MTCNN
from sklearn.metrics import accuracy_score
from sklearn.preprocessing import LabelEncoder
from sklearn.preprocessing import Normalizer
from sklearn.svm import SVC
from skimage.io import imread
from skimage.color import rgb2gray, gray2rgb
from skimage.transform import resize
import numpy as np
```

```python
import matplotlib.pyplot as plt
import os
```

### 操作步骤

运行以下步骤，使用您下载的预训练FaceNet模型实现人脸识别：

1. 使用以下代码行加载预训练模型：

```python
model = load_model('models/facenet_keras.h5')
```

2. 定义以下函数，从给定图像中提取单个人脸。首先，使用mtcnn库中的人脸检测模块检测图像中的第一张人脸（提取相应的边界框），然后提取人脸，将其调整为所需大小并返回：

```python
def extract_face(image_file, required_size=(160, 160)) :
    image = imread(image_file)
    image = gray2rgb(image) if len(image.shape) < 3 else \
    image[...,:3]
    detector = MTCNN()
    results = detector.detect_faces(image)
    x1, y1, width, height = results[0]['box']
    x2, y2 = abs(x1) + width, abs(y1) + height
    # extract the face
    face = image[y1:y2, x1:x2]
    return resize(face, required_size)
```

3. 定义以下函数，从给定文件夹加载所有人脸图像，提取人脸，并将其作为列表返回：

```python
def load_faces(folder) :
    faces = []
    for filename in os.listdir(folder) :
        face = extract_face(folder + filename)
        faces.append(face)
    return faces
```

4. 定义以下函数，加载训练数据集，该数据集包含每个类别（即每位数学家）的一个子文件夹以及相应的图像。从每个子文件夹加载所有人脸，并使用每个人脸对应的类别名称（即数学家名称）为图像打标签：

```python
def load_dataset(folder):
    X, y = [], []
    for sub_folder in os.listdir(folder):
        path = folder + sub_folder + '/'
        if not os.path.isdir(path): continue
        faces = load_faces(path)
        labels = [sub_folder for _ in range(len(faces))]
        print('>loaded %d examples for class: %s' % (len(faces), \
            sub_folder))
        X.extend(faces)
        y.extend(labels)
    return np.array(X), np.array(y)
```

5. 加载训练和测试图像数据集。将所有图像保存为单个压缩文件（整个过程耗时较长；您只需执行一次，此后即可直接使用压缩数据集进行训练）：

```python
X_train, y_train = load_dataset('images/mathematicians/train/')
print(X_train.shape, y_train.shape)
# (72, 160, 160, 3) (72,)
X_test, y_test = load_dataset('images/mathematicians/test/')
print(X_test.shape, y_test.shape)
# (36, 160, 160, 3) (36,)
np.savez_compressed('images/6-mathematicians.npz', X_train,
    y_train, X_test, y_test)
```

6. 解压缩上一步中创建的训练和测试数据集：

```python
data = np.load('images/6-mathematicians.npz')
X_train, y_train, X_test, y_test = data['X_train'], \
    data['y_train'], \
    data['X_test'], \
    data['y_test']
```



如果你绘制从训练数据集中随机选取的某些训练图像，将得到如下截图所示的输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_377_0.png)

- 7. 定义以下函数以获取每个给定面部的面部嵌入。使用预训练模型，对提取的输入面部进行预测以获取面部嵌入：

```
def get_embedding(model, face):
    yhat = model.predict(np.expand_dims(face, axis=0))
    return yhat[0]
```

- 8. 加载压缩数据并将其解压到训练和测试数据集中。将训练和测试数据集中的每个面部转换为嵌入。使用以下代码以压缩格式保存面部嵌入：

```
data = np.load('images/6-mathematicians.npz')
X_train, y_train, X_test, y_test = data['X_train'], \\
    data['y_train'], data['X_test'], data['y_test']

print('Loaded: ', X_train.shape, y_train.shape, X_test.shape,
      y_test.shape)
# Loaded:  (72, 160, 160, 3) (72,) (36, 160, 160, 3) (36,)
X_train_em = []
for face in X_train:
    X_train_em.append(get_embedding(model, face))
X_train_em = np.asarray(X_train_em)
X_test_em = []
for face in X_test:
    X_test_em.append(get_embedding(model, face))
X_test_em = np.asarray(X_test_em)
np.savez_compressed('models/6-mathematicians-embeddings.npz', \\
    X_train_em, y_train, X_test_em, y_test)
```

- 9. 加载训练和测试图像嵌入，归一化输入向量，并使用以下代码对训练和测试数据集的目标进行标签编码（作为拟合 SVM 模型之前的预处理步骤）：

```
data = np.load('models/6-mathematicians-embeddings.npz')
X_train, y_train, X_test, y_test = data['X_train'], \\
data['y_train'], data['X_test'], data['y_test']
print('Dataset: train=%d, test=%d' % (X_train.shape[0], \\
X_test.shape[0]))
# Dataset: train=72, test=36
in_encoder = Normalizer(norm='l2')
X_train, X_test = in_encoder.transform(X_train), \\
in_encoder.transform(X_test)
out_encoder = LabelEncoder()
out_encoder.fit(y_train)
y_train, y_test = out_encoder.transform(y_train), \\
out_encoder.transform(y_test)
```

- 10. 最后，在训练嵌入数据集中训练 scikit-learn 的线性 SVM 模型，预测测试嵌入数据集，并计算训练和测试数据集上的预测准确率：

```
model_svc = SVC(kernel='linear', probability=True)
model_svc.fit(X_train, y_train)
yhat_train, yhat_test = model_svc.predict(X_train), \\
model_svc.predict(X_test)
score_train, score_test = accuracy_score(y_train, yhat_train), \\
accuracy_score(y_test, yhat_test)
print('Accuracy: train=%.3f, test=%.3f' % (score_train*100, \\
score_test*100))
# Accuracy: train=100.000, test=94.444
```

如你所见，在测试数据集上获得的准确率超过 94%。

### 工作原理...

如果你从测试数据集中随机选择一个面部，并预测其类别（这里是数学家的姓名）以及预测概率，将得到如下输出：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_379_0.png)

如你所见，使用在面部嵌入上训练的线性 SVM 分类模型，贝叶斯的面部被正确识别，概率约为 80%。

可以通过以下步骤将面部识别问题转换为嵌入空间中的多类分类问题：

- 1. 首先，使用 FaceNet 预训练模型，从一组标记的数学家训练图像中提取高质量的面部嵌入特征。
- 2. 使用这些嵌入特征，连同类标签（数学家的姓名），训练线性 SVM 分类器模型（使用 scikit-learn）。
- 3. 最后，使用（刚刚训练好的）分类器模型识别这些数学家的新的、未见过的测试图像：首先提取嵌入特征（使用 FaceNet），然后使用分类器根据给定的编码面部嵌入来预测数学家的姓名（连同置信度）。

上述步骤在下面的截图中以图示形式展示（为简化起见，仅展示三个类别标签）：

![](img/7e2c8bbde0ebcd14e54d2862adfee480_380_0.png)

FaceNet 模型期望输入为 160×160×3 的彩色面部图像（任何图像都需要调整大小为此形状），并输出长度为 128 的面部嵌入向量。

步骤 2 中观察到的 `mtcnn` 库的 `mtcnn` 模块中的 `MTCNN()` 函数，已使用默认权重用于创建面部检测器。

在步骤 10 中，使用带有线性内核的 `SVC` 类来训练分类器，该分类器在给定从任何数学家的新面部图像中提取的（使用预训练的 FaceNet 模型）嵌入时，通过预测最可能的类别（即数学家的姓名）来执行面部识别，并使用训练好的 SVM 模型。

> 本来可以使用简单的 **欧氏（L2）距离** 来代替 **SVM**。（查找与目标面部嵌入具有最小 L2 距离的训练面部嵌入的 `class` 标签。
> 尝试自己实现此功能。）

### 另请参阅

有关此配方的更多详细信息，请参阅以下链接：

- 1. https://machinelearningmastery.com/how-to-develop-a-face-recognition-system-using-facenet-in-keras-and-an-svm-classifier/
- 2. https://stackoverflow.com/questions/47068709/your-cpu-supports-instructions-that-this-tensorflow-binary-was-not-compiled-to-u
- 3. https://arxiv.org/pdf/1503.03832.pdf
- 4. https://sandipanweb.wordpress.com/2018/01/07/classifying-a-face-image-as-happy-unhappy-and-face-recognition-using-deep-learning-convolution-neural-net-with-keras-in-python/
- 5. https://www.youtube.com/watch?v=eHsErlPJWUU&list=PLD63A284B7615313A&index=14
- 6. https://www.youtube.com/watch?v=d2XB5-tuCWU
- 7. https://www.youtube.com/watch?v=EZmaYcdLfhM
- 8. https://www.thehindubusinessline.com/info-tech/google-backs-eus-proposed-facial-recognition-ban-microsoft-disagrees/article30616303.ece
- 9. https://arxiv.org/ftp/arxiv/papers/1604/1604.02878.pdf
