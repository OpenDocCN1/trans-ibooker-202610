

对于每个工具，我们将训练一个模型，为每个特征分配权重。要计算 \( r_{it} \)，即工具 \( i \) 在试验 \( t \) 中的回报，我们使用 \( c_i \)，即工具的截距项；\( w_{ij} \)，即特征 \( j \) 在工具 \( i \) 上的回归权重；以及 \( f_{tj} \)，即试验 \( t \) 中特征 \( j \) 的随机生成值：

```
\( r_{it} = c_i + \sum_{j=1}^{|w_i|} w_{ij} * f_{tj} \)
```

这意味着每个工具的回报计算为该工具市场因子特征回报乘以其权重之和。我们可以使用历史数据（也称为进行线性回归）为每个工具拟合线性模型。如果 VaR 计算的时间跨度为两周，则回归将历史中每个（重叠的）两周间隔视为一个标记点。

值得注意的是，我们本可以选择一个更复杂的模型。例如，模型不必是线性的：它可以是回归树，或者明确包含领域特定知识。

既然我们已有用于从市场因素计算工具损失的模型，就需要一个模拟市场因素行为的过程。一个简单的假设是，每个市场因素回报遵循正态分布。为了捕捉市场因素通常相关的事实——当纳斯达克下跌时，道琼斯也可能遭遇损失——我们可以使用带有非对角协方差矩阵的多元正态分布：

```
\( m_t \sim \mathcal{N} (\mu, \Sigma) \)
```

其中 \( \mu \) 是因素回报的经验均值向量，\( \Sigma \) 是因素回报的经验协方差矩阵。

与之前一样，我们本可以选择更复杂的市场模拟方法，或为每个市场因素假设不同类型的分布，或许使用尾部更肥的分布。

## 获取数据

下载历史股票价格数据集，并将其放置在 `data/stocks/` 目录中。

```
$ mkdir stocks && cd stocks
$ wget https://raw.githubusercontent.com/sryza/aas/master/ch09-risk/data/stocks.zip
$ unzip stocks.zip
```

很难找到大量格式良好的历史价格数据。本章使用的数据集是从 Yahoo! 下载的。

我们还需要风险因素的历史数据。对于我们的因素，我们将使用以下项目的值：

- iShares 20年以上国债债券 ETF (NASDAQ: TLT)
- iShares 美国信用债债券 ETF (NYSEARCA: CRED)
- SPDR 黄金信托 (NYSEARCA: GLD)

下载并放置因素数据。

```
$ cd .. && mkdir factors && cd factors
$ wget https://raw.githubusercontent.com/sryza/aas/master/ch09-risk/data/factors.zip
$ unzip factors.zip
$ ls factors
...
NASDAQ%3ATLT.csv  NYSEARCA%3ACRED.csv  NYSEARCA%3AGLD.csv
```

让我们来看一下我们的一个因素。

```
$ !head -n 5 data/factors/NASDAQ%3ATLT.csv
...
Date,Open,High,Low,Close,Volume
31-Dec-13,102.29,102.55,101.17,101.86,7219195
30-Dec-13,102.15,102.58,102.08,102.51,4491711
27-Dec-13,102.07,102.31,101.69,101.81,4755262
26-Dec-13,102.35,102.36,102.01,102.10,4645323
24-Dec-13,103.23,103.35,102.80,102.83,4897009
```

数据集下载完成后，我们将开始准备数据集。

## 准备数据

GOOGL 的 Yahoo! 格式数据的前几行如下：

```
$ !head -n 5 data/stocks/GOOGL.csv
...
Date,Open,High,Low,Close,Volume
31-Dec-13,556.68,561.06,553.68,560.92,1358300
30-Dec-13,560.73,560.81,555.06,555.28,1236709
27-Dec-13,560.56,560.70,557.03,559.76,1570140
26-Dec-13,557.56,560.06,554.90,559.29,1338507
24-Dec-13,558.04,558.18,554.60,556.48,734170
```

让我们启动 PySpark shell。

```
$ pyspark --driver-memory 4g
```

将工具数据集读入为 DataFrame。

```
stocks = spark.read.csv("data/stocks/", header='true',
inferSchema='true')
```

```
stocks.show(2)
...
```

| 日期 | 开盘价 | 最高价 | 最低价 | 收盘价 | 成交量 |
|------|--------|--------|--------|--------|--------|
| 2013-12-31 | 4.40 | 4.48 | 3.92 | 4.07 | 561247 |
| 2013-12-30 | 3.93 | 4.42 | 3.90 | 4.38 | 550358 |

DataFrame 中缺少工具符号。让我们使用每行对应的输入文件名来添加它。

```
from pyspark.sql import functions as fun
stocks = stocks.withColumn("Symbol", fun.input_file_name()).\
withColumn("Symbol",
fun.element_at(fun.split("Symbol", "/"), -1)).\
withColumn("Symbol",
fun.element_at(fun.split("Symbol", "\."), 1))
stocks.show(2)
...
```

