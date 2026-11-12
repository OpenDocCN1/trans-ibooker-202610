\nabla_{\mathbf{w}} Q = \frac{\partial Q(\mathbf{w})}{\partial \mathbf{w}} = \frac{\partial (\mathbf{y}^\top \mathbf{y} - 2 \mathbf{y}^\top \mathbf{\Phi w} + \mathbf{w}^\top \mathbf{\Phi}^\top \mathbf{\Phi w})}{\partial \mathbf{w}}
= -2 \mathbf{\Phi}^\top \mathbf{y} + 2 \mathbf{\Phi}^\top \mathbf{\Phi w}
= \begin{bmatrix} \frac{\partial Q(\mathbf{w})}{\partial w_1} \\ \frac{\partial Q(\mathbf{w})}{\partial w_2} \\ \vdots \\ \frac{\partial Q(\mathbf{w})}{\partial w_p} \end{bmatrix} = \begin{bmatrix} -2 \sum_{i=1}^n \phi_i^1 y_i + 2 \sum_{i=1}^n \phi_i^1 \sum_{j=1}^p \phi_i^j w_j \\ -2 \sum_{i=1}^n \phi_i^2 y_i + 2 \sum_{i=1}^n \phi_i^2 \sum_{j=1}^p \phi_i^j w_j \\ \vdots \\ -2 \sum_{i=1}^n \phi_i^p y_i + 2 \sum_{i=1}^n \phi_i^p \sum_{j=1}^p \phi_i^j w_j \end{bmatrix}

Differentiating w.r.t. the weight vector **w**

Each resulting partial derivative in the gradient vector is only a function of the respective weight

By setting each element of the gradient vector to zero, we can solve for the set of weights at the stationary point

**Figure 2.9 Deriving the gradient vector of the cost function, where each entry is expanded for full clarity. By setting each partial derivative to zero, we can solve for the optimal weights at the stationary point. Note that when solving for the system of equations, each entry in the gradient vector is only a function of the current weight and independent of other weights.**

Since each equation is linear in the respective weight, the derivation above makes it easy to solve for the optimal weights at the stationary point. Such derivation also forms the basis for deriving the closed-form solution in linear regression.