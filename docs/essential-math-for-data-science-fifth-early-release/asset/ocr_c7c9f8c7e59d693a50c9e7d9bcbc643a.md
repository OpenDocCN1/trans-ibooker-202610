The critical value is approximately ±2. 262, and we can calculate that in Python as shown in Example 5-16. This captures 95% of the center area of our T-distribution.

# Example 5-16. Calculating the critical value from a T-distribution

```
```python
from scipy.stats import t

n = 10
lower_cv = t(n-1).ppf(.025)
upper_cv = t(n-1).ppf(.975)

print(lower_cv, upper_cv)
# -2.262157162740992 2.2621571627409915
```
```

If our test value happens to fall outside this range of (-2.262, 2.262) then we can reject our null hypothesis. To calculate the test value t, we need to use this formula below. Again r is the correlation coefficient and n is the sample size.

![](bbox=[0.34, 0.46, 0.66, 0.6])

Let's put the whole test together in Python as shown in Example 5-17. If our test value falls outside the critical range of 95% confidence, we accept that our correlation was not by chance.

# Example 5-17. Testing significance for linear-looking data

```
```python
from scipy.stats import t
from math import sqrt

# sample size
n = 10

lower_cv = t(n-1).ppf(.025)
upper_cv = t(n-1).ppf(.975)

# correlation coefficient
```
```