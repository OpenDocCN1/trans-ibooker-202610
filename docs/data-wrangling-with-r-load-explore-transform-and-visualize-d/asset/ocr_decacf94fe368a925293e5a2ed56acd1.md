Figure 9.19: Identifying null columns in the DataFrame

If you want to identify the number of empty values in each column to understand the data completeness, use the following function:

```
output_data.isna().sum()
```

```
Position       0
player_number  12
Name           0
status         0
stat1          0
stat1_val      0
stat2          0
stat2_val      0
stat3          538
stat3_val      538
stat4          538
stat4_val      538
team           0
dtype: int64
```

Figure 9.20: Identifying the completeness of columns within the DataFrame

Optionally, you can visualize the data using matplotlib as shown here. This provides an idea of the columns with null values: