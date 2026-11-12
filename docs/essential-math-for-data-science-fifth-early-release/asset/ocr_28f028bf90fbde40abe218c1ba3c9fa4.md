Let's continue looking at our data from Figure 5-13. In Example 5-18, take our dataframe code from earlier that calculates the correlation coefficient and then simply square it. That will multiply each correlation coefficient by itself.

# Example 5-18. Creating a correlation matrix in Pandas

```
```python
import pandas as pd
```
```

```
```python
# Read data into Pandas dataframe
df = pd.read_csv('https://bit.ly/2KF29Bd', delimiter=",")

# Print correlations between variables
coeff_determination = df.corr(method='pearson') ** 2
print(coeff_determination)
```
```

```
```python
# OUTPUT:
#
# x y
# x 1.000000 0.916971
# y 0.916971 1.000000
```
```

A coefficient of determination of .916971 is interpreted as “91.6971% of the variation in x is explained by y (and vice versa), and the remaining 8.3029% is noise caused by other uncaptured variables. 0.916971 is a pretty good coefficient of determination, showing that x and y explain each other’s variance. But there could be other variables at play making up that remaining 0.083029. Remember, correlation does not equal causation, so there could be other variables contributing to the relationship we are seeing.