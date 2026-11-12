
![](img/8e0dcc1636d032e7728a07e97751e446_171_0.png)

Figure 4-6. Adding vectors is commutative

## Scaling Vectors

Scaling is growing or shrinking a vector's length. You can grow/shrink a vector by multiplying or scaling it with a single value, known as a **scalar**. Figure 4-7 is vector \(\vec{v}\) being scaled by a factor of 2, which doubles it.

![](img/8e0dcc1636d032e7728a07e97751e446_172_0.png)

Figure 4-7. Scaling a vector

Mathematically you multiply each element of the vector by the scalar value.

\(\vec{v} = \begin{bmatrix} 3 \\ 2 \end{bmatrix}\)

\(2\vec{v} = 2 \begin{bmatrix} 3 \\ 2 \end{bmatrix} = \begin{bmatrix} 3 \times 2 \\ 2 \times 2 \end{bmatrix} = \begin{bmatrix} 6 \\ 4 \end{bmatrix}\)

Performing this scaling operation in Python is as easy as multiplying a vector by the scalar, as coded in Example 4-6.

## Example 4-6. Scaling a number in Python using NumPy

```
from numpy import array

v = array([3,1])

# scale the vector
scaled_v = 2.0 * v

# display scaled vector
print(scaled_v)  # [6 2]
```

Here in Figure 4-8 is \(\vec{v}\) being scaled down by factor of 0.5, which halves it.

\(\vec{v} = \begin{bmatrix} 3 \\ 1 \end{bmatrix}\)

![](img/8e0dcc1636d032e7728a07e97751e446_174_0.png)

\(0.5\vec{v} = \begin{bmatrix} 3 \\ 1.5 \end{bmatrix}\)

Figure 4-8. Scaling down a vector by half

## MANIPULATING DATA IS MANIPULATING VECTORS

Every data operation can be thought of in terms of vectors, even simple averages.

Take scaling for example. Let’s say we were trying to get the average house value and average square footage for an entire neighborhood. We would add the vectors together to combine their value and square footage respectively, giving us one giant vector containing both total value and total square footage. We then scale down the vector by dividing by the number of houses N, which really is multiplying by 1/N. We now have a vector containing the average house value and average square footage.

An important detail to note here is that scaling a vector does not change its direction, only its magnitude. But there is one slight exception to this rule as visualized in Figure 4-9. When you multiply a vector by a negative number, it flips the direction of the vector as shown below:

![](img/8e0dcc1636d032e7728a07e97751e446_176_0.png)

Figure 4-9. A negative scalar flips the vector direction

When you think about it though, scaling by a negative number has not really changed direction in that it still exists on the same line. This segues us into a key concept called linear dependence.

## Span and Linear Dependence

These two operations, adding two vectors and scaling them, brings about a simple but powerful idea. With these two operations, we can combine two vectors and scale them to create any resulting vector we want. Figure 4-10 shows six examples of taking two vectors \(\vec{v}\) and \(\vec{w}\), and scaling and combining. These vectors \(\vec{v}\) and \(\vec{w}\), fixed in two different directions, can be scaled and added to create ANY new vector \(\vec{v + w}\).

![](img/8e0dcc1636d032e7728a07e97751e446_178_0.png)

![](img/8e0dcc1636d032e7728a07e97751e446_178_1.png)

![](img/8e0dcc1636d032e7728a07e97751e446_178_2.png)

![](img/8e0dcc1636d032e7728a07e97751e446_178_3.png)

![](img/8e0dcc1636d032e7728a07e97751e446_178_4.png)

![](img/8e0dcc1636d032e7728a07e97751e446_178_5.png)

Figure 4-10. Scaling two added vectors allows us to create any new vector

AGAIN, \(\vec{v}\) and \(\vec{w}\) are fixed in direction, except for flipping with negative scalars, but we can use scaling to freely create any vector composed of \(\vec{v + w}\).

This whole space of possible vectors is called **span**, and in most cases our span can create unlimited vectors off those two vectors, simply by scaling and summing them. When we have two vectors in two different directions, they are **linearly independent** and have this unlimited span.

