

degrees at the coordinate (1,0), as shown in Figure 13.15. The bug continues until it’s made a complete trip.

![](img/6e45221a8147f2442c653a3d4a866570_826_0.png)

**Figure 13.15.** “Bug on a circle” about 42 degrees

Each point on the circle corresponds to an angle, which we call _theta_. For example, the point on the circle that is 90 degrees counterclockwise from the starting point has a value of 90 degrees—or rather, the equivalent in radians (pi / 2). Figure 13.15 shows the bug having traveled about 42 degrees (roughly equal to 0.733 radians).

At each point on the circle, the X coordinate of the bug’s position is given by

```
cosine(theta)
```

Likewise, the Y coordinate of the bug’s position is given by

```
sine(theta)
```

By tracking the bug’s journey, we get a set of points corresponding to a trip around the circle. Each point on this journey corresponds to the following (x, y) coordinates:

Click here to view code image

```
(cosine(theta), sine(theta))
```

Therefore, to graph a complete circle, we get a set of points corresponding to many angles on this imaginary trip, from 0 to 2 * pi (equal to 360 degrees). Then we graph the resulting (x, y) pairs. And we’ll get 1,000 data points to get a nice, smooth curve.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
theta = np.linspace(0, 2 * np.pi, 1000)
plt.plot(np.cos(theta), np.sin(theta))
plt.show()
```

If you run these statements, they should draw a circle, but the result looks more like an ellipse. The solution is to specify a plt.axis setting that forces X and Y units to be equally spaced on the screen.

```
plt.axis('equal')
```

Now, let's plug this statement into the complete application, including the **import** statements.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
theta = np.linspace(0, 2 * np.pi, 1000)
plt.plot(np.cos(theta), np.sin(theta))
plt.axis('equal')
plt.show()
```

Now the code produces the perfect circle onscreen, as shown in Figure 13.16.

![](img/6e45221a8147f2442c653a3d4a866570_829_0.png)

**Figure 13.16.** A perfect circle

## 13.8 CREATING PIE CHARTS

The versatility of the **numpy** and **matplotlib** packages extends even to pie charts. This is an effective way of illustrating the relative size of several pieces of data.

The syntax for the `plt.pie` function is shown here. As with other plotting functions, there are other arguments, but only the most important are shown here.

![](img/6e45221a8147f2442c653a3d4a866570_830_0.png)

Click here to view code image

```
plt.pie(array_data, labels=None, colors=None)
```

The first argument, _array_data_, is a collection containing a relative size for each category. The _labels_ argument is a collection of strings that label the corresponding groups referred to in the first argument. The _colors_ argument is a collection of strings specifying color, using the values listed earlier in Table 13.2. And all the collections must have the same length.

This is a simple function to use once you see an example. Suppose you have data on the off-hours activities of your development team, and you want to see a chart. Table 13.4 summarizes the data to be charted in this example.

### Table 13.4. Weekly Activity of Development Team

| Activity | Hours per week (average) | Color |
|---|---|---|
| Poker | 3.7 | black ('k') |
| Chess | 2.5 | green ('g') |
| Comic books | 1.9 | red ('r') |
| Exercise | 0.5 | cyan ('c') |

It’s an easy matter to place each column of data into its own list. Each list has exactly four members in this case.

Click here to view code image

```
A_data  = [3.7, 2.5, 1.9, 0.5]
A_labels = ['Poker', 'Chess', 'Comic Books',
            'Exercise']
A_colors = ['k', 'g', 'r', 'c']
```

