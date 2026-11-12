

### 带有所有已学习和保留的权重和特征的原始模型
### 使用个体权重级稀疏性（即 Lasso 正则化）移除某些权重
### 使用组级别稀疏性（即组 Lasso 正则化）移除隐藏节点/特征（以及所有连接权重）
### 使用弹性网正则化诱导权重级和节点级的稀疏性

图 6.11 简单 FCN 中诱导稀疏性的学习。左上图是原始 FCN，其中所有权重都已学习和保留。右上图是使用 Lasso 通过将其值设置为零来禁用某些连接权重（由虚线表示）的示例。左下图使用组 Lasso 通过将所有输入和输出权重设置为零来移除一个节点（由虚线圆和线条表示）。右下图应用弹性网正则化来结合 Lasso 和组 Lasso，实现权重级和节点级的稀疏性。

通过 Dropout 促进稀疏性的副作用与在节点级选择中使用组 Lasso 相似，尽管 Dropout 不涉及对节点的显式正则化项。

在下一节中，我们将探讨从零实现 Dropout 操作，并使用 MNIST 数据集在 FCN 和 CNN 中应用它。

## 6.2.3 Dropout 实战

Dropout 操作包括根据预设概率丢弃一些节点，并放大其余节点的权重。在这里，我们指定丢弃节点的概率（这也是一个超参数），而不是保留节点的概率。这既适用于下文将要介绍的示例，也适用于 PyTorch 中提供的 Dropout 层。

实现 Dropout 时，第一部分涉及生成一个布尔数值的指示列表，用于与待评估层中的节点相乘，无论是输入层还是隐藏层。当一个节点乘以零，或者等价于 Python 中的 `FALSE` 值时，该节点的结果特征值将变为零，并流入后续操作，这相当于丢弃或掩码该节点。它还确保在反向传播过程中，不会有误差梯度流回被丢弃的神经元。布尔列表的长度与节点数量相同，它是通过将一组随机数与预设的 Dropout 概率进行比较生成的，这些随机数服从 0 到 1 之间的均匀分布。

第二部分是放大节点的特征值，这等同于放大它们的输出权重。原因是丢弃一些节点后，其余节点需要在值上进行放大，以补偿被丢弃的节点。具体来说，假设 Dropout 比率为 p，那么每个剩余节点将乘以 1/(1 - p)。

以下代码清单实现了一个 Dropout 类，可以应用于输入层或任何隐藏层。实例化该类时，它接受默认 Dropout 比率为 0.5，通常在 0.5 到 1 的范围内变化。当 `__init__()` 函数自动触发时，缩放因子也会被计算并存储为属性。接下来，`forward()` 函数指定如何执行 Dropout 操作，并在应用此类时自动触发。具体来说，我们首先创建一个名为 `selected_ind` 的指示列表，以确定要掩码/屏蔽哪些节点。布尔数值列表是通过将 0 到 1 范围内的一组均匀生成的数字与预设的 `dropout_rate` 进行比较来创建的。指示列表随后被转换为浮点数据类型，并包装在一个变量中，其梯度传播被禁用，因为不涉及学习。最后，我们根据指示列表掩码节点，并放大其余的幸存节点。

### 清单 6.1 定义自定义 Dropout 类

```
class Dropout(nn.Module):
    def __init__(self, dropout_rate=0.5):
        super(Dropout, self).__init__()
        self.dropout_rate = dropout_rate
        if self.dropout_rate < 1: #A
            self.scaling_fct = 1.0 / (1.0-dropout_rate) #A
        else: #A
            self.scaling_fct = 0.0 #A

    def forward(self, input):
        if not self.training: #B
            return input

        selected_ind = torch.Tensor(input.shape).uniform_(0,1) > self.dropout_rate #C
        if input.is_cuda: #D
            selected_ind = Variable(selected_ind.type(torch.cuda.FloatTensor),
                requires_grad=False) #D
        else: #D
            selected_ind = Variable(selected_ind.type(torch.FloatTensor),
                requires_grad=False) #D

        return torch.mul(selected_ind, input) * self.scaling_fct #E
```

- #A 为剩余节点定义缩放因子，以补偿被丢弃的节点
- #B 仅在训练模式下应用 Dropout
- #C 相同大小的指示张量，用于标记所选节点
- #D 如果可用则使用 GPU
- #E 丢弃一些节点并放大其余节点

