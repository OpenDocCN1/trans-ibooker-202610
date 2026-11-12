The area of a rectangle is _A_ = length × width, so we could easily sum the areas of the rectangles. Would that give us a good approximation of the area under the curve? What if we packed 100 rectangles? 1000? 100,000? As we increase the number of rectangles while decreasing their width, would we not get closer to the area under the curve? Yes we would, and it is yet another case where we increase/decrease something towards infinity to approach an actual value.

Let's try it out in Python. First we need a function that approximates an integral which we will call `approximate_integral()`. The arguments _a_ and _b_ will specify the min and max of the _x_ range respectively. _n_ will be the number of rectangles to pack, and _f_ will be the function we are integrating. We implement the function in Example 1-29, and then use it to integrate our function _f_(_x_) = _x_<sup>2</sup> + 1 with 5 rectangles, between 0.0 and 1.0.

# Example 1-29. An integral approximation in Python

```
```python
def approximate_integral(a, b, n, f):
    delta_x = (b - a) / n
    total_sum = 0

    for i in range(1, n + 1):
        midpoint = 0.5 * (2 * a + delta_x * (2 * i - 1))
        total_sum += f(midpoint)

    return total_sum * delta_x

def my_function(x):
    return x**2 + 1

area = approximate_integral(a=0, b=1, n=5, f=my_function)
print(area) # prints 1.33
```
```

So we get an area of 1.33. What happens if we use 1000 rectangles? Let's try it out in Example 1-30.

# Example 1-30. An integral approximation in Python

```
```python
area = approximate_integral(a=0, b=1, n=1000, f=my_function)
print(area) # prints 1.333333250000001
```
```