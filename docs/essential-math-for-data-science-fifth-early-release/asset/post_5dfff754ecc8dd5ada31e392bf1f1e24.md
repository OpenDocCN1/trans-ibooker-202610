
Note also that variable names can be subscripted so that several instances of a variable name can be used. For practical purposes, just treat these as separate variables. If you encounter variables x1, x2, and x3, just treat them as three separate variables as shown in Example 1-5.

### Example 1-5. Expressing subscripted variables in Python

```
x1 = 3  # or x_1 = 3
x2 = 10 # or x_2 = 10
x3 = 44 # or x_3 = 44
```

## Functions

Functions are expressions that define relationships between two or more variables. More specifically, a function takes input variables (also called domain variables or independent variables), plugs them into an expression, and then results in an output variable (also called dependent variable).

Take this simple linear function:

```
y = 2x + 1
```

For any given x value, we solve the expression with that x to find y. When x = 1, then y = 3. When x = 2, y = 5. When x = 3, y = 7 and so on as shown in Table 1-1.

### Table 1-1. Differential use theory = 2x + 1

| x | 2x + 1 | y |
|---|---|---|
| 0 | 2(0) + 1 | 1 |
| 1 | 2(1) + 1 | 3 |
| 2 | 2(2) + 1 | 5 |
| 3 | 2(3) + 1 | 7 |

Functions are useful because they model a predictable relationship between variables, such as how many fires y can we expect at x temperature. We will use linear functions to perform linear regressions in Chapter 5.

Another convention you may see for the dependent variable y is to explicitly label it a function of x, such as f(x). So rather than express a function as y = 2x + 1 we can also express it as:

f(x) = 2x + 1

### Example 1-6. Declaring a linear function in Python

```
def f(x):
    return 2 * x + 1

x_values = [0, 1, 2, 3]

for x in x_values:
    y = f(x)
    print(y)
```

When dealing with real numbers, a subtle but important feature of functions is they often have an infinite number of x values and resulting y values. Ask yourself this: how many x values can we put through the function $y = 2x + 1$? Rather than just 0, 1, 2, 3... why not 0, 0.5, 1, 1.5, 2, 2.5, 3 as shown in Table 1-2 below?

### Table 1-2. Differential equation for y = 2x + 1

| x | 2x + 1 | y |
|---|---|---|
| 0.0 | 2(0) + 1 | 1 |
| 0.5 | 2(.5) + 1 | 2 |
| 1.0 | 2(1) + 1 | 3 |
| 1.5 | 2(1.5) + 1 | 4 |
| 2.0 | 2(2) + 1 | 5 |
| 2.5 | 2(2.5) + 1 | 6 |
| 3.0 | 2(3) + 1 | 7 |

Or why not do quarter steps for \(x\)? \(\frac{1}{10}\) of a step? We can make these steps infinitely small effectively showing \(y = 2x + 1\) is a **continuous function**, where for every possible value of \(x\) there is a value for \(y\). This segues us nicely to visualize our function as a line as shown in Figure 1-1.

![](img/8e0dcc1636d032e7728a07e97751e446_20_0.png)

Figure 1-1. Graph for function y = 2x + 1

When we plot on a two-dimensional plane with two number lines (one for each variable) it is known as a **Cartesian plane**, x-y plane, or **coordinate plane**. We trace a given x value and then look up the corresponding y value, and plot the intersections as a line. Notice that due to the nature of real numbers (or decimals, if you prefer), there are an infinite number of x values. This is why when we plot the function f(x) we get a continuous line with no breaks in it. There are an infinite number of points on that line, or any part of that line.

If you want to plot this using Python, there are a number of charting libraries from Plotly to matplotlib. Throughout this book we will use SymPy to do many tasks, and the first we will use is plotting a function. SymPy uses matplotlib so make sure you have that package installed. Otherwise it will print an ugly text-based graph to your console. After that, just declare the x variable to SymPy using symbols(), declare your function, and then plot it as shown in Example 1-7 and Figure 1-2.

