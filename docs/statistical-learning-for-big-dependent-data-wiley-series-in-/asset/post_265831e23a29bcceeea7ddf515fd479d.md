
- 1. Define a measure of distance between two points.
- 2. Calculate the distances from the point to be classified, \(x_0\), to all the points in the sample.
- 3. Choose the \(m\) sample points closest to \(x_0\). Calculate the proportions of these \(m\) points which belong to each of the populations. Classify the point \(x_0\) to the population with the greatest frequency among the \(m\) points.

This method is known as the m-NN. In the specific case of m = 1 NN classifies a new element in the population to which its closest element belongs. A key problem is the choice of m. A possibility is to try different values, obtain the classification error as a function of m, and choose the value of m which leads to the smallest observed classification error.

One of the advantages of this simple method is that it only looks at the distribution of points around the new data to be classified. Thus, if some of the features are not relevant and the information useful for classification is contained in a space of smaller dimension, the local approximation makes that the distance computed could be a good representation of the larger space. For time series analysis, any of the distances and dissimilarity measures presented in Section 5.1 can be used.

NN does not work well when the dimension of the feature space, p, is large. Then, the distance of a point to all the others is usually relatively high and this deteriorates the performance of the method. For instance, suppose data defined by a uniform distribution of points in a hypercube of side one and dimension p. Consider an internal hypercube of side L < 1. It will include a proportion of points of the population (the initial one) equal to \(p_r = L^p\). For large p, for instance p = 500, a large hypercube with L = 0.99 will include points of the population with probability p = 0.0065. Thus, if we take a sample of the population with n = 1000, the expected number of points in this hypercube is 6.5. The high-dimensional space is very wide in relative terms and local distances may not work well in high dimension. Some adaptive nearest neighbors have been proposed for this situation; see Hastie et al. (2001). Another possibility is to reduce the dimension of the space by projecting the data in some useful directions, as the extreme kurtosis directions; see Peña and Prieto (2001) and Peña et al. (2010). The efficiency and usefulness of these methods requires further research.

## 5.6.2 Support Vector Machines

A popular method for discrimination is the SVM; see Vapnik and Chapelle (2000), Cherkassky and Mulier (1998), Scholkopf and Smola (2001), and the references therein. We present the method here first for the case of two states. This procedure approaches the classification problem slightly different from the previous ones: (1) instead of looking for a global solution that depends on all the points in the sample, it searches for a local solution that depends mostly on the points that define the border between the two states; (2) instead of looking for a reduction of the space of variables and solving the problem in the smaller space, it looks for a larger space, including nonlinear transformations of the original variables, where the points may be separated linearly. perfectly. A hyperplane in \( R^p \) is a subspace of dimension \( p - 1 \) and is defined by a linear equation

```
\( w'x = b, \quad (5.59) \)
```

