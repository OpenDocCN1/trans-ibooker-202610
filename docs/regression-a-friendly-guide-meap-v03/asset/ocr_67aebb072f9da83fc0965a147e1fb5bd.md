What we observed in the first 10 rows of the dataset turns out to be true, as we would have guessed based on our own experiences and common sense: chronic conditions are indeed more likely as people get older. This plot is a bit noisy, though, and there are far too many rows in this table to digest at a glance— the ages range from 0 to 100, so cohorts has 101 rows. To get a smoother plot with a concise tabular summary, let’s organize the patients by decade and compute the percentage in each age group with a chronic condition:

```
chronic["Decade"] = 10*round(chronic.Age / 10)
decades = chronic.groupby("Decade", as_index=False).agg(
    Total=("Condition","size"),
    Afflicted=("Condition","sum"),
    Rate=("Condition","mean"))
decades
```

Decade Total Afflicted Rate
0 0.0 2440 179 0.073361
1 10.0 1946 368 0.189106
2 20.0 2622 729 0.278032
3 30.0 2673 990 0.370370
4 40.0 3962 2070 0.522463
5 50.0 4426 2953 0.667194
6 60.0 5356 4127 0.770538
7 70.0 3748 3111 0.830043
8 80.0 3357 2831 0.843312
9 90.0 849 726 0.855124
10 100.0 54 46 0.851852

Summarized in this way, the incidence rates essentially increase monotonically from one age group to the next older age group. These cohort-dependent percentages, which are just the averages of Condition for each group, are empirical probabilities, and they clearly show how terrible the null model is for this data. Someone under 30 has less than a 30% chance of having a chronic condition and someone over 60 has more than a 75% chance of having a chronic condition, yet the null model predicts the chance for each of these people to be 57.7%. Knowing and using a patient’s age is much better than ignoring it! In contrast to plotting the raw 0’s and 1’s, plotting these age group percentages is very useful:

```
sns.scatterplot(data=decades, x="Decade", y="Rate")
```