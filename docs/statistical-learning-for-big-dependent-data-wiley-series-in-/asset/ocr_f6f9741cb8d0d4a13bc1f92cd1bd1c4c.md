where the $\lambda_i \ge 0$ so that when the constraints hold for all points $F_L \le w'w/2$, and if the constraint $y_i(w'x_i + w_0) \ge 1$ is not satisfied for the variables $(y_j, x_j)$, it increases $F_L$. Taking the derivatives with respect to $w$ and $w_0$

$$\frac{\partial F_L}{\partial w} = 0 = w - \sum_{i=1}^n \lambda_i y_i x_i,$$ (5.64)

$$\frac{\partial F_L}{\partial w_0} = 0 = \sum_{i=1}^n \lambda_i y_i,$$ (5.65)

and inserting in Eq. (5.63) these equations, we obtain the dual problem

$$F_D = \frac{1}{2}w'w - w'w + \sum_{i=1}^n \lambda_i = \sum_{i=1}^n \lambda_i - \frac{1}{2} \sum_{i=2}^n \sum_{j=1}^n \lambda_i \lambda_j y_i y_j x_i' x_j,$$ (5.66)

This is a problem in the $n$ variables $\lambda_i$ that verify the restrictions $\lambda_i \ge 0$ and $\sum_{i=1}^n \lambda_i y_i = 0$. It is called the dual problem and minimizing $F_L$ to find the $p+1$ variables with their restrictions that define the hyperplane is equivalent to maximizing $F_D$ with respect the $n$ variables $\lambda_i$ with their restrictions. The second problem is easier to solve and also the objective function only depends on the data through the scalar products of the feature vectors. It is a convex optimization problem with a quadratic criterion and linear constraints that can be solved by standard optimization methods. See, for instance, Hastie et al. (2001).

Note also that the hyperplane is defined by the scalar products between the sample points, because by Eq. (5.64) it can be written as

$$f(x) = w'x + w_0 = \sum_{i=1}^n \lambda_i y_i x_i' x + w_0.$$ (5.67)

The hyperplane is defined by the points with $\lambda_i > 0$ and those with $\lambda_i = 0$ have no role in the equation of the hyperplane. Points with $\lambda_i > 0$ are called support vectors and verify $y_i(w'x_i + w_0) = 1$. See Figure 5.32. The classification is made with the sign of $w'x_i + w_0$. Note that the restrictions define a space bounded by the limiting hyperplanes $w'x_i + w_0 = 1$ and $w'x_i + w_0 = -1$ and inside this space there exists no data points. The data contained in these limiting hyperplanes is the support vectors, that play a key role because if they are modified the classification will change, a property that apply only to the support vectors. This is an important difference with Fisher's LDF or logistic regression. In these approaches a small perturbation of any point, from $x_i$ to $x_i + \epsilon u$ for small $\epsilon$, may produce a change in the classification. In SVM, this would happen for points close to the limiting hyperplanes but has no effect for points far away from both hyperplanes.

For normal separable data with the same covariance matrix, the solution provided by Fisher LDF is optimal, but when the covariance matrices are different and the data is not normal, SVM provides a useful alternative. Usually in these cases, the logistic regression would provide a solution similar to that of SVM.