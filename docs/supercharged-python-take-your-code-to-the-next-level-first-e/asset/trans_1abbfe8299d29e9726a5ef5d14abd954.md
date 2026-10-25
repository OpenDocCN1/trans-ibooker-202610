


edges. Print the results. (Hint: There is more than one way to do this, but slicing is particularly efficient.)

**5** Perform a similar operation for a \( 5 \times 5 \times 5 \) “cube,” leaving all visible portions set to 1, while the inner \( 3 \times 3 \times 3 \) cube is zeroed out. Then print the five horizontal planes constituting this cube.

## 13. Advanced Uses of "numpy"

Your introduction to the world of **numpy** has just begun. One of the most interesting things you can do is plot charts, building on top of **numpy** data types and functions. You’ll need to download and import the **matplotlib** package as well as **numpy**.

There are many things you can set, such as color, range, extent, and other factors, but getting started is simple. After covering the plotting capabilities, this chapter examines other advanced uses of the **numpy** package:

- Financial applications
- Linear algebra: dot products and outer products
- Operations on fixed-length records of all kinds
- Reading and writing large amounts of data

### 13.1 ADVANCED MATH OPERATIONS WITH "NUMPY"

Before you start plotting curves, you’ll need to know about the mathematical functions operating on the standard **numpy** **ndarray** type.

Table 13.1 lists the most common of the functions. These are extremely useful in plotting graphs. Most of them take an array as argument and return an array of the same shape.

| Operation | Description |
|---|---|
| `numpy.cos(A)` | Return the cosine of each element in A, as explained in Chapter 11, “The Random and Math Packages.” The input to this function is assumed to be in radians and not degrees. That’s assumed for the other trig functions as well. |
| `numpy.sin(A)` | Return the sine of each element in A. As with `cos`, the results are returned in an array of matching shape. |
| `numpy.tan(A)` | Return the tangent of each element. |
| `numpy.exp(A)` | Return e raised to the power of each element in A. |
| `numpy.power(X, Y)` | Raise each element in X by its corresponding value in Y. These two arrays must have the same shape, or one or both must be scalar. |
| `numpy.deg2rad(A)` | Convert degrees to radians; the argument may be an array of the same shape or may be scalar. |
| `numpy.abs(A)` | Take the absolute value of each element in A. |
| `numpy.log(A)` | Take the natural logarithm of each element in A. |
| `numpy.log10(A)` | Take the logarithm base 10 of each element in A. |
| `numpy.log2(A)` | Take the logarithm base 2 of each element in A. |
| `numpy.sqrt(A)` | Take the square root of each element in A. |
| `numpy.arccos(A)` | Inverse cosine function. |
| `numpy.arcsin(A)` | Inverse sine function. |
| `numpy.arctan(A)` | Inverse tangent function. |
| `numpy.cosh(A)` | Hyperbolic cosine. |
| `numpy.sinh(A)` | Hyperbolic sine. |
| `numpy.tanh(A)` | Hyperbolic tangent. |
| `numpy.append(A, B)` | Create a new array by appending the contents of array B onto the end of array A. |
| `numpy.pi` | Get the value of pi. |
| `numpy.e` | Get the value of e. |

These functions have additional arguments. A common argument is the _out_ argument, which names an output array in which to place the results. Such an array must match the source array in size and shape. Here’s an example:

```python
import numpy as np
A = np.linspace(0, np.pi, 10)
B = np.empty(10)
np.sin(A, out=B)
```

This last line puts the results into B. The following statement would have the same effect, except that every time you call it, you allocate a new array. In some cases, it’s more efficient to use an existing array to store the results.

Click here to view code image

```python
import numpy as np
A = np.linspace(0, np.pi, 10)
B = np.sin(A)
```

To see all the arguments available to be called for each of these functions, use the `np.info` command.

![](img/6e45221a8147f2442c653a3d4a866570_789_0.png)

Click here to view code image

```python
numpy.info(numpy.function_name)
```

