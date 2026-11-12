

## 代码清单 7.10 构建用于不同正则化类型训练 FCN 的类

```
class MLP:
    def __init__(self, lr=1e-3, num_epochs=10, l1_weight=0, l2_weight=0):
        self.lr = lr
        self.num_epochs = num_epochs
        self.l1_weight = l1_weight
        self.l2_weight = l2_weight
        self.train_loss = []
        self.test_accuracy = []
        self.model = nn.Sequential(
            nn.Flatten(),
            nn.Linear(28 * 28 * 1, 64),
            nn.ReLU(),
            nn.Linear(64, 32),
            nn.ReLU(),
            nn.Linear(32, 10)
        )
        self.model.cuda()
        self.criterion = nn.CrossEntropyLoss().cuda()
        self.optimizer = torch.optim.SGD(self.model.parameters(), lr=self.lr)
```

接下来，我们构建一个 fit() 方法来执行模型训练。以下代码片段显示，该方法需要两个输入参数：训练集和测试集的数据加载器。对于每个 epoch，我们首先提取训练数据集的当前批次，并执行常规前向传播以获得预测值和相应的损失。如果 LASSO 或岭回归惩罚的权重被激活，我们会将额外的惩罚项添加到当前损失中。为了计算这些惩罚项，我们首先使用 self.model.parameters() 获取模型的所有参数，然后将其存储在列表中以供后续计算。这些惩罚项使用函数 compute_l1_loss() 和 compute_l2_loss() 进行计算，这两个函数将在稍后介绍。在对损失进行反向传播并更新权重后，我们还会在每个 epoch 结束时在测试集上对模型进行评分。每次迭代的训练损失和每个 epoch 的测试集准确率分别存储在属性 train_loss 和 test_accuracy 中。

## 代码清单 7.11 模型拟合的类方法

```
def fit(self, trainloader, testloader):
    for epoch in range(self.num_epochs): #A
        print(f'Starting epoch {epoch+1}')
        for i, data in enumerate(trainloader, 0):
            X_train, y_train = data #B
            X_train, y_train = Variable(X_train).cuda(), Variable(y_train).cuda() #C
            self.optimizer.zero_grad() #D
            y_predicted = self.model(X_train) #E
            loss_train = self.criterion(y_predicted, y_train) #F

            if self.l1_weight != 0: #G
                l1_parameters = []
                for parameter in self.model.parameters():
                    l1_parameters.append(parameter.view(-1))
                l1 = self.l1_weight * self.compute_l1_loss(torch.cat(l1_parameters))
                loss_train += l1

            if self.l2_weight != 0: #H
                l2_parameters = []
                for parameter in self.model.parameters():
                    l2_parameters.append(parameter.view(-1))
                l2 = self.l2_weight * self.compute_l2_loss(torch.cat(l2_parameters))
                loss_train += l2

            loss_train.backward() #I
            self.optimizer.step() #J
            self.train_loss.append(loss_train.item()) #K

            with torch.no_grad(): #L
                X_test, y_test = iter(testloader).next()
                X_test = X_test.cuda()
                y_test_pred = self.predict(X_test.cpu()) #M
                self.test_accuracy.append(np.mean((y_test == y_test_pred).numpy())) #N
                print('Test accuracy: {}'.format(self.test_accuracy[-1]))

        return self
```

- #A 遍历训练数据的 DataLoader
- #B 获取输入
- #C 如果可用，发送到 GPU
- #D 将梯度归零
- #E 执行前向传播
- #F 计算损失
- #G 添加 L1 损失分量
- #H 添加 L2 损失分量
- #I 执行后向传播
- #J 执行优化
- #K 添加训练损失
- #L 获取测试集性能
- #M 获取测试集预测
- #N 存储测试集准确率

类中还有几个其他函数。首先，`predict()` 函数用于在评估模式下对测试集数据进行评分，具体是通过设置 `self.model.eval()` 来实现；每个观测值的最终预测是通过 `torch.max(outputs.data, 1)` 命令，在所有数字类别中选择 logit 最大的那个。有关 `predict()` 函数的代码片段如下，它与我们在前一章中介绍的函数相同。

## 代码清单 7.12 模型预测的类方法

