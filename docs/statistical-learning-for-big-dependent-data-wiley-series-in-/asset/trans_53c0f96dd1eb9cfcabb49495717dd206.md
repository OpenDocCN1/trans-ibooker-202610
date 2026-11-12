

在该算法中，\(\nabla_\theta\) 表示对 \(\theta\) 的偏导数，\(L(f(.), y)\) 表示 \(f(.)\) 与 \(y\) 之间的损失函数，其中 \(f(.)\) 表示深度网络的输出。SGD 算法收敛的充分条件是

```
\(\sum_{j=1}^\infty \epsilon_j = \infty \quad \text{and} \quad \sum_{j=1}^\infty \epsilon_j^2 < \infty.\)
```

在实践中，可以从初始学习率 \(\epsilon_0\) 开始，并按线性方式降低学习率，直到迭代 \(\tau\)，如下所示

```
\(\epsilon_j = (1 - \alpha)\epsilon_0 + \alpha\epsilon_\tau,\)
```

其中 \(\alpha = j/\tau\)。迭代 \(\tau\) 之后，学习率变为常数。在某些应用中，甚至可以使用 \(\ell = 1\)，即单个数据点。

另一种有用的训练方法是 Polyak (1964) 的 **动量**，可以总结如下：

### 带动量的 SGD:

- 1. 指定学习率 \(\epsilon\) 和动量参数 \(\alpha\)。
- 2. 设初始参数为 \(\theta\)，初始速度为 \(\mathbf{v}\)。
- 3. 直到满足停止准则，重复以下步骤
    - 抽取一个包含 \(\ell\) 个数据点的小批量，例如 \(\{(\mathbf{x}^{(i)}, y^{(i)})|i = 1, \dots, \ell\}\)。
    - 计算梯度估计：
      \(\hat{\boldsymbol{g}} = \frac{1}{\ell} \nabla_\theta \sum_i L(f(\mathbf{x}^{(i)}, \theta), y^{(i)}).\)
    - 计算速度更新：\(\mathbf{v} = \alpha \mathbf{v} - \epsilon \hat{\boldsymbol{g}}\)。
    - 更新：\(\theta = \theta + \mathbf{v}\)。

训练深度网络还有许多其他可用方法，包括坐标下降、自适应学习率（AdaGrad 算法）、Hinton (2012) 的 RMSProp 算法，以及 Kingma 和 Ba (2014) 的 Adam 方法。同样，有兴趣的读者可参阅 Goodfellow et al. (2016)。

## 8.4 一些应用

我们使用多个真实数据集，演示 RNN 和 DL 的一些应用。对于 RNN 示例，我们使用 R 包 **rnn**；对于其他示例，我们使用包 **keras**，该包可从 RStudio 的 https://keras.rstudio.com 获得。该网站还包含安装信息以及在 R 中运行机器学习的多个示例。有关更多信息，有兴趣的读者可参阅 Chollet 和 Allaire (2020)。另外，R CRAN 上 **keras** 的 vignettes 包含使用该包的多个教程，例如基本分类和基本回归。

### 8.4.1 包：keras

RStudio 中提供的 **keras** 包是一个在 R 中学习 DL 的有用包。它为应用提供了一些内置函数。用户可以使用三种方法之一构建 NN 模型：(a) 顺序模型，(b) 函数式应用程序编程接口 (API)，以及 (c) 自定义模型。对于初学者，顺序模型易于学习。这是本书中使用的方法。一旦读者熟悉该包，就可以使用其他两种方法构建更复杂的 NN 模型。该包包含多种不同类型的网络层，包括 layer_dense（传统 NN 的全连接层）、layer_dropout（控制随机丢弃某些输入节点）、layer_lstm（LSTM 层）等众多层。该包还包含第 8.3.3 节中的各种激活函数。顺序方法允许用户为 NN 指定一层接一层，并选择每一层的激活函数。建议读者通过实验来了解更多关于在 R 中进行 DL 的内容。

### 示例 8.4

在本示例中，我们考虑从 1948 年 1 月到 2020 年 2 月的美国月度失业率。数据已进行季节调整，可直接从 FRED（圣路易斯联邦储备银行的经济数据库）下载。事实上，可以通过 R 包 quantmod 从互联网下载数据。详情见附带的 R 命令和输出。或者，数据也在文件 unrate4820.txt 中。图 8.9 显示了美国月度失业率数据的时间图。

失业率精确到小数点后一位，这使其适用于 R 包 rnn（一个将整数转换为二进制代码的循环神经网络包）。设 u_t 为观测到的失业率。我们使用 z_t = 10u_t，使 z_t 取整数值。然而，在与其他统计方法进行预测比较时，我们使用 u_t。样本量为 866，我们保留最后

