![](bbox=[0.253, 0.088, 0.747, 0.465])

Figure 5-6. The residuals are the differences between the line and the points

Points above the line will have a positive residual, and points below the line will have a negative residual. In other words, it is the subtracted difference between the predicted y-values (derived from the line) and the actual y-values (which came from the data). Another name for residuals are **errors**, because they reflect how wrong our line is in predicting the data.

Let's calculate these differences between these 10 points and the line y = 1.93939x + 7.73333 in code Example 5-2.

# Example 5-2. Calculating the residuals for a given line and data

```
import pandas as pd

# Import points
points = pd.read_csv('https://bit.ly/3goOAnt',
                    delimiter=",").itertuples ()

# Test with a given line
m = 1.93939
b = 4.73333
```