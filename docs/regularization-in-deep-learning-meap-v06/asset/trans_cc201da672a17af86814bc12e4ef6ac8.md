

```
l2_norm = sum(p.pow(2.0).sum() for p in self.model.parameters()) #F
loss_train = loss_train + self.l2_penalty * l2_norm #F
self.optimizer.zero_grad() #G
loss_train.backward() #G
self.optimizer.step() #G
with torch.no_grad(): #H
    y_test_pred = self.predict(X_test)
    loss_test = self.criterion(y_test_pred, y_test)
    self.test_loss.append(loss_test.item())

return self
```

```
#A Initialize weights to be an array of zeros
#B Iteration counter
#C convert to tensor
#D forward pass to calculate prediction
#E get current MSE loss
#F add L2 penalty
#G backward pass to perform SGD
#H calculate and store test loss without gradient
```

请注意，我们为测试集额外提供了一个 `predict()` 函数。这是与上一章中使用的相同的函数，该函数首先将模型转换为评估模式，执行评分，然后将模式切换回训练模式。

```
def predict(self, x):
    model = self.model.eval()
    outputs = model(x)
    model = self.model.train()
    return outputs
```

让我们在训练集和测试集上测试该类。在下面的代码清单中，我们实例化了一个学习率为 0.001、训练轮数总计 10,000 的 `FCN_regression` 类。迭代次数远多于之前，因为与之前的全批量梯度下降更新相比，SGD 由于其随机性需要更多的迭代才能收敛。接着我们调用 `fit()` 方法训练模型，并绘制训练集和测试集上的 MSE 结果。

### 代码清单 7.9 FCN 模型的模型训练与结果可视化

```
model = FCN_regression(input_size=X_train.shape[1], lr=1e-3, num_epochs=10000)
model.fit(X_train, y_train, X_test, y_test)
x_axis = list(range(model.num_epochs))
plt.plot(x_axis, model.train_loss, label = "train")
plt.plot(x_axis, model.test_loss, ':', label = "test")
plt.legend()
plt.show()
```

运行上述命令生成图 7.12。随着训练损失逐渐收敛，我们对测试集 MSE 有一个有趣的观察：一条典型的从欠拟合到过拟合的 U 形曲线。然而，测试集 MSE 在第 4000 轮之后趋于稳定。这可能与 SGD 算法本身带来的隐式正则化效应有关（下一章将详细介绍），因为模型训练过程中并未使用任何显式正则化技术。

![](img/ee98e91e6845f41077b163bec833ee40_241_0.png)

图 7.12 使用单层 FCN 模型的训练集和测试集 MSE。

现在，让我们通过将 L2 惩罚设置为 0.1 来激活它并观察其影响。欢迎调整此超参数，并使用附带的笔记本 https://github.com/jackliu333/regularization_in_deep_learning/blob/main/Chapter_7.ipynb 探索结果。如图 7.13 所示，添加 L2 惩罚对训练损失曲线没有明显变化，但导致不同轮次下测试集 MSE 显著降低。因此，对模型权重的大小进行控制有助于模型在测试集上泛化得更好。

![](img/ee98e91e6845f41077b163bec833ee40_242_0.png)

图 7.13 使用单层 FCN 模型对比未正则化和岭回归的训练及测试集损失。

在下一节中，我们将探讨另一种常用的正则化项：$L1$ 惩罚，也称为 LASSO。

## 7.3 通过 LASSO 进行稀疏估计

基于 l1 范数的惩罚也称为 LASSO，即最小绝对收缩与选择算子（Least Absolute Shrinkage and Selection Operator）。对权重绝对值之和进行惩罚时，我们实际上同时进行了变量选择和模型估计。变量选择是 LASSO 的自然副产品：使用权重的绝对值对其大小进行惩罚时，某些权重将被设为零，从而相当于从完整特征空间中丢弃这些不重要的特征。最终模型中保留下来的特征被认为更为重要，得到的模型也更加简洁。因此，对权重的 l1 范数进行正则化有助于促进线性方程组的稀疏解。

在探讨使用 l1 范数的稀疏性驱动因素之前，让我们从几何角度重新审视 l2 范数，并以此为基础建立对基于 LASSO 的稀疏性的直觉。

### 7.3.1 岭回归的几何解释

岭回归通过对权重的 L2 范数进行惩罚，使我们能够求解欠定方程组。也就是说，当方程组中未知变量的数量多于方程数量时，岭惩罚可以产生一个具有最小 L2 范数权重的唯一解。相比之下，若不对权重使用任何惩罚，欠定方程组则有无穷多个解。

让我们来看一个简单的线性方程案例，包含两个输入变量、一个输出变量和一个单一观测值。换句话说，我们希望在 y = x^T w = x1w1 + x2w2 中求解 w = [w1, w2]。由于只有一个可用观测值，满足该方程的 w 解有无穷多个。从图形上看，我们可以代入观测值 (x, y)，在以 w1 和 w2 为坐标轴的坐标系中得到一条直线。这条直线表示线性方程所施加的约束下所有可能的（无穷多个）解，直线上的每一点都代表一个解。请参见图 7.14 的示意图。

![](img/ee98e91e6845f41077b163bec833ee40_243_0.png)

图 7.14 岭回归几何解释的可视化。所有可能的解共同构成这条直线。使用 L2 范数在直线与圆的交点处给出一个唯一解，此时半径最小。任何其他点都将对应更大的半径，因此也有更大的 L2 范数。

在此过程中，我们不断从原点绘制圆并增大半径，直到与直线相交。这个交点即代表优化问题的解。虽然直线是无数个同样优良解的集合，但使用 L2 范数强制在众多圆中选取半径最小的那个唯一解，每个圆对应一个不同的 L2 范数。为了看出 L2 范数与圆半径之间的对应关系，请注意 ||w||_2 = sqrt(w1^2 + w2^2)。因此，圆上的所有点（解）共享相同的 L2 范数。

### 7.3.2 引入 L0 范数

在某些情况下，我们也希望得到一个满足线性方程且尽可能多权重为零的解，即稀疏解。一种直接的方法是对权重的 L0 范数进行惩罚，以得到一些权重为零的稀疏解。

L0 范数定义为向量中非零元素的个数。在我们的语境中，||w||0 = #{i | wi != 0}，其中 i ∈ {1, 2}。对 L0 范数进行惩罚意味着寻找非零元素数量最少的最稀疏解，这是一个 NP-hard 问题，需要评估所有可能的特征子集。图 7.15 说明了在 L0 范数约束下求解线性方程的场景，其中解必须位于任意一条坐标轴上，以便只有一个权重为零。请注意，我们依然在寻找具有最小 L0 范数的解；因此，任何从交点延伸出来的解都被排除。在此情况下，加粗的点表示最优解，它通过将 w2 = 0 给出一个稀疏解。

![](img/ee98e91e6845f41077b163bec833ee40_244_0.png)

图 7.15 基于 L0 范数惩罚求解线性方程的几何解释。虽然所有解都必须位于直线上，但 L0 惩罚只要求解位于任意一条坐标轴上即可产生稀疏解。在 L0 惩罚约束下求解线性方程是一项 NP-hard 任务，因为此前常用的凸优化技术（将梯度设为零）在此情况下失效。

仅有两个权重的情况很简单。然而，想象一下有几十个权重需要共同学习。在这种情况下，快速弄清楚哪些权重应该为零就成为一项非平凡的任务。换句话说，使用 L0 范数求解此问题的复杂度

