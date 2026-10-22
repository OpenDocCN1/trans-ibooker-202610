
each species, as determined by the International Union for Conservation of Nature (IUCN) and recorded in their Red List [9]. To prepare the sharks dataset for later use, we eliminate the species whose extinction risks are unknown as well as those whose lengths are unknown— note that the name sharks is just a convenience, since cartilaginous fish also include rays, skates, and sawfish. Finally, we’ll use the power law from the regression model to fill in the missing weights. When we plot the results, it’s easy to spot the imputed values, as they fall right on the regression line:

```
>>> sharks = pd.read_csv("chondrichthyes.csv")
>>> sharks = sharks.join( fish.set_index("Species"), on="Species")
>>> sharks = sharks[ sharks.Length.notna() ]
>>> sharks = sharks[ sharks.Category != "Data Deficient" ]
>>> sharks.head()
```

|   | Species | Category | Length | Weight |
|---|---------|----------|--------|--------|
| 0 | Hexanchus griseus | Near Threatened | 482.0 | 590000.0 |
| 1 | Lamna nasus | Vulnerable | 350.0 | 230000.0 |
| 2 | Leucoraja lentiginosa | Least Concern | 43.0 | NaN |
| 3 | Dactylobatus clarkii | Least Concern | 59.0 | NaN |
| 4 | Pseudobatos percellens | Endangered | 100.0 | 955.1 |

```
>>> imp = np.exp( train_fit.params.Intercept )*np.power( sharks.Length, train_fit.params.Slope )
>>> sharks.Weight = sharks.Weight.where( sharks.Weight.notna(), imp )
>>> sharks
```

|   | Species | Category | Length | Weight |
|---|---------|----------|--------|--------|
| 0 | Hexanchus griseus | Near Threatened | 482.0 | 590000.00 |
| 1 | Lamna nasus | Vulnerable | 350.0 | 230000.00 |
| 2 | Leucoraja lentiginosa | Least Concern | 43.0 | 863.95074 |
| 3 | Dactylobatus clarkii | Least Concern | 59.0 | 2017.5638 |
| 4 | Pseudobatos percellens | Endangered | 100.0 | 955.10000 |
| ... | ... | ... | ... | ... |
| 1179 | Pateobatis jenkinsii | Vulnerable | 130.0 | 16775.968 |
| 1180 | Pristiophorus cirratus | Least Concern | 137.0 | 19308.807 |
| 1181 | Squatina californica | Near Threatened | 152.0 | 25511.414 |
| 1182 | Urogymnus granulatus | Vulnerable | 141.0 | 20857.643 |
| 1185 | Callorhinchus capensis | Least Concern | 122.0 | 5250.0000 |

809 rows × 4 columns

```
>>> sharks = sharks.assign(LogLength = np.log(sharks.Length),
    LogWeight = np.log(sharks.Weight))
>>> sns.scatterplot( data=sharks, x="LogLength", y="LogWeight")
```

```
>>> plt.title("Log(Weight) versus Log(Length) for sharks")
```

Figure 2.17. Scatterplot of log(Weight) versus log(Length) for cartilaginous fish

![](img/f25e2c651d2b8852d1c723f946646ff0_65_0.png)

After all of this work to prepare the sharks dataset, let’s save it for later use:

```
>>> sharks.to_csv("sharks.csv",index=False)
```

This dataset will now be ready to go when we return to it in Chapter 3.
Before moving on to the next example, though, we need to address one aspect of the sharks scatterplot. We can see that the weights for the short cartilaginous fish were missing in the species dataset, since the points in the lower left of the scatterplot are on the regression line. This looks, at first glance, like an example of extrapolation— using values of the explanatory variable outside the training set to make predictions— but it’s not, as the training data included short fish from other classes. While extrapolation is not a problem here, we’ll see a genuine example of it in the next example.

## 2.3.3 Predicting college enrollments

Undergraduate enrollments in the United States grew in the 1980s, were mostly flat during the 1990s, and then climbed steadily until around 2011, when they started to decline:

```
>>> enroll = pd.read_csv("enrollment.csv")
>>> enroll.head()
>>> enroll.Births = enroll.Births / 1000000
>>> enroll.Enrollment = enroll.Enrollment / 1000000
>>> enroll.head()
```

