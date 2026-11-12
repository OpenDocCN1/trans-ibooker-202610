
$t = c_1 + 1, ..., T$, and by (6.32), the first value we can reconstruct is $t = c_1 + c_2$. The constraint $\|\boldsymbol{a}\| = 1$ is included for identification purpose, because if we multiply $\boldsymbol{a}_h$ in Eq. (6.31) by $g$ and divide $\boldsymbol{b}_h$ by $g$, the solution remains the same.

The second ODPC is then defined as a linear combination of the lagged series that can be used to optimally reconstruct the residuals from the first component. Higher-order components are defined similarly. Finally, the reconstruction of $\boldsymbol{z}_t$ using the first $v$ one-sided DPCs are given by

\(\widehat{z}^v_t = \sum_{i=1}^v \sum_{h=0}^{c_2^i} \widehat{\boldsymbol{b}}^i_h \widehat{f}^i_{t-h}, \quad c_{\max}^v + 1 \le t \le T,\)

where now \(c^i_1\) and \(c^i_2\) are the values of \(c_1\) and \(c_2\) for the \(i\)th ODPC and \(c_{\max}^v = \max\{c_1^i + c_2^i | i = 1, ..., v\}\).

We now describe an algorithm to compute the ODPC. Since their definition is sequential, it suffices to propose an algorithm to compute the first component. In what follows, to keep the notation light, we drop the superscript indicating the component number. In order to derive an algorithm to compute \(\widehat{\boldsymbol{a}}\), and \(\widehat{\boldsymbol{B}}\), we need first to express the objective function in Eq. (6.33) in a more manageable form. To this end, we introduce some further notation. Let \(\boldsymbol{Z}_h\) be the \(T^* \times k\) data matrix of observations, i.e.

