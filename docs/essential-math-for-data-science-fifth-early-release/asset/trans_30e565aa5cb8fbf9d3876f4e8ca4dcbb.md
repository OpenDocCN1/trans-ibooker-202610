

虽然过程基本相同，但在神经网络上使用随机梯度下降存在一个复杂性。一层中的节点将其权重和偏差馈送到下一层，然后下一层应用另一组权重和偏差。这创造了一个像洋葱一样嵌套的结构，我们需要从输出层开始解开。

在梯度下降过程中，我们需要确定应该调整哪些权重和偏差，以及调整多少，以减少总体成本函数。单次预测的成本将是神经网络输出 \(A_2\) 的平方减去预测值 Y。

\(C = (A_2 - Y)^2\)

但让我们深入一层。这个激活后的输出 \(A_2\) 只是经过激活函数处理后的 \(Z_2\)。

\(A_2 = sigmoid(Z_2)\)

而 \(Z_2\) 则是将输出权重和偏差应用于来自隐藏层的激活输出 \(A_1\) 的结果。

\(Z_2 = W_2A_1 + B_2\)

\(A_1\) 是基于 \(Z_1\) 构建的，\(Z_1\) 经过 ReLU 激活函数处理。

\(A_1 = ReLU(Z_1)\)

最后，\(Z_1\) 是输入 X 值经过隐藏层加权和加偏后的结果。

\(Z_1 = W_1X + B_1\)

我们需要找到包含在 \(W_1, B_1, W_2,\) 和 \(B_2\) 矩阵和向量中的权重和偏差，以最小化我们的损失。通过微调它们的斜率，我们可以改变对最小化损失影响最大的权重和偏差。但是，对权重或偏差的每一点微小调整都会一直传播到外层的损失函数。这时链式法则可以帮助我们找出这种影响。

让我们专注于寻找输出层权重 \(W_2\) 与成本函数 \(C\) 之间的关系。权重 \(W_2\) 的变化会导致未激活输出 \(Z_2\) 的变化。然后这会改变激活后的输出 \(A_2\)，从而改变成本函数 \(C\)。使用链式法则，我们可以定义 \(C\) 对 \(W_2\) 的导数如下。

\(\frac{dC}{dW_2} = \frac{dZ_2}{dW_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2}\)

当我们将这三个梯度相乘时，我们就得到了一个衡量指标，表明对 $W_2$ 的改变会使成本函数 $C$ 变化多少。

现在我们将计算这三个导数。让我们使用 SymPy 来计算成本函数对 A2 的导数，如示例 7-7 所示。

### 示例 7-7. 计算成本函数对 A2 的导数

```python
from sympy import *

A2, y = symbols('A2 Y')
C = (A2 - Y) ** 2
dC_dA2 = diff(C, A2)
print(dC_dA2) # 2*A2 - 2*Y
```

\(\frac{dC}{dA_2} = 2A_2 - 2y\)

接下来，让我们求出 A2 对 Z2 的导数。请记住，A2 是激活函数的输出，在这里是逻辑函数。因此，我们实际上是在求解一个 sigmoid 曲线的导数。

### 示例 7-8. 求解 A2 对 Z2 的导数

```python
from sympy import *

Z2 = symbols('Z2')

logistic = lambda x: 1 / (1 + exp(-x))

A2 = logistic(Z2)
dA2_dZ2 = diff(A2, Z2)
print(dA2_dZ2) # exp(-Z2)/(1 + exp(-Z2)) ** 2
```

\(\frac{dA_2}{dZ_2} = \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2}\)

Z2 对 w2 的导数结果将仅为 A1，因为它只是一个线性函数，并将返回斜率。

### 示例 7-9. Z2 对 W2 的导数

```python
from sympy import *

A1, W2, B2 = symbols('A1, W2, B2')

Z2 = A1*W2 + B2
dz2_dW2 = diff(Z2, W2)
print(dZ2_dW2) # A1
```

\(\frac{dZ_2}{dW_2} = A_1\)

汇总以上内容，以下是求解 $W_2$ 中某个权重的变化如何影响成本函数 $C$ 的导数。

$$ \frac{dC}{dW_2} = \frac{dZ_2}{dW_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2} = (A_1) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (2A_2 - 2y) $$

当我们运行带有三个输入 R、G 和 B 值的输入 X 时，我们将拥有 $A_1, A_2, Z_2$ 和 $y$ 的值。

### 不要在数学中迷失！

此时很容易在数学中迷失，并忘记最初想要实现的目标，即求解输出层中某个权重（$W_2$）对成本函数的导数。当你发现自己陷入细节并忘记自己想要做什么时，请后退一步，去散散步，喝杯咖啡，提醒自己想要完成什么。如果做不到，你应该从头开始重新来过，一步步推导到你迷失的地方。

然而，这只是神经网络的一个组成部分，即 $W_2$ 的导数。以下是示例 7-10 中用于链式法则所需的其余偏导数的 SymPy 计算。

### 示例 7-10. 计算神经网络所需的所有偏导数

```python
from sympy import *

W1, W2, B1, B2, A1, A2, Z1, Z2, X, Y = symbols('W1 W2 B1 B2 A1 A2 Z1 Z2 X Y')

# Calculate derivative of cost function with respect to A2
C = (A2 - Y)**2
dc_dA2 = diff(C, A2)
print("dc_dA2 = ", dc_dA2) # 2*A2 - 2*Y

# Calculate derivative of A2 with respect to Z2
logistic = lambda x: 1 / (1 + exp(-x))
A2 = logistic(Z2)
dA2_dZ2 = diff(A2, Z2)
print("dA2_dZ2 = ", dA2_dZ2) # exp(-Z2)/(1 + exp(-Z2)) ** 2

# Calculate derivative of Z2 with respect to A1
_Z2 = A1*W2 + B2
dZ2_dA1 = diff(_Z2, A1)
print("dZ2_dA1 = ", dZ2_dA1) # W2

# Calculate derivative of Z2 with respect to W2
dZ2_dW2 = diff(_Z2, W2)
print("dZ2_dW2 = ", dZ2_dW2) # A1
```

```python
# Calculate derivative of Z2 with respect to B2
dZ2_dB2 = diff(Z2, B2)
print("dZ2_dB2 = ", dZ2_dB2) # 1
```

```python
# Calculate derivative of A1 with respect to Z1
relu = lambda x: Max(x, 0)
_A1 = relu(Z1)

d_relu = lambda x: x > 0 # Slope is 1 if positive, 0 if negative
dA1_dZ1 = d_relu(Z1)
print("dA1_dZ1 = ", dA1_dZ1) # Z1 > 0
```

```python
# Calculate derivative of Z1 with respect to W1
_Z1 = X*W1 + B1
dZ1_dW1 = diff(Z1, W1)
print("dZ1_dW1 = ", dZ1_dW1) # X
```

```python
# Calculate derivative of Z1 with respect to B1
dZ1_dB1 = diff(Z1, B1)
print("dZ1_dB1 = ", dZ1_dB1) # 1
```

请注意，ReLU 是手动计算的，而不是使用 SymPy 的 diff() 函数。这是因为导数适用于平滑曲线，而不是 ReLU 上存在的锯齿状拐点。但我们可以轻松地通过声明正数的斜率为 1、负数的斜率为 0 来绕过这个问题。这是有道理的，因为负数是斜率为 0 的平直线。而正数则保持不变，具有 1 对 1 的斜率。

这些偏导数可以链式组合，以创建相对于权重和偏差的新偏导数。让我们求解 $W_1$、$W_2$、$B_1$ 和 $B_2$ 相对于成本函数的所有四个偏导数。我们已经讲解了 $\frac{dC}{dW_2}$。让我们将其与我们需要的其他三个链式导数一起展示。

\[
\frac{dC}{dW_2} = \frac{dZ_2}{dW_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2} = (A_1) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (2A_2 - 2y)
\]

\[
\frac{dC}{dB_2} = \frac{dZ_2}{dB_2} \frac{dA_2}{dZ_2} \frac{dC}{dA_2} = (1) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (2A_2 - 2y)
\]

\[
\frac{dC}{dW_1} = \frac{dC}{dA_2} \frac{dA_2}{dZ_2} \frac{dZ_2}{dA_1} \frac{dA_1}{dZ_1} \frac{dZ_1}{dW_1} = (2A_2 - 2y) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (W_2) (Z_1 > 0) (X)
\]

\[
\frac{dC}{dB_1} = \frac{dC}{dA_2} \frac{dA_2}{dZ_2} \frac{dZ_2}{dA_1} \frac{dA_1}{dZ_1} \frac{dZ_1}{dB_1} = (2A_2 - 2y) \left( \frac{e^{-Z_2}}{(1 + e^{-Z_2})^2} \right) (W_2) (Z_1 > 0) (1)
\]

我们将使用这些链式梯度来计算成本函数 C 相对于 W1、B1、W2 和 B2 的斜率。

## 自动微分

如你所见，即使使用链式法则和像 SymPy 这样的符号库，解开导数仍然很繁琐。这就是为什么可微分编程库正在兴起，例如 Google 开发的 JAX 库。它与 NumPy 几乎完全相同，只是它允许对打包为矩阵的参数计算导数。

如果你想了解更多关于自动微分的信息，这个 YouTube 视频很好地解释了它。

https://youtu.be/wG_nFlawSSY

## 随机梯度下降

我们现在准备好整合链式法则以执行随机梯度下降了。为了保持简单，我们在每次迭代中只采样一个训练记录。批量梯度下降和小批量梯度下降常用于神经网络和深度学习，但每次迭代只处理一个样本就已经涉及足够多的线性代数和微积分了。

让我们来看看我们神经网络的完整实现，带有反向传播的随机梯度下降，如示例 7-11 所示。

## 示例 7-11. 使用随机梯度下降实现神经网络

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

all_data = pd.read_csv("https://tinyurl.com/y2qmhfsr")

# Learning rate controls how slowly we approach a solution
# Make it too small, it will take too long to run.
# Make it too big, it will likely overshoot and miss the solution.
L = 0.05

# Extract the input columns, scale down by 255
all_inputs = (all_data.iloc[:, 0:3].values / 255.0)
all_outputs = all_data.iloc[:, -1].values

# Split train and test data sets
X_train, X_test, Y_train, Y_test = train_test_split(all_inputs, all_outputs,
test_size=1 / 3)
n = X_train.shape[0]

# Build neural network with weights and biases
```

