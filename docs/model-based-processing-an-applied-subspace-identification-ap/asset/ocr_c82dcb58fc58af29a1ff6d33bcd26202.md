Figure 4.2 LKF design for RC-circuit problem. (a) Estimated state (voltage) and error. (b) Filtered voltage measurement and error (innovations). (c) WSSR and zero-mean/whiteness tests.

```
K(t) = 2 * \tilde{P}(t|t-1) / (4 \tilde{P}(t|t-1) + 4) (Gain)
\hat{x}(t|t) = \hat{x}(t|t-1) + K(t) e(t) (Updated State)
\tilde{P}(t|t) = \tilde{P}(t|t-1) / (\tilde{P}(t|t-1) + 1) (Updated Covariance)
```

The estimator is also designed using MATLAB, and the results are shown in Figure 4.2. In Figure 4.2a, we see the estimated state (voltage) and estimation error as well as the corresponding confidence bounds. Note that the processor