But in what case are we limited in the vectors we can create? Think about it and read on.

What happens when two vectors exist in the same direction, or exist on the same line? The combination of those vectors is also stuck on the same line, limiting our span to just that line. No matter how you scale it, the resulting sum vector is also stuck on that same line. This makes them **linearly dependent** as shown in Figure 4-11.

![](img/8e0dcc1636d032e7728a07e97751e446_180_0.png)

Figure 4-11. Linearly dependent vectors

The span here is stuck on the same line as the two vectors it is made out of. Because the two vectors exist on the same underlying line, we cannot flexibly create any new vector through scaling. Any resulting vector is stuck on that line.

In 3 or more dimensions, when we have a linearly dependent set of vectors we often get stuck on a plane in a smaller number of dimensions. Here is an example of being stuck on a 2-dimensional plane even though we have 3-dimensional vectors as declared in Figure 4-12.

![](img/8e0dcc1636d032e7728a07e97751e446_181_0.png)

Figure 4-12. Linear dependence in 3 dimensions, note our span is limited to a flat plane

Later we will learn a simple tool called the determinant to check for linear dependence, but why do we care whether two vectors are linear dependent or independent? A lot of problems become difficult or unsolvable when they are linearly dependent. For example, when we learn about systems of equations later in this chapter, a linearly dependent set of equations can cause variables to disappear and make the problem unsolvable. But if you have linear independence, that flexibility to create any vector you need from two or more vectors becomes invaluable to solve for a solution!

## Linear Transformations

This concept of adding two vectors with fixed direction, but scaling them to get different combined vectors, is hugely important. This combined vector, except in cases of linear dependence, can point in any direction and have any length we choose. This sets up an intuition for linear transformations where we use a vector to transform another vector in a functional-like manner.

## Basis Vectors

Imagine we have two simple vectors \(\hat{i}\) and \(\hat{j}\) (“i-hat” and “j-hat”). These are known as **basis vectors**, which are used to describe transformations on other vectors. They typically have a length of 1 and point in perpendicular positive directions as visualized in Figure 4-13.

![](img/8e0dcc1636d032e7728a07e97751e446_183_0.png)

Figure 4-13. Basis vectors $\hat{i}$ and $\hat{j}$

Think of the basis vectors as building blocks to build or transform any vector. Our basis vector is expressed in a 2 x 2 matrix, where the first column is $\hat{i}$ and the second column is $\hat{j}$. 

```
$$\hat{i} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$$
\hat{j} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}

basis = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}
```

A **matrix** is a collection of vectors (such as \(\hat{i}, \hat{j}\)), that can have multiple rows and columns, and is a convenient way to package data. We can use \(\hat{i}\) and \(\hat{j}\) to create any vector we want by scaling and adding them. Let's start with each having a length of 1 and showing the resulting vector \(\vec{v}\) in Figure 4-14.

![](img/8e0dcc1636d032e7728a07e97751e446_185_0.png)

Figure 4-14. Creating a vector off of basis vectors

I want vector \(\vec{v}\) to land at \([3, 2]\). What happens to \(\vec{v}\) if we stretch \(\hat{\imath}\) by a factor of 3 and \(\hat{\jmath}\) by a factor of 2? First we scale them individually as shown here:

\(3\hat{\imath} = 3\begin{bmatrix} 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 3 \\ 0 \end{bmatrix}\)

$2\hat{j} = 2 \begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} 0 \\ 2 \end{bmatrix}$

If we stretched space in these two directions, what does this do to $\vec{v}$? Well it is going to stretch with $\hat{i}$ and $\hat{j}$. This is known as a **linear transformation**, where we transform a vector with stretching, squishing, shearing, or rotating by tracking basis vector movements. In this case of Figure 4-15, scaling $\hat{i}$ and $\hat{j}$ has stretched space along with our vector $\vec{v}$. 

![](img/8e0dcc1636d032e7728a07e97751e446_186_0.png)

Figure 4-15. A linear transformation
