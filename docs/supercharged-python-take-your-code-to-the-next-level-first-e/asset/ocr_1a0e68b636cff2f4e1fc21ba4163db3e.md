```
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
```