Now we plug these figures in to a pie-chart plot, add a title, and display. The aspect ratio of the pie chart can be fixed using a `plt.axis('equal')` statement, just as we did for the circle; otherwise, the pie will appear as an ellipse rather than a circle.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
plt.pie(A_data, labels=A_labels, colors=A_colors)
plt.title('Relative Hobbies of Dev. Team')
plt.axis('equal')
plt.show()
```

Figure 13.17 shows the resulting pie chart.

# Relative Hobbies of Dev. Team

![](img/6e45221a8147f2442c653a3d4a866570_832_0.png)

**Figure 13.17.** A pie chart

## 13.9 DOING LINEAR ALGEBRA WITH "NUMPY"

Before wrapping up these two chapters on the **numpy** package, we’ll take a look at one of the most useful areas, at least to a mathematician or engineer: linear algebra, which often involves vectors (arrays) and matrixes (multidimensional arrays).

It isn’t necessary to use separate “vector” or “matrix” collection types with **numpy**. You don’t even need to download or import new subpackages. You just apply the appropriate functions.

### 13.9.1 The Dot Product

As these last two chapters have shown, you can multiply arrays with scalars, and arrays with arrays. The requirement in the second case is that two arrays multiplied together must have the same shape. We can summarize this relationship as follows:

```
`(A, B) * (A, B) => (A, B)`
```

You can multiply an array of shape A, B to another array of shape A, B and get a third array of the same shape. But with **numpy**, you can also multiply arrays together by using the dot-product function, `dot`, but it has slightly more complex rules.

![](img/6e45221a8147f2442c653a3d4a866570_833_0.png)

```
`numpy.dot(A, B, out=None)`
```

A and B are two arrays to be combined to form a dot product; the `out` argument, if specified, is an array of the correct shape in which to store the results. The “correct shape” depends on the size of A and B, as explained here.

The dot product of two one-dimensional arrays is simple. The two arrays must have the same length. The action is to multiply each element in A to its corresponding element in B, and then sum those products, producing a single scalar value.

Click here to view code image

```
D. P. = A[0]*B[0] + A[1]*B[1] + ... + A[N-1] * B[N-1]
```

Here’s an example:

Click here to view code image

```
>>> import numpy as np
>>> A = np.ones(5)
>>> B = np.arange(5)
>>> print(A, B)
[1. 1. 1. 1. 1.] [0 1 2 3 4]
>>> np.dot(A, A)
5.0
>>> np.dot(A, B)
10.0
>>> np.dot(B, B)
30
```

You should be able to see that the dot product of B with B is equal to 30, because that product is equal to the sum of the squares of its members:

Click here to view code image

```
D. P. = 0*0 + 1*1 + 2*2 + 3*3 + 4*4
= 0 + 1 + 4 + 9 + 16
= 30
```

We can generalize this:

```
D. P.(A, A) = sum(A * A)
```

The dot product between a couple of two-dimensional arrays is more complex. As with ordinary multiplication between arrays, the shapes must be compatible. However, they need only match in one of their dimensions. Here is the general pattern that describes how a dot product works with two-dimensional arrays:

```
(A, B) * (B, C) => (A, C)
```

Consider the following 2 × 3 array, combined with a 3 × 2 array, whose dot product is a 2 × 2 array.

Click here to view code image

```
A = np.arange(6).reshape(2, 3)
B = np.arange(6).reshape(3, 2)
C = np.dot(A, B)
print(A, B, sep='\n\n')
print('\nDot product:\n', C)

[[0 1 2]
 [3 4 5]]

[[0 1]
 [2 3]
 [4 5]]

Dot product:
[[10 13]
 [28 40]]
```

Here’s the procedure.

- Multiply each item in the first row of A by each corresponding item in the first column of B. Get the sum (10). This becomes C[0,0].
- Multiply each item in the first row of A by each corresponding item in the second column of B. Get the sum (13). This becomes C[0,1].
- Multiply each item in the second row of A by each corresponding item in the first column of B. Get the sum (28). This becomes C[1,0].
- Multiply each item in the second row of A by each corresponding item in the second column of B. Get the sum (40). This becomes C[1,1].

You can also take the dot product of a one-dimensional array combined with a two-dimensional array. The result is that the array shapes are evaluated as if they had the following shapes:

```
`(1, X) * (X, Y) => (1, Y)`
```

For example, you could take the dot product of [10, 15, 30] and the following array, which we’ll call B:

```
`[[0 1]
[2 3]
[4 5]]`
```

This next statement shows the dot product between a one-dimensional array and the two-dimensional array, B. The resulting dot product has a shape of (1, 2):

Click here to view code image