接下来，我们将 FCN 模型定义为名为 `MLP()` 的类。如以下代码清单所示，我们使用 PyTorch 的 sequential 容器，通过其 `add_module()` 函数将多个层串联在一起。`dropout_rates` 参数是一个包含两个条目的列表，分别对应输入层和所有隐藏层的 Dropout 概率。在指定输入层与第一个隐藏层之间的连接后，所有剩余层都在 `for` 循环中配置，这使得扩展到具有不同完全连接隐藏层配置的架构变得方便。由于我们使用 MNIST 数据集，我们的网络从 1x28x28 像素的输入维度和 10 个类别的输出维度开始。

### 清单 6.2 定义 FCN 模型

```
class MLP(nn.Module):
    def __init__(self, hidden_layers=[800, 800], dropout_rates=[0, 0]):
        super(MLP, self).__init__()
        self.model = nn.Sequential()
        self.model.add_module("input", nn.Linear(28*28, hidden_layers[0]))
        self.model.add_module("dropout_input", Dropout(dropout_rate=dropout_rates[0]))
        self.model.add_module("tanh", nn.Tanh())
        # add hidden layers
        for i, d in enumerate(hidden_layers[:-1]):
            self.model.add_module("dropout_hidden_"+str(i+1),
            Dropout(dropout_rate=dropout_rates[1]))
            self.model.add_module("hidden_"+str(i+1), nn.Linear(hidden_layers[i],
            hidden_layers[i+1]))
            self.model.add_module("tanh_hidden_"+str(i+1), nn.Tanh())
        self.model.add_module("final", nn.Linear(hidden_layers[-1], 10))

    def forward(self, x):
        # flatten to one-dimensional array
        x = x.view(x.size()[0], 28*28)
        x = self.model(x)
        return x
```

请注意，我们在非线性激活函数生效之前使用了 Dropout，这在计算效率上提供了 slight 提升。然而，在实践中，Dropout 层通常放置在激活层之后。

我们可以实例化一个示例模型，并按以下代码片段检查其架构。

```
>>> model = MLP()
>>> print(model)
MLP(
  (model): Sequential(
    (input): Linear(in_features=784, out_features=800, bias=True)
    (dropout_input): Dropout()
    (tanh): Tanh()
    (dropout_hidden1): Dropout()
    (hidden_1): Linear(in_features=800, out_features=800, bias=True)
    (tanh_hidden_1): Tanh()
    (final): Linear(in_features=800, out_features=10, bias=True)
  )
)
```

接下来，我们将构建一个 `MLPClassifier` 类，以更好地管理训练流水线中的各个组件，包括数据加载、模型训练和结果保存。如以下代码清单所示，我们首先创建在实例化时稍后使用的几个实用变量。这些包括用于在 GPU 环境中启动 MLP 模型的隐藏层和 Dropout 配置（`hidden_layers` 和 `dropout_rates`）、训练批量大小（`batch_size`）、最大训练轮数（`max_epoch`）、损失函数（`criterion`）和优化例程（`optimizer`），以及用于保存模型性能指标（`loss` 和 `test_accuracy`）的变量。

### 清单 6.3 定义模型训练的包装器

```
class MLPClassifier:
    def __init__(self, hidden_layers=[800, 800], dropout_rates=[0, 0], batch_size=128, \
        max_epoch=10, lr=0.1):
        self.hidden_layers = hidden_layers
        self.dropout_rates = dropout_rates
        self.batch_size = batch_size
        self.max_epoch = max_epoch
        self.model = MLP(hidden_layers=hidden_layers, dropout_rates=dropout_rates)
        self.model.cuda()
        self.criterion = nn.CrossEntropyLoss().cuda()
        self.optimizer = optim.SGD(self.model.parameters(), lr=lr)
        self.loss = []
        self.test_accuracy = []
```

然后，我们将向类添加一个 `fit()` 函数来处理训练过程。如以下代码片段所示，我们首先使用 `dataloader` 类加载训练集和测试集，其中训练集分批次提供，测试集一次性全部提供，用于每隔 `test_score_freq` 个轮次进行一次评分。在每个训练迭代中，我们遵循同样的熟悉流水线：准备批量训练数据（`X_train` 和 `y_train`）、清除当前梯度以避免梯度累积、使用当前模型进行预测（存储在 `y_pred` 中）、计算当前损失（`loss`）、将损失反向传播以计算偏导数梯度，最后执行一步梯度下降更新以获得新模型。我们使用 `running_loss` 监控不同轮次中每个批次的平均损失。默认情况下，每隔五个轮次对测试集进行一次评分，模型会对测试集中的所有 10,000 张图像进行预测以计算预测准确率。

