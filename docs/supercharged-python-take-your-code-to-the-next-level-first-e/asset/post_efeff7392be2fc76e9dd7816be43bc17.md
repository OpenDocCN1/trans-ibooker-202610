
```
a = s.find(""")
b = s.rfind(""")
if a == -1 or b == -1:
    return ''
return s[a+1:b]
```

```
def do_printvar(s, sym_tab):
    """Print Variable function.
    Print named variable after looking it up in
    sym_tab, which was passed from main module.
    """
    wrd = s.split()[0]
    print(sym_tab[wrd], end=' ')
```

```
def do_input(s, sym_tab):
    """Get Input function.
    Get input from the end user and place it in
    the named variable, using a reference to the
    symbol table (sym_tab) passed in as a reference.
    """
    wrd = input()
    if '.' in wrd:
        sym_tab[s] = float(wrd)
    else:
        sym_tab[s] = int(wrd)
```

```
pip3 install pandas
pip3 install pandas_datareader

pip3 install pandas
pip3 install pandas_datareader

column = np.array(column, dtype='float')
```

```
'''File stock_load.py ----

Does the work of loading a stock, given its ticker symbol.
Depends on files: None

'''
# pip install pandas_datareader
import pandas_datareader.data as web

def load_stock(ticker_str):
    ''' Load stock function.
    Given a string, ticker_str, load information
    for the indicated stock, such as 'MSFT,' into a Pandas
    data frame (df) and return it.
    '''
    df = web.DataReader(ticker_str, 'yahoo')
    df = df.reset_index()
    return df

# Get a data frame (stock_df) and print it out.
if __name__ == '__main__':
    stock_df = load_stock('MSFT') # 'msft' also Ok.
    print(stock_df)
    print(stock_df.columns)
```

| Date | High | ... | Volume | Adj Close |
|---|---|---|---|---|
| 0 | 2010-01-04 | 31.100000 | ... | 38409100.0 | 24.720928 |
| 1 | 2010-01-05 | 31.100000 | ... | 49749600.0 | 24.728914 |
| 2 | 2010-01-06 | 31.080000 | ... | 58182400.0 | 24.577150 |
| 3 | 2010-01-07 | 30.700001 | ... | 50559700.0 | 24.321552 |
| 4 | 2010-01-08 | 30.879999 | ... | 51197400.0 | 24.489288 |
| 5 | 2010-01-11 | 30.760000 | ... | 68754700.0 | 24.177786 |

```
Index(['Date', 'High', 'Low', 'Open', 'Close', 'Volume',
'Adj Close'], dtype='object')
```

```
import pandas_datareader.data as web
```

```
def load_stock(ticker_str):
    """ Load stock function.

    Given a short string, ticker_str, load information
    for the indicated stock, such as 'MSFT,' into a Pandas
    data frame (df) and return it.
    """
    df = web.DataReader(ticker_str, 'yahoo')
    df = df.reset_index()
    return df
```

```
# Get a data frame (stock_df) and print it out.
if __name__ == '__main__':
    stock_df = load_stock('MSFT') # 'msft' also Ok.
    print(stock_df)
    print(stock_df.columns)
```

```
df = web.DataReader(ticker_str, 'yahoo')
'''File stock_plot_v1.py ------------------------------------

Does the minimum to plot the closing price
of two fixed stocks. Depends on file stock_load.py
'''
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock

def do_plot(stock_df):
    ''' Do Plot function.
    Use stock_df, a stock data frame read from the web.
    '''
    column = stock_df.Close       # Extract price.

    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column)   # Plot it.
    plt.show()                        # Show the plot.

# Run two test cases.
if __name__ == '__main__':
    stock_df = load_stock('MSFT')
    do_plot(stock_df)
    stock_df = load_stock('AAPL')
    do_plot(stock_df)
```

```
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock

def do_plot(stock_df):
    """ Do Plot function.
    Use stock_df, a stock data frame read from the web.
    """
    column = stock_df.Close          # Extract price.
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column)  # Plot it.
    plt.show()                       # Show the plot.

column = stock_df.Close              # Extract price.

column = np.array(column, dtype='float')

plt.plot(stock_df.Date, column)      # Plot it.

def do_plot(stock_df):
    """ Do Plot function.
    Use stock_df, a stock data frame read from web.
    """
    column = stock_df.Close          # Extract price.
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label='closing price')
    plt.legend()
    plt.title('MSFT Stock Price')
    plt.show()                       # Show the plot.
```

```
def makeplot(stock_df, field, my_str):
    column = getattr(stock_df, field)
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()

def do_plot(stock_df, name_str):
    makeplot(stock_df, 'Close', 'closing price')
    plt.title(name_str + ' Stock Price')
    plt.show()

# Run test if this is main module.
if __name__ == '__main__':
    stock1_df = load_stock('MSFT')
    stock2_df = load_stock('AAPL')
    do_duo_plot(stock1_df, stock2_df)

def do_duo_plot(stock1_df, stock2_df):
    ''"Revised Do Plot function.
    Take two stock data frames this time.
    Graph both.
    ""
    makeplot(stock1_df, 'Close', 'MSFT')
    makeplot(stock2_df, 'Close', 'AAPL')

    plt.title('MSFT vs. AAPL')
    plt.show()
```

```
def makeplot(stock_df, field, my_str):
    column = getattr(stock_df, field)
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()
```

```
'''File stock_plot_v2.py
-------------------------------------------

Plots a graph showing two different stocks.
Depends on stock_load.py
'''
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock

def do_duo_plot(stock1_df, stock2_df, name1, name2):
    ''' Do plot of two stocks.
    Arguments are data frames, which, in the symbol column,
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
    do_duo_plot(stock1_df, stock2_df, 'MSFT', 'AAPL')
```

```
stock1_df = load_stock('IBM')
stock2_df = load_stock('DIS')
do_duo_plot(stock1_df, stock2_df, 'IBM', 'Disney')

Index([['Date', 'High', 'Low', 'Open', 'Close', 'Volume',
        'Adj Close'], dtype='object')

'''File stock_plot_v3.py
----------------------------------------

Plots daily highs and lows for a stock.
Depends on stock_load.py
'''

import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock

def do_highlow_plot(stock_df, name_str):
    ''' Do plot of daily highs and lows.
    Use high_price and low_price columns for one stock,
    which are passed through a stock data frame (stock_df).
    '''

    makeplot(stock_df, 'High', 'daily highs')
    makeplot(stock_df, 'Low', 'daily lows')
    plt.title('High/Low Prices for ' + name_str)
    plt.show()
```

```
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

def do_volume_plot(stock_df, name_str):
    """ Plot the daily volume of a stock passed in as a
    data frame (stock_df).
    """

    makeplot(stock_df, 'Volume', 'volume')
    plt.title('Volume for ' + name_str)
    plt.show()

stock_df = stock_df[-60:].reset_index()
```

```
def do_volume_plot(stock_df, name_str):
    """ Plot the daily volume of a stock passed in as a
    data frame(stock_df). GRAPH LAST 60 DAYS OF DATA ONLY.
    """
    stock_df = stock_df[-60:].reset_index()
    makeplot(stock_df, 'Volume', 'volume')
    plt.title('Volume for ' + name_str)
    plt.show()

plt.subplot(nrows, ncols, cur_row)

plt.subplot(2, 1, 1)      # Do first "row."

# Plot the top half of the graph here.

plt.subplot(2, 1, 2)      # Do second "row."

# Plot the bottom half of the graph here.

plt.show()
```
