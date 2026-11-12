
(or a deep network structure). In the second part of this chapter, we discuss some basic concepts of NN and DL, and demonstrate their applications using dependent data. These methods are semi-parametric statistical models because a NN typically consists of some sophisticated model, formed by layers of activation functions, with many unknown parameters, referred to as biases and weights in the machine learning literature. For further details of DL, readers are referred to Schmidhuber (2015) and Goodfellow et al. (2016), among many others.

## 8.1 REGRESSION TREES AND RANDOM FORESTS

Tree-based methods are widely used in machine learning and in decision-making. They use recursive partitions and simple models to explore the structure of the data. These trees are usually called CART (classification and regression trees) and were introduced by Breiman et al. (1984). We explain first their use in regression and, second, for classification. For prediction, the methods essentially partition the predictor space into non-overlapping subregions and use the sample mean (or the majority voting) of the dependent variable in each subregion as its prediction. One can think of a regression tree with multiple predictors as a multivariate step function. The key question in applying regression tree is to find efficient ways to perform the partition of the predictor space so as to minimize the sum of squared forecast errors (or misclassification rates) in the training sample.

We start with binary trees by discussing ways to grow and prune a regression tree. In the literature, the size of a tree is often referred to as the number of partitions, or the number of resulting subregions of the predictor space. The starting point of a tree denotes the whole predictor space, i.e. no partition, and the largest tree for a given data set corresponds to the tree in which every data point becomes a leaf. For prediction, the smallest tree means using the sample mean in prediction and the largest tree means using a single data point in prediction. Therefore, neither the smallest nor the largest tree is useful in practice. For tree-based methods to be useful, one needs to select a proper tree, where prediction is made by using the conditional mean of the response when the explanatory variables take values in a defined region of the predictor space. To this end, we need to discuss ways to grow and to prune a regression tree.

### 8.1.1 Growing Tree

