The following code produces the same result as shown in Figure 17-5:

```
sns.scatterplot(x='sepal_length', y='sepal_width',
    size='petal_length', style='species',
    hue='species', data=iris)
plt.grid('on')
plt.show()
```

# Plotting Lines

You can also show continuous data such as time-series data along a line. Time-series data has timestamp data in at least one column or has an index. A great example of a time series is a table of daily temperature records. Let’s create a time-series dataframe to demonstrate the line plots.

```
df = pd.DataFrame(np.random.randn(100, 4),
    index=pd.date_range("1/1/2020",
        periods=100),
    columns=list("ABCD"))
df = df.cumsum()
```

You can use the function relplot() to draw the line as follows:

```
sns.relplot(x=df.index, y='A', kind="line", data=df)
plt.xticks(rotation=45)
plt.show()
```