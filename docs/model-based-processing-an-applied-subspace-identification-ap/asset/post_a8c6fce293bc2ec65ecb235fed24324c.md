
all of its properties can be decomposed into subregions (layers): first into a topological-metric-space, next into a topological-normalized-metric-space, and finally into a topological-normalized-metric-inner product-space with each subregion (layer) inheriting some of the properties of its predecessor. In identification, subspace refers primarily to a vector (state) space in which its essential characteristics can be captured by a “smaller” space – the subspace. For example, an Nth order state-space dynamic system can be approximated by an Mth order (M < N) system capturing the majority of its properties. This is the concept that we will employ throughout this text.

Subspace identification (SID) is a technique to extract (identify) a black-box model in generic state-space form from uncertain input/output data using robust, numerically stable, linear algebraic methods. SID (simply) performs a black-box identification of a generic state-space model (A, B, C, D) from noisy measurements as illustrated in Figure 1.9. Unlike MBID discussed previously, it does not require any specific structure of the state-space model, only its order (number of states) that can be derived directly from the embedded linear algebraic decomposition. SID methods are constrained to linear state-space models.

More specifically, compared to the MBID processors that iteratively estimate parameters of the previous section [3, 7], we investigate subspace identification techniques [16–18]. These techniques enable the capability to extract a large number of parameters compared to the prediction error (P-E) methods that are quite time-consuming and not practical for large parametric problems coupled with the need for potential real-time applications. Subspace identification techniques are robust and reliable and capable of real-time application,

![](img/4bb66282061cdabd9d492d7740bb0233_43_0.png)

Figure 1.9 Identification of a sinusoidal model from noise-free data: spectral frequencies (identified) at 1 Hz, 2, 5, 10, and 20 Hz.

but they are not as accurate as the optimization-based P-E methods [8]. In fact, the SID methods are typically used as “starters” for P-E processors enabling a reasonable starting point for the numerical optimizers, thereby ensuring faster convergence. In this text, we concentrate on the SID approach, presented in Chapters 6 and 7.

Consider the following example of sinusoids in noise to illustrate this process.

**Example 1.7** Suppose we are given an ideal measurement consisting of five sinusoids at frequencies ranging from 1 to 20 Hz and we would like to develop a model of this process, after performing an identification, that is, our underlying model is simply a sum of sinusoids of varying amplitudes such that

$$y(t)=\sum_{i=1}^{5} A_i\sin 2\pi f_i t \quad \text{with } f_i=\{1,2,5,10,20\}\ \text{Hz}$$  \tag{1.1}

We could simply perform a Fourier transform in this noise-free case and locate the peaks and magnitudes to extract the sinusoids or perform a parametric estimation using a nonlinear (sinusoid) optimization technique yielding a model-based identification. However, another approach is to perform a *black-box* identification choosing a generic model-class (e.g. state-space) that captures these dynamics in the form of a transfer function, estimate the underlying state-space model, and using it, calculate the corresponding impulse response. With this representation, the corresponding frequencies can be extracted from the eigenvalues (poles) of the identified model. The data were processed by an identifier using the proper order (20-states or 2 × No. sinusoids), and the results are shown in Figure 1.9, both in the Fourier (power) spectrum obtained from the simulated noise-free data along with the estimated (identified) frequencies (squares) annotated on the plot. There is perfect agreement. This example illustrates the concept of identification (black-box) and becomes more and more important as the SNR decreases and the spectral peaks are no longer available for detection and location. □

This completes the introduction to subspace identification. Next we discuss then notation that will be employed in this text.

## 1.6 Notation and Terminology

The notation used throughout this text is standard in the literature. Where necessary, vectors are represented by boldface, lowercase, **x**, and matrices by boldface, uppercase, **A**. The *adjoint* of the matrix A is annotated as adj(A) and its corresponding *determinant* by det(A). We define the notation $\mathbb{N}$ to be a shorthand way of writing $1,2,\ldots,N$. It will be used in matrices, $A(N)$ to mean

which is precisely the mean of the discrete PMF(see Appendix A for more details).

