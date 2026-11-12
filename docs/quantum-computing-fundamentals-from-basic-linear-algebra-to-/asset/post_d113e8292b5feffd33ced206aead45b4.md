
base field. This is often called the cardinality of the vector space V. A set of vectors B form a basis in a vector space V, if every vector in V can be written as a finite linear combination of the vectors in B. Then B is the set of basis vectors for the vector space V. It is certainly possible that a vector space V can have more than one basis set B. However, all such bases must have the same number of elements, as that is the dimension of the vector space V.

Another concept from linear algebra that is used frequently in lattice-based cryptography is orthogonality. Two elements in a vector space (u and v) are said to be orthogonal if their dot product is zero. A dot product is the sum of the products of the corresponding elements of the two vectors. Essentially, the dot product is used to produce a single number, a scalar, from two vectors or two matrices. This is contrasted with the tensor product. In math, a tensor is an object with multiple indices, such as a vertex or array. The tensor product of two vector spaces V and W, V ⊗ W, is also a vector space.

Unimodular matrices are also used in some lattice-based algorithms. A unimodular matrix is a square matrix of integers with a determinant of +1 or -1. A determinant is a value that is computed from the elements of a square matrix. The determinant of a matrix A is denoted by |A|. Here is an example of a unimodular matrix:

```
[ 2 3 2 ]
[ 4 2 3 ]
[ 9 6 7 ]
```

Recall from Chapter 1 that we take three submatrix determinants to get the determinant of this matrix:

```
2[ 2 3 ] -3[ 4 3 ] +2[ 4 2 ]
    [ 6 7 ]   [ 9 7 ]   [ 9 6 ]
```

Recall that each determinant is computed by ad – bc. Thus, we have

```
2[(2 * 7) – (3 * 6)] – 3 [(4 * 7) – (3 * 9)] + 2[(4 * 6) – (2 * 9)]
```

which in turn gives us

```
2[14 – 18] – 3[28 – 27] + 2[24 – 18]
```

which is

```
-8 – (3) + 12
```

```
= 1
```

Cyclic lattices are also used in some cryptographic applications. A cyclic lattice is a lattice that is closed under a rotational shift operator. Equation 13.2 provides a more rigorous definition.

```
A Lattice L ⊆ Zⁿ is cyclic if ∀x ∈ L: rot(x) ∈ L
```

### EQUATION 13.2 Definition of Cyclic Lattice

### 13.1.1 Shortest Integer Problem

The Shortest Integer Problem, sometimes called Short Integer Solution (SIS), is as follows: Given an m x n lattice A, which is composed of m uniformly random vectors (which are integers), also stated as A ∈ z_q^{n x m}, find a nonzero short integer vector v such that Ax = 0 mod q. This problem forms the basis for Ajtai cryptographic primitive. There are other ways of stating this problem that you will find in some of the literature; however, this form is used here because it is the clearest.

### 13.1.2 Shortest Vector Problem

The Shortest Vector Problem (SVP) is frequently used as a basis for lattice-based cryptographic systems. This problem is, given a particular lattice and a norm N for a lattice L, find the shortest nonzero vector in V as measured by N in L. Put more formally, the SVP is to find the shortest vector in the vector space V, as measured by a norm, N. Remember that a norm is a mathematical function that assigns a positive integer to each vector in a vector space, which is the vector's length or size. The shortest vector must be a nonzero vector.

Put even more formally, given lattice L, vector v, and norm n, then

```
N(v) = λL
```

where λL denotes the length of the shortest nonzero vector in the lattice L.

There are several different approaches to solving this problem, none of which solve the problem in a time that is of practical use, even with a quantum computer. This means that the SVP is a good foundation for lattice-based cryptography. Later in this chapter, we will review the methods for solving lattice-based problems. Keep in mind that the lattices used are so large that one cannot simply look and find the solution to this or other lattice-based problems.

A variation of the Shortest Vector Problem is the Gap Shortest Vector Problem (GapSVP). This problem begins with a fixed function of the dimension of the lattice N. We will refer to this function as β, which is a fixed function of the dimension of the lattice L. Given a basis for the lattice, the algorithm is attempting to determine if λ(L) ≤ 1 or λ > β.

### 13.1.3 Closest Vector Problem

Another mathematical problem used in lattice-based cryptography is the Closest Vector Problem (CVP). This problem is, given a particular vector space V, along with a metric M for a lattice L and a vector v that is in the vector space V, but not necessarily in the lattice L, how does one find the vector in the lattice L that is closest to the vector v? This problem is related to the previously discussed Shortest Vector Problem and is also computationally hard to solve. In fact, the CVP is a generalization of the SVP.

