Initialize $\mathbf{w}^t$ at the beginning of optimization when $t = 0$
While $\| \nabla RSS(\mathbf{w}^t) \|_1 > \tau$:
For each $j$ in $[1, ..., d]$:
$$ w_j^{(t+1)} \leftarrow (1 - 2\eta\lambda)w_j^{(t)} + 2\eta \sum_{i=1}^n \mathbf{\Phi}_{ij} \left( y_i - \sum_{k=1}^d \mathbf{\Phi}_{ik} w_k^{(t)} \right) $$
$t \leftarrow t + 1$

# Figure 7.8 The coordinate-wise gradient descent optimization algorithm for ridge regression.

Before we jump into the implementation, let us look at one more important detail: handling the bias term.

# 7.2.3 Handling the bias term

The bias term, also called the intercept term, refers to a weight that does not multiply any feature in the model and acts as a level shift. When adding the bias term, the model becomes $y_i = \sum_{j=0}^d w_j \mathbf{\Phi}_{ij} + \epsilon_i = w_0 \mathbf{\Phi}_{i0} + w_1 \mathbf{\Phi}_{i1} + \dots + w_d \mathbf{\Phi}_{id} + \epsilon_i$ for the \(i^{th}\) observation, where \(\mathbf{\Phi}_{i0} = 1\) is an entry of value one added manually based on the bias trick covered in chapter 2, and there is a corresponding new entry of \(w_0\) at the beginning of the weight vector \(\mathbf{w}\).

Based on the formulation of the objective function for ridge regression, the total cost now becomes \(Q(\mathbf{w}) = RSS(\mathbf{w}) + \lambda ||\mathbf{w}||_2\), where the first entry \(w_0\) is also penalized and encouraged to be small in magnitude. However, penalizing the bias term is not advised as it does not contribute to the overfitting behavior of the model.

There are two workarounds for the bias term. We can choose not to penalize the bias term, making \(Q(\mathbf{w}) = RSS(w_0, w_{rest}) + \lambda ||w_{rest}||_2\), where the squared \(L2\) norm includes all the weights except for the first entry. To implement this in the closed-form solution of ridge regression, i.e., \(\mathbf{w}^{ridge} = (\mathbf{\Phi}^T \mathbf{\Phi} + \lambda \mathbf{I})^{-1} \mathbf{\Phi}^T \mathbf{y}\), we can set the first entry of the identity matrix \(\mathbf{I}\) to zero, giving a modified version of the identity matrix \(\mathbf{I}^{modified}\) and the corresponding analytic form \(\mathbf{w}^{ridge} = (\mathbf{\Phi}^T \mathbf{\Phi} + \lambda \mathbf{I}^{modified})^{-1} \mathbf{\Phi}^T \mathbf{y}\). As for the gradient descent algorithm, we can simply add a special case for \(w_0\) to make it follow the update rule based on RSS alone, namely \(w_0^{(t+1)} \leftarrow w_0^{(t)} + 2\eta \sum_{i=1}^n \mathbf{\Phi}_{ij} (y_i - \sum_{k=1}^d \mathbf{\Phi}_{ik} w_k^{(t)})\). See figure 7.9 for an illustration on the relevant changes to both the closed-form solution and the iterative optimization schedule.