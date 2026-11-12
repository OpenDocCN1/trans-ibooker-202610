where the matrix **Y** is given by

```
$$Y = (B \otimes I_{T^*})Z^B,$$
```

and the matrix $Z^B$ is formed by the matrices $Z_{l, c_1}$ that have the lagged values for the computation of the component. This solution should be standardized to unit norm.

On the other hand, for a fixed $F_{c_1, c_2}$, the optimal $B$ can also be computed by least squares method

```
$$\widehat{B} = (F'_{c_1, c_2} F_{c_1, c_2})^{-1} F'_{c_1, c_2} Z_{c_1+c_2}.\tag{6.36}$$
```

We can apply an alternating least squares algorithm for computing $\widehat{a}$ and $\widehat{B}$. Let $a^{(i)}$, $B^{(i)}$, and $f^{(i)}$ be the values of $a$, $B$, corresponding to the $i$th iteration. Let $\delta \in (0, 1)$ be a tolerance parameter to stop the iterations. In our implementation, we take $\delta = 10^{-4}$. As before, the algorithm is defined by giving an initial value of the component, say $f^{(0)}$, and describing the rule to compute $B^{(i+1)}$, $a^{(i+1)}$, and $f^{(i+1)}$ from $f^{(i)}$. This can be done as follows:

+   1. Given $f^{(i)}$, define $B^{(i+1)}$ by Eq. (6.36), where $F_{c_1, c_2}$ corresponds to $f^{(i)}$.
+   2. Compute $a_p^{(i+1)}$ by Eq. (6.35) with $B = B^{(i+1)}$, and let $a^{(i+1)} = a_p^{(i+1)} / \|a_p^{(i+1)}\|$.
+   3. The $t$th coordinate of $f^{(i+1)}$ is given by Eq. (6.31) with $a = a^{(i+1)}$.

We stop the iteration when the relative decrease in the MSE is smaller than $\delta$. Clearly in this algorithm at each step the MSE decreases and, therefore, it converges to a local minimum. To obtain a global minimum, the initial value $f^{(0)}$ should be sufficiently close to the optimal one. We propose to take $f^{(0)}$ as the last $T - c_1$ coordinates of the first ordinary PC of the data.

# 6.4.3 Model Selection and Forecasting

In practice, the number of components and the number of lags in each component need to be chosen. To simplify the notation, assume that for each component $c_1 = c_2$, that is, the number of lags of $z_t$ used to define the $i$th DPC and the number of lags of $\widehat{f}_t^i$ used to reconstruct the original series are the same. One possible approach to choose the number of components and the number of lags is to minimize the cross-validated forecasting error in a stepwise fashion. Suppose we want to make $h$-step ahead forecasts. Fix a maximum number of lags $G_{\max}$ and a window size $w$. Then, given $g \in \{1, \dots, G_{\max}\}$, we compute the first ODPC with $g$ lags, using periods $1, \dots, T - h - t + 1$ for $t = 1, \dots, w$, and for each of these fits we compute an $h$-step ahead forecast and the corresponding MSE $E_{t,h}$. The cross-validation estimate of the forecasting error is then

```
$$\widehat{MSE}_{1,g} = \frac{1}{w} \sum_{t=1}^w E_{t,h}.$$
```

We choose for the first component the value $g^{*,1}$ that minimizes $\widehat{MSE}_{1,g}$. Then, we fix the first component computed with $g^{*,1}$ lags and repeat the procedure with the second component. If the optimal cross-validated forecasting error using the two components, $\widehat{MSE}_{2,g}$, is larger than the one using only one component, $\widehat{MSE}_{1,g}$, we