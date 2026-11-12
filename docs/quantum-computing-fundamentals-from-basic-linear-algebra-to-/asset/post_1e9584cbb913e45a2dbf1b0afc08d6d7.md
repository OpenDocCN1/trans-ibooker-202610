
Suppose you remove the second column and second row, as shown in Equation 1.6.

| 2 | 1 | 4 | 5 | 3 |
| --- | --- | --- | --- | --- |
| 3 | 0 | 0 | 2 | 1 |
| 2 | 1 | 2 | 2 | 1 |
| 4 | 1 | 1 | 2 | 4 |
| 1 | 1 | 2 | 0 | 3 |

### EQUATION 1.6 Removing Rows and Columns

You are now left with the matrix shown in Equation 1.7.

| 2 | 4 | 5 | 3 |
| --- | --- | --- | --- |
| 2 | 2 | 2 | 1 |
| 4 | 1 | 2 | 4 |
| 1 | 2 | 0 | 3 |

### EQUATION 1.7 Submatrix

The matrix shown in Equation 1.7 is a submatrix of the original matrix.

## 1.3.4 Identity Matrix

An identity matrix is actually rather simple. Think back to the identity property of groups. An identity matrix accomplishes the same goal: multiplying a matrix by its identity matrix leaves it unchanged. To create an identity matrix, just have all the elements along the main diagonal set to 1 and the rest to 0. Consider the following matrix:

| 3 | 2 | 1 |
| --- | --- | --- |
| 1 | 1 | 2 |
| 3 | 0 | 3 |

Now consider the identity matrix. It must have the same number of columns and rows, with its main diagonal set to all 1s and the rest of the elements all 0s. The identity matrix looks like this:

| 1 | 0 | 0 |
| --- | --- | --- |
| 0 | 1 | 0 |
| 0 | 0 | 1 |

If you multiply the original matrix by the identity matrix, the product will be the original matrix. You can see this in Equation 1.8.

![](img/49061be7fe11d9f7f3e56114ab15acdc_34_0.png)

### EQUATION 1.8 Matrix Multiplication

The examples so far have focused not only on square, 2 × 2 or 3 × 3, matrices, but also on matrices that only consist of integers. A matrix can consist of rational numbers, real numbers, and even complex numbers; however, because the goal was just to give you an introduction to the concepts of matrix algebra, those nuances have been omitted.

One application of matrix algebra is with linear transformations. A linear transformation is sometimes called a linear mapping or even just a linear function. It is essentially just some mapping between two vector spaces that has the operations of addition and scalar multiplication.

> **Note**
> 
> Wolfram MathWorld defines a vector space as follows: “A vector space V is a set that is closed under finite vector addition and scalar multiplication.” Another way of putting this is, a vector space is a collection of objects (in our case, integers) called vectors, which may be added together and multiplied (“scaled”) by numbers, called scalars.

## 1.3.5 Deeper Into the Matrix

After the preceding section, you should be basically comfortable with the fundamental concepts and operations on matrices. If you are not, then please review before continuing into this section. Now we will explore a few more concepts that you will need later in this book.

We will start with something easy—the length of a vector, which is computed using the Pythagorean theorem:

||vector|| = √(x² + y²)

Consider the vector:

[2, 3, 4]

Its length is

√(2² + 3² + 4²) = 5.38

This is simple but will be quite important as we move forward. Now we will add just a bit more detail to this concept. The nonnegative length is called the norm of the vector. Given a vector v, this is written as ||v||. This will be important later on in this book. One more concept to remember on lengths/norms: if the length is 1, then this is called a unit vector.

Next, we will turn our attention to another relatively easy computation—the determinant of a matrix.

The determinant of a matrix A is denoted by |A|. An example of a determinant in a generic form follows:

|A|[[a b],[c d]] = ad - bc

A more concrete example might help elucidate this concept:

|A|[[2 3],[1 2]] = (2)(2) - (3)(1) = 1

A determinant is a value that is computed from the individual elements of a square matrix. It provides a single number, also known as a scalar value. Only a square matrix can have a determinant. The calculation for a 2 x 2 matrix is simple enough; we will explore more complex matrices in just a moment. However, what does this single scalar value mean? There are many things one can do with a determinant, most of which we won't use in this text. It can be useful in solving linear equations (i.e., changing variables in integrals—yes, linear algebra and calculus go hand in hand); however, what is immediately useable for us is that if the determinant is nonzero, then the matrix is invertible. This will be important later in this book when we discuss various quantum algorithms.

