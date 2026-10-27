# 4.2.1 Noise Margin

To filter out all noise is, for practical purposes, neither technically feasible nor cost effective. A next step is to mitigate the effects of unfiltered noise by using a NM having a value sufficiently large to unambiguously declare that a data value is caused by degradation:

$FD_i = CBD_i - NM$

which is Eq. (3.7), and where

$CBD_i = CBD_{FD} + CBD_m + N_n$  (4.1)

where $CBD_m$ and $N_n$ represent any unfiltered variations in the data not related to the degradation of interest and $CBD_{FD}$ represents data related to the degradation of interest, and where the magnitude of NM is defined as the following:

$NM \ge \max\{CBD_m, NM_n, m, n = 1, 2, ...\}$  (4.2)

Of course, any NM you use introduces an offset type of error in the prognostic information; more detail on that is presented in later chapters.

# 4.2.2 Definition of a Degradation-Signature Model

The NM in Eq. (4.2) is used to mitigate unfiltered noise. Ideally, all noise is filtered and there is no need for a noise margin ($NM = 0$). Then we can consider the more general relation:

$FD_i = CBD_i - NM = CBD_i = g(FD_0)f(dP_i, P_0) + C_0$

where, in general, $g(FD_0)$ equals a constant value, and $C_0$ is a constant including noise. We then define $f(dP_i, P_0)$ to represent a degradation-signature model. From this definition,

$f(dP_i, P_0) = FD_i/FD_0 - C_1 \quad (C_1 = C_0/FD_0)$  (4.3)

which is a dimensionless, univariate ratio. Later in this chapter, we shall define a set of degradation-signature models and their characteristic signatures.

# 4.2.3 Feature Data: Nominal Value

An important step is to determine a nominal value, $FD_0$, to use for $FD_i$ in the absence of degradation. One method of determination is to measure and average a number of data samples for a sample population of the prognostic target: that method introduces sampling variation that, as we saw in Chapter 1, is characterized by metrics such as variance and standard deviation. Sampling variation occurs because it is not possible to manufacture prognostic targets that have exactly the same nondegraded value.

Yet another method is to use a nominal value specified by the manufacturer/supplier of the prognostic target – of course, we then have to account for the specified or presumed manufacturing/measurement tolerances. A more dynamic method is to use data samples taken from each instance of the prognostic target, save the applicable sample average or sample mean, and use that value for $FD_0$.

These are just some of the methods available: we need to evaluate, select, and implement whatever methods are necessary to filter, cancel, or otherwise mitigate noise and thus enable us to meet the accuracy and precision requirements for our PHM system.