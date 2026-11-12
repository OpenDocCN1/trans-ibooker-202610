
## 2.6 Empirical risk minimization

So far we have assumed that the available data set (**X**,**y**) is a collection of i.i.d. samples generated from 𝕡(**x**,**y**), the underlying joint probability distribution that is unknown to us. The goal of model development is to iteratively train a model *f*₩ using the dataset (**X**,**y**) and ultimately identify the optimal model *f*⁕₩ from the total class of candidate models *F*₩. The optimality is identified when the model trained using the training subset (**X**,**y*)ᵗᵉˢᵗ results in a minimal cost *Q*(**w**) on the test subset (**X**,**y*)ᵗᵉˢᵗ, where (**X**,**y*)ᵗᵉˢᵗ is used as a proxy to the future unseen data.

The test set error is a more indicative reflection of the model's generalization error. The model will produce a low or even zero training error when its complexity increases. Although using techniques such as feature engineering enhances the model's flexibility to fit the training set better, the actual impact still needs to be assessed using the test set. The toy example we have been studying so far only focuses on the training set error. The trained model still needs to rely on a separate test set for a more objective performance assessment.

The model cost *Q*(**w**) on the test set tells us how far off our prediction is compared to the true outcome. However, the test set (**X**,**y*)ᵗᵉˢᵗ is only one of the many possible realizations that could be drawn from 𝕡(**x**,**y**). What we really want is to build a model that produces the minimal expected cost across all different variations of (**X**,**y*)ᵗᵉˢᵗ.

The term expectation means we need to consider all possible realizations from 𝕡(**x**,**y**) when assessing the cost *Q*(**w**) from the model *f*₩. For now, we will write *Q*(**w**) as L(*y*,*f*₩(**x**)) to highlight the fact that the cost now is also a function of the random input data pair (**x**,**y**), which may vary due to the random sampling from 𝕡(**x**,**y**). The expected cost is also called the *risk*, which measures the true generalization error of the model *f*₩ over all possible future (**X**,**y**). Mathematically, the risk for model *f*₩ is denoted as:

```
$R(f_\mathbf{w}) = \mathbb{E}[Q(\boldsymbol{w})] = \mathbb{E}[\mathrm{L}(y, f_\mathbf{w}(\boldsymbol{x}))] = \int \mathrm{L}(y, f_\mathbf{w}(\boldsymbol{x})) \mathrm{d}\mathrm{P}(\boldsymbol{x}, y)$
```

where the expectation is expanded as an integral over all possible realizations of data based on the joint probability distribution 𝕡(**x**,**y**). In linear regression, the risk takes the form of the expected squared error R(*f*₩) = 𝔼[**y** − *f*₩(**X**)]² for all possible variations of (**X**,**y**).

Minimizing the expected risk would produce an optimal model that generalizes best to future test set on average. However, since the joint data distribution 𝕡(**x**,**y**) is unknown, the true risk is instead approximated via an empirical risk term, i.e., the mean squared error based on the pre-allocated test set (**X**,**y*)ᵗᵉˢᵗ:

```
$R_{\text{empirical}}(f_\mathbf{w}) = \frac{1}{n} \sum_{i=1}^{n} L\left(y_i, f_\mathbf{w}(\mathbf{x}_i)\right) = \frac{1}{n} \sum_{i=1}^{n} \left(y_i - f_\mathbf{w}(\mathbf{x}_i)\right)^2$
```

Where *n* refers to the number of examples in the test set. The empirical risk minimization then states that the optimal model *f*⁕₩ is the one that minimizes the empirical risk:

$f_w^* = \text{argmin}_{f_w \in F_w} R_{empirical}(f_w)$

It should be noted that the summation in the definition of \(R_{empirical}(f_w)\) only covers a small test subset of the original dataset. Based on the law of large numbers, the empirical risk \(R_{empirical}(f_w)\) will be a good approximation to the true risk \(R(f_w)\) given enough samples in the test set.

### 2.6.1 More on the model

So far, we have been using \(f_w\) to denote the model \(f\) we are trying to develop based on a set of weights **w**. For now, we will use \(f\) to denote the model for a more general discussion. The choice of a model can be linear or nonlinear, parametric or non-parametric. In linear regression, the model \(f\) is a linear model parameterized by **w**.

Model training means building a model that represents the real mapping relationship between the input variable **x** and output variable y. Denote the real mapping function as \(g\), which is non-random, consistent and contains the true relationship between **x** and y; this is exactly the relationship we want our model to estimate. Assuming an additive random noise \(\epsilon\) in the data generating process, we can then express the underlying process as \(y = g(\mathbf{x}) + \epsilon\), where \(\epsilon\) is an additive random noise that assumes a normal distribution with a zero mean and fixed variance \(\sigma^2\).

