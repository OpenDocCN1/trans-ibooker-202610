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
```