```
def predict(self, x):
    model = self.model.eval()
    outputs = model(Variable(x))
    _, pred = torch.max(outputs.data, 1)
    model = self.model.train()
    return pred
```

接下来，让我们看看 `compute_l1_loss()` 和 `compute_l2_loss()`。如以下代码片段所示，这两个函数都执行逐元素操作（LASSO 取绝对值，岭回归对权重取平方），然后对所有权重求和，以产生一个标量的数值。

```
def compute_l1_loss(self, w):
    return torch.abs(w).sum()

def compute_l2_loss(self, w):
    return torch.square(w).sum()
```

我们现在可以准备训练和测试数据集了。在以下代码清单中，我们首先下载 MNIST 数据集，然后分别将其加载到 `trainloader` 和 `testloader` 中。`trainloader` 的批量大小为 128，而 `testloader` 的批量大小为测试集的总大小；我们希望一次性对所有测试集观测值进行评分。

## 代码清单 7.13 准备训练集和测试集

```
transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize([0.5], [0.5])])
trainset = MNIST(root='data/', train=True, download=True, transform=transform)
testset = MNIST(root='data/', train=False, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=128, shuffle=True)
testloader = torch.utils.data.DataLoader(testset, batch_size=len(testset), shuffle=False)
```

现在我们已准备好初始化模型并使用数据集进行训练。在以下代码清单中，我们指定设备为 GPU（如果可用），并准备训练总共四个不同的模型：未正则化模型、LASSO 正则化模型、岭回归正则化模型以及采用组合正则化方案的模型。我们通过在列表推导式中调用 `fit()` 方法来训练每个模型。

## 代码清单 7.14 训练四个 MLP 模型

```
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu') #A
model_list = [MLP(num_epochs=50, l1_weight=0, l2_weight=0),
    MLP(num_epochs=50, l1_weight=0.001, l2_weight=0),
    MLP(num_epochs=50, l1_weight=0, l2_weight=0.001),
    MLP(num_epochs=50, l1_weight=0.001, l2_weight=0.001)] #B
[tmp_model.fit(trainloader, testloader) for tmp_model in model_list] #C
```

```
# A set optimization device
# B model configuration and initialization
# C start training
```

在 Google Colab 的单个 GPU 实例上，对四个模型各训练 50 个 epoch，整个过程大约需要 1 小时。为了防止会话超时，保存输出也可能有所帮助。有关完整的训练代码，请查看附带的笔记本：https://github.com/jackliu333/regularization_in_deep_learning/blob/main/Chapter_7_2.ipynb

请注意，采用组合正则化方案的模型称为弹性网（elastic net），它是指在目标函数中将系数的 L1 范数和 L2 范数的加权和作为联合惩罚项。如果我们的目标仅是约束模型权重，可以只正则化 L2 范数；如果我们希望同时进行特征选择和模型估计，可以只正则化 L1 范数；如果两者都想实现，则应用弹性网惩罚。

训练完成后，我们可以比较四个模型的预测性能。在以下代码清单中，我们绘制了四个模型在不同迭代次数下的训练损失曲线。

## 代码清单 7.15 绘制训练损失曲线

```
mlp_train_loss = np.array([tmp_model.train_loss for tmp_model in model_list])
labels = ['MLP no regularization',
          'MLP with L1 regularization',
          'MLP with L2 regularization',
          'MLP with both L1 and L2']
styles = ['-', '-.', '--', ':']
plt.figure(figsize=(8, 7))
for i, r in enumerate(mlp_train_loss):
    plt.plot(np.array(range(1, len(r)+1)), r, styles[i], label=labels[i], alpha=0.6);
plt.legend(loc=4);
plt.xlabel('Iterations');
plt.ylabel('Loss in training set');
plt.show()
```

运行上述代码会生成图 7.25。该图表明，所有损失曲线都相对颠簸，这是使用 SGD 作为优化器时的典型特征。如果切换到 Adam 优化器，损失曲线会更平滑。此外，未正则化模型在各次迭代中的训练损失最低，与其他模型的较高损失曲线相比，这可能是过拟合的迹象。在超参数 λ 取相同值的情况下，L1 范数模型相比 L2 范数模型具有相对更强的正则化效果。

