

introduced in Section 9.12, “Setting and Getting Attributes Dynamically.” Here this technique is a major coding convenience.

Figure 15.3 displays the result of the `do_duo_plot` function.

![](img/6e45221a8147f2442c653a3d4a866570_946_0.png)

**Figure 15.3.** Graphing two stocks: MSFT versus AAPL

If you look closely at the code, you should see a flaw. “MSFT” and “AAPL” are _hard-coded_. That’s fine when Microsoft and Apple are the two stocks you want to track. But what if you want to look at others— say, “IBM” and “DIS” (Walt Disney Co.)?

A good design goal is to create flexible functions; you should avoid hard-coding them so that you don’t have to revise the code very much to accommodate new values.

Therefore, for this listing of the latest version of the stock_plot module— which we'll call version 2— we've revised the code so that the do_duo_plot function prints the appropriate labels and title, depending on the stocks passed to it.

Click here to view code image

```python
'''File stock_plot_v2.py
------------------------------------

Plots a graph showing two different stocks.
Depends on stock_load.py
'''
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock
def do_duo_plot(stock1_df, stock2_df, name1, name2):
    ''' Do plot of two stocks.
    Arguments are data frames, which, in the symbol
    column,
    contain the ticker string.
    '''
    makeplot(stock1_df, 'Close', name1)
    makeplot(stock2_df, 'Close', name2)
    plt.title(name1 + ' vs. ' + name2)
    plt.show()
# Make a plot: do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str):
    column = getattr(stock_df, field)
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()
# Run test if this is main module.
if __name__ == '__main__':
    stock1_df = load_stock('MSFT')
    stock2_df = load_stock('AAPL')
    do_duo_plot(stock1_df, stock2_df, 'MSFT',
'AAPL')
```

Now we can control the two stocks that are chosen by changing relatively little code. For example, here’s how you’d plot IBM versus Disney.

Click here to view code image

```python
stock1_df = load_stock('IBM')
stock2_df = load_stock('DIS')
do_duo_plot(stock1_df, stock2_df, 'IBM',
'Disney')
```

Figure 15.4 displays the resulting graph, showing IBM versus the “Mouse House” (Disney).

![](img/6e45221a8147f2442c653a3d4a866570_948_0.png)

Figure 15.4. Graph of IBM versus Disney stock prices

There is a caveat to charting stocks this way: Without the help of color printing or color displays, it may not be easy to differentiate between the lines. Hopefully, even in this book (the printed version) the differences between the lines should show up in contrasting shading. But if this isn’t satisfactory, one approach you might experiment with is using different styles for the two lines, as described in Chapter 13.

Hmm . . . past performance, as brokers like to say, is no guarantee of future results. But if it were, our money would be on the Mouse.

## 15.8 VARIATIONS: GRAPHING OTHER DATA

Let’s go back and revisit the information available to us in the stock-market data frames. Section 15.3 printed the index for these frames.

Click here to view code image

```python
Index(['Date', 'High', 'Low', 'Open', 'Close',
       'Volume',
       'Adj Close'], dtype='object')
```

So the data frame provides seven columns of data, as shown in Table 15.2.

# Table 15.2. Column Names in a “pandas” Stock Data Frame

| Column | Description |
|---|---|
| Date | The date corresponding to a given row (which has data for one day’s stock report). |
| High | Highest price recorded for the day. |
| Low | Lowest price recorded for the day. |
| Open | Opening price of the stock on that day— that is, the price when trading opened that morning. |
| Close | Closing price for the day— that is, at the end of one day’s trading. |
| Volume | Number of shares sold on the day in question. For major securities like Microsoft, this can run into the tens of millions. |
| Adj Close | Adjusted closing price. |

Armed with this information, you can experiment by graphing different columns against the `Date` column, which holds the dates corresponding to each row. For example, we might, as an exercise, want to plot both the daily highs and the daily lows for a given stock.

The following code listing— we’ll call this version 3— produces a combined high/low graph for a stock. New and altered lines, as usual, are shown in bold.

Click here to view code image

```python
'''File stock_plot_v3.py -------------------------------------
---
Plots daily highs and lows for a stock.
Depends on stock_load.py
'''
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock
def do_highlow_plot(stock_df, name_str):
    ''' Do plot of daily highs and lows.
    Use high_price and low_price columns for one
    stock,
    which are passed through a stock data frame
    (stock_df).
    '''
    makeplot(stock_df, 'High', 'daily highs')
    makeplot(stock_df, 'Low', 'daily lows')
    plt.title('High/Low Prices for ' + name_str)
    plt.show()
# Make a plot: do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str):
    column = getattr(stock_df, field)
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()
# Run test if this is main module.
if __name__ == '__main__':
    stock_df = load_stock('MSFT')
    do_highlow_plot(stock_df, 'MSFT')
```

Figure 15.5 shows the resulting graph, charting both the daily high and the daily low stock prices.

![](img/6e45221a8147f2442c653a3d4a866570_952_0.png)

# Figure 15.5. Graphing highs and lows of Microsoft stock

What else can we do with the data? Another useful piece of information is volume: the number of shares sold on any given day. Here’s another plotting function, with new lines in bold.

Click here to view code image

```python
def do_volume_plot(stock_df, name_str):
    """ Plot the daily volume of a stock passed in
    as a
    data frame (stock_df).
    """
    makeplot(stock_df, 'Volume', 'volume')
    plt.title('Volume for ' + name_str)
    plt.show()
```

When this function is run and is passed the data frame for MSFT, it produces the graph shown in Figure 15.6.

![](img/6e45221a8147f2442c653a3d4a866570_953_0.png)

**Figure 15.6.** Chart of daily volume for a stock

The numbers on the left represent not millions of shares, but tens of millions. Because those numbers are too large to print without taking up large amounts of screen real estate, the number 1e8 is printed at the top of the graph. Consequently, figures on the left should be interpreted as multiplies of 10 million, which is equal to 1e8 (10 to the eighth power).

## 15.9 LIMITING THE TIME PERIOD

One thing we haven’t controlled until now is the time period covered by the chart. What if you wanted to see the data for the stock in the past three months rather than 10 years?

Each day marks an active trading day: This includes Monday through Friday but not holidays. That’s why there are roughly 240 “days” in a calendar year, and not 365. By the same logic, a month is roughly 20 days, and three months is roughly 60 days.

That being noted, the technique for restricting the duration of a data frame is to use our old friend, slicing. As you may recall from Chapter 3, the expression that gets the last N items of a string, list, or array is

```
[-N:]
```

We can apply this operation to a pandas data frame as well. The effect is to get the most recent N rows of data, ignoring everything else. Therefore, to restrict the data frame to the past three months (60 days), you use the following statement:

Click here to view code image

```python
stock_df = stock_df[-60:].reset_index()
```

The reset_index method is also called, because it’s necessary to keep the data accurate in this case.

When this statement is plugged in to the previous example, we’ll get the last three calendar months (60 days) of volume data on a given stock rather than an entire year.

Click here to view code image

```python
def do_volume_plot(stock_df, name_str):
    """ Plot the daily volume of a stock passed in
```

