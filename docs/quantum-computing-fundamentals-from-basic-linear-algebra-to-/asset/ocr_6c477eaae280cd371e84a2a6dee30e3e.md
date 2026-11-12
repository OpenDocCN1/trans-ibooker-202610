![](bbox=[0.066, 0.118, 0.428, 0.185])

# EQUATION 1.8 Matrix Multiplication

The examples so far have focused not only on square, 2 × 2 or 3 × 3, matrices, but also on matrices that only consist of integers. A matrix can consist of rational numbers, real numbers, and even complex numbers; however, because the goal was just to give you an introduction to the concepts of matrix algebra, those nuances have been omitted.

One application of matrix algebra is with linear transformations. A linear transformation is sometimes called a linear mapping or even just a linear function. It is essentially just some mapping between two vector spaces that has the operations of addition and scalar multiplication.

> **Note**
> 
> Wolfram MathWorld defines a vector space as follows: “A vector space V is a set that is closed under finite vector addition and scalar multiplication.” Another way of putting this is, a vector space is a collection of objects (in our case, integers) called vectors, which may be added together and multiplied (“scaled”) by numbers, called scalars.

# 1.3.5 Deeper Into the Matrix

After the preceding section, you should be basically comfortable with the fundamental concepts and operations on matrices. If you are not, then please review before continuing into this section. Now we will explore a few more concepts that you will need later in this book.

We will start with something easy—the length of a vector, which is computed using the Pythagorean theorem:

```
||vector|| = √ x2 + y2
```

Consider the vector:

```
[2, 3, 4]
```

Its length is

```
√2² + 3² + 4² = 5.38
```

This is simple but will be quite important as we move forward. Now we will add just a bit more detail to this concept. The nonnegative length is called the norm of the vector. Given a vector v, this is written as ||v||. This will be important later on in this book. One more concept to remember on lengths/norms: if the length is 1, then this is called a unit vector.