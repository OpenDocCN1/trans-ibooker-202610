Under some mild regularity condition on the error distribution and a properly chosen range of the penalty parameter \(\lambda\), say \(\lambda \asymp \sqrt{\log(k)/n}\), the Lasso estimator is consistent as follows:

\([\hat{\beta}(\lambda) - \beta^0]'\Sigma_X[\hat{\beta}(\lambda) - \beta^0] = o_p(1), \quad n \to \infty, \qquad (7.10)\)

if the design is fixed and the basic sparsity assumption

\(s_0 = \|\beta^0\|_0 = o \left( \sqrt{\frac{n}{\log(k)}} \right) \qquad (7.11)\)

holds. The consistency result of Eq. (7.10) continues to hold if the design is random provided that the true parameters satisfy

\(\|\beta^0\|_1 = o((n / \log(n))^{1/4}). \qquad (7.12)\)

See Greenshtein and Ritov (2004) and Bartlett et al. (2012).

Let \(S\) be a subset of \(\{1, \dots, k\}\), i.e. \(S \subset \{1, \dots, k\}\). Let \(\beta_S\) be a \(k\)-dimensional vector such that its \(j\)th element is \(\beta_j\) if \(j \in S\) and is \(0\), otherwise. Similarly, \(\beta_{S^c}\) be a \(k\)-dimensional vector such that its \(j\)th element is \(\beta_j\) if \(j \notin S\) and is \(0\), otherwise, where \(S^c\) denotes the complement of \(S\) with respect to \(\{1, \dots, k\}\). Clearly, \(\beta = \beta_S + \beta_{S^c}\). We can then consider further properties of the Lasso estimator.

# **Lemma 7.2** On the set \(F\) of Eq. (7.9) with \(\lambda > 2\lambda_0\), we have

\(2\|X(\hat{\beta} - \beta^0)\|_2^2/n + \lambda\|\hat{\beta}_{S^c}\|_1 \le 3\|\hat{\beta}_{S_0} - \beta_{S_0}\|_1.\)

The lemma can be shown by the basic inequality and triangular inequality. See Lemma 6.3 of Bühlmann and van de Geer (2011). By Lemma 7.2, we have

\(\|\hat{\beta}_{S^c}\|_1 \le 3\|\hat{\beta}_{S_0} - \beta_{S_0}\|_1.\)

For the linear model of Eq. (76), we say that the *compatibility condition* is met for the set \(S_0\) if for some \(\phi_0 > 0\) and for all \(\beta\) satisfying \(\|\beta_{S^c}\|_1 \le 3\|\beta_{S_0}\|_1\), the inequality condition

\(\|\beta_{S_0}\|_1^2 \le (\beta' \hat{\Sigma} \beta) s_0 / \phi_0^2 \qquad (7.13)\)

holds. Note that the coefficient 3 of the inequality is rather arbitrary, but commonly used in the literature. It can be replaced by any constant greater than 1. This compatibility condition is similar to a condition on the smallest eigenvalue of \(\hat{\Sigma}\) commonly seen in the literature if one replaces \(\|\beta_{S_0}\|_1^2\) by its upper bound \(s_0\|\beta_{S_0}\|_2^2\). In practice, \(S_0\) is unknown so that the compatibility condition cannot be verified. If \(s_0\), the cardinality of \(S_0\) is known, then one can verify the condition by checking all subsets of \(\{1, \dots, k\}\) with cardinality \(s_0\), resulting in the *restricted eigenvalue assumption* of Bickel et al. (2009).

# **Theorem 7.2** Suppose that the compatibility condition holds for \(S_0\). Then, on the set \(F\) and for \(\lambda \ge 2\lambda_0\), we have

\(\|X(\hat{\beta} - \beta^0)\|_2^2/n + \lambda\|\hat{\beta} - \beta^0\|_1 \le 4\lambda^2 s_0 / \phi_0^2.\)