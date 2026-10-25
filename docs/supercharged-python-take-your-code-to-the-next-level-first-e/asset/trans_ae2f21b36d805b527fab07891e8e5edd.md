
Wow, it works!

And it’s clear how to tweak this example. The following statement replaces 180 with 360, thereby doubling the period of averaging so that for any given day, the previous 360 days’ prices are averaged in to produce the moving-average line rather than 180.

Click here to view code image

```
makepot(stock_df,'Close','360 day average',
360)
```

## 15.12 向用户提供选择

要构建可用的应用程序，你需要向用户提供选择，并让他们决定绘制哪些数据。在任何给定的一天，可能会显示成千上万种图表。

最重要的选择是显示信息的股票。这很简单，只需提示用户输入股票代码，例如 IBM、DIS、MSFT 或 GOOGL。

然后，应用程序可以打印过去十年收盘价的简单图表，也可以从菜单中选择打印几种类型的图表之一。你还可以添加一个“退出”命令。

一个合理的起始应用程序设计可能是提供以下菜单：

Click here to view code image

+   0. Quit
+   1. Print simple closing-price graph.
+   2. Print highs and lows.
+   3. Print price/volume subplots.
+   4. Print price with 180-day moving average.

为了支持这些操作，`stock_plot` 模块需要包含几个函数，每个函数执行不同的操作。这只是从之前的部分复制和粘贴代码的问题。
下面的清单显示了 `stock_plot` 模块的最终版本。

> Click here to view code image

```
# file stock_plot_v4.py ----
---
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
    plt.subplot(2, 1, 1)      # Plot top half.
    makeplot(stock_df, 'Close', 'price')
    plt.title(name_str + ' Price/Volume')
    plt.subplot(2, 1, 2)      # Plot bottom
    half.
    makeplot(stock_df, 'Volume', 'volume')
    plt.show()

def do_movingavg_plot(stock_df, name_str):
    """ Do Moving-Average Plot function.
    Plot price along with 180-day moving average
    line.
    """
    makeplot(stock_df,'Close', 'closing price')
    makeplot(stock_df,'Close', '180 day average',
    180)
    plt.title(name_str + ' Stock Price')
    plt.show()

# Do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str, avg=0):
    column = getattr(stock_df, field)
    if avg:      # Only plot avg if not 0!
        column = column.rolling(avg,
    min_periods=1).mean()
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

现在还需要编写一个模块，提示用户选择，然后调用相应的函数。该模块将提示输入股票代码，然后——如果找到了相关股票——要求进行菜单选择。

我们本可以从一个不尝试捕获异常（运行时错误）的版本开始，但考虑到可能出错的所有情况（连接不佳、输入无效的股票代码），添加错误检查（即异常处理代码）后，这个应用程序会更加有用。错误检查对于许多类型的应用程序都是一件好事，但在这种情况下，它是必不可少的。

以下应用程序通过重新提示用户，直到成功或直到用户输入空字符串来处理错误，在这种情况下，应用程序会优雅地终止。如第 8 章所示，根据需要进行重新提示，因为异常处理和 while 循环的结合。循环永远不会退出，直到函数返回；异常处理的一个副作用是跳转到 except 语句，然后（作为结果）再次从循环顶部开始。

Click here to view code image

```
# file stock_demo.py ------------------------------------
from stock_plot_v4 import *
menu_str = ('Menu choices:\n' +
'0. Exit\n' +
'1. Simple Plot of Prices\n' +
'2. Plot of Daily High/Low Prices\n' +
'3. Plot of Price with Volume Subplot\n' +
'4. Price plus Moving Average\n')
prompt_msg = 'Enter stock symbol (ENTER to exit): '
def main():
    while True:
        # Prompt and re-prompt user until a valid
        stock
        # symbol is entered OR user exits.
        try:
            s = input(prompt_msg)
            s = s.strip()
            if not s:        # On empty string,
                exit.
                return
            stock_df = load_stock(s)
            n = int(input(menu_str + 'Input choice:
'))
            if n < 0 or n > 4:
                n = 0
            if n == 0:
                return
            fn = [do_simple_plot, do_highlow_plot,
                  do_volume_subplot, do_movingavg_plot]
            [n-1]
            fn(stock_df, s)
        except:
            print('Couldn\'t find stock. Re-try.')
main()
```

这里采用的技术之一是使用开放括号来启用行延续，帮助创建多行字符串，`menu_str`。

另一种技术是索引函数名称列表（可调用对象）并调用相应的命令。还有其他方法可以达到同样的效果——最明显的方法是使用一系列 `if/elif` 语句——但这里使用的技术紧凑且高效。

这个模块的一个有趣之处在于，它调用了 `load_stock` 函数，尽管该函数既未定义在本模块中，也未定义在它直接导入的模块 `stock_plot_v4.py` 中。但该模块导入了 `stock_load.py`。因此，`stock_demo` 间接导入了 `stock_load`。

还有许多可能的改进，但这些留给了章末的建议问题。

# 第 15 章总结

在这本书中，我们已经走得很远了。在第 1 章中，我们的 Python 程序展示了几个打印一些值的语句。然后，仍在第 1 章中，我们使用 Python 打印了诸如斐波那契数之类的复杂序列。但是，在本章及之前的章节中，我们添加了列表、矩阵和复杂数据框等 Python 对象。

能够从互联网上获取信息，将其加载到数据框和数组中，最后将其绘制出来，这真是太棒了。图形编程是你可以掌握的最具挑战性的任务之一，但得益于其包，Python 将问题减少到仅几行代码。甚至在第 13 章中也涉及了三维图形。

所以，如果我们问“Python 有什么用？”答案是“任何事情，只要有人为它编写了包！”

还有其他一些关于这门语言的事情可以说。在大多数程序中你最常想做的事情——获取用户输入，然后将输入分解为单词、短语和数字——在 Python 中得到了极好的支持。而正则表达式包将这些能力提升到了更高的水平。

Python 在列表、字符串、字典和集合方面的内置能力非常强大，以至于最大的挑战之一就是了解你拥有的所有选项。你可以很长一段时间都没有意识到集合是自排序的，或者你可以在不编写循环的情况下获取列表的总和。学习每个 Python 快捷方式可能需要一段时间。我们写这本书，部分目的就是为了缩短你掌握这种丰富语言所需的时间。

Python 也存在偶尔的危险，你在沿着 Python 河流顺流而下时可能会遇到的危险弯道。我们也在本书中尝试解决了大部分这些问题，所以当你最终与 Python 面对面时，你会发现它不是草丛中的蛇，而是一个新朋友和伙伴。

# 第 15 章复习


