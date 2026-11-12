```
```python
def z_score(x, mean, std):
    return (x - mean) / std

def z_to_x(z, mean, std):
    return (z * std) + mean

mean = 140000
std_dev = 3000
x = 150000

# Convert to Z-score and then back to X
z = z_score(x, mean, std_dev)
back_to_x = z_to_x(z, mean, std_dev)

print("Z-Score: {}".format(z)) # Z-Score: 3.333
print("Back to X: {}".format(back_to_x)) # Back to X: 150000.0
```
```

Above the z_score() function will take an x-value and scale it in terms of standard deviations, given a mean and standard deviation. The z_to_x() function takes a Z-score and converts it back to an x-value. Studying the two functions you can see their algebraic relationship, one solving for the Z-score and the other for the x-value. We then turn an x-value of 8.0 into a Z-score of 3.333 and then turn that Z-score back into an x-value.