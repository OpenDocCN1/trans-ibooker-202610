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