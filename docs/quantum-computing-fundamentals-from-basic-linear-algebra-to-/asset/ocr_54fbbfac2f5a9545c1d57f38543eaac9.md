Figure 14.2 shows the general relationship of the elements in the system.

![](bbox=[0.07, 0.15, 0.48, 0.37])

FIGURE 14.2 Matsumoto-Imai cryptosystem

The Matsumoto-Imai cryptosystem, while shown to have security flaws, did pave the way for other multivariate algorithms. As you will see later in this chapter, many of those are robust and could potentially be quantum-resistant algorithms. It is also noteworthy that many algorithms have been created that are variations of Matsumoto-Imai.

# 14.3 Hidden Field Equations

Hidden Field Equations (HFE) is a public key cryptography system that was invented by Jacques Patarin and first publicized at Eurocrypt in 1996. The cryptographic security is based on the difficulty of finding solutions to a system of multivariate quadratic equations, sometimes referred to as the MQ problem. This algorithm was created as an improvement of the Matsumoto-Imai system.

The basis of the mathematics for this particular cryptography algorithm is not overly complex, but a few new concepts will need to be introduced. We begin with a base field we will call Fq. Then we have two extension fields over that base field. Our extension fields are Fq^n and Fq^m. Let us briefly review extension fields. First, keep in mind the definition of a field discussed in Chapter 1 and reviewed in the introduction to this chapter. If we have a field F, then we have a field E ⊆ F, such that the operations of E are those of F, and we can say that E is a subfield of F and that F is an extension field of E.

Let us examine this in slightly different terminology. Consider our previous field E. We also have a field F that contains all of E, plus additional elements. However, the operations in E and F are the same. This would make field F an extension of field E.

First, consider some finite field Fq, with q as a power of two, and some extension field G. The basis of G is B1, B2, ..., Bn.

Next, consider some value h such that h = q^θ + 1 for some value of θ and 0 < h < q^n. And along with h = q^θ + 1, consider the greatest common denominator, gcd (h, q^n - 1) = 1. Put in other terms, h and