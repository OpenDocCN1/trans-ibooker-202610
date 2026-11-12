
### 1.2.1.5 Fields

A field is an algebraic system consisting of a set, an identity element for each operation, two operations, and their respective inverse operations. You can think of a field as a group that has two operations rather than one, and it has an inverse for both of those operations. It is also the case that every field is a ring, but not every ring will necessarily be a field. For example, the set of integers is a ring, but not a field, if you consider the operations of addition and multiplication. The inverse of multiplication, division, won't always yield an integer.

A classic example of a field is the field of rational numbers. Each number can be written as a ratio (i.e., a fraction), such as x/y (x and y could be any integers you like), and the additive inverse is simply -x/y. The multiplicative inverse is just y/x. Fields are often used in cryptography, and you will see them again in Chapter 11, “Current Asymmetric Algorithms,” Chapter 13, “Lattice-Based Cryptography,” and Chapter 15, “Other Approaches to Post-Quantum Cryptography.”

## 1.3 Matrix Math

Before delving into matrix math, you need to understand what a matrix is. A matrix is a rectangular arrangement of numbers in rows and columns. Rows run horizontally, and columns run vertically. The dimensions of a matrix are stated as m x n, where m is the number of rows and n is the number of columns. Here is an example:

```
[ 1  2 ]
[ 2  0 ]
[ 3  1 ]
```

A matrix is just an array that is arranged in columns and rows. Vectors are simply matrices that have one column or one row. The examples in this section focus on 2 x 2 matrices, but a matrix can be of any number of rows and columns; it need not be a square. A vector can be considered a 1 x m matrix. A vector that is vertical is called a column vector, and one that is horizontal is called a row vector. Matrices are usually labeled based on column and row:

```
[ a_ij  a_ij ]
[ a_ij  a_ij ]
```

The letter i represents the row, and the letter j represents the column. A more concrete example is shown here:

```
[ a_11  a_12 ]
[ a_21  a_22 ]
```

This notation is commonly used for matrices including row and column vectors.

There are different types of matrices, the most common of which are as follows:

- Column matrix: A matrix with only one column.
- Row matrix: A matrix with only one row.
- Square matrix: A matrix that has the same number of rows and columns.
- Equal matrices: Two matrices are considered equal if they have the same number of rows and columns (the same dimensions) and all their corresponding elements are exactly the same.
- Zero matrix: Contains all zeros.

Each of these has a role in linear algebra, which you will see as you proceed through the chapter.

### 1.3.1 Matrix Addition and Multiplication

If two matrices are of the same size, then they can be added to each other by simply adding each element together. You start with the first row and first column in the first matrix and add that to the first row and first column of the second matrix thus in the sum matrix. This is shown here:

![](img/49061be7fe11d9f7f3e56114ab15acdc_29_0.png)

Consider the following more concrete example:

![](img/49061be7fe11d9f7f3e56114ab15acdc_29_1.png)

Multiplication, however, is a bit more complicated. You can multiply two matrices only if the number of columns in the first matrix is equal to the number of rows in the second matrix. If that is the case, then you multiply each element in the first row of the first matrix by each element in the second matrix's first column. Then you multiply each element of the second row of the first matrix by each element of the second matrix's second column. Let's first examine this using variables rather than actual numbers. The example also uses square matrices to make the situation even simpler.

```
[ a b ]   [ e f ]
[ c d ]   [ g h ]
```

This is multiplied in the following manner:

```
a * e + b * g   (a11 * b11 + a12 * b21)
a * f + b * h   (a11 * b12 + a12 * b22)
c * e + d * g   (a11 * b11 + a12 * b21)
c * f + d * h   (a11 * b11 + a12 * b21)
```

Thus, the product will be

```
(a*e + b*g)   (a*f + b*h)
(c*e + d*g)   (c*f + d*h)
```

It is worthwhile to memorize this process. Now, consider this implemented with a concrete example:

```
[ 1 2 ]   [ 2 2 ]
[ 3 1 ]   [ 1 3 ]
```

We begin with

```
1 * 2 + 2 * 1 = 6
1 * 2 + 2 * 3 = 8
3 * 2 + 1 * 1 = 7
3 * 2 + 1 * 3 = 9
```

