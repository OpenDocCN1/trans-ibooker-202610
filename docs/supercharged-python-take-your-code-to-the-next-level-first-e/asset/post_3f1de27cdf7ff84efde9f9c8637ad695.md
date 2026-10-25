

| 模块 | 描述 |
|------|------|
| `stock_demo` | This module prints a menu and prompts the user to choose a command as well as select a stock. |
| `stock_load` | Downloads a data frame from the Internet. |
| `stock_plot` | Takes the information downloaded and plots it. This chapter develops four versions of this module, culminating in `stock_plot_v4`. |

## 15.2 INTRODUCING THE PANDAS PACKAGE

Say hello to the **pandas** package. Like **numpy**, it provides sophisticated storage. But **pandas** also comes with a built-in data reader that gets information from the Internet.

Before you can run any of the code in this chapter, you’ll need to install **pandas** and **pandas_datareader**. From the DOS Box (Windows) or Terminal application (Mac), type the following, one at a time. Each command takes a few seconds to complete.

Click here to view code image

```
pip install pandas
pip install pandas_datareader
```

If you’re running on a Macintosh, remember, you’ll likely need to use **pip3** if **pip** doesn’t work:

Click here to view code image

```
pip3 install pandas
pip3 install pandas_datareader
```

We also assume, because it will become important in this chapter, that you’ve installed both the **numpy** and **matplotlib** packages, as described in Chapters 12 and 13. But if not, you should download them now.

```
pip install numpy
pip install matplotlib
```

Or, in the Macintosh environment, use these commands:

```
pip3 install numpy
pip3 install matplotlib
```

Note carefully the exact spelling of **matplotlib**, which has a “mat” but no “math.”

The **pandas** package creates a _data frame_, which is like a rudimentary table or database used for storing large amounts of information. Data frames have their own binary format. For that reason, they have to be translated into a **numpy** format before they can be plotted. Here’s the statement that does that:

Click here to view code image

```
column = np.array(column, dtype='float')
```

# 15.3 “STOCK_LOAD”: A SIMPLE DATA READER

Now, let’s use a simple **pandas**-based application to read useful information. You can, if you choose, enter the following program into a text editor yourself and save it as `stock_load.py`. Entering the comments (including the doc strings), as usual, is not required if you just want to get this to run.

Click here to view code image

```
'''File stock_load.py ------------------------------------
Does the work of loading a stock, given its ticker symbol.
Depends on files: None
'''
# pip install pandas_datareader
import pandas_datareader.data as web
def load_stock(ticker_str):
    ''' Load stock function.
    Given a string, ticker_str, load information
    for the indicated stock, such as 'MSFT,' into a
    Pandas
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

Assuming you can enter this program (or copy it from
**brianoverland.com/books**) and run it, congratulations.
You’ve just downloaded information on Microsoft stock (MSFT)
for the past 10 years.

This is far too much information to display in a small space,
so **pandas** displays only some of the information, using ellipses
(...) to show that there was more information than could have
been shown, as a practical matter. Here’s some sample output:

Click here to view code image

|    Date    |   High   | ... |   Volume   |   Adj   |
|------------|----------|-----|------------|---------|
| Close      |          |     |            |         |
| 0  2010-01-04 | 31.100000 | ... | 38409100.0 |         |
| 24.720928   |          |     |            |         |
| 1  2010-01-05 | 31.100000 | ... | 49749600.0 |         |
| 24.728914   |          |     |            |         |
| 2  2010-01-06 | 31.080000 | ... | 58182400.0 |         |
| 24.577150   |          |     |            |         |
| 3  2010-01-07 | 30.700001 | ... | 50559700.0 |         |
| 24.321552   |          |     |            |         |
| 4  2010-01-08 | 30.879999 | ... | 51197400.0 |         |
| 24.489288   |          |     |            |         |
| 5  2010-01-11 | 30.760000 | ... | 68754700.0 |         |
| 24.177786   |          |     |            |         |

The program prints more information than this; the output
shown here gives only the first 10 lines printed.

After printing all the data on Microsoft stock for 10 years, the program then prints the structure of the data frame itself— specifically, a list of columns.

Click here to view code image

```
Index(['Date', 'High', 'Low', 'Open', 'Close',
 'Volume',
 'Adj Close'], dtype='object')
```

Let’s examine what this application does. It starts with a statement that imports the **pandas** data-reader package so that it can be referred to by a short name, **web**.

Click here to view code image

```
import pandas_datareader.data as web
```

Most of the work of this module is done by one function, `load_stock`, which has the following definition:

Click here to view code image

```
def load_stock(ticker_str):
    ''' Load stock function.
    Given a short string, ticker_str, load
    information
    for the indicated stock, such as 'MSFT,' into a
    Pandas
    data frame (df) and return it.
    '''
    df = web.DataReader(ticker_str, 'yahoo')
    df = df.reset_index()
    return df
```

If you’ve read Chapter 14, you may recall that testing the attribute __name__ serves a special purpose: It tells the app that if the current module is being run directly (thus making it the main module), then execute the lines that follow.

Click here to view code image

```
# Get a data frame (stock_df) and print it out.
if __name__ == '__main__':
    stock_df = load_stock('MSFT') # 'msft' also Ok.
    print(stock_df)
    print(stock_df.columns)
```

The load_stock function is called, passing the Microsoft stock ticker name (MSFT). Most of the work of this function is done by the third line of code.

Click here to view code image

```
df = web.DataReader(ticker_str, 'yahoo')
```

This is almost too easy.

For a server name, the program uses yahoo. We believe this server will continue to be reliable in the foreseeable future, but, if necessary, you can search the Internet for another financial- data server.

The next step is to call the reset_index method. This method updates the index information for the columns. It’s not obvious that you need to use this, but it turns out to be necessary. None of the code in this chapter works without it.

```
df = df.reset_index()
```

Finally, the data fame is returned to the module-level code.
That code prints both the data frame itself and a summary of
the columns in the data frame; we’ll return to that summary
later.

# 15.4 PRODUCING A SIMPLE STOCK CHART

The next step in creating a stock-market application is to plot
the data— although this section will do it in a minimal way, not
putting up legends, titles, or other information at first.

Here are the contents of version 1 of the second module,
stock_plot.

Click here to view code image

```
"""File stock_plot_v1.py ------------------------------
Does the minimum to plot the closing price
of two fixed stocks. Depends on file stock_load.py
"""
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock
def do_plot(stock_df):
    """ Do Plot function.
    Use stock_df, a stock data frame read from the
    web.
    """
    column = stock_df.Close    # Extract
    price.
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column)    # Plot it.
    plt.show()
# Show the
plot.
# Run two test cases.
if __name__ == '__main__':
    stock_df = load_stock('MSFT')
    do_plot(stock_df)
    stock_df = load_stock('AAPL')
    do_plot(stock_df)
```

This module builds on the functionality of the first module,
stock_load.py, by taking the data produced by the
load_stock function; getting the data it needs from the data
frame; converting it to **numpy** format; and plotting the graph.

Before we do anything else, the correct packages or modules
(or both) need to be imported. The **numpy** and **matplotlib**
packages need to be imported as we did in Chapters 12 and 13.
But we also need to import load_stock from the module
developed in the previous section, stock_load.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
from stock_load import load_stock
```

After the data frame is read from the Internet, the do_plot
function does most of the work. This function is passed a
**pandas** data frame called stock_df.

Click here to view code image

```
def do_plot(stock_df):
    """ Do Plot function.
    Use stock_df, a stock data frame read from the
```

