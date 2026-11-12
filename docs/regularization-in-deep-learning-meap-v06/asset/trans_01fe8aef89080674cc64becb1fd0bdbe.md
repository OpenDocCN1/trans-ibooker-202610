

## 清单 7.3 使用梯度下降创建未正则化线性回归的类

```
class GD_regression:
    def __init__(self, lr=1e-3, num_epochs=1000, l2_penalty=0):
        self.lr = lr
        self.num_epochs = num_epochs
        self.l2_penalty = l2_penalty
        self.train_loss = []
        self.test_loss = []
```

接下来，我们将定义两个实用函数来支持坐标式梯度下降算法。在以下代码片段中，我们定义函数 `calculate_mse()` 来计算均方误差，以及 `feature_derivative_ridge()` 函数以获取每个特征的导数。`calculate_mse()` 是我们在上一章中使用的同一实用函数，我们首先生成存储在 `f_star` 中的预测列表，计算 `resid` 中的残差，最后返回 `Q` 中的总均方误差。`feature_derivative_ridge()` 函数实现了前面讨论的精确解析式导数解，其中我们将第一个偏置项的权重情况单独分离，因为它不受惩罚。请注意，这两个函数都包含在 `GD_regression` 类的方法中。

## 清单 7.4 计算 MSE 和导数的类方法

```
def calculate_mse(self, X, y, weights): #A
    f_star = X@weights #B
    resid = y - f_star #C
    Q = np.dot(resid.T, resid).mean() #D
    return Q

def feature_derivative_ridge(self, errors, feature, weight, l2_penalty,
    feature_is_constant): #E
    if feature_is_constant == True: #F
        derivative = np.dot(feature, errors) * 2
    elif feature_is_constant == False: #G
        derivative = np.dot(feature, errors) * 2 + 2*l2_penalty*weight
    else:
        derivative = 'derivative not exist!!!'
    return derivative
```

- # A 计算均方误差的函数
- # B 生成模型预测
- # C 计算残差
- # D 作为最终代价的 MSE
- # E 计算特征级解析式导数的函数
- # F 计算对应偏置项权重的导数
- # G 计算所有其他权重的导数

最后，我们将定义 `fit()` 方法来执行拟合和估计过程。如以下代码片段所示，我们首先初始化存储在 `weights` 中的零数组，并开始由输入参数 `num_epochs` 指定的总轮数的优化循环。这与基于特征规范 RSS 的停止条件不同，这里出于方便而使用。我们在每个 epoch 中计算所有观测值的预测和误差，然后进入对所有权重的循环以执行坐标式梯度下降更新。接着，我们使用训练集和测试集对当前权重进行评分，并将结果分别存储在 train_loss 和 test_loss 中。

## 清单 7.5 模型拟合的类方法

```python
def fit(self, X_train, y_train, X_test, y_test): #A
    weights = np.zeros(X_train.shape[1]) #B
    epoch = 0 #C
    while epoch < self.num_epochs: #D
        epoch += 1 #E
        predictions = np.dot(X_train, weights) #F
        errors = predictions.reshape(-1,1) - y_train.reshape(-1,1) #G
        for i in range(len(weights)): #H
            if i == 0: #I
                derivative = self.feature_derivative_ridge(errors,
                X_train[:,i].reshape(1,-1), weights[i], self.l2_penalty, True) #J
            else:
                derivative = self.feature_derivative_ridge(errors,
                X_train[:,i].reshape(1,-1), weights[i], self.l2_penalty, False) #K
                weights[i] = weights[i] - self.lr * derivative #L

        self.train_loss.append(self.calculate_mse(X_train, y_train, weights)) #M
        self.test_loss.append(self.calculate_mse(X_test, y_test, weights)) #M
        # store the final weights
        self.weights = weights
    return self
```

- #A 估计模型系数的函数
- #B 将权重初始化为零数组
- #C 迭代计数器
- #D 当未达到最大迭代次数时
- #E 递增迭代计数器
- #F 基于当前权重计算预测值
- #G 将误差计算为预测值 – 输出
- #H 对每个权重执行坐标式梯度下降
- #I 计算第 i 个权重的导数
- #J 当 i=0 时计算常量权重的导数
- #K 计算所有其他权重的导数
- #L 对每个坐标/权重执行梯度下降更新
- #M 使用训练集和测试集进行评分

让我们训练模型 100 个 epoch 并观察训练损失和测试损失的变化。在以下代码清单中，我们使用 num_epochs=100 实例化一个 GD_regression 类并将其存储在 model 对象中，随后调用 fit() 方法使用训练集和测试集训练和评估模型。然后我们绘制两条损失曲线。

## 清单 7.6 模型训练与结果可视化

```python
model = GD_regression(num_epochs=100)
model.fit(X_train, y_train, X_test, y_test)
x_axis = list(range(model.num_epochs))
plt.plot(x_axis, model.train_loss, label = "train")
plt.plot(x_axis, model.test_loss, ':', label = "test")
plt.legend()
plt.show()
```

