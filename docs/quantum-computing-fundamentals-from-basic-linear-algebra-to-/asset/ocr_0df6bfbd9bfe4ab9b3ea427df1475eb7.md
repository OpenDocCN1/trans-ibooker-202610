Given the relationship of the CVP to the SVP, you might assume there is a GapCVP, much as there is a GapSVP. If you assume that, you are indeed correct. With GapSVP, the input consists of a lattice basis and a vector v, and the algorithm must determine if one of the following is correct:

+   - There is a lattice vector w such that the distance between w and v is at most 1.
+   - Every vector in the lattice is at a distance greater than β from v.

# # 13.2 Cryptographic Algorithms

Lattice problems are effective for forming cryptographic algorithms. In 1996, the first lattice-based cryptographic algorithm was published. This was created by Milos Ajtai and based on the Short Integer Solution (SIS). This algorithm has been shown to have various vulnerabilities, so it is not actually used for cryptography today. However, it was the first demonstration of the application of lattices to cryptography and thus is usually mentioned in any discussion of lattice-based cryptography. In this section, we will review the three most widely used lattice-based cryptographic algorithms.

# ## 13.2.1 NTRU

NTRU (N-th degree Truncated polynomial Ring Units) was first publicly described in 1996 by Jeffrey Hoffstein, Jill Pipher, and Joseph Silverman. There have also been additional variants of this algorithm developed since its initial publication. NTRU can best be defined as a group of related cryptographic algorithms. This group of algorithms has been one of the most studied lattice-based cryptosystems. It is a very important group of algorithms, particularly due to the fact that two variations of NTRU have made it past round two of the NIST project to find a quantum-resistant cryptography standard.

NTRU is based on the Shortest Vector Problem in a lattice. The security of NTRU is predicated on the computational difficulty of factoring certain polynomials in a given truncated polynomial ring. This requires that we briefly explore the concept of polynomial rings, for those readers who might not be familiar with them.

A polynomial ring is a ring formed from the set of polynomials in one or more indeterminates (i.e., variables) with coefficients in another ring. Recall the discussion of rings in Chapter 1. A ring is a set with two operations, an identity element, and the inverse operation of the first operation. A polynomial is an expression that consists of variables and coefficients. Another term for the variable is indeterminate, as it was in the definition of a polynomial ring.

Consider, as an example of a polynomial ring, the ring R[X]. This is a ring in X over a field r with the coefficients of p. This can be defined as follows:

```
p = p₀ + p₁X + p₂X² + ... + pₙ₋₁Xⁿ⁻¹ + pₙXⁿ
```

A truncated polynomial ring is a ring made up of polynomials only up to some set degree. Essentially, the degree of the polynomial is limited or truncated.