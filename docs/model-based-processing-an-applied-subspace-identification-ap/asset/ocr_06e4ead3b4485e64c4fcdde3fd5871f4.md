Note also that this expression can also be written as

Pr(x1, x2) = Pr(x2 | x1) Pr(x1) (A.11)

Substituting this equation into Eq. (A.10) gives Bayes' rule, that is,

Pr(x1 | x2) = Pr(x2 | x1) Pr(x1) / Pr(x2) (A.12)

If we use the definition of joint mass function and substitute into the previous definitions, then we can obtain the probabilistic chain rule [1–3],

Pr(x1, ..., xN) = Pr(x1 | x2, ..., xN) Pr(x2 | x3, ..., xN) ... Pr(xN-1 | xN) Pr(xN) (A.13)

Along with these definitions follows the idea of conditional expectation, that is,

E{xi | xj} = Σi Xi Pr(xi | xj) (A.14)

With the conditional expectation defined, we list some of their basic properties:

+   1) Ex{X|Y} = E{X}, if X and Y are independent
+   2) E{X} = Ey{E[X|Y]}
+   3) Ex{g(y)X|Y} = g(y)E{X|Y}
+   4) Ex,y{g(Y)X} = Ey{g(Y)E{X|Y}}
+   5) Ex{c|Y} = c
+   6) Ex{g(Y)|Y} = g(Y)
+   7) Ex,y{cX + dY|Z} = cE{X|Z} + dE{Y|Z}

The concepts of information and entropy can also be extended to the case of more than one random variable. We define the mutual information between two random variables, xi and xj as

I(xi; xj) = log_b [P_X(xi | xj) / P_X(xi)] (A.15)

and the average mutual information between X(ωi) and X(ωj) as

I(Xi; Xj) = Ex,xj{I(xi, xj)} = Σi Σj P_X(xi, xj) I(xi, xj) (A.16)

which leads to the definition of joint entropy as

H(Xi; Xj) = - Σi Σj P_X(xi, xj) log_b P_X(xi, xj) (A.17)

This completes the section on probability theory. Next let us consider an important multivariable distribution and its properties.