The most interesting part of **pandas**, for now, is the data reader, which must be installed as shown earlier. This data reader helps download information.

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
```