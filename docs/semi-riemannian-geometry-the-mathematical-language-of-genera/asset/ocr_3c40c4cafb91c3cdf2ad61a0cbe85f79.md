# Chapter 2

# Matrices and Determinants

In this chapter, we review some of the basic results from the theory of matrices and determinants.

# 2.1 Matrices

Let us denote by Mat_{m×n} the set of m × n matrices (that is, m rows and n columns) with real entries. When m = n, we say that the matrices are square. It is easily shown that with the usual matrix addition and scalar multiplication, Mat_{m×n} is a vector space, and that with the usual matrix multiplication, Mat_{m×m} is a ring.

Let P be a matrix in Mat_{m×n}, with

```
P = [p_j^i] = [ p_1^1  ...  p_n^1 ; ... ; p_1^m  ...  p_n^m ]
```

The transpose of P is the matrix P^T in Mat_{n×m} defined by

```
P^T = [p_i^j] = [ p_1^1  ...  p_1^m ; ... ; p_n^1  ...  p_n^m ]
```

The row matrices of P are

```
[p_1^1 ... p_n^1]   [p_1^2 ... p_n^2]   ...   [p_1^m ... p_n^m]
```