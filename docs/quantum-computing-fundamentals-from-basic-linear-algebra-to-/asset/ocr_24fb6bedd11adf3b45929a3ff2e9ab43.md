with the symbol \(\Phi\) (x). The coefficients are in the field of integers mod q. Multiplication and addition are reduced mod \(\Phi\) (x). The key exchange algorithm can be generally described as follows:

There is some polynomial often expressed as

a(x) = a_0 + a_1x + a_2x^2 + ... a_{n-3}X^{n-3} + a_{n-2}X^{n-2} + a_{n-1}X^{n-1}.

The coefficients of this polynomial are integers mod q. The polynomial itself is a cyclotomic polynomial. For those readers not familiar with cyclotomic polynomials, this topic is a bit more rigorous mathematically. You don't necessarily need to have a full understanding of it to have a general working knowledge of RLWE. However, for the sake of completeness, it will be described here.

The short definition is that cyclotomic polynomials are polynomials whose complex roots are primitive roots of unity. However, that is only helpful if one is familiar with roots of unity, and roots in general. The nth root of unity, where n is a positive integer, is some number x that satisfies the rather simple equation shown in Equation 13.13.

x^n = 1

# EQUATION 13.13 Roots of Unity

This explains roots of unity, and I suspect that is rather simpler than you might have feared. However, it does not explain the primitive roots of unity. A given root of unity, we will call it n, is said to be primitive if it is not the mth root of unity for some smaller m. To put this another way, assume you have found a root of unity (x^n = 1). In order to be considered a primitive root of unity, it must also satisfy the condition shown in Equation 13.14.

x^m \neq 1 for m = 1, 2, 3, 4, ..., x-1.

# EQUATION 13.14 Primitive Root of Unity

Now for those readers seeking an even more rigorous mathematical definition, the nth cyclotomic polynomial is the polynomial given by the formula shown in Equation 13.15.

\(\Phi_n(x) = \prod_{\zeta} (x - \zeta)\)

# EQUATION 13.15 Nth Cyclotomic Polynomial

Do not be overly concerned if this explanation is still rather opaque to you. This is completely normal if you don't have a solid background in number theory. However, you can certainly continue on and understand the gist of RLWE, even with a rather hazy understanding of roots of unity.

Now, continuing with key generation, there is usually an initiator for the key exchange; we will use the symbol I to represent that party. Then there is a respondent we will represent with R. Both parties know the values q, n, and a(x) and can generate small polynomials according to the distribution X\alpha with