Note that the random variable \(\epsilon\) represents errors due to measurement error or other sources of error in the real world, and is assumed to be independence of the random variable **x** in the regression setting. The goal of linear regression is thus to build a linear model \(f\) that approximates the real function \(g\) as close as possible in the presence of an additive, independent and random noise. Considering the limited representativeness of the available dataset **(x,y)** for all different variations of datasets from \(\mathbb{P}(\mathbf{x},\mathbf{y})\), we would want to build a model \(f\) whose expected output matches that of \(g\). Mathematically, we would choose a model \(f\) such that \(\mathbb{E}[f(\mathbf{X})] = \mathbb{E}[y|\mathbf{x} = \mathbf{X}] = \mathbb{E}[g(\mathbf{X}) + \epsilon|\mathbf{x} = \mathbf{X}] = \mathbb{E}[g(\mathbf{X})] + \mathbb{E}(\epsilon) = g(\mathbf{X})\).

### 2.6.2 Bias and variance decomposition

As introduced in chapter 1, when training a model \(f\), there are two sources of error to be balanced off: bias and variance. The bias refers to the error due to the model's insufficient capacity to perfectly fit \(g\); this is the underfitting scenario. For example, fitting a nonlinear quadratic pattern \(g\) using a linear model \(f\). The variance refers to the error due to fitting a spurious relationship that is corrupted by the random noise \(\epsilon\) in the data; this is the overfitting scenario. For example, fitting a linear pattern \(g\) with a linear model \(f\), yet \(f \neq g\).

Following the discussion on the true statistical risk, we now look at ways to decompose the expected risk for a new unseen input \(\mathbf{X}_{new}\). For notational simplicity, we will denote \(f(\mathbf{X}_{new}) = f\) and \(g(\mathbf{X}_{new}) = g\). Using some mathematical derivation, the expected squared error can now be decomposed into three components: the squared bias and the variance of the model, which are considered to be reducible by the training process, plus the variance of the noise, which is considered to be irreducible. Note that it is totally fine to skip the derivation and jump to the outcome of the decomposition process. The main takeaway is that the risk can be decomposed into these three different pieces. Figure 2.22 illustrates the derivation process.

```
R(f) = E[(y - f(X_new))^2 | x = X_new]
= E[(y - f)^2]
= E[(y - g + g - f)^2]
= E[(y - g)^2] + E[(g - f)^2] + 2E[(y - g)(g - f)]
= E[(g + \epsilon - g)^2] + E[(g - f)^2] + 2E[(gy - fy - g^2 + fg)]
= E[\epsilon^2] + E[(g - f)^2] + {2(g^2 - gE[f] - g^2 + gE[f])}
= \sigma^2 + E[(g - E[f] + E[f] - f)^2]
= \sigma^2 + E[(g - E[f])^2] + E[(f - E[f])^2]
= [\sigma^2] + [Bias^2[f] + Var[f]]
```

- Box 1: We are using a trick here by adding and subtracting an additional term
- Box 2: Plugging in the definition of y gives the variance of the noise
- Box 3: This is the irreducible part due to measurement error or other underlying and unknown sources of error
- Box 4: All terms get cancelled out after introducing the expectation; a result of using the trick
- Box 5: Another use of the same trick
- Box 6: Details omitted as it involves the same derivation process as above
- Box 7: This is the definition of the bias and variance for the model; the reducible part that is handled by the training procedure

Figure 2.22 Decomposing the risk into the irreducible part, i.e. the variance of the error term, and the reducible part, i.e. the squared bias and variance of the model. Note that the trick of adding and subtracting an additional term while expanding the squares is used twice in the derivation.

The above derivation has used the fact that E[g] = g as g is deterministic and non-random, along with E[y] = f and E[\epsilon] = 0, where y is the observed target for x_new. In addition, E[f y] = E[f] E[y] = E[f] E[g + \epsilon] = E[f] (E[g] + E[\epsilon]) = g E[f].

Minimizing the risk essentially boils down to reducing one or more of the three sources of error. Since random noise is not something we can control, it is an irreducible source of error. Bias and variance, however, are reducible elements controlled by the training procedure. A simple model tends to have a high bias and low variance, while a complex model is typically associated with low bias and high variance. This corresponds to the classical statistical view on bias and variance tradeoff: we can only strive to reduce one at a specific cost of the other. This tradeoff essentially limits a model’s capability to generalize to the future test set by minimizing the two sources of error simultaneously. These three sources of error also jointly form a lower bound on the expected error for a future unseen dataset.

However, as we will learn in later chapters, over-parameterized models such as smoothing spline and neural networks can continue lowering the variance even when the bias is zero, which seems to contradict the statistical view.