### Example 1-7. Charting a linear function in Python using SymPy

```python
from sympy import *

x = symbols('x')
f = 2*x + 1
plot(f)
```

![](img/8e0dcc1636d032e7728a07e97751e446_22_0.png)

Figure 1-2. Using SymPy to graph a linear function

Example 1-8 and Figure 1-3 are another example showing the function f(x) = x^2 + 1.

### Example 1-8. Charting a linear function in Python using SymPy

```
from sympy import *
x = symbols('x')
f = x**2 + 1
plot(f)
```

![](img/8e0dcc1636d032e7728a07e97751e446_23_0.png)

Figure 1-3. Using SymPy to graph an exponential function

Notice in Figure 1-3 we do not get a straight line but rather a smooth, symmetrical curve known as a parabola. It is continuous but not linear, as it does not produce values in a straight line. Curvy functions like this are mathematically harder to work with, but we will learn some tricks to make it not so bad.

> **CURVILINEAR FUNCTIONS**
> 
> When a function is continuous but curvy, rather than linear and straight, we call it a curvilinear function.

Note that functions can utilize multiple input variables and not just one. For example, we can have a function with independent variables x and y. Note that y is not dependent like in previous examples.

\(f(x, y) = 2x + 3y\)

Since we have 2 independent variables (x and y) and 1 dependent variable (the output of f(x, y)) we need to plot this graph on 3 dimensions to produce a plane of values rather than a line as shown in Example 1-9 and Figure 1-4.

### Example 1-9. Declaring a function with 2 independent variables in Python:

```
from sympy import *
from sympy.plotting import plot3d

x, y = symbols('x y')
f = 2*x + 3*y
plot3d(f)
```

![](img/8e0dcc1636d032e7728a07e97751e446_24_0.png)

Figure 1-4. Using SymPy to graph a 3-dimensional function

No matter how many independent variables you have, your function will typically only output one dependent variable. When you solve for multiple dependent variables, you will likely be using separate functions for each one.

## Summations

I promised not to use equations full of Greek symbols in this book. However, there is one that is so common and useful that it would be remiss to not cover it. A **summation** is expressed as a sigma \(\Sigma\) and adds elements together.

For example, if I want to iterate the numbers 1 through 5, multiply each by 2, and sum them here is how I would express that using a summation. Example 1-10 shows how to execute this in Python.

![](img/8e0dcc1636d032e7728a07e97751e446_25_0.png)

### Example 1-10. Performing a summation in Python

```
summation = sum(2*i for i in range(1,6))
print(summation)
```

Note that i is a placeholder variable representing each consecutive index value we are iterating in the loop, which we multiply by 2 and then sum all together. When you are iterating data, you may see variables like x_i indicating an element in a collection at index i.

### THE RANGE() FUNCTION

Recall that the `range()` function in Python is end exclusive, meaning if you invoke `range(1, 4)` it will iterate the numbers 1, 2, and 3. It excludes the 4 as an upper boundary.

It is also common to see $n$ represent the number of items in a collection, like the number of records in a dataset. Here is one such example where we iterate a collection of numbers of size $n$, multiply each one by 10, and sum them.

![](img/8e0dcc1636d032e7728a07e97751e446_26_0.png)

In Example 1-11 we use Python to execute this expression on a collection of 4 numbers. Note that in Python (and most programming languages in general) we typically reference items starting at index 0, while in math we start at index 1. Therefore we shift accordingly in our iteration by starting at 0 in our `range()`.

### Example 1-11. Summation of elements in Python

```
x = [1, 4, 6, 2]
n = len(x)

summation = sum(10*x[i] for i in range(0,n))
print(summation)
```

That is the gist of summation. In a nutshell, a summation $\Sigma$ says “add a bunch of things together” and uses an index $i$ and a maximum value $n$ to express each iteration feeding into the sum. We will see them throughout this book.
