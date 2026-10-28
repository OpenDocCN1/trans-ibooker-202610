If we have no other changes and decide to remove all other records with U for gender, we can do so in one line:

```
```python
df = df[df['Gender'].str.contains('F|M')]
df['Gender'].value_counts()
```
```

F    1864
M    1616
Name: Gender, dtype: int64

> **Exercise 14.16**
> 
> Why is there a vertical bar “|” in the argument to *contains*? Hint: see *section 12.2.3*.

There are now gaps in the index names compared to the integer positions. Use *reset_index* to set them to the same values based on the new number of rows.

```
```python
df.reset_index(drop=True, inplace=True)
```
```

In the last example, we had data in a column, but we decided it was incorrect. What happens if the data is missing? **pandas** uses np.nan as the value for any missing data. Test for this “not a number” value using *isna*.

```
```python
df[df["Breed"].isna()]
```
```

|    | Locality    | Postcode | Breed | Colour | Gender |
|---:|:------------|---------:|:------|:-------|:-------|
| 437  | DANDENONG NORTH | 3175 | NaN | GINGER | F |
| 2461 | KEYSBOROUGH     | 3173 | NaN | TAB    | F |
| 2566 | DANDENONG       | 3175 | NaN | TABWHI | F |

You can use *isnull* as an alias for *isna*.

If you look at the original file from which we loaded the DataFrame, you will see that the missing values are empty strings between commas. The **Colour** column is missing some data, but the **Locality** column is complete.

```
```python
df[df["Colour"].isna()]
```
```

|    | Locality   | Postcode | Breed | Colour | Gender |
|---:|:-----------|---------:|:------|:-------|:-------|
| 1205 | SPRINGVALE  | 3171 | DOMLH | NaN | F |
| 2462 | KEYSBOROUGH | 3173 | TAB   | NaN | F |