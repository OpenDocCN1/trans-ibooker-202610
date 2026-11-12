
## 4.3.4 定义优化过程

要更新模型参数，我们可以使用 PyTorch 提供的优化器，包括 SGD 及其变体。使用默认优化器可以轻松地将优化扩展到甚至数百万个参数。像 SGD 这样的典型优化器包括两个输入参数：要更新的模型参数和要使用的学习率。实例化优化器后，我们可以调用其 step() 方法来更新权重，方法是在使用 backward() 方法反向传播成本并计算梯度之后。

请记住，优化过程是一个迭代过程。预测函数在每次迭代中根据当前权重生成输出。然后，成本函数使用预测值和目标标签来计算成本。优化步骤为每个权重推导成本梯度，并使用梯度下降类型的算法更新所有权重。此过程重复进行，直到收敛，即权重或成本在训练迭代进行过程中基本保持不变且变化不大。

以下代码片段通过使用 SGD 实例化优化器来定义优化过程，该优化器将用于支持模型权重的迭代更新。

```
from torch import optim
optimizer = optim.SGD(model.parameters(), lr = 0.01)
```

## 4.3.5 通过迭代训练更新权重

我们现在可以将上述组件整合为一个完整的训练循环。基于预定义的模型和数据加载器，以下代码清单定义了训练函数，该函数通过完整遍历训练数据集一次来执行 SGD 优化。我们还跟踪权重在不同训练批次中更新时的成本变化，然后根据指定的 epoch 数重复此过程。

### 清单 4.9 定义训练函数

```
def train(model, loaders, verbose=True):
    model.train()#A
    total_img = len(loaders['train'].dataset) #B
    for batch, (X, y) in enumerate(loaders['train']): #C
        X, y = X.to(device), y.to(device) #D
        pred = model(X) #E
        cost = cost_func(pred, y) #F
        optimizer.zero_grad() #G
        cost.backward() #H
        optimizer.step() #I
        if(batch) % 100 == 0: #J
            loss, current_img_idx = cost.item(), batch * len(X)
            print(f"cost: {cost:>7f} [{current_img_idx:>5d}/{total_img:>5d}]")
```

- #A 通过指定训练模式来控制某些层的行为
- #B 提取图像总数以跟踪训练进度
- #C 提取并遍历每批训练数据
- #D 传递到 GPU 以进行更快处理
- #E 调用前向方法（内部）以生成预测
- #F 计算当前成本
- #G 清除现有梯度，否则梯度会累积
- #H 执行反向传播并计算梯度
- #I 使用 SGD 更新权重
- #J 每 100 个批次打印一次成本；每个批次包含 100 对图像-标签

让我们尝试运行一个 epoch 的训练过程，并观察预测成本的下降。以下代码显示，随着训练的进行，成本稳步下降，表明模型的预测能力在提高。

```
>>> train(model, loaders)
cost: 2.305853 [ 0/60000]
cost: 2.079413 [10000/60000]
cost: 0.855676 [20000/60000]
cost: 0.454673 [30000/60000]
cost: 0.475409 [40000/60000]
cost: 0.320870 [50000/60000]
```

我们还将定义一个函数来检查模型在测试集上的性能。以下代码清单生成模型预测，并通过与目标标签比较来计算测试准确率。由于不需要学习，在生成预测时，模型被设置为评估模式并置于 `torch.no_grad()` 上下文中。

### 清单 4.10 定义测试函数

```
def test(model, loaders, verbose=True):
    model.eval() #A
    total_img = len(loaders['test'].dataset) #B
    correct = 0
    with torch.no_grad(): #C
        for X, y in loaders['test']:
            X, y = X.to(device), y.to(device)
            pred = model(X)
            correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    correct /= total_img #D
    if verbose:
        print(f"Test accuracy: {correct:>0.3f}")
```

- #A 通过指定评估模式来控制某些层的行为
- #B 提取测试集中的图像总数
- #C 禁用梯度计算
- #D 为每个批次添加正确预测的数量

我们可以使用此函数来检查先前训练的模型的性能。

