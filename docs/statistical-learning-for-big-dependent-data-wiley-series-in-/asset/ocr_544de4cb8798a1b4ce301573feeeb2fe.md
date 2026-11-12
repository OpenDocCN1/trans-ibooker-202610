but this measure depends on both the cross-correlations and the autocorrelations of the two series. As

```
|R_{xy,h}| = |R_{x,h}| \left| R_{y,h} - C_{xy,h} R_{x,h}^{-1} C'_{xy,h} \right|,
```

if the series are unrelated, and C_{xy,h} = 0, then |R_{xy,h}| = |R_{x,h}|/|R_{y,h}| and this value can be far away from one. In fact, this value will be very small if both series have strong autocorrelations. For instance, |R_{x,1}| = 1 - \rho_x^2 will be very small if the first autocorrelation coefficient is close to 1. Second, if the series are identical then |R_{xy,h}| = 0 and TC = 1, but a small value of the determinant, or a large value of TC, does not imply a strong relationship between the series. For instance, if |R_{x,h}| is very small, because there are strong autocorrelations in series x_t, then, by (5.17), |R_{yx,h}| will also be small.

A measure of the average linear dependency between two series x_t and y_t is the generalized cross-correlation measure, GCC(x_t, y_t), defined by Alonso and Peña (2019) as

```
GCC(x_t, y_t) = 1 - \left( \frac{|R_{yx,h}|}{|R_{x,h}| |R_{y,h}|} \right)^{1/(h+1)}
= 1 - \frac{|R_{y,h} - C_{xy,h} R_{x,h}^{-1} C'_{xy,h}|^{1/(h+1)}}{|R_{y,h}|^{1/(h+1)}}.
```

The GCC verifies: (1) GCC(x_t, y_t) = GCC(y_t, x_t) so the measure is symmetric; (2) 0 <= GCC(x_t, y_t) <= 1 for Fischer's inequality (see Lütkepohl, 1996); (3) GCC(x_t, y_t) = 1 if and only if there is a perfect linear dependency among the series; and (4) GCC(x_t, y_t) = 0 if and only if all the cross-correlation coefficients are zero.

To prove (3), as GCC(x_t, y_t) = 1 implies |R_{yx,h}| = 0, because the denominator is bounded, and then there exists at least a row (column) which is a linear combination of the others. To prove (4), write GCC(x_t, y_t) as

```
GCC(x_t, y_t) = 1 - \left| \begin{pmatrix} R_{x,h}^{-1} & 0_{xy,h} \\ 0_{yx,h} & I_{y,h} \end{pmatrix} \begin{pmatrix} R_{x,h} & C_{xy,h} \\ C'_{xy,h} & R_{y,h} \end{pmatrix} \begin{pmatrix} I_{x,h} & 0_{xy,h} \\ 0_{yx,h} & R_{y,h}^{-1} \end{pmatrix} \right|^{1/(h+1)}
= 1 - \left| \begin{pmatrix} I_{x,h} & R_{x,h}^{-1} C_{xy,h} R_{y,h}^{-1} \\ C_{xy,h} & I_{y,h} \end{pmatrix} \right|^{1/(h+1)}.
```

By the Hadamard's inequality, the right-hand side determinant is smaller or equal to 1 and equality is achieved if and only if the matrix is diagonal, that is, if and only if C_{xy,h} = 0_{xy,h}.

Notice that for h = 0 the GCC(x_t, y_t) is just the squared correlation coefficient between the two variables. Also, for any h, when both series are white noise and \rho_{xy}(h) != 0 for some h != 0 and \rho_x(j) = 0 for all j != h, then GCC(x_t, y_t) = \rho_{xy}^2(h). In general, for h > 0, it can be shown (see Alonso and Peña, 2019) that the GCC(x_t, y_t) represents the increase in accuracy in prediction of the bivariate model with respect to the univariate models and it can be interpreted as an average squared correlation coefficient when we explain the residuals of an AR fitting of one variable by the values of the other.