Also, as mentioned, we will use the symbol ~ to mean “distributed according to” as in x ~ N(m, v) defining the random variable x as Gaussian distributed with mean m and variance v. We may also use the extended notation: N(x : m, v) to include the random variable x as well. When sampling, we use the non-conventional right arrow “action” notation → to mean “draw a sample from” a particular distribution such as x_i → Pr(x) – this again will be clear from the context. When resampling, that is, replacing samples with new ones, we use the “block” right arrow such as x_j ↔ x_i meaning new sample x_j replaces current sample x_i. In a discrete (finite) probabilistic representation, we define a purely discrete variate as x_k(t) := Pr(x(t) = X_k) meaning that x can only take on values (integers) k from a known set X = {X_1, …, X_k, …, X_N} at time t. We also use the symbol □ to mark the end of an example.

We define two projection operator projection operators: (i) orthogonal and (ii) oblique or parallel. The orthogonal projection operator “projects” • onto Y as P_•|Y or its complement P_•|Y^⊥, while the oblique projection operator “projects” • onto Y “along” Z as P_•|Y||Z. For instance, orthogonally projecting the vector x, we have P_x|Y or P_x|Y^⊥, while obliquely projecting x along Z is P_x|Y||Z (see Appendix B). The matrix projection operator evolves from decompositions such as the LQ-decomposition (see Appendix C), where the operator is given in terms of its orthonormal bases as P = Q × Q′.

## 1.7 Summary

In this chapter, we introduced the concept of model-based signal processing (MBSP) based on the idea of incorporating more and more available a priori information into the processing scheme. Various signals were classified as deterministic or random. When the signal is random, the resulting processors are called estimation filters or estimators. A procedure was defined (estimation procedure) leading to a formal decomposition, which will be used throughout this text. After a couple of examples motivating the concept of model-based signal processing, a more formal discussion followed with a concrete RC-circuit example completing the chapter. Next we introduced the idea of model-based identification (MBID), which is equivalent to a variety of P-E methods, where a model developed from the underlying problem phenomenology is incorporated directly into the processor. After discussing the identification of the RC-circuit parameters, the subspace identification (SID) approach we introduced conceptually and illustrated through a sinusoidal identification example. Finally, the notation utilized throughout the text was discussed completing this introductory chapter.

## MATLAB Notes

MATLAB is a command oriented vector–matrix package with a simple yet effective command language featuring a wide variety of embedded C language constructs making it ideal for signal processing and graphics. All of the algorithms we have applied to the examples and problems in this text are MATLAB-based in solution ranging from simple simulations to complex applications. We will develop these notes primarily as a summary to point out to the reader many of the existing commands that already perform the signal processing operations discussed in the presented chapter and throughout the text.

More specifically, MATLAB consists of a variety of “toolboxes” enabling the users to access a large number of numerically stable and accurate algorithms rapidly along with a corresponding suite of demonstration examples. For this text, the most important toolboxes are the signal processing (signal), system identification (ident), control system (control), and the statistics (stats), which can be used to solve the majority of problems introduced in this text. Throughout we will refer to specific commands that can be applied to a variety of problems and provide meaningful solutions.

## References

- 1. Rossing, T., Dunn, F., Hartmann, W. et al. (2007). *Handbook of Acoustics*. New York: Springer.
- 2. Hayes, M. (1996). *Statistical Digital Signal Processing and Modeling*. New York: Wiley.
- 3. Candy, J. (2006). *Model-Based Signal Processing*. Hoboken, NJ: Wiley/IEEE Press.
- 4. Bucker, H. (1976). Use of calculated sound fields and matched-field detection to locate sound in shallow water. *J. Acoust. Soc. Am.* 59: 329–337.
- 5. Tolstoy, A. (1993). *Matched Field Processing for Ocean Acoustics*. Hackensack, NJ: World Scientific.
- 6. Candy, J. (2016). *Bayesian Signal Processing, Classical, Modern and Particle Filtering*, 2e. Hoboken, NJ: Wiley/IEEE Press.
- 7. Ljung, L. (1999, 2e). *System Identification: Theory for the User*. NJ: Prentice-Hall.
- 8. Ljung, L. and Soderstrom, T. (1983). *Theory and Practice of Recursive Identification*. Cambridge, MA: M.I.T. Press.
- 9. Juang, J. (1994). *Applied System Identification*. Upper Saddle River, NJ: Prentice-Hall PTR.
- 10. Aoki, M. (1990). *State Space Modeling of Time Series*, 2e. London: Springer.
- 11. Sorenson, H. (1986). *Parameter Estimation*. New York: IEEE Press.
