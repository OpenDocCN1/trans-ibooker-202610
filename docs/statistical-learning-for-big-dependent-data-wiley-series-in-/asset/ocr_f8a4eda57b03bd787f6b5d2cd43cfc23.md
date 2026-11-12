stop and the ODPCA has one component with \(g^{*,1}\) lags; otherwise, we add the second component defined using \(g^{*,2}\) lags and proceed as before.

The same stepwise approach could be applied to minimize an information criterion instead of the cross-validated forecasting error. This would reduce the computational burden significantly. The following criterion, inspired by the IC\(_{p3}\) criterion proposed by Bai and Ng (2002), seems to give good results in practice. Let \(\hat{\sigma}_{1,g}^2\) be the reconstruction MSE for the first ODPCA defined using \(g\) lags. Let \(T^{*,1,g} = T - 2g\). Then we choose the value \(g^{*,1}\) among \(1, \dots, G_{max}\) that minimizes

```
\(\text{BNG}_{1,g} = \log(\hat{\sigma}_{1,g}^2) + (g+1) \frac{\log(\min(T^{*,1,g}, g))}{\min(T^{*,1,g}, g)}.\)
```

Note that \((g+1)\) is the number of static factors implied by fitting a single component with \(g\) lags. Suppose now that we have computed \(q-1\) DPCs, each with \(c_1 = c_2 = g^{*,i}\) lags, \(i = 1, \dots, q-1\). Let \(\hat{\sigma}_{q,g}^2\) be the reconstruction MSE for the fit obtained using components, where the first \(q-1\) components are defined using \(g^{*,i}, i = 1, \dots, q-1\) and the last component is defined using \(g\) lags. Let \(T^{*,q,g} = T - \max\{2g^{*,1}, \dots, 2g^{*,q-1}, 2g\}\). Let \(g^{*,q}\) be the value among \(1, \dots, G_{max}\) that minimizes

```
\(\text{BNG}_{q,g} = \log(\hat{\sigma}_{q,g}^2) + \left( \sum_{i=1}^{q-1} (g^{*,i} + 1) + g + 1 \right) \frac{\log(\min(T^{*,q,g}, g))}{\min(T^{*,q,g}, g)}.\)
```

Note that \(\sum_{i=1}^{q-1} (g^{*,i} + 1)\) is the number of static factors implied by fitting \(q-1\) dynamic components, each with \(g^{*,i}\) lags. If \(\text{BNG}_{q^{*,g}, g}\) is larger than \(\text{BNG}_{q-1, g^{*,q-1}}\) we stop and the final model is the ODPC with \(q-1\) components. Otherwise, we add the \(q\)th component defined using \(g^{*,q}\) and continue as before.

After fitting \(q\) DPCs to the data, each with \((c_1, c_2)\) lags, \(i = 1, \dots, q\), we can build forecasts as follows. Suppose we have decided upon a procedure to forecast each of these DPCs separately. For instance, we can fit ARMA models to the components in an automatic fashion. Let \(\hat{f}_j^{i|h}\) for \(h > 0\) be the forecast of \(f_{T+h}^i\) with information until time \(T\). Then we can obtain a \(h\)-step ahead forecast of \(z_T\) as

```
\(\hat{z}_{T+v|T} = \sum_{i=1}^{q} \sum_{h=0}^{c_2} \hat{b}_h^i \hat{f}_{T+v-h|T}^i.\)
```

# 6.4.4 One Sided DPC and GDFM Estimation

The one-sided DPC or ODPC provides a useful way to estimate GDFM. In fact, it was proved by Peña et al. (2019) that asymptotically, when both the number of series and the sample size go to infinity, the reconstruction obtained with ODPC converges in mean square to the common part of the GDFM. Hence, the ODPC provides an alternative way to estimate the common part of DFMs with a finite number of lags. For nonstationary time series, we will present next a generalized version of these time domain DPCs to estimate nonstationary GDFM.