Every good mathematician, as well as accountant, should know the following.

> * Exponential growth (such as compound interest), however slow, must eventually overtake linear growth, however fast.

This is an amazing fact, especially when you consider that it implies that compound growth of .001% on a single dollar must eventually overtake a steady income of a million a year! This is quite true, by the way, but it would take lifetimes for the compounding fund to overtake the million-dollar fund.

This dynamic is easy to show with a graph. Start by creating a **numpy** array, A, representing values along an axis of time. Set it to measure the passing of 60 years.

Click here to view code image

```
A = np.linspace(0, 60, 60)
```

Then we plot a linear-growth function of $2 a year versus a compound-growth function using 10 percent a year—which is mathematically equivalent of raising the number 1.1 to a power, N, where N is the number of years.

Click here to view code image

```
2 * A # Formula for increase of $2 a year
1.1 ** A # Formula for growth of 10% a year
```

We’ll use a format string to specify that the first curve is made