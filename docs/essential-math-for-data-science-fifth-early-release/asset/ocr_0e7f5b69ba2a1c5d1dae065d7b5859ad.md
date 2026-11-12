decreases. This again indicates a weaker correlation.

# CALCULATING THE CORRELATION COEFFICIENT

For those that are mathematically curious how the correlation coefficient is calculated, here is the formula:

\( r = \frac{n \sum xy - (\sum x)(\sum y)}{\sqrt{n \sum x^2 - (\sum x^2)} \sqrt{n \sum y^2 - (\sum y^2)}} \)

If you want to see this fully implemented in Python, I like to use one-line `for` loops to do those summations. Example 5-15 shows a correlation coefficient implemented from scratch in Python.

# Example 5-15. Calculating correlation coefficient from scratch in Python

```
```python
import pandas as pd
from math import sqrt

# Import points from CSV
points =
list(pd.read_csv("https://bit.ly/2KF29Bd").itertuples())
n = len(points)

numerator = n * sum(p.x * p.y for p in points) - \\
    sum(p.x for p in points) * sum(p.y for p in points)

denominator = sqrt(n*sum(p.x**2 for p in points) - sum(p.x for p in points)**2) \\
    * sqrt(n*sum(p.y**2 for p in points) - sum(p.y for p in points)**2)

corr = numerator / denominator

print(corr)

# OUTPUT:
# 0.9575860952087218
```
```