What about a 3 x 3 matrix, such as that shown in Equation 1.9?

| a1 | b1 | c1 |
| --- | --- | --- |
| a2 | b2 | c2 |
| a3 | b3 | c3 |

### EQUATION 1.9 3 x 3 Matrix

This calculation is substantially more complex. There are a few methods to do this. We will use one called “expansion by minors.” This method depends on breaking the 3 x 3 matrix into 2 x 2 matrices. The 2 x 2 matrix formed by b2, c2, b3, c3, shown in Equation 1.10, is the first.

[[a1 b1 c1],[a2 [b2 c2]],[a3 [b3 c3]]]

### EQUATION 1.10 3 x 3 Matrix Determinant, Part 1

This one was rather simple, as it fits neatly into a contiguous 2 x 2 matrix. However, to find the next one, we have a bit different selection, as shown in Equation 1.11.

![](img/49061be7fe11d9f7f3e56114ab15acdc_36_0.png)

### EQUATION 1.11 3 x 3 Matrix Determinant, Part 2

The next step is to get the lower-left corner square matrix, as shown in Equation 1.12.

![](img/49061be7fe11d9f7f3e56114ab15acdc_36_1.png)

### EQUATION 1.12 3 x 3 Matrix Determinant, Part 3

As with the first one, this one forms a very nice 2 x 2 matrix. Now what shall we do with these 2 x 2 matrices? The formula is actually quite simple and is shown in the Equation 1.13. Note that “det” is simply shorthand for determinant.

![](img/49061be7fe11d9f7f3e56114ab15acdc_36_2.png)

### EQUATION 1.13 3 x 3 Matrix Determinant, Part 4

We take the first column and multiply it by its cofactors, and with a bit of simple addition and subtraction, we arrive at the determinant for a 3 x 3 matrix. A more concrete example might be useful. Let us calculate the determinant for this matrix:

![](img/49061be7fe11d9f7f3e56114ab15acdc_36_3.png)

This leads to

3 * det [ 1 2 ; 0 3 ] = 3 * ((1 * 3) - (2 * 0)) = 3(3) = 9

2 * det [ 1 2 ; 3 3 ] = 2 * ((1 * 3) - (2 * 3)) = 2(-3) = -6

1 * det [ 1 1 ; 3 0 ] = 1 * ((1 * 0) - (1 * 3)) = 1(-3) = -3

And that leads us to 9 - (-6) + (-3) = 12.

Yes, that might seem a bit cumbersome, but the calculations are not overly difficult. We will end our exploration of determinants at 3 x 3 matrices, but, yes, one can take the determinant of larger square matrices. One can calculate determinants for matrices that are 4 x 4, 5 x 5, and as large as you like. However, the goal in this chapter is to give you a general foundation in linear algebra, not to explore every nuance of linear algebra.

As you move forward in this book, a great deal of the focus will be on vectors. A vector is simply a matrix that is either a single column or a single row. This brings us to some interesting properties of vectors. Like the determinant for square matrices, these properties give us information. Let's begin with the dot product. The dot product of two vectors is simply the two vectors multiplied. Consider vectors X and Y. Equation 1.14 shows what the dot product would be.

∑ XᵢYᵢ

### EQUATION 1.14 Dot Product

Examining a concrete example to see how this works should be helpful. Consider two column vectors:

| 1 | 3 |
| --- | --- |
| 2 | 2 |
| 1 | 1 |

The dot product is found as follows: (1 * 3) + (2 * 2) + (1 * 1) = 8.

That is certainly an easy calculation to perform, but what does it mean? Put more frankly, why should you care what the dot product is? Recall that vectors can also be described graphically. You can use the dot product, along with the length of the vectors, to find the angle between the two vectors. We already know the dot product is 8. Recall that length:

||vector|| = √(x² + y²)

Thus, the length of vector X is √(1² + 2² + 1²) = 2.45.

The length of vector Y is √(3² + 2² + 1²) = 3.74.

Now we can easily calculate the angle. It turns out that the cos θ = dot product / length of X * length of Y, or the following:

cos θ = 8 / (2.45)(3.74) = 0.87307
