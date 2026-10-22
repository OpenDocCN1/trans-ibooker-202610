This gives us the simplest version of the *null model* in the present context: based on this data, a randomly chosen individual from the relevant population has a 57.7% chance of having at least one of the 14 chronic conditions listed earlier. To draw reasonable conclusions from this data and from models based on it, it’s important to specify this population carefully— it consists of people who sought and received care from doctors in the United States in 2009, since the NAMCS collects data from American physicians about the patients who visited them. People who did not participate in the American health care system that year, either by choice or due to lack of access, are not represented; chronic conditions may or may not have occurred at the same rate among those people, but the data only tell us about Americans who visited their doctors in 2009. This *sample proportion* of 57.7% is likely very close to the actual *population proportion*, which can never be known exactly; thanks to the large sample size of 31,433 people, we can be confident that the population proportion is between 57.1% and 58.3% (the standard error is about 0.3%).

# 4.1.2 Chronic conditions and age

The sample proportion is the sum of all of the values of Condition divided by the number of patients in the dataset— in other words, 57.7% is just the average of this variable. Just as with simple linear regression, the null model uses the average of the observed responses to predict all future responses; the response being predicted here is the chance that a randomly selected person has at least one chronic condition. This ignores any predictor variables that might be available, so including other information like Age should yield much better predictions. To confirm this suspicion, let’s plot the percentage of patients of each age who have a chronic condition:

```
cohorts = (chronic.groupby("Age", as_index=False)
    .agg(Number = ("Condition","size"),
          Rate = ("Condition","mean")))
cohorts
```

Age  Number  Rate
0    0       796   0.026382
1    1       505   0.063366
2    2       320   0.090625
3    3       258   0.143411
4    4       299   0.100334