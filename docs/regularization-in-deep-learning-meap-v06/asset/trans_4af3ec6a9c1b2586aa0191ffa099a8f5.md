
## 清单 6.11 检查测试集性能

```
def validation(model, dataloader, *args):
    all_predictions = torch.tensor([]).to(device)
    all_true_labels = torch.tensor([]).to(device)

    with torch.no_grad():
        n_correct = []
        n_class_correct = []
        n_class_samples = []
        n_samples = 0

        for arg in args:
            n_correct.append(len(arg))
            n_class_correct.append([0 for i in range(len(arg))])
            n_class_samples.append([0 for i in range(len(arg))])

        for pictures in dataloader:
            images = pictures['image'].to(device)
            outputs = model(images)
            labels = [pictures['labels'][label].to(device) for label in pictures['labels']]

        #A
        for i, out in enumerate(outputs):
            _, predicted = torch.max(outputs[out], 1) #B
            n_correct[i] += (predicted == labels[i]).sum().item() #C
            if i == 0: #D
                n_samples += labels[i].size(0)

            for k in range(len(predicted)):
                label = labels[i][k]
                pred = predicted[k]
                if label == pred: #E
                    n_class_correct[i][label] += 1
                n_class_samples[i][label] += 1 #F

    return n_correct, n_samples, n_class_correct, n_class_samples

def class_acc(n_correct, n_samples, n_class_correct, n_class_samples, class_list):
    for i in range(len(class_list)):
        print("---")
        acc = 100.0 * n_correct[i] / n_samples
        print(f"Overall class performance: {round(acc,1)} %")
        for k in range(len(class_list[i])):
            acc = 100.0 * n_class_correct[i][k] / n_class_samples[i][k]
            print(f'Accuracy of {class_list[i][k]}: {round(acc,1)} %')
    print("---")
```

#A 在嵌套列表中获取目标输出
#B 获取批次中所有样本的最大 logit 对应的预测类别
#C 记录正确预测的累计数量
#D 获取直到当前批次的累计样本数量
#E 获取每个类中每个类别的正确预测数量
#F 获取每个类中每个类别的预测总数量

我们可以为每个标签准备类名称列表并传递给函数，如以下代码片段所示。

```
classes_brand = list(brand_dict.values())
classes_vehicle_type = list(vehicle_types_dict.values())
classes_epoch = ['2009 and earlier','2010 and later']
class_list = [classes_brand,classes_vehicle_type,classes_epoch]

n_correct, n_samples, n_class_correct, n_class_samples = validation(model, test_loader,
    classes_brand, classes_vehicle_type, classes_epoch)
```

>>> class_acc(n_correct, n_samples, n_class_correct, n_class_samples,class_list)
----------------------------------------
整体类别性能: 90.8 %
Audi 的准确率: 83.6 %
BMW 的准确率: 89.6 %
Chevrolet 的准确率: 78.6 %
Dodge 的准确率: 84.3 %
Ford 的准确率: 75.7 %
Other 的准确率: 96.2 %
----------------------------------------
整体类别性能: 87.8 %
Convertible 的准确率: 85.0 %
Coupe 的准确率: 67.0 %
SUV 的准确率: 91.9 %
Van 的准确率: 85.1 %
Other 的准确率: 92.4 %
----------------------------------------
整体类别性能: 90.6 %
2009 年及以前的准确率: 88.6 %
2010 年及以后的准确率: 91.4 %

结果表明，该模型在品牌和时代标记分类任务上均可实现超过 90% 的整体准确率，且仅需训练十个时代。这些相似任务之间的协同效应帮助 MTL 模型在有限的训练预算内获得良好的性能。

## 6.4 总结

- 归纳偏置是在模型训练过程中引入的一种先验假设，用于提升其预测性能，例如卷积神经网络中的平移不变性
- 卷积算子通过特殊的卷积层强制执行权重共享
- Dropout 随机禁用某些节点以促进稀疏表示
- 多任务学习 (MTL) 是一种同时学习多个任务的训练框架
- MTL 对更具可推广性的假设引入了归纳偏置，并为相关任务执行隐式数据增强
- 深度神经网络中流行的 MTL 框架包括硬参数共享和软参数共享，其中前者是最主流的方法，后者在研究界正获得日益关注

## 7 通过目标函数进行正则化

### 本章内容

- 基于范数的惩罚及其在模型估计过程中的正则化应用
- 岭回归中的基于 L2 范数的惩罚及其实现
- LASSO 回归中的基于 L1 范数的惩罚及其实现
- 岭回归和 LASSO 回归封闭式解的推导
- 前馈神经网络训练中不同正则化方案效果的实证分析

在训练神经网络时，目标函数是前向传播的终点。目标函数也称为代价函数，它根据网络当前的参数配置来确定预测质量。它通过梯度下降指导下一次迭代中的权重更新。通过汇总所有观测值的预测质量，目标函数提供了一个单一指标来总结下游优化的误差量。如果预测值与目标值（平均而言）接近，该单一指标的值较小，否则较大。

目标函数的方向性同时适用于回归和分类场景。我们知道，均方误差 (MSE) 和交叉熵损失 (CEL) 分别是回归和分类的常用指标。MSE 的下界为零，当算法完美拟合所有训练数据点时，MSE 为零。然而，由于使用了 SoftMax 操作，要得到精确的零 CEL 是不可能的。在两种情况下，最小化这些经验损失函数都能保持优化迭代继续进行。在使用大型神经网络时，我们通常会运行优化直到模型拟合训练集，并在此过程中配合某种形式的正则化，以提高模型在测试集上泛化的可能性。

回顾图 7.1 中所示的整体模型训练工作流。目标函数的主要作用是将模型预测与相应的目标标签进行比较，以分类场景为例（不失一般性）。它提供一个直接的标量值，通常以待最小化的代价形式，来总结当前模型的预测能力。当优化过程的目标是构建一个在训练集上表现良好的准确模型时，优化目标函数意味着通过不断更改权重将预测误差最小化，直到预测误差为零。换言之，目标函数中仅最大化预测准确率 _单独_ 的最终状态是一个完美拟合训练数据集的模型。

![](img/ee98e91e6845f41077b163bec833ee40_222_0.png)

图 7.1 整体模型训练工作流。目标函数是前向传播的最后一站，之后诸如梯度下降之类的优化过程将启动，以优化预设模型架构的参数。

然而，无论模型在训练数据集上是否表现出完美的拟合，复杂模型都容易过拟合。这种复杂性通常会导致对新测试集（即使只有轻微变化）的不稳定预测。事实证明，我们可以在目标函数中包含多个指标。例如，我们可以不再单独优化预测准确率，而是引入一个针对模型复杂度的额外惩罚项，使得得到的模型在预测准确率和模型复杂度之间进行权衡。换言之，一个复杂且可能过拟合的模型将获得较低的预测代价，但同时会因模型复杂度而承受显著的惩罚，导致目标函数的总值较高。另一方面，简单模型在惩罚项上具有较低值，但缺乏预测能力，导致目标函数值较高。
