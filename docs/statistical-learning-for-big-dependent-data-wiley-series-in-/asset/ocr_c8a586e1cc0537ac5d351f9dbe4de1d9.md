![](bbox=[0.245, 0.1, 0.798, 0.306])

Figure 7.5 Coefficient estimates of Lasso linear regression for the growth rates of US IPI from 1960 to 2019. The penalty parameter is selected by the 10-fold CV.

![](bbox=[0.245, 0.4, 0.798, 0.62])

Figure 7.6 Coefficients versus $\ell_1$-norm of elastic net linear regression for the monthly growth rates of U.S. IPI from 1960 to 2019. The penalty is $0.8\ell_1 + 0.2\ell_2$.

estimated coefficient. If a higher threshold, around 0.05, is chosen, then we would select about seven predictors, instead of nine mentioned earlier.

Next, turn to the elastic net with a linear combination defined by the $\alpha$ parameter of the $\ell_1$- and $\ell_2$-penalties. Figure 7.6 shows the coefficient estimates versus $\ell_1$ norm of elastic net regression with $\alpha = 0.8$. This coefficient profile plot works in the same manner as that of Figure 72 and, as expected, it provides similar results. Figure 77 shows the results of a 10-fold CV versus the logarithm of penalty parameter $\lambda$. From the plot, a choice of $log(\lambda) \approx -2.5$ seems reasonable. Specifically, the CV selects $\lambda \approx 0.0846$. Figure 78 shows the estimated coefficients $\hat{\beta}_j$ when $\lambda$ is selected by the CV. Based on the plot, we find that there are 10 predictors with $|\hat{\beta}_j| > 0.04$. These predictors contain the same selections as those of the Lasso regression shown