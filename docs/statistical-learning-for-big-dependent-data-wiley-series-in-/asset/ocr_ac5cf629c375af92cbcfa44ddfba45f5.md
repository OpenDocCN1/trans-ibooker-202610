In the algorithm, \(\nabla_\theta\) denotes the partial derivative with respect to \(\theta\) and \(L(f(.), y)\) denotes a loss function between \(f(.)\) and \(y\) with \(f(.)\) representing the output of a deep network. A sufficient condition for the convergence of SGD algorithm is that

```
\(\sum_{j=1}^\infty \epsilon_j = \infty \quad \text{and} \quad \sum_{j=1}^\infty \epsilon_j^2 < \infty.\)
```

In practice, one can start with an initial learning rate \(\epsilon_0\) and decrease the rate linearly until iteration \(\tau\) as

```
\(\epsilon_j = (1 - \alpha)\epsilon_0 + \alpha\epsilon_\tau,\)
```

with \(\alpha = j/\tau\). After iteration \(\tau\), the learning rate becomes a constant. In some application, one can even use \(\ell = 1\), i.e. a single data point.

Another useful training method is the **momentum** of Polyak (1964), which can be summarized as follows:

# SGD with momentum:

+   1. Specify a learning rate \(\epsilon\) and a momentum parameter \(\alpha\).
+   2. Let the initial parameter be \(\theta\) and initial velocity be \(\mathbf{v}\).
+   3. While stopping criterion is not met, do
+       * Sample a minibatch of \(\ell\) data points, say \(\{(\mathbf{x}^{(i)}, y^{(i)})|i = 1, \dots, \ell\}\).
+       * Compute the gradient estimate:
+         \(\hat{\boldsymbol{g}} = \frac{1}{\ell} \nabla_\theta \sum_i L(f(\mathbf{x}^{(i)}, \theta), y^{(i)}).\)
+       * Compute the velocity update: \(\mathbf{v} = \alpha \mathbf{v} - \epsilon \hat{\boldsymbol{g}}\).
+       * Update: \(\theta = \theta + \mathbf{v}\).

There are many other methods available to train a deep network, including the coordinate decent, adaptive learning rates (the AdaGrad algorithm), the RMSProp algorithm of Hinton (2012), and the Adam method of Kingma and Ba (2014). Again, interested readers are referred to Goodfellow et al. (2016).

# 8.4 SOME APPLICATIONS

We demonstrate some applications of RNN and DL using several real data sets. For the RNN example, we use the R package **rnn** and for other examples, we use the package **keras**, which is available from the RStudio at https://keras.rstudio.com. The web also contains information on installation and several examples for running machine learning in R. Interested readers are referred to Chollet and Allaire (2020) for further information. Also, the vignettes of **keras** on R CRAN include several tutorials on using the package such as basic classification and basic regression.

# 8.4.1 The Package: keras

The package **keras** available in RStudio is a useful package to learn DL in R. It has some built-in functions for applications. Users can build a NN model by