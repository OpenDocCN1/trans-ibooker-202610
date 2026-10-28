# Exercise 14.21

Why might you not want to create dummy columns from the Breed and Colour columns?

# 14.7 Cats by gender in each locality

Our goal in this section is to create and plot a DataFrame containing the number of female and male cats per locality. We begin with a version of building the DataFrame via comprehensions.

The unique function returns a numpy array of the unique values in a pandas column.

```
localities = df["Locality"].unique()
localities
```

array(['DANDENONG NORTH', 'DANDENONG', 'SPRINGVALE', 'SPRINGVALE SOUTH', 'KEYSBOROUGH', 'NOBLE PARK NORTH', 'NOBLE PARK', 'BANGHOLME', 'DANDENONG SOUTH', 'LYNDHURST'], dtype=object)

```
type(localities)
```

numpy.ndarray

To make it easier for users to understand the results, we put the localities in alphabetical order. The numpy sort method sorts an array in place.

```
localities.sort()
localities
```

array(['BANGHOLME', 'DANDENONG', 'DANDENONG NORTH', 'DANDENONG SOUTH', 'KEYSBOROUGH', 'LYNDHURST', 'NOBLE PARK', 'NOBLE PARK NORTH', 'SPRINGVALE', 'SPRINGVALE SOUTH'], dtype=object)

Now I use two list comprehensions to create lists of female and male cat counts per locality. Note how I filter the rows of the DataFrame by localities within each comprehension.

```
number_of_male_cats = [
    df[df["Locality"] == locality]["M"].sum()
    for locality in localities
]
```