```
>>> test(model, loaders)
Test accuracy: 0.911
```

使用一个 epoch 训练的模型达到 91.1% 的准确率，这非常高。通过训练更多的 epoch，我们可以做得更好，如下所示。

```
>>> num_epochs = 10
>>> for t in range(num_epochs):
>>>     print(f"Epoch {t+1}\n-----------------------------------")
>>>     train(model, loaders, verbose=False)
>>>     test(model, loaders)
>>> print("Done!")
Epoch 1
-----------------------------------
Test accuracy: 0.911
Epoch 2
-----------------------------------
Test accuracy: 0.943
Epoch 3
-----------------------------------
Test accuracy: 0.959
Epoch 4
-----------------------------------
Test accuracy: 0.967
Epoch 5
-----------------------------------
Test accuracy: 0.973
Epoch 6
-----------------------------------
Test accuracy: 0.974
Epoch 7
-----------------------------------
Test accuracy: 0.975
Epoch 8
-----------------------------------
Test accuracy: 0.977
Epoch 9
-----------------------------------
Test accuracy: 0.981
Epoch 10
-----------------------------------
Test accuracy: 0.980
Done!
```

通过训练十个 epoch，准确率现在提高到了 98%！

## 4.4 关于泛化的更多内容

此前，我们提到，由于 SGD 具有良好的泛化性能，因此广泛用于深度神经网络模型。在该过程中，我们并未显式地使用任何正则化。因此，SGD 算法似乎具有隐式正则化效果，除了神经网络的广泛模型类和特定的输入数据类型（即，结构化图像数据）之外。来自 SGD 的隐式正则化将承诺即使在训练误差达到零时，测试误差也会进一步降低，从而在测试损失曲线上导致“双下降”。Smith 等人（2021）在其论文《随机梯度下降中隐式正则化的起源》中最近研究了隐式正则化的效果，我们将在后续章节中讨论。

现在让我们更仔细地研究这一特性。以下代码清单定义了一个只有一个全连接隐藏层的简单神经网络。设置简单的架构使得在我们更改模型复杂度时（即，调整隐藏层中的节点数量）容易关注性能影响。在 `__init__` 函数中的模型初始化代码中，我们使用 `nn.Linear()` 定义两个全连接层，一个作为隐藏层，另一个作为输出层。请注意，我们为隐藏层指定输入大小为 1*28*28，为输出层指定默认输出大小为 10；这些基于 MNIST 数据的特性。我们还将隐藏层大小 `num_nodes` 作为初始化函数中的显式输入参数，以便以后创建不同大小的模型。`forward()` 函数与我们之前的示例中执行类似的工作。唯一需要注意的是，我们在开头直接展平输入数据以适应全连接层，因为此处未使用卷积层。

### 清单 4.11 定义单层神经网络模型类

```
import torch.nn as nn
import torch.nn.functional as F
class SimpleNN(nn.Module):
    def __init__(self, num_nodes, num_classes=10): #A
        super(SimpleNN, self).__init__()
        self.fc = nn.Linear(1 * 28 * 28, num_nodes) #B
        self.out = nn.Linear(num_nodes, num_classes) #C

    def forward(self, x): #D
        x = x.view(x.shape[0], -1) #F
        x = F.relu(self.fc(x))
        output = self.out(x)
        return output
```

- #A 模型初始化代码
- #B 单个全连接层
- #C 输出 10 个类的最终全连接层
- #D 模型架构代码
- #F 除了批量大小之外展平所有维度

接下来，我们需要一个函数来告诉我们模型在训练集和测试集上的表现如何。这些将在我们改变隐藏层大小时指示模型的泛化性能。以下代码清单定义了一个名为 `check_accuracy` 的函数，该函数根据 `type` 参数计算整个训练集或测试集的预测准确率。该代码在很大程度上遵循与之前示例中 `test()` 函数相同的结构，不同之处在于我们将输入图像加载器泛化为基于用户指定参数；这使我们免于为训练集和测试集编写两个相似的函数。