|   | BirthYear | Births   | EnrollmentYear | Enrollment |
|---|-----------|----------|----------------|------------|
| 0 | 1950      | 3.632000 | 1970           | 7.368644   |
| 1 | 1955      | 4.097000 | 1975           | 9.679455   |
| 2 | 1960      | 4.257850 | 1980           | 10.475055  |
| 3 | 1965      | 3.760358 | 1985           | 10.596674  |
| 4 | 1966      | 3.606274 | 1986           | 10.797975  |

```
>>> trend = enroll.query("EnrollmentYear > 1980 & EnrollmentYear < 2020")
>>> sns.lineplot( data=trend, x="EnrollmentYear", y="Enrollment")
>>> sns.scatterplot( data=trend, x="EnrollmentYear", y="Enrollment")
>>> plt.title("Undergraduate enrollments, 1985 to 2018")
>>> plt.xlabel("Enrollment year")
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.18. Undergraduate enrollments since 1985

![](img/f25e2c651d2b8852d1c723f946646ff0_67_0.png)

These are the full-time fall undergraduate enrollments since 1985 at all postsecondary institutions in the U.S., data collected and provided by the National Center for Education Statistics (NCES) [10]. The recent trend is troubling for many colleges and universities, particularly those that have accumulated significant debt from capital improvements and other efforts aimed at attracting some of these millions of students— and their tuition, fees, and spending— to their campuses and communities. Some of these schools have been caught off guard by these developments and are now grappling with the dire consequences of poor planning and budgeting.

How might colleges and universities predict enrollments and plan better for the future? One reasonable strategy would be to survey high school students regularly about their postsecondary plans, try to determine which schools and programs appeal to which students, and then model the choices the students might make as seniors. This could be effective, but it would also be very expensive and difficult to manage. Another possibility that is much simpler and costs almost nothing to implement is to use readily available data on how many children are born each year to estimate how many undergraduates there will be later, since some of those babies are the students who enroll in college roughly 20 years later.

To see if this approach will work, let’s look at a scatterplot of NCES data on undergraduate enrollments paired with births data from the National Center for Health Statistics (NCHS) [11]:

```
>>> train = enroll.query("BirthYear < 1999")
>>> sns.scatterplot(data=train, x="Births", y="Enrollment")
>>> plt.title("Enrollments versus Births 20 years earlier")
>>> plt.xlabel("Births (millions)")
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.19. Undergraduate enrollments versus births 20 years earlier

![](img/f25e2c651d2b8852d1c723f946646ff0_69_0.png)

The horizontal coordinate of each of these points is the number of births for a certain year, and the vertical coordinate of that point is the full-time undergraduate enrollment 20 years later; births and enrollments are in millions of people. The obvious problem with this plot is that these two variables do not seem to be linearly related— this does not look like a good candidate for linear regression. Adding information about birth years will clarify some things, though:

```
>>> train = train.assign( BirthDecade = train.BirthYear - (train.BirthYear % 10) )
>>> train.BirthDecade = train.BirthDecade.astype("category")
>>> sns.scatterplot(data=train, x="Births", y="Enrollment", hue="BirthDecade")
>>> plt.xlabel("Births (millions)")
```

```
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.20. Undergraduate enrollments versus births, by birth decade

![](img/f25e2c651d2b8852d1c723f946646ff0_70_0.png)

We can now see that the scatterplot has distinct components corresponding to different birth decades: the two bottom points correspond to those born in 1950 and 1955, there are horizontal layers corresponding to births in the 1960s and 1970s, and there is a strong positive linear association for the remaining points, those that correspond to births in the 1980s and 1990s. This latter group is the subset of points on which we’ll focus:

```
>>> train = train.query("BirthYear > 1978")
```

```
>>> train.BirthDecade = train.BirthDecade.cat.remove_unused_categories()
>>> sns.scatterplot(data=train, x="Births", y="Enrollment", hue="BirthDecade")
>>> plt.xlabel("Births (millions)")
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.21. Undergraduate enrollments versus births since 1979

![](img/f25e2c651d2b8852d1c723f946646ff0_71_0.png)

The birth years in this scatterplot range from 1979, the point in the bottom left corner, to 1998; we stop at 1998 because the most recent enrollment figure from the NCES is for 2018. To assess the linear relationship evident in this plot, let’s check the correlation:
