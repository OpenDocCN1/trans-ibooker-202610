Now let us walk through this algorithm, step by step.

The first step is to select a polynomial variable. This is often denoted with the symbol $\alpha$.

Now you will input the message you wish to encrypt. That message is frequently denoted with a capital M and has a length of mn. The values of mn are defined by the length of M, as you will see in the next steps.

The next step is to convert the message M into a collection of m polynomials. These polynomials are denoted by $p_i$ and are polynomials within some polynomial ring R. Recall the definition of a polynomial ring from Chapter 13: A polynomial ring is a ring formed from the set of polynomials in one or more indeterminates with coefficients in another ring. Recall that we discussed rings in Chapter 1, “Introduction to Essential Linear Algebra.” A ring is a set with two operations, an identity element, and the inverse operation of the first operation. A polynomial is an expression that consists of variables and coefficients. Another term for variable is indeterminate, as it was in the definition of a polynomial ring.

The next step is to compute the Fourier coefficients of each $p_i$. These Fourier coefficients are defined as $a_i$ and are fixed.

For the next step, you perform pointwise multiplication of the Fourier coefficients $p_i$ with the Fourier coefficients of $a_i$, for each i. This will then be followed by using an inverse fast Fourier transform to yield m polynomials $f_n$, each of a degree less than 2n.

For the next step, you will compute the following formula:

```
$f = \sum_{i=1}^m(f_i) mod p \text{ and } \alpha^n + 1$
```

Finally, you convert the f to n log(p) bits and output those bits.

The steps are summarized as follows:

+   STEP 1. Select a polynomial variable.
+   STEP 2. Input the message you wish to encrypt.
+   STEP 3. Convert the messages into a collection of m polynomials.
+   STEP 4. Compute the Fourier coefficients.
+   STEP 5. Perform pointwise multiplication of the Fourier coefficients $p_i$ with the Fourier coefficients of $a_i$.
+   STEP 6. Compute the formula $f = \sum_{i=1}^m (f_i) mod p \text{ and } \alpha^n + 1$.

SWIFFT, unlike many other provably secure algorithms, is considered fast. There are some known attacks on SWIFFT. Two are the generalized birthday attack and the inversion attack. The generalized birthday attack requires $2^{106}$ operations to succeed. Inversion attacks take $2^{445}$ operations. This means that it is impractical to break SWIFFT.

For those readers who would like to delve deeper into SWIFFT, the following link may be helpful: https://www.alnrosen.net/PAPERS/lattices/swifft.pdf.