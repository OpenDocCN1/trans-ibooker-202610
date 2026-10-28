Look at the diagonal of numbers going from the upper left to the lower right. These compare columns to themselves, and the columns must be highly correlated!

We can ignore any row or column that includes the Id because we assigned these values randomly.

The interesting positions in the table are where one of the rows is Age, and the other is 1995 Concerts.

```
```python
cc.loc["Age", "1995 Concerts"]
```
```

0.8480108438760597

This number is positive and much closer to 1.0 than 0.0. Therefore, the two columns are highly correlated, and one increases as the other does. I interpret this to mean that the older you are, the more concerts you likely attended in 1995.

See where I used ["Age", "1995 Concerts"] to get the correlation coefficient? This indexing retrieved the value from a specific row and column. That’s why in section 14.1.4, I used [0, 1] to get the values from the first rows, second positions of the covariance and correlation matrices.

# 14.6 Converting categorical data

The GENDER column contains *categorical data* rather than numeric. The items in the column belong to a fixed set of values, which are usually strings. In this case, the values are 'F' and 'M'. While we can check if an item is equal to one of these, it is often easier to convert the categorical column to multiple numeric “dummy columns” containing 0 and 1.

Here are the first two rows of df:

```
```python
df.head(2)
```
```

|   | Locality | Postcode | Breed | Colour | Gender |
|---|---|---:|---|---|---|
| 0 | DANDENONG NORTH | 3175 | DOMSH | TAB | F |
| 1 | DANDENONG NORTH | 3175 | DOMLH | BLAWHI | M |

and this is what we get when we use *get_dummies* on the GENDER column:

```
```python
pd.get_dummies(df, columns=["Gender"]).head(2)
```
```

|   | Locality | Postcode | Breed | Colour | Gender_F | Gender_M |
|---|---|---:|---|---|---:|---:|
| 0 | DANDENONG NORTH | 3175 | DOMSH | TAB | 1 | 0 |
| 1 | DANDENONG NORTH | 3175 | DOMLH | BLAWHI | 0 | 1 |