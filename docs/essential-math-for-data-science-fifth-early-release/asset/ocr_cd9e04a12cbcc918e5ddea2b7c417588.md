You can quickly calculate the slope m between two points using the simple rise-over-run formula:

$$m = \frac{y_2 - y_1}{x_2 - x_1}$$ $$
$$m = \frac{4.41 - 4.0}{2.1 - 2.0}$$ $$
$$m = 4.1$$

If I made the x step between the two points even smaller, like x = 2 and x = 2.00001 which would result in f(2) = 4 and f(2.00001) = 4.00004, that would get really close to the actual slope of 4. So the smaller our step is to the neighboring value, the closer we get to the slope value at a given point in the curve. Like so many important concepts in math, we find something meaningful as we approach infinitely large or infinitely small values.

Example 1-20 shows a derivative calculator implemented in Python.

```
Example 1-20. A derivative calculator in Python
def derivative_x(f, x, step_size):
    m = (f(x + step_size) - f(x)) / ((x + step_size) - x)
    return m

def my_function(x):
    return x**2

slope_at_2 = derivative_x(my_function, 2, .00001)
print(slope_at_2) # prints 4.000010000000827
```

Now the good news is there is a cleaner way to calculate the slope anywhere on a function. We have already been using SymPy to plot graphs, but I will show you how it can also do tasks like derivatives for you using the magic of symbolic computation.

When you encounter an exponential function like f(x) = x^2 the derivative function will make the exponent a multiplier and then decrement the exponent by 1, leaving us with the derivative \frac{d}{dx} x^2 = 2x. The \frac{d}{dx} indicates