where \( w = (w_1, ..., w_p) \neq 0 \), \( x = (x_1, ..., x_p)' \) and \( b \) is a real number. If the hyperplane contains the origin, its equation becomes \( w'x = 0 \): all vectors going from the origin to a point in the hyperplane, \( x \), are orthogonal to \( w \), which is the vector defining the hyperplane. If the hyperplane does not contain the origin, \( b \neq 0 \). Let \( x_0 \) be the projection of the origin onto the hyperplane, then \( w'x_0 = b \). Consequently, we can rewrite the Eq. (5.59) as

```
\( w'x = w'x_0 \quad \text{or} \quad w'x + w_0 = 0, \)
```

where \( w_0 = -w'x_0 \). In general, a hyperplane in \( R^p \) partitions \( R^p \) into two separated subspaces, namely \( w'x + w_0 > 0 \) and \( w'x + w_0 < 0 \).

### 5.6.2.1 Linearly Separable Problems

Suppose that we have a training set with \( n \) elements, \( \{(y_i, x_i)\}\), where \( y_i \) is the binary classification variable, which, to simplify things here, takes the possible values \( -1 \) and \( +1 \). If the set is linearly separable all the observations of a group, for example, those of \( y_i = -1 \), are found on one side of a hyperplane and all the points of the other group, with \( y_i = 1 \), are on the other side. Therefore, points in one side verify that \( w'x_i + w_0 < 0 \) and those at the other that \( w'x_i + w_0 > 0 \). Also, for some value \( m \), that depends on the data, we have the equivalent relations \( w'x_i + w_0 \le -m \), and \( w'x_i + w_0 \ge m \). These two inequalities can be written jointly as

```
\( y_i(w'x_i + w_0) \ge m, \quad \text{for } i = 1, ..., n, \quad (5.60) \)
```

and we want to find the hyperplane that makes \( m \), the margin or separation between the groups as large as possible. Figure 5.32 illustrates the problem we want to solve. In the maximization problem we must take into account that the hyperplane that

![](img/d4c84ae3c2178f4c506af3dd2ed13122_300_0.png)

Figure 5.32 Two linearly separable groups in \( R^2 \), the separating plane and the margin. The blue circles group has two support vectors and the red star one has five.

maximizes m with the condition in Eq. (5.60) is not unique, as any other hyperplane, \(c(\mathbf{w}'\mathbf{x}_i + w_0) = 0\), for any \(c \neq 0\) is also a solution. To identify the hyperplane, we define \(\mathbf{u} = \mathbf{w}'/\|\mathbf{w}\|\), and the problem to be solved is

max(m)  
such that \(\|\mathbf{u}\| = 1\) and \(y_i(\mathbf{u}'\mathbf{x}_i + w_0) \geq m\). (5.61)

We write this optimization problem in a slightly different way, which is easier to solve. First, note that the distance of a point \(\mathbf{x}_i\) to a general hyperplane, \(\mathbf{w}'\mathbf{x} + w_0 = 0\) is the distance between this point and its projection on the hyperplane \(\mathbf{x}_i(p)\). The vector \(\mathbf{x}_i - \mathbf{x}_i(p)\) is orthogonal to the hyperplane, so that we can write

\(\mathbf{x}_i - \mathbf{x}_i(p) = d_i\mathbf{u},\)

where \(\mathbf{u}\) is a vector of unit length orthogonal to the hyperplane and \(d_i\) is the distance to the hyperplane. If we multiply this equation by \(\mathbf{u}'\) and use that \(\mathbf{x}_i(p)\) is in the hyperplane, so that \(\mathbf{w}'\mathbf{x}_i(p) + w_0 = 0\), then

\(\mathbf{u}'(\mathbf{x}_i - \mathbf{x}_i(p)) = \frac{\mathbf{w}'}{\|\mathbf{w}\|}[\mathbf{x}_i - \mathbf{x}_i(p)] = \frac{\mathbf{w}'\mathbf{x}_i + w_0}{\|\mathbf{w}\|} = d_i.\)

Therefore, the equivalent condition to Eq. (5.60) is, for some \(M\) value

\(y_i\frac{(\mathbf{w}'\mathbf{x}_i + w_0)}{\|\mathbf{w}\|} \geq M \qquad \text{for } i = 1, \dots, n\)

or

\(y_i(\mathbf{w}'\mathbf{x}_i + w_0) \geq \|\mathbf{w}\|M, \qquad \text{for } i = 1, \dots, n.\)

The norm of the vector \(\mathbf{w}\) is arbitrary, but has to be defined to identify the hyperplane. In fact, comparing with Eq. (5.61) if \(\|\mathbf{w}\| = 1\), then \(M = m\). we need to fix the scale with any of these variables and a simple solution is to take

\(\|\mathbf{w}\| = 1/M,\)

and maximizing the distance \(M\) is the same as minimizing the norm \(\|\mathbf{w}\|\). This approach also has the advantage that we do not need the restriction \(\|\mathbf{w}\| = 1\), because the hyperplane is perfectly defined by the solution. The optimization problem is then

\(\min \frac{1}{2}\|\mathbf{w}\|^2,\)  
subject to \(y_i(\mathbf{w}'\mathbf{x}_i + w_0) \geq 1, \quad i = 1, \dots, n.\) (5.62)

This is a quadratic optimization problem with \(p + 1\) variables, the components of the vector \(\mathbf{w}\) plus \(w_0\). The function to be minimized can be written using the Lagrange multipliers as

\(F_L = \frac{1}{2}\mathbf{w}'\mathbf{w} - \sum_{i=1}^n \lambda_i[y_i(\mathbf{w}'\mathbf{x}_i + w_0) - 1],\) (5.63)