Given the relationship of the CVP to the SVP, you might assume there is a GapCVP, much as there is a GapSVP. If you assume that, you are indeed correct. With GapSVP, the input consists of a lattice basis and a vector v, and the algorithm must determine if one of the following is correct:

- There is a lattice vector w such that the distance between w and v is at most 1.
- Every vector in the lattice is at a distance greater than β from v.

## 13.2 Cryptographic Algorithms

Lattice problems are effective for forming cryptographic algorithms. In 1996, the first lattice-based cryptographic algorithm was published. This was created by Milos Ajtai and based on the Short Integer Solution (SIS). This algorithm has been shown to have various vulnerabilities, so it is not actually used for cryptography today. However, it was the first demonstration of the application of lattices to cryptography and thus is usually mentioned in any discussion of lattice-based cryptography. In this section, we will review the three most widely used lattice-based cryptographic algorithms.

### 13.2.1 NTRU

NTRU (N-th degree Truncated polynomial Ring Units) was first publicly described in 1996 by Jeffrey Hoffstein, Jill Pipher, and Joseph Silverman. There have also been additional variants of this algorithm developed since its initial publication. NTRU can best be defined as a group of related cryptographic algorithms. This group of algorithms has been one of the most studied lattice-based cryptosystems. It is a very important group of algorithms, particularly due to the fact that two variations of NTRU have made it past round two of the NIST project to find a quantum-resistant cryptography standard.

NTRU is based on the Shortest Vector Problem in a lattice. The security of NTRU is predicated on the computational difficulty of factoring certain polynomials in a given truncated polynomial ring. This requires that we briefly explore the concept of polynomial rings, for those readers who might not be familiar with them.

A polynomial ring is a ring formed from the set of polynomials in one or more indeterminates (i.e., variables) with coefficients in another ring. Recall the discussion of rings in Chapter 1. A ring is a set with two operations, an identity element, and the inverse operation of the first operation. A polynomial is an expression that consists of variables and coefficients. Another term for the variable is indeterminate, as it was in the definition of a polynomial ring.

Consider, as an example of a polynomial ring, the ring R[X]. This is a ring in X over a field r with the coefficients of p. This can be defined as follows:

```
p = p₀ + p₁X + p₂X² + ... + pₙ₋₁Xⁿ⁻¹ + pₙXⁿ
```

A truncated polynomial ring is a ring made up of polynomials only up to some set degree. Essentially, the degree of the polynomial is limited or truncated.

For those readers who would like a more rigorous mathematical treatment of polynomial rings, I recommend the following resources:

https://people.seas.harvard.edu/~saili/am106/fall09/lec16.pdf

http://www.math.umd.edu/~immortal/MATH403/lecturenotes/ch16.pdf

Now with that preliminary mathematical information covered, we can return to the NTRU algorithm. NTRU specifically utilizes the truncated polynomial ring shown in Equation 13.3.

$R = Z[x]/(x^N - 1)$

### EQUATION 13.3 NTRU's Truncated Polynomial Ring

Z is the set of integers, with X being an element of the set of integers.

N is understood to be prime. The key generation process begins with the selection of N, followed by the selection of p and q that must satisfy the criteria that gcd(p,q) = 1. q is often some power of 2, and p is usually relatively small.

The next step is the generation of two polynomials, usually labeled f and g, each having a degree at most N–1. The two polynomials have coefficients in {–1,0,1} (e.g., –x^3, –x^2+x–1). The polynomial f must also have inverses for both modulo p and modulo q. If f does not have inverses both for modulo p and q, then a different f value must be selected. The next step is to actually calculate the inverse of f mod p and f mod q. These are usually designated as f_p and f_q, respectively. The public key is generated according to Equation 13.4.

$h = pf * g (mod q)$

### EQUATION 13.4 NTRU Key Generation, Step 2

The h is the public key. The polynomials f, fp, and g are the private key. Now that we have a key, how do we apply it to encryption and decryption? We will use the traditional Alice and Bob for this discussion. Assume some message m that is in the form of a polynomial. We already have h and q. Alice wants to encrypt the message m and send it to Bob. Alice now chooses some random polynomial r, usually with small coefficients. Now to encrypt, Alice performs the equation shown in Equation 13.5.

$e = r * h + m (mod q)$

### EQUATION 13.5 NTRU Key Generation, Step 3

When Bob receives this message, he will need to decrypt it. Bob takes the encrypted message e and uses Equation 13.6 to decrypt.

$a = f * e (mod q)$

### EQUATION 13.6 NTRU Key Generation, Step 4
