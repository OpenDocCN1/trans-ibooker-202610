null value for cmp_fname_c1, less than 2% of the records have a non-null value for cmp_fname_c2. To create a useful classifier, we need to rely on variables that are almost always present in the data—unless their missingness indicates something meaningful about whether the record matches.

Once we have an overall feel for the distribution of the variables in our data, we want to understand how the values of those variables are correlated with the value of the `is_match` column. Therefore, our next step is to compute those same summary statistics for just the subsets of the `parsed` DataFrame that correspond to matches and nonmatches. We can filter DataFrames using either SQL-style `where` syntax or with Column objects using the DataFrame API and then use `describe` on the resulting DataFrames:

```
matches = parsed.where("is_match = true")
match_summary = matches.describe()

misses = parsed.filter(col("is_match") == False)
miss_summary = misses.describe()
```

The logic inside the string we pass to the `where` function can include statements that would be valid inside a `WHERE` clause in Spark SQL. For the filtering condition that uses the DataFrame API, we use the `==` operator on the "`is_match`" column object to check for equality with the boolean object, `False`, because that is just Python, not SQL. Note that the `where` function is an alias for the `filter` function; we could have reversed the `where` and `filter` calls in the above snippet and everything would have worked the same way.

We can now start to compare our `match_summary` and `miss_summary` DataFrames to see how the distribution of the variables changes depending on whether the record is a match or a miss. Although this is a relatively small data set, doing this comparison is still somewhat tedious—what we really want is to transpose the `match_summary` and `miss_summary` DataFrames so that the rows and columns are swapped, which would allow us to join the transposed DataFrames together by variable and analyze the summary statistics, a practice that most data scientists know as “pivoting” or “reshaping” a data set. In the next section, we’ll show you how to perform these transforms.

# # Pivoting and Reshaping DataFrames