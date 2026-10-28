(threat or nonthreat) is made and the arrival is processed; however, if not enough data is available to make the decision, then another measurement is obtained.

For our problem, we typically have information about the background, disturbance, and noise parameters, but we rarely have the source information. Therefore, we still can make a decision, but require estimates of the unknown parameters, that is, \hat{\Theta} \to \Theta. In this case, we must construct a composite or generalized likelihood ratio test (GLRT). Therefore, from the batch likelihood decision function, we can consider the two cases of the GLRT: (i) parameters are random or (ii) parameters are deterministic but unknown.

Here we assume that \Theta is deterministic but unknown. Therefore, the approach is to estimate the unknown parameter vector \hat{\Theta} \to \Theta under each hypothesis and proceed with the simple testing. A maximum likelihood estimate \hat{\Theta}_{ML} can be used to create the GLRT such that

```
\mathcal{L}(T_M; \Theta) = \frac{\max_{\Theta_1} \Pr[T_M|\Theta_1; \mathcal{H}_1]}{\max_{\Theta_0} \Pr[T_M|\Theta_0; \mathcal{H}_0]} \tag{8.55}
```

This is the approach we employ initially. The batch solution for the GLRT can also be extended to the sequential case as before giving the solution by simply replacing \hat{\Theta}_{ML} \to \Theta, that is,

```
\mathcal{L}(T_m; \hat{\Theta}) = \mathcal{L}(T_{m-1}; \hat{\Theta}) \times \frac{\Pr[\tau_m|T_{m-1}, \hat{\Theta}_1; \mathcal{H}_1]}{\Pr[\tau_m|T_{m-1}, \hat{\Theta}_0; \mathcal{H}_0]}; \quad m = 0,1,\dots,M \tag{8.56}
```

Anticipating Gaussian models (exponential family [38]) for our unknown parameters, we develop the logarithmic form of the sequential likelihood decision function. Simply taking the natural logarithm of Eq. (8.56), that is, \Lambda(T_M; \Theta) := \ln \mathcal{L}(T_M; \Theta), we obtain the log-likelihood sequential decision function as

```
\Lambda(T_m; \hat{\Theta}) = \Lambda(T_{m-1}; \hat{\Theta}) + \ln \Pr[\tau_m|T_{m-1}, \hat{\Theta}_1; \mathcal{H}_1] - \ln \Pr[\tau_m|T_{m-1}, \hat{\Theta}_0; \mathcal{H}_0] \tag{8.57}
```

Using these formulations, we develop the detection algorithm for our problem next. We should note that we only consider the “threat detection problem” in this section.

# 8.3.5 Sequential Detection for Fission Processes

Here we start with the results of Section 8.3.4 and incorporate the physics of the fission process. For fission detection, we start with the simple neutron