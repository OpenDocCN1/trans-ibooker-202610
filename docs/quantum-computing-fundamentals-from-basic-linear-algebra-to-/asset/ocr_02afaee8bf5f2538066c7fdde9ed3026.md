this chapter. For the casual reader, it is sufficient to understand that these are quantum gates. For the reader wishing a bit more depth, let us first look at the matrices for these gates. These are shown in Equation 8.9 through Equation 8.11.

![](bbox=[0.11, 0.2, 0.48, 0.3])

# EQUATION 8.9 Ising XX Coupling Gate

![](bbox=[0.11, 0.38, 0.44, 0.47])

# EQUATION 8.10 Ising YY Coupling Gate

![](bbox=[0.11, 0.55, 0.38, 0.65])

# EQUATION 8.11 Ising ZZ Coupling Gate

As you can see, these are all 4x4 matrices. To refresh your memory, e is Euler's number, \(\phi\) is the azimuthal angle, and, of course, i is the base imaginary number. The trigonometric function you are certainly familiar with already.

# 8.2.9 Gottesman-Knill Theorem

When discussing quantum gates, we must also discuss the Gottesman–Knill theorem. This was proposed by Daniel Gottesman and Emanuel Knill and states that circuits that consist only of gates from the qubit Pauli group’s normalizer can be perfectly simulated in polynomial time on a classic probabilistic computer. This requires us to define a few terms we have not previously encountered in