| 日期 | 开盘价 | 最高价 | 最低价 | 收盘价 | 成交量 | 符号 |
|------|--------|--------|--------|--------|--------|--------|
| 31-Dec-13 | 1884.00 | 1900.00 | 1880.00 | 1900.0 | 546 | CLDN |
| 30-Dec-13 | 1889.00 | 1900.00 | 1880.00 | 1900.0 | 1656 | CLDN |

我们将以类似的方式读取和处理因素数据集。

```
factors = spark.read.csv("data/factors", header='true',
inferSchema='true')
factors = factors.withColumn("Symbol", fun.input_file_name()).\
withColumn("Symbol",
fun.element_at(fun.split("Symbol", "/"), -1)).\
withColumn("Symbol",
fun.element_at(fun.split("Symbol", "\."), 1))
```

我们筛选掉历史不足五年的工具：

```
from pyspark.sql import Window
stocks = stocks.withColumn('count', fun.count('Symbol').\
over(Window.partitionBy('Symbol'))).\
filter(fun.col('count') > 260*5 + 10)
```

不同类型的工具可能在不同日期交易，或者数据可能因其他原因存在缺失值，因此确保我们的不同时间序列对齐非常重要。首先，我们需要将所有时间序列修剪到同一时间段。为此，我们将首先将 Date 列的类型从字符串转换为日期。

```
stocks = stocks.withColumn('Date',
fun.to_date(fun.to_timestamp(fun.col('Date'), 'dd-MMM-yy')))
```

```
stocks.printSchema()
...
root
 |-- Date: date (nullable = true)
 |-- Open: string (nullable = true)
 |-- High: string (nullable = true)
 |-- Low: string (nullable = true)
 |-- Close: double (nullable = true)
 |-- Volume: string (nullable = true)
 |-- Symbol: string (nullable = true)
 |-- count: long (nullable = false)
```

让我们修剪工具的时间段以进行对齐。

```
from datetime import datetime

stocks = stocks.filter(fun.col('Date') >= datetime(2009, 10,
23)).\
    filter(fun.col('Date') <= datetime(2014, 10, 23))
```

我们也将在因素 DataFrame 中转换 Date 列的类型并修剪时间段。

```
factors = factors.withColumn('Date',
fun.to_date(fun.to_timestamp(fun.col('Date'), 'dd-MM-yy')))

factors = factors.filter(fun.col('Date') >= datetime(2009, 10,
23)).\
    filter(fun.col('Date') <= datetime(2014, 10,
23))
```

几千个工具和 3 个因素的历史记录足够小，可以在本地读取和处理。即使对于拥有数十万个工具和数千个因素的更大规模模拟，情况也是如此。尽管我们迄今为止一直使用 PySpark 进行数据预处理，但在实际运行模拟时，分布式系统（如 PySpark）的需求应运而生，因为模拟可能需要对每个工具进行大量计算。我们可以将 PySpark DataFrame 转换为 Pandas 数据框，并通过执行内存操作继续轻松地处理它。

```
stocks_pd_df = stocks.toPandas()
factors_pd_df = factors.toPandas()

factors_pd_df.head(5)
```

| | 日期 | 开盘价 | 最高价 | 最低价 | 收盘价 | 成交量 | 符号 |
|---|---|---|---|---|---|---|---|
| 0 | 2013-12-31 | 102.29 | 102.55 | 101.17 | 101.86 | 7219195 | NASDAQ%3ATLT |
| 1 | 2013-12-30 | 102.15 | 102.58 | 102.08 | 102.51 | 4491711 | NASDAQ%3ATLT |
| 2 | 2013-12-27 | 102.07 | 102.31 | 101.69 | 101.81 | 4755262 | NASDAQ%3ATLT |
| 3 | 2013-12-26 | 102.35 | 102.36 | 102.01 | 102.10 | 4645323 | NASDAQ%3ATLT |
| 4 | 2013-12-24 | 103.23 | 103.35 | 102.80 | 102.83 | 4897009 | NASDAQ%3ATLT |

在下一节中，我们将使用这些 Pandas 数据框，尝试拟合一个线性回归模型，以根据因素回报预测工具回报。

## 确定因子权重

请记住，VaR 处理的是 _特定时间跨度内_ 的损失。我们关心的不是工具的绝对价格，而是这些价格在给定时间段内的波动。在我们的计算中，我们将该长度设置为两周。以下函数使用 pandas 的 `rolling` 方法，将价格时间序列转换为两周间隔内的重叠价格变动序列。请注意，我们使用 10 而不是 14 来定义窗口，因为金融数据不包括周末：

```
n_steps = 10
def my_fun(x):
    return ((x.iloc[-1] - x.iloc[0]) / x.iloc[0])

stock_returns =
stocks_pd_df.groupby('Symbol').Close.rolling(window=n_steps).apply(my_fun)
factors_returns =
factors_pd_df.groupby('Symbol').Close.rolling(window=n_steps).apply(my_fun)
```