\(\boldsymbol{Z}_h = \begin{bmatrix} \boldsymbol{z}'_{h+1} \\ \boldsymbol{z}'_{h+2} \\ \vdots \\ \boldsymbol{z}'_{T+T^*} \end{bmatrix},\)

and consider the sequence of matrices \(\boldsymbol{Z}_h\) for \(h = 0, ..., (c_1 + c_2)\). The matrix \(\boldsymbol{Z}_{c_1+c_2}\) includes the observations to be reconstructed, and the matrices \(\boldsymbol{Z}_0, ..., \boldsymbol{Z}_{c_1+c_2-1}\) are used to compute the component. We also need, for \(l = c_1, ..., (c_1 + c_2)\), the sequence of \(T^* \times k(c_1 + 1)\) matrices \(\boldsymbol{Z}_{l, c_1} = [\boldsymbol{Z}_l, \boldsymbol{Z}_{l-1}, ..., \boldsymbol{Z}_{l-c_1}]\) that include the lagged values for the computation of the component in columns \(\boldsymbol{F}_{c_1, c_2} = [\boldsymbol{f}_{(c_1+c_2)}, \boldsymbol{f}_{(c_1+c_2)-1}, ..., \boldsymbol{f}_{c_1}]\) with the values of the component in columns \(\boldsymbol{f}_j = (f_{j+1}, ..., f_{T-(c_1+c_2)+j})'\). These last matrices are related by

\(\boldsymbol{F}_{c_1, c_2} = (\boldsymbol{Z}_{c_1+c_2, c_1} \boldsymbol{a}, ..., \boldsymbol{Z}_{c_1, c_1} \boldsymbol{a}),\)

where \(\boldsymbol{F}_{c_1, c_2} = \boldsymbol{F}_{c_1, c_2}(\boldsymbol{a})\), even though this dependence is not made explicit in the notation. The reconstruction of \(\boldsymbol{Z}_{c_1+c_2}\) can be written as a matrix \(\boldsymbol{Z}^R_{c_1+c_2}\) of the same dimension, \(T^* \times k\),

\(\boldsymbol{Z}^R_{c_1+c_2} = \boldsymbol{F}_{c_1, c_2} \boldsymbol{B}.\)

Then, \((\widehat{\boldsymbol{a}}, \widehat{\boldsymbol{B}})\) can be obtained by minimizing

\(\| \boldsymbol{Z}_{c_1+c_2} - \boldsymbol{F}_{c_1, c_2} \boldsymbol{B} \|^2_F = \| \text{vec}(\boldsymbol{Z}_{c_1+c_2}) - \text{vec}(\boldsymbol{Z}^R_{c_1+c_2}) \|^2,\)

with the restriction \(\| \boldsymbol{a} \| = 1\), where \(\| \cdot \|_F\) is the Frobenius norm.

Then, it can be shown (see Peña et al. 2019) that the least squares estimate of \(\widehat{\boldsymbol{a}}\) is

\(\widehat{\boldsymbol{a}} = (\boldsymbol{Y}' \boldsymbol{Y})^{-1} \boldsymbol{Y}' \text{vec}(\boldsymbol{Z}_{c_1+c_2}),\)

where the matrix **Y** is given by

$$Y = (B \otimes I_{T^*})Z^B,$$

and the matrix $Z^B$ is formed by the matrices $Z_{l, c_1}$ that have the lagged values for the computation of the component. This solution should be standardized to unit norm.

On the other hand, for a fixed $F_{c_1, c_2}$, the optimal $B$ can also be computed by least squares method

$$\widehat{B} = (F'_{c_1, c_2} F_{c_1, c_2})^{-1} F'_{c_1, c_2} Z_{c_1+c_2}.\tag{6.36}$$

We can apply an alternating least squares algorithm for computing $\widehat{a}$ and $\widehat{B}$. Let $a^{(i)}$, $B^{(i)}$, and $f^{(i)}$ be the values of $a$, $B$, corresponding to the $i$th iteration. Let $\delta \in (0, 1)$ be a tolerance parameter to stop the iterations. In our implementation, we take $\delta = 10^{-4}$. As before, the algorithm is defined by giving an initial value of the component, say $f^{(0)}$, and describing the rule to compute $B^{(i+1)}$, $a^{(i+1)}$, and $f^{(i+1)}$ from $f^{(i)}$. This can be done as follows:

- 1. Given $f^{(i)}$, define $B^{(i+1)}$ by Eq. (6.36), where $F_{c_1, c_2}$ corresponds to $f^{(i)}$.
- 2. Compute $a_p^{(i+1)}$ by Eq. (6.35) with $B = B^{(i+1)}$, and let $a^{(i+1)} = a_p^{(i+1)} / \|a_p^{(i+1)}\|$.
- 3. The $t$th coordinate of $f^{(i+1)}$ is given by Eq. (6.31) with $a = a^{(i+1)}$.

We stop the iteration when the relative decrease in the MSE is smaller than $\delta$. Clearly in this algorithm at each step the MSE decreases and, therefore, it converges to a local minimum. To obtain a global minimum, the initial value $f^{(0)}$ should be sufficiently close to the optimal one. We propose to take $f^{(0)}$ as the last $T - c_1$ coordinates of the first ordinary PC of the data.

### 6.4.3 Model Selection and Forecasting

In practice, the number of components and the number of lags in each component need to be chosen. To simplify the notation, assume that for each component $c_1 = c_2$, that is, the number of lags of $z_t$ used to define the $i$th DPC and the number of lags of $\widehat{f}_t^i$ used to reconstruct the original series are the same. One possible approach to choose the number of components and the number of lags is to minimize the cross-validated forecasting error in a stepwise fashion. Suppose we want to make $h$-step ahead forecasts. Fix a maximum number of lags $G_{\max}$ and a window size $w$. Then, given $g \in \{1, \dots, G_{\max}\}$, we compute the first ODPC with $g$ lags, using periods $1, \dots, T - h - t + 1$ for $t = 1, \dots, w$, and for each of these fits we compute an $h$-step ahead forecast and the corresponding MSE $E_{t,h}$. The cross-validation estimate of the forecasting error is then

$$\widehat{MSE}_{1,g} = \frac{1}{w} \sum_{t=1}^w E_{t,h}.$$

We choose for the first component the value $g^{*,1}$ that minimizes $\widehat{MSE}_{1,g}$. Then, we fix the first component computed with $g^{*,1}$ lags and repeat the procedure with the second component. If the optimal cross-validated forecasting error using the two components, $\widehat{MSE}_{2,g}$, is larger than the one using only one component, $\widehat{MSE}_{1,g}$, we stop and the ODPCA has one component with \(g^{*,1}\) lags; otherwise, we add the second component defined using \(g^{*,2}\) lags and proceed as before.

The same stepwise approach could be applied to minimize an information criterion instead of the cross-validated forecasting error. This would reduce the computational burden significantly. The following criterion, inspired by the IC\(_{p3}\) criterion proposed by Bai and Ng (2002), seems to give good results in practice. Let \(\hat{\sigma}_{1,g}^2\) be the reconstruction MSE for the first ODPCA defined using \(g\) lags. Let \(T^{*,1,g} = T - 2g\). Then we choose the value \(g^{*,1}\) among \(1, \dots, G_{max}\) that minimizes

\(\text{BNG}_{1,g} = \log(\hat{\sigma}_{1,g}^2) + (g+1) \frac{\log(\min(T^{*,1,g}, g))}{\min(T^{*,1,g}, g)}.\)

Note that \((g+1)\) is the number of static factors implied by fitting a single component with \(g\) lags. Suppose now that we have computed \(q-1\) DPCs, each with \(c_1 = c_2 = g^{*,i}\) lags, \(i = 1, \dots, q-1\). Let \(\hat{\sigma}_{q,g}^2\) be the reconstruction MSE for the fit obtained using components, where the first \(q-1\) components are defined using \(g^{*,i}, i = 1, \dots, q-1\) and the last component is defined using \(g\) lags. Let \(T^{*,q,g} = T - \max\{2g^{*,1}, \dots, 2g^{*,q-1}, 2g\}\). Let \(g^{*,q}\) be the value among \(1, \dots, G_{max}\) that minimizes

\(\text{BNG}_{q,g} = \log(\hat{\sigma}_{q,g}^2) + \left( \sum_{i=1}^{q-1} (g^{*,i} + 1) + g + 1 \right) \frac{\log(\min(T^{*,q,g}, g))}{\min(T^{*,q,g}, g)}.\)

Note that \(\sum_{i=1}^{q-1} (g^{*,i} + 1)\) is the number of static factors implied by fitting \(q-1\) dynamic components, each with \(g^{*,i}\) lags. If \(\text{BNG}_{q^{*,g}, g}\) is larger than \(\text{BNG}_{q-1, g^{*,q-1}}\) we stop and the final model is the ODPC with \(q-1\) components. Otherwise, we add the \(q\)th component defined using \(g^{*,q}\) and continue as before.

After fitting \(q\) DPCs to the data, each with \((c_1, c_2)\) lags, \(i = 1, \dots, q\), we can build forecasts as follows. Suppose we have decided upon a procedure to forecast each of these DPCs separately. For instance, we can fit ARMA models to the components in an automatic fashion. Let \(\hat{f}_j^{i|h}\) for \(h > 0\) be the forecast of \(f_{T+h}^i\) with information until time \(T\). Then we can obtain a \(h\)-step ahead forecast of \(z_T\) as

\(\hat{z}_{T+v|T} = \sum_{i=1}^{q} \sum_{h=0}^{c_2} \hat{b}_h^i \hat{f}_{T+v-h|T}^i.\)

### 6.4.4 One Sided DPC and GDFM Estimation

The one-sided DPC or ODPC provides a useful way to estimate GDFM. In fact, it was proved by Peña et al. (2019) that asymptotically, when both the number of series and the sample size go to infinity, the reconstruction obtained with ODPC converges in mean square to the common part of the GDFM. Hence, the ODPC provides an alternative way to estimate the common part of DFMs with a finite number of lags. For nonstationary time series, we will present next a generalized version of these time domain DPCs to estimate nonstationary GDFM.