运行上述命令生成图 7.10，两条曲线在第 20 个 epoch 左右趋于稳定。请注意，训练集占总数据集的 80%，因此在相同的线性模型下，其 MSE 高于测试集。随着底层模型变得非线性且更加复杂，当模型进入临界和超参数化模式时，我们预期训练损失将低于测试损失，这是第 3 章涵盖的主题。

![](img/ee98e91e6845f41077b163bec833ee40_237_0.png)

### 图 7.10 不同 epoch 的训练集和测试集 MSE 损失。

让我们向目标函数激活 $L_2$ 范数惩罚，并观察其对模型估计的影响。这次，我们在创建模型类时设置 `l2_penalty=10`，并将四条损失曲线一起绘制，如以下代码清单所示。

## 清单 7.7 带 l2 惩罚的模型训练

```
model_ridge = GD_regression(num_epochs=100, l2_penalty=10)
model_ridge.fit(X_train, y_train, X_test, y_test)
x_axis = list(range(model.num_epochs))
plt.plot(x_axis, model.train_loss, label = "train-without-ridge")
plt.plot(x_axis, model.test_loss, ':', label = "test-without-ridge")
plt.plot(x_axis, model_ridge.train_loss, '--', label = "train-with-ridge")
plt.plot(x_axis, model_ridge.test_loss, '-.', label = "test-with-ridge")
plt.legend()
plt.show()
```

运行上述命令生成图 7.11，其中 $L_2$ 范数惩罚略微增加了训练集和测试集的损失。这来自岭回归所鼓励的收缩权重。

![](img/ee98e91e6845f41077b163bec833ee40_238_0.png)

### 图 7.11 未正则化回归与岭回归的训练集和测试集损失比较。

我们可以通过查看结果权重的 $L_2$ 范数来进一步验证收缩效果。如以下代码片段所示，我们使用 `numpy` 中的 `norm` 函数来计算两个模型权重向量的 $L_2$ 范数。结果表明，与未正则化模型相比，岭回归在估计权重中具有较低的 $L_2$ 范数。

```
>>> from numpy import linalg as LA
>>> print(LA.norm(model.weights, 2))
>>> print(LA.norm(model_ridge.weights, 2))
187.12047119912242
164.13282992694337
```

我们将在下一节中使用神经网络实现相同的线性回归模型。当只有一个输入层和一个输出层（也称为单层神经网络）时，神经网络模型本质上等价于线性回归模型。这是基于这样一个事实：输入层和输出层之间的矩阵乘法步骤（第 4 章涵盖的主题，且没有激活步骤）恢复了线性回归操作。

## 使用单层神经网络的线性回归

为了学习线性回归模型，让我们定义一个单层全连接网络（FCN）。输入大小等于特征数量，输出大小为一，因为每个输入观测值只有一个标量输出。在以下代码清单中，我们定义一个名为 FCN_regression 的类来存储必要的属性和方法。初始化时，该类需要几个输入参数：input_size 中的输入特征数量，lr 中的 SGD 学习率，num_epochs 中的 epoch 数量，以及 l2_penalty 中 L2 惩罚的标量超参数大小。我们还将模型架构定义为存储在 model 中的单个线性层，将 nn.MSELoss() 作为存储在 criterion 中的损失函数，并将 SGD 作为存储在 optimizer 中的优化器。

## 清单 7.8 创建单层 FCN 的类

```
class FCN_regression:
    def __init__(self, input_size, lr=1e-3, num_epochs=1000, l2_penalty=0):
        self.lr = lr
        self.num_epochs = num_epochs
        self.l2_penalty = l2_penalty
        self.train_loss = []
        self.test_loss = []
        self.model = nn.Linear(input_size, 1)
        self.criterion = nn.MSELoss()
        self.optimizer = torch.optim.SGD(self.model.parameters(), lr=self.lr)
```

接下来，我们将在类中创建一个 fit() 方法来执行模型估计任务。如以下代码片段所示，我们将训练集和测试集作为训练和评分的输入，由于我们使用 PyTorch，因此需要将其转换为张量格式。沿用相同的零初始化策略，我们执行与第 4 章中训练典型神经网络相同的训练过程。具体来说，我们执行前向传播以获取训练集的模型预测 y_predicted，计算 loss_train 中的损失，如有需要则添加 L2 惩罚，清除相关变量中的历史梯度，通过反向传播自动计算当前梯度，最后执行由 num_epochs 指定的总 epoch 数的随机梯度下降更新。此外，我们还在每一轮训练后在无梯度设置下计算测试集 MSE。

```
def fit(self, X_train, y_train, X_test, y_test):
    weights = np.zeros(X_train.shape[1]) #A
    epoch = 0 #B
    X_train = torch.from_numpy(X_train).float() #C
    X_test = torch.from_numpy(X_test).float() #C
    y_train = torch.from_numpy(y_train).float() #C
    y_test = torch.from_numpy(y_test).float() #C

    for epoch in range(self.num_epochs):
        y_predicted = self.model(X_train) #D
        loss_train = self.criterion(y_predicted, y_train) #E
        self.train_loss.append(loss_train.item())
```

