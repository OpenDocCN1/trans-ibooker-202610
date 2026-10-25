## 股票绘图模块 `stock_plot_v4.py`

### 核心绘图函数

```python
def do_split_plot(stock_df, name_str):
    """ Do Plot function, with subplotting.
    Use stock_df, a stock data frame read from web.
    """
    plt.subplot(2, 1, 1)      # Plot top half.
    makeplot(stock_df, 'Close', 'price')
    plt.title(name_str + ' Price/Volume')
    plt.subplot(2, 1, 2)      # Plot bottom half.
    makeplot(stock_df, 'Volume', 'volume')
    plt.show()

data_set = selected_column.rolling(n, min_periods=m).mean()

moving_avg = column.rolling(180, min_periods=1).mean()

# Make a plot: do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str, avg=0):
    column = getattr(stock_df, field)
    if avg:   # Only work if avg is not 0!
        column = column.rolling(avg, min_periods=1).mean()
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()
```

### 移动平均线绘图函数

```python
def do_movingavg_plot(stock_df, name_str):
    """ Do Moving-Average Plot function.
    Plot price along with 180-day moving average line.
    """
    makeplot(stock_df,'Close', 'closing price')
    makeplot(stock_df,'Close', '180 day average', 180)
    plt.title(name_str + ' Stock Price')
    plt.show()

makeplot(stock_df,'Close', '360 day average', 360)
```

### 菜单选项

- 0. 退出
- 1. 打印简单收盘价图表。
- 2. 打印最高价和最低价。
- 3. 打印价格/成交量子图。
- 4. 打印带有 180 日移动平均线的价格。

### 完整模块代码

```python
# file stock_plot_v4.py ----
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock

def do_simple_plot(stock_df, name_str):
    """ Do Plot function.
    Plot a simple graph of closing price.
    """
    makeplot(stock_df,'Close', 'closing price')
    plt.title(name_str + ' Stock Price')
    plt.show()

def do_highlow_plot(stock_df, name_str):
    """ Do High/Low Plot function.
    Plot both highs and lows for stock, then show.
    """
    makeplot(stock_df, 'High', 'daily highs')
    makeplot(stock_df, 'Low', 'daily lows')
    plt.title('High/Low Graph for ' + name_str)
    plt.show()

def do_volume_subplot(stock_df, name_str):
    """ Do Volume Subplot function.
    Plot closing price and volume subplots.
    """
    plt.subplot(2, 1, 1)    # Plot top half.
    makeplot(stock_df, 'Close', 'price')
    plt.title(name_str + ' Price/Volume')
    plt.subplot(2, 1, 2)    # Plot bottom half.
    makeplot(stock_df, 'Volume', 'volume')
    plt.show()

def do_movingavg_plot(stock_df, name_str):
    """ Do Moving-Average Plot function.
    Plot price along with 180-day moving average line.
    """

    makeplot(stock_df,'Close', 'closing price')
    makeplot(stock_df,'Close', '180 day average', 180)
    plt.title(name_str + ' Stock Price')
    plt.show()

# Do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str, avg=0):
    column = getattr(stock_df, field)
    if avg:    # Only plot avg if not 0!
        column = column.rolling(avg, min_periods=1).mean(
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()

if __name__ == '__main__':
    name_str = 'GOOGL'
    stock_df = load_stock(name_str)
    do_movingavg_plot(stock_df, name_str)
    do_simple_plot(stock_df, name_str)
    do_volume_subplot(stock_df, name_str)
    do_highlow_plot(stock_df, name_str)
```
