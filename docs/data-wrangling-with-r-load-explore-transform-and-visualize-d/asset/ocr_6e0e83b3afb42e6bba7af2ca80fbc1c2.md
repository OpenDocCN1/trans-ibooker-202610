```
output_data.isna().sum().plot.bar()
```

]: <AxesSubplot:>

![](bbox=[0.22, 0.18, 0.84, 0.6])

Figure 9.21: Visualizing the data completeness of the DataFrame

Then, we can explore the dataset in terms of the distribution of data values in certain columns, which will give you an idea of your dataset. The following function provides the count of values, the most frequently occurring value and least frequently occurring value, and also the overall count of distinct values (4058 unique values):