For example, the following commands, given from within IDLE, provide manageable chunks of information on specific numpy functions:

```python
import numpy as np
np.info(np.sin)
np.info(np.cos)
np.info(np.power)
```

Most of the functions listed here are designed to operate on a single numpy array. A few functions have variations. The numpy power function takes at least two arguments: X and Y. Either or both can be an array; but if they are both arrays, they must have the same shape. The effect of the function is to raise each X value to a power specified by the corresponding element of Y.

For example, the following statements raise each of the elements in array A to the power of 2 (that is, to square each element).

```python
>>> import numpy as np
>>> A = np.arange(6)
>>> print(A)
[0 1 2 3 4 5]
>>> print(np.power(A, 2))
[ 0 1 4 9 16 25]
```

Other functions are often used in conjunction with the numpy linspace function, which in turn is heavily used in plotting equations, as you’ll see in Section 13.3, “Plotting Lines with 'numpy' and 'matplotlib.'”

For example, the following statements combine the **linspace** function with the **sin** function and the constant, **pi**, to get a series of 10 values that reflects the value of the sine function as its inputs increase from 0 to pi and then decrease back to 0.

Click here to view code image

```python
>>> A = np.linspace(0, np.pi, 10)
>>> B = np.sin(A, dtype='float16')
>>> print(B)
[0.000e+00 3.420e-01 6.431e-01 8.657e-01 9.849e-01
9.849e-01 8.662e-01 6.431e-01 3.416e-01 9.675e-04]
```

In this example, the data type **float16** was chosen so as to make the numbers easier to print. But a still better way to do that is to use some of the formatting techniques from Chapter 5. Now the results are easier to interpret.

Click here to view code image

```python
>>> B = np.sin(A)
>>> print(' '.join(format(x, '5.3f') for x in B))
0.000 0.342 0.643 0.866 0.985 0.985 0.866 0.643
0.342
0.000
```

This small data sample demonstrates the behavior of the sine function. The sine of 0 is 0, but as the inputs increase toward pi/2, the results approach 1.0; then they approach 0 again as the inputs increase toward pi.

## 13.2 DOWNLOADING "MATPLOTLIB"

We can use all this data to plot graphs in Python with the help of **numpy** and another package: **matplotlib**, which has to be downloaded and imported.

The first step in downloading a package is to bring up the DOS Box (Windows) or Terminal application (Macintosh).

As explained in Chapter 12, every Python download should come with the **pip** utility or with **pip3**. Use the **install** command of the **pip** utility. Assuming you’re connected to the Internet, the following command directs the utility to download the **matplotlib** software.

```
pip install matplotlib
```

If you use **pip** at the command line and it is not recognized, try using **pip3** (which is the name of the utility when Python 3 is installed on a Macintosh system).

```
pip3 install matplotlib
```

> **Note**
>
> If neither the **pip** nor **pip3** command worked, check how you spelled **matplotlib**. The spelling is tricky. To see the range of commands possible, type
> ```
> pip help
> ```

## 13.3 PLOTTING LINES WITH "NUMPY" AND "MATPLOTLIB"

Now you’re ready to have some fun. After **matplotlib** is downloaded, import it.

Click here to view code image

```python
import numpy as np
import matplotlib.pyplot as plt
```

You don’t have to give **matplotlib.pyplot** the short name **plt**, but **plt** is widely used and recognized. The full name, **matplotlib.pyplot**, is cumbersome, and most programmers use **plt** by convention.

Two of the major functions used for plotting include **plt.plot** and **plt.show**. The syntax shown here is a simplified view; we’ll show more of it later.

![](img/6e45221a8147f2442c653a3d4a866570_793_0.png)

```python
plt.plot( [X,] Y [,format_str] )
plt.show()
```

In this syntax, square brackets are not intended literally but indicate optional items.

The simplest calls to **plot** usually involve two one-dimensional array arguments: X and Y. Where X is omitted, it’s assumed to be the array [0, 1, 2... N-1], where N is the length of


