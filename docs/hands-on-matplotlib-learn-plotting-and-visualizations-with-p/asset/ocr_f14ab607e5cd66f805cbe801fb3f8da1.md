# Dataframe in Pandas

A dataframe is a two-dimensional labeled data structure with columns that can be different datatypes. You can create dataframes from series, Ndarrys, lists, and dictionaries.

Dataframes have labels, which are collectively called an *index*. You can easily view and manipulate the data in the dataframes. The data is stored in a rectangular grid format in dataframes.

You can create a dataframe from a list as follows. The following is a dictionary:

```
data = {'city': ['Delhi', 'Delhi', 'Delhi',
    'Bangalore', 'Bangalore', 'Bangalore'],
    'year': [2020, 2021, 2022, 2020, 2021, 2022,],
    'population': [10.0, 10.1, 10.2, 5.2, 5.3, 5.5]}
```

Let’s create a dataframe from this, as shown here:

```
df1 = pd.DataFrame(data)
print(df1)
```

The output is as follows:

|    | city     | year | population |
|----|----------|------|------------|
| 0  | Delhi    | 2020 | 10.0       |
| 1  | Delhi    | 2021 | 10.1       |
| 2  | Delhi    | 2022 | 10.2       |
| 3  | Bangalore| 2020 | 5.2        |
| 4  | Bangalore| 2021 | 5.3        |
| 5  | Bangalore| 2022 | 5.5        |

You can see the top five records as follows:

```
df1.head()
```

The output is as follows:

|    | city  | year | population |
|----|-------|------|------------|
| 0  | Delhi | 2020 | 10.0       |
| 1  | Delhi | 2021 | 10.1       |
| 2  | Delhi | 2022 | 10.2       |