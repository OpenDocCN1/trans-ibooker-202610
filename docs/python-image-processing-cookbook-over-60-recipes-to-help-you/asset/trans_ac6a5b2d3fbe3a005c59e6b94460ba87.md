

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

## 工作原理...

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

## 使用深度学习模型进行年龄、性别和情绪识别

面部图像的年龄估计可以被视为使用 CNN 之后跟随预期 softmax 值细化的深度分类问题（可以使用 **深度期望（DEX）模型** 来实现）。在本配方中，你将首先学习如何使用预训练的深度学习模型（在其顶部添加了两个分类层的 **WideResNet**，使用单个 CNN 同时估计年龄和性别）来从面部图像进行年龄和性别识别。我们将使用名人面部数据集中的面部图像进行年龄和性别识别。然后，你将使用另一个预训练的深度学习模型实现情绪识别，但这次你需要使用面部检测器检测面部（你也可以使用迁移学习，并在自己的图像上使用你的分类器，但这留给你作为练习去尝试）。

## 准备工作

从 https://drive.google.com/drive/folders/0BxYs69jJi4kU0I1YUQyY1ZDRUE 和 https://drive.google.com/file/d/0B6yZu81NrMhSVOzYWZrenJxD1E 下载预训练的深度学习模型。将模型提取到 models 文件夹中的相应路径。导入所需的库以开始：

```
import cv2
import dlib
import numpy as np
from keras.models import load_model
from keras import backend as K
from keras.models import model_from_json
from glob import glob
import matplotlib.pyplot as plt
```

## 操作步骤...

执行以下步骤，在 Keras 中使用预训练模型实现年龄、性别和情绪识别：

- 1. 首先，从提供的 JSON 文件加载预训练模型的配置，并从预训练权重文件加载模型权重，使用以下代码片段：

```
json_file = open('models/model.json', 'r')
loaded_model_json = json_file.read()
json_file.close()
loaded_model = model_from_json(loaded_model_json)
loaded_model.load_weights('models/weights.29-3.76_utk.hdf5')
```

- 2. 你将使用 dlib 的正面面部检测器来检测和提取输入面部图像中的面部。使用下一行代码获取 dlib 的面部检测器：

```
detector = dlib.get_frontal_face_detector()
```

- 3. 对于使用 dlib 检测器在输入图像中检测到的每个面部，使用深度学习模型以面部作为输入运行前向传递来预测检测到的面部的年龄和性别，并提取预测的年龄和性别：

```
img_size = 64
for img_file in glob('images/musicians/*.jpg'):
    img = cv2.cvtColor(cv2.imread(img_file), cv2.COLOR_BGR2RGB)
    detected = detector(img, 0)
    if len(detected) > 0:
```

