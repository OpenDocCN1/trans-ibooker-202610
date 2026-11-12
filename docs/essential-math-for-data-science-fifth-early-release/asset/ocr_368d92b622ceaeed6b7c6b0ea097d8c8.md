b = \frac{\sum y}{n} - m \frac{\sum x}{n}

# Example 5-5. Calculating m and b for a simple linear regression

```
import pandas as pd

# Load the data
points = list(pd.read_csv('https://bit.ly/2KF29Bd',
delimiter=",").iteruples())

n = len(points)

m = (n*sum(p.x*p.y for p in points) - sum(p.x for p in points) *
     sum(p.y for p in points)) / (n*sum(p.x**2 for p in points) -
     sum(p.x for p in points)**2)

b = (sum(p.y for p in points) / n) - m * sum(p.x for p in points) /
n

print(m, b)
# 1.9393939393939394 4.7333333333333325
```

These equations to calculate m and b are derived from calculus, and we will do some calculus work with SymPy later in this chapter if you have the itch to discover where formulas come from. For now, you can plug in the number of data points n as well as iterate the x and y values to do the operations above.

Going forward, we will learn approaches that are more oriented towards contemporary techniques that cope with larger amounts of data. Closed form equations tend to not scale well.

> # COMPUTATIONAL COMPLEXITY
> 
> The reason the closed form equations do not scale well with larger datasets is due to a computer science concept called *computational complexity*, which measures how long an algorithm takes as a problem size grows. This might be worth getting familiar with, and two great YouTube videos on the topic are below:
> 
> **P vs. NP and the Computational Complexity Zoo** https://youtu.be/YX40hbAHx3s
> 
> **What Is Big O Notation?** https://www.youtube.com/watch?v=Q_1M2JaijjQ