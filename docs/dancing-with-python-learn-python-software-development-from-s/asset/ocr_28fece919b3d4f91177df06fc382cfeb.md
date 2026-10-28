The values are the same as the first three in the previous computation. Other sklearn functions, methods, and properties give us the transformed lower-dimensional data that we can use in a new DataFrame for further processing.

For now, I drop the redundant M column.

```
del df['M']
df.head(2)
```

| Locality | Breed | Colour | F |
|---|---|---|---|
| 0 | -0.9188 | -0.1091 | 0.8073 | 0.9324 |
| 1 | -0.9188 | -0.3923 | -1.4010 | -1.0726 |

If I use or develop a supervised machine learning to determine a cat’s gender from its Locality, Breed, and Colour, the F column is the label in the training set.

# Exercise 15.14

Optional: I cover linear algebra in Chapter 5 of Dancing with Qubits. [D.W.Q] If you are familiar with that, research how PCA is related to the eigenvectors and eigenvalues of the DataFrame covariance matrix.

# 15.5 Clustering

Suppose I have a CSV dataset containing 75 (x, y) geometric coordinates. I load these into the xy_df pandas DataFrame and look at its descriptive statistical summary:

```
xy_df = pd.read_csv("src/examples/clustering-xy.csv")
xy_df.describe()
```

|   | x | y |
|---|---|---|
| count | 75.0000 | 75.0000 |
| mean | 7.5733 | 4.5401 |
| std | 4.0102 | 2.1265 |
| min | 1.9796 | 1.1947 |
| 25% | 3.4182 | 2.8896 |
| 50% | 7.0173 | 3.6819 |
| 75% | 12.2170 | 6.9615 |
| max | 13.5643 | 8.2785 |