Consider the data \(\{(x_t, y_t)|t=1, ..., n\}\), where \(x_t = (x_{1t}, ..., x_{kt})'\) is a realization of the predictor \(X_t\) and \(y_t\) is a realization of the dependent variable \(Y_t\). In this chapter, we follow the conventional statistical literature by using \(n\) to denote the sample size. A _regression tree_ is typically a binary tree constructed by growing and pruning branches to better describe the relationship between \(Y_t\) and \(X_t\). See, for instance, Figure 8.1. It contains a stem (or trunk), several branches (or nodes), and many leaves. The leaves are called _terminal nodes_ and each branch gives a split on the predictor space. At the stem, the predictor space is \(R^k\). That is, there is no splitting in the space. For the \(i\)th predictor \(x_{it}\), let \(x_{i(1)} \le x_{i(2)} \le \cdots \le x_{i(n)}\) be its order statistics. Any real number \(\eta \in [x_{i(1)}, x_{i(n)}]\) partitions \(R^k\) into the following two subregions:

```
\(R_{i,1} = \{x_t | x_{it} \le \eta\}, \quad R_{i,2} = \{x_t | x_{it} > \eta\}.\)
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_442_0.png)

**Figure 8.1** Regression tree for the monthly growth rate of US IP index. There are 732 predictors.

One can compute the sum of squares of residuals of \(y_t\) of the partition as

```
\(SS(i, \eta) = \sum_{x_t \in R_{i,1}} (y_t - \bar{y}_1)^2 + \sum_{x_t \in R_{i,2}} (y_t - \bar{y}_2)^2,\)
```

where \(\bar{y}_1\) and \(\bar{y}_2\) are the sample means of \(y_t\) in \(R_{i,1}\) and \(R_{i,2}\), respectively. In practice, \(\eta\) assumes the order statistics of \(x_{it}\) and the best partition is selected by

```
\(\hat{\eta}_i = \arg \min_{\eta \in \{x_{(i1)}, \dots, x_{(in)}\}} SS(i, \eta),\)
```

where the subscript \(i\) is added to signify that the partition is based on the \(i\)th predictor. The resulting sum of squared forecast errors is \(SS(i, \hat{\eta}_i)\). The first branch of the tree is then determined by the \(i_1\)th predictor with threshold \(\hat{\eta}_{i_1}\), where \(i_1\) is given by

```
\((i_1, \hat{\eta}_{i_1}) = \arg \min_{1 \le i \le k} SS(i, \hat{\eta}_i),\)
```

and the associated two subregions are \(R_{i_1,1}\) and \(R_{i_1,2}\). Therefore, the first split ends with two leaves, namely \(R_{i_1,1}\) and \(R_{i_1,2}\).

Next, the growing process is repeated for \(R_{i_1,1}\) and \(R_{i_1,2}\) separately. The only modification for \(R_{i_1,1}\) is that \(x_{i_1,t}\) only assumes values less than or equal to \(\hat{\eta}_{i_1}\). For \(R_{i_1,2}\), \(x_{i_1,t}\) assumes values greater than \(\hat{\eta}_{i_1}\). In theory, this growing process can continue until every subregion contains a single observation (or a small prespecified number of data points). But such a fully grown tree would result in overfitting and is not useful in practice. The method used to avoid overfitting of a regression tree is called _pruning_, which we discuss next. Note that there exist some variants in growing trees. For instance, instead of using order statistics to perform the partition, one can randomly select some values of each predictor to do the partition.

### 8.1.2 Pruning

The number of splits of a binary regression tree is called the *depth* of the tree. For depths greater than zero, the number of leaves (or terminal nodes) is equal to the depth of the tree plus one and is often referred to as the size of the tree. Denote a given tree with depth m by $T_m$. It splits the predictor space $R^k$ into $m + 1$ subregions, denoted by $\{R_j|j = 1, ..., m + 1\}$, where, for simplicity, the predictors used in the splitting are dropped. The sum of squared of forecast errors of $T_m$ is given by

```
$$SS(T_m) = \sum_{j=1}^{m+1} \sum_{x_t \in R_j} (y_t - \bar{y}_j)^2,$$
```

where $\bar{y}_j$ denotes the sample mean of $y_t$ in $R_j$, that is, the observed conditional mean of the response when the explanatory variables belong to $R_j$. To prune the tree, one often employs the cost of complexity criterion as

```
$$C(T_m, \lambda) = SS(T_m) + \lambda |T_m|,$$
```

where $|T_m|$ denotes the size of the tree and $\lambda > 0$ is a penalty parameter, which governs the trade-off between tree complexity and the goodness of fit of the tree. In practice, $\lambda$ can be chosen by cross-validation or out-of-sample forecast. For a given $\lambda$, one selects the tree by

```
$$T_{\hat{m}} = \arg \min_m C(T_m, \lambda).$$
```

Clearly, a large $\lambda$ prefers a smaller tree and $\lambda = 0$ corresponds the largest tree allowed.

The R packages **tree** and **rpart** can be used to perform tree-based methods for regression and classification. We use **tree** in the following demonstration.

### 8.1.3 Classification Trees

When the objective is the classification of new observations into G possible classes, the splitting criterion is modified as follows. In the first split, as before, for each predictor $x_{it}$ we find a real number $\eta \in [x_{i(1)}, x_{i(n)}]$ that splits the data in an optimal classification, according to some criterion. In the next partitions, we continue splitting each subsample, as in regression trees, but now using the criterion chosen for optimal classification. This optimality criterion can be defined in several ways. The first one is by minimizing the proportion of observations wrongly classified. Thus, at each partition $R_j$ the value of $\eta$ is chosen to produce the smallest proportion of classification errors:

```
$$p_{j\hat{g}} = \arg \max_g p_{j,g},$$
```

where $p_{j,g}$, for $g = 1, ..., G$, is the proportion of observations in the partition that belong to class g and the split is made classifying all the observations in the more frequent class. Then, the proportion of classification errors for partition j will be:

```
$$CE_j = 1 - p_{j\hat{g}}.$$
```

Another criterion is to minimize the average proportion of classification errors. The relative errors in each class, $1 - p_{j,g}$, are weighted by the proportion of observations
