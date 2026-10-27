**Table 3.1** Range of failure thresholds: Lot 1 and Lot 2.

| Boundary condition | High-end failure level | Low-end failure level |
|---|---:|---:|
| Lot 1: Nominal frequency 27 kHz | 43.2 | 45.9 |
| Lot 2: Nominal frequency 21 kHz | 33.6 | 35.7 |

**Example 3.4** You evaluate the results of your CBD modeling and decide that you need to define at least two signature models for every SMPS lot in which the nominal resonant frequency varies from a nominal value by more than, for example, 0.3 kHz (1.25% of 24 kHz); the exact range is application dependent. Additionally, manufacturing variations cause the output filtering capacitance to vary, and you calculate the nominal resonant frequency of a given SMPS as ranging from 21 to 27 kHz (refer back to Table 3.1): the spread of 6 kHz and a 0.3 kHz increment mean you need to define and use 20 CBD-based models.

# 3.2.2 FFP Signature

One way to simplify modeling is to normalize CBD signature data to create FFP signature data, and at the same time apply a noise margin to the data (see Figure 3.9). The procedure is to subtract from CBD an NM to create a FDᵢ value, then subtract a nominal FD₀ value and divide the result by the nominal FD₀ value:

$$FD_i = CBD_i - NM \tag{3.7}$$

$$FFP_i = (FD_i - FD_0)/FD_0 \tag{3.8}$$

$$\text{FFP signature} = FFP = \{FFP_m, FFP_{m+1}, \ldots FFP_{m+n-1}, FFP_{m+n}\} \tag{3.9}$$

FFP signatures are preferable to CBD signatures because the effect of noise on prediction accuracy is mitigated by the use of a noise margin and because normalization results in relative units of measure in amplitude instead of units of measure such as kHz. Instead of a model for a nominal frequency of 24 kHz, a model for a nominal frequency of 24.3 kHz, a model for a nominal frequency of 24.6 kHz, and so on, you need only one model, which reduces both modeling and processing complexity when producing prognostic information.

**Example 3.5** You decide to transform your CBD signature data into FFP signature data by using Eq. (3.8) and the following parameter values: $FD_0 = 24.0\ \text{kHz}$ and $NM = 0.6\ \text{kHz}$. The plot of the resulting FFP signature is the left-side plot in Figure 3.10. You examine the plot and conclude the following:

+   - The amplitude values are no longer Hz or kHz units of measure: they are relative units of measure having no units.
+   - Instead of labeling the y-axis as [AU] for arbitrary units or [ratio], you choose to use a more informative designation, [kHz/kHz].
+   - The signature starts at a value of about 0.1 instead of the less-than-0 value you expected.