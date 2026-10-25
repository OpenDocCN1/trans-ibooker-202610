```
moving_avg = column.rolling(180,
min_periods=1).mean()
```

To make the rest of the code easy to write, let's first modify
the `makeplot` function so that it accommodates an optional
argument to create a moving-average line. The call to **rolling**
has to be made while `column` is still a **pandas** column, and not
a **numpy** column.

Click here to view code image

```
# Make a plot: do the boring, repetitive stuff.
def makeplot(stock_df, field, my_str, avg=0):
    column = getattr(stock_df, field)
    if avg:         # Only work if avg is not 0!
        column = column.rolling(avg,
min_periods=1).mean()
    column = np.array(column, dtype='float')
    plt.plot(stock_df.Date, column, label=my_str)
    plt.legend()
```

Notice that this gives `makeplot` an additional argument,
`avg`, but that argument, in effect, is optional; it has a default
value of 0.

Now let’s graph both a stock and its 180-day moving average,
corresponding roughly to six months. New and altered lines, as
usual, are in bold.

Click here to view code image

```
def do_movingavg_plot(stock_df, name_str):
    ''' Do Moving-Average Plot function.
    Plot price along with 180-day moving average
```