The final answer is

```
[ 6 8 ]
[ 7 9 ]
```

Now you can see why, as previously stated, you can multiply two matrices only if the number of columns in the first matrix is equal to the number of rows in the second matrix.

It is important to remember that matrix multiplication, unlike traditional multiplication (with scalar values), is not commutative. Recall that the commutative property states the following: a * b = b * a. If a and b are scalar values, then this is true; however, if they are matrices, this is not the case. For example, consider the matrix multiplication shown in Equation 1.1.

```
\begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 2 & 3 \end{bmatrix} = \begin{bmatrix} 8 & 11 \\ 9 & 13 \end{bmatrix}
```

### EQUATION 1.1 Matrix Multiplication

Now if you simply reverse the order, you can see that an entirely different answer is produced, as shown in Equation 1.2.

```
\begin{bmatrix} 1 & 1 \\ 2 & 3 \end{bmatrix} \begin{bmatrix} 2 & 3 \\ 1 & 4 \end{bmatrix} = \begin{bmatrix} 3 & 7 \\ 7 & 15 \end{bmatrix}
```

### EQUATION 1.2 Matrix Multiplication Is Not Commutative

This example illustrates the rather important fact that matrix multiplication is not commutative.

### 1.3.2 Matrix Transposition

Matrix transposition simply reverses the order of rows and columns. While the focus so far has been on 2 x 2 matrices, the transposition operation is most easily seen with a matrix that has a different number of rows and columns. Consider the matrix shown in Equation 1.3.

```
\begin{bmatrix} 2 & 3 & 2 \\ 1 & 4 & 3 \end{bmatrix}
```

### EQUATION 1.3 3 x 2 Matrix

To transpose it, the rows and columns are switched, creating a 2 x 3 matrix. The first row is now the first column. You can see this in Equation 1.4.

```
\begin{bmatrix} 2 & 1 \\ 3 & 4 \\ 2 & 3 \end{bmatrix}
```

### EQUATION 1.4 Matrix Transposed

If you label the first matrix A, then the transposition of that matrix is labeled A^T. Continuing with the original matrix being labeled A, a few properties of matrices need to be described, as outlined in Table 1.2.

### TABLE 1.2 Basic Properties of a Matrix

| Property | Explanation |
|---|---|
| \((A^T)^T = A\) | If you transpose the transposition of A, you get back to A. |
| \((cA)^T = cA^T\) | The transposition of a constant, c, multiplied by an array, A, is equal to multiplying the constant c by the transposition of A. |
| \((AB)^T = B^T A^T\) | Multiplying A by B and then transposing the product is equal to B transposed multiplied by A transposed. |
| \((A + B)^T = A^T + B^T\) | Adding the matrix A and the matrix B and then transposing the sum is equal to first transposing A and B and then adding those transpositions. |
| \(A^T = A\) | If a square matrix is equal to its transpose, it is called a symmetric matrix. |

Table 1.2 is not exhaustive; rather, it is a list of some of the most common properties regarding matrices. These properties are not generally particularly difficult to understand; however, there is an issue with why you would apply them. What do they mean? All too often, introductory linear algebra texts focus so intensely on helping a student to learn how to do linear algebra that the meaning behind operations is lost. Let us take just a moment to explore what transpositions are. A transposition is rotating about the diagonal. Remember that matrices can be viewed graphically. Consider a simple row matrix:

```
[1 2 4]
```

Transposing that row matrix creates a column matrix:

```
[ 1 ]
[ 2 ]
[ 4 ]
```

Think of a vector as a line in some space. For now, we will limit ourselves to 2D and 3D space. You can then think of a matrix as a transformation on a line or set of lines.

### 1.3.3 Submatrix

A _submatrix_ is any portion of a matrix that remains after deleting any number of rows or columns. Consider the \(5 \times 5\) matrix shown in Equation 1.5.

```
[ 2 2 4 5 3 ]
[ 3 8 0 2 1 ]
[ 2 3 2 2 1 ]
[ 4 3 1 2 4 ]
[ 1 2 2 0 3 ]
```

### EQUATION 1.5 5 x 5 Matrix
