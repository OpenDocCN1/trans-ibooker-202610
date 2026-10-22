the future? One reasonable strategy would be to survey high school students regularly about their postsecondary plans, try to determine which schools and programs appeal to which students, and then model the choices the students might make as seniors. This could be effective, but it would also be very expensive and difficult to manage. Another possibility that is much simpler and costs almost nothing to implement is to use readily available data on how many children are born each year to estimate how many undergraduates there will be later, since some of those babies are the students who enroll in college roughly 20 years later.

To see if this approach will work, let’s look at a scatterplot of NCES data on undergraduate enrollments paired with births data from the National Center for Health Statistics (NCHS) [111]:

```
>>> train = enroll.query("BirthYear < 1999")
>>> sns.scatterplot(data=train, x="Births", y="Enrollment")
>>> plt.title("Enrollments versus Births 20 years earlier")
>>> plt.xlabel("Births (millions)")
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.19. Undergraduate enrollments versus births 20 years earlier