![](img/d4c84ae3c2178f4c506af3dd2ed13122_464_0.png)

图 8.9 1948 年 1 月至 2020 年 2 月的美国月度失业率。该图由 R 包 quantmod 的 chartSeries 命令生成。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_465_0.png)

图 8.10 美国月度失业率预测子样本中观测数据（圆圈）、RNN 预测（蓝色）和 AR(13) 模型预测（红色）的时间图。

200 个数据点用于样本外比较。预测子样本从 2003 年 7 月到 2020 年 2 月。

作为基准模型，我们使用标量 AR(13) 模型。因此，我们使用 z_{t-1}, ..., z_{t-13} 作为 RNN 网络的输入变量，此外，我们使用三个大小为 (5,5,5) 的隐藏层。所选激活函数是包含偏置项的 logistic（即 sigmoid）函数。训练以 0.05 的学习率和 20 个 epoch 进行。图 8.10 显示了预测子样本中观测数据（圆圈）、RNN 预测（蓝色）和 AR(13) 预测（红色）的时间图。从图中可以看出，RNN 和标量 AR(13) 模型都能相当好地跟踪观测数据，但 RNN 在 2010 年左右有相对较大的偏差。标量 AR(13) 模型的均方根误差（RMSE）、平均绝对误差和中位绝对误差（MedAE）分别为 0.15、0.12 和 0.11。另一方面，RNN 的三个指标分别为 0.22、0.13 和 0.1。RNN 相对较大的 RMSE 是由前文提到的 2010 年左右的较大误差引起的。

### R 演示：RNN 包

```
> require(quantmod)
> require(rnn)
> getSymbols("UNRATE",src="FRED")
> chartSeries(UNRATE,theme="white")
> rate <- as.numeric(UNRATE[,1])
> x <- rate*100 ### change rate to integers
> require(SLBDD)
> mm <- rnnStream(x,h=13,nfore=200) #A R command to setup the input and
## output for the RNN neural network. See R scripts below.
> names(mm)
[1] "Xfit" "Yfit" "yp" "Xp" "X" "yfit" "newX"
## Xfit, Yfit, and newX are arrays in binary of length 8.
> Xo <- mm$X
```

```
> dim(Xo)
[1] 653 13
> yfit <- mm$yfit
> Xo=data.frame(Xo)
> newX=data.frame(mm$newX)
> m1 <- lm(yfit~.,data=Xo)
> m1
Call:
lm(formula = yfit ~ ., data = Xo) ## output truncated

Coefficients:
(Intercept)       zz        zz.1       zz.2       zz.3       zz.4       zz.5
0.880015    0.984881   0.223721   -0.053961   -0.068886   0.008282  -0.116379
        zz.8       zz.9      zz.10      zz.11      zz.12
-0.027701 -0.108302  0.131595  -0.140236   0.112020

> y1 <- predict (m1,newdata=newX)
> yp <- mm$yp  # observed rates
> mean(abs(yp-y1)) # should be divided by 10
[1] 1.195978
> sqrt(mean((yp-y1)^2))
[1] 1.499084
> median(abs(y1-yp))
[1] 1.076162
#
# > m2 <- trainr(Y=mm$Yfit,X=mm$Xfit,learningrate = 0.05,
#   hidden_dim = c(5,5,5),use_bias=TRUE,numepochs = 20)
Trained epoch: 1 - Learning rate: 0.05
Epoch error: 3.8143465630104
...
...
Trained epoch: 20 - Learning rate: 0.05
Epoch error: 2.12295450909187
> y2 <- predictr(m2,mm$Xp)
> y3 <- bin2int(y2) # convert back to integers
> mean(abs(yp-y3))
[1] 1.305
> sqrt(mean((yp-y3)^2))
[1] 2.164486
> median(abs(y3-yp))
[1] 1

> tdx <- c(6+c(1:200))/12+2003  ## x-axis for plotting
> plot(tdx,yp/10,xlab='year',ylab='rate',ylim=c(2.3,11),pch='o',cex=0.6)
> lines(tdx,y1/10,col="red")
> lines(tdx,y3/10,col="blue")
```

```
## Below is the R script of rnnStream
"rnnStream" <- function(z,h=25,nfore=200){
# z: input in integer values
# h: number of lags used as input
# nfore: data points in the testing subsample.
if(!is.integer(z))z=as.integer(z)
ist <- h+1
nT <- length(z)
y <- c( z[ist:nT] )
X <- NULL
for (i in 1:){
```

