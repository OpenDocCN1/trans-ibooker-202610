
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_246_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_246_1.png)

**Figure 5.19** Experimental data: temperature measurements for a jet engine.

$\{X_{Di}\}$ is a CBD signature. When we apply this method to, for example, temperature data from each of four engines on the same aircraft (top plots in Figure 5.21), we obtain the differential signatures shown in the bottom plots in Figure 5.21.

Although one differential-distance signature in Figure 5.21 is visually increasing, a computational routine would use the largest amplitude value of all four signatures to create a single differential-degradation signature, as shown in Figure 5.22: compare that to the original temperature data shown in Figure 5.19. In Chapter 7, we will provide an example of how to further condition this signature using an algorithm to mitigate large-amplitude perturbations and data smoothing.

**Noisy Data (Spikes & Glitches): Four Engines**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_248_0.png)

**Differential Distance Signature Comparison: 2 vs 4 Engines**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_248_1.png)

**Figure 5.21** Temperature data and differential signatures: four engines on an aircraft.

### Example 5.12

Careful examination of the signature at the crossover amplitude of 0% reveals that there are two crossover points, due to, you conclude, the noise in the data. You decide to employ a moving-average method of data smoothing:

- (i) use a rolling average of three data points;
- (ii) load a three-slot first-in-first-out (FIFO) data queue with the very first data point acquired during the initialization stage;
- (iii) store the FIFO queue and a pointer to a next slot in the queue;
- (iv) when the next data point is acquired, store that value in the data queue;
- (v) update and store the data pointer;
- (vi) calculate a rolling average of the data queue;
- (vii) use the rolling average as the current amplitude to calculate the FFP signature;
- (viii) use the node definition area to store the data queue and pointers;

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_249_0.png)

**Figure 5.22** Composite differential-distance signature.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_249_1.png)

**Figure 5.23** Example of a noisy FFP signature.

- (ix) reduce the NM from 4% to 2%.

Figure 5.25 shows the plot of the smoothed FFP signature: the error in detecting degradation is improved from 15 to 5 days, which is a 67% error reduction.

### Example 5.13

You evaluate the characteristic curve of the data plotted in Figure 5.25 and decide the signature is probably the result of a power function #3 with a power of 1/2. You then transform the FFP signature into a DPS using the following expression (from Chapter 4):

$$DPS_i = 1 - 1/(FFP_i + 1)^{1/n}$$

which results in the plot shown in Figure 5.26.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_250_0.png)

**Figure 5.24** Example of a noisy FFP signature: calculated nominal FD value, reduced NM value.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_250_1.png)

**Figure 5.25** Example of a smoothed (3-point moving average) FFP signature.

### Example 5.14

You define functional failure to occur when the FFP signature reaches a value 0.65, which is transformed into a DPS-based FL:

$$FL_{FFP} = 0.65 \rightarrow FL_{DPS} = 1 - 1/(FL_{FFP} + 1)^2 = 0.63$$

The $FL_{DPS}$ value is used to transform DPS data into FFS data, as plotted in Figure 5.27.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_251_0.png)

**Figure 5.26** Example of a DPS from a smoothed FFP signature.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_251_1.png)

**Figure 5.27** Example of an FFS from a smoothed FFP signature.

### Example 5.15

You calculate and plot the point-by-point FNL using the procedure listed in Table 5.1:

$$TTF = t_{FAILURE} - t_{ONSET}$$
$$IDEAL\_FFS_i = 100(t_i - t_{ONSET})/TTF$$
$$FNL_i = FFS_i - IDEAL\_FFS_i$$

improving the linearity of FFS data to further reduce nonlinearity (Figure 5.31). We reduced total nonlinearity from 13.5% to 8.0%.

In this section, we apply the same data-conditioning methods to the input data shown in Figure 5.6 to demonstrate that the methodology is extendable to other sets of data. Data similar to that shown in Figure 5.6 is first transformed into an FFP signature as shown on the top of Figure 5.32.

### Conditioning FFP Signature Data

We perform the first step in a conditioning procedure, dynamically measuring an average value of the feature rather than using a nominal manufactured value: this lets us

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_255_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_255_1.png)

**Figure 5.32** Ripple voltage: plots of an unsmoothed (top) and smoothed (bottom) FFP signature.

use a smaller NM value. But noise is a problem, even though we used a relatively large margin (5% of the nominal feature value); there is an area of uncertainty in detecting the onset of degradation, as indicated in the plot. Rather than increase the NM, it is better to smooth the FFP data and then evaluate how much NM is required.

### Example 5.16

You apply a four-point rolling average method, evaluate the result, and decide that instead of increasing NM, you can reduce it. The result is the FFP signature plotted on the bottom of Figure 5.32. The FFP signature is much smoother, which allows you to reduce the NM value, resulting in a reduced offset error in detecting degradation.

You transform the FFP signature into DPS and then define functional failure to occur when the FFP signature exceeds 1.0: at that threshold, the ripple voltage has doubled in amplitude compared to the nominal value. You convert the $FL_{FFP}$ value to an $FL_{DPS}$ value of 0.75,

$$FL_{FFP} = 1.0 \rightarrow FL_{DPS} = 1 - 1/(FL_{FFP} + 1)^2 = 0.75$$

and use the 0.75 threshold to transform the DPS data into FFS data. A plot of the DPS from an unsmoothed FFP signature is shown on the top of Figure 5.33, and the DPS from a smoothed FFP signature is shown on the bottom of Figure 5.33. The corresponding plots of the FFS data are shown in Figure 5.34; there is a clear difference in the appearance of the two sets of plots.

Analysis of the data indicates that functional failure is detected when a data point reaches or exceeds the specified failure threshold, as indicated in the plots.

### Reevaluating NM

It is a good idea, after employing data smoothing, to reevaluate your choice of the value for NM. It will often be the case that you can lower the NM, which will typically reduce any offset error, such as the 5.7 hours encountered in Example 5.3.

### Example 5.17

After evaluating the plot shown in Figure 5.33, you decide to change the NM from 3.0 mV to 2.0 mV and rerun the FFP, the DPS, the FL, and the FFS models. The FFS plots for an NM of 3.0 mV and an NM of 2.0 mV are shown in Figure 5.34. The detection of onset of degradation (when FFS > 0) for an NM of 3.0 mV occurs at time = 153.7 days; and for an NM of 2.0 mV, detection occurs at time = −151.3 days: the offset error is reduced from 5.75 days (3.7%) to 3.25 days (2.1%).

### Conditioning FFS Data

The FFP signature is transformed into a DPS, a functional-failure level is defined and converted to a DPS-based FL, and then the DPS data is transformed into FFS data. As each FFS data point is created, it is subjected to the heuristic method from Section 5.4 to further linearize the FFS data.

## 5.5 Summary: Non-Ideal Data, Effects, and Conditioning

This chapter presented topics related to non-ideal data. You learned that noise is an issue in achieving high accuracy in prognostic data: those issues become evident when

**DPS from Unsmoothed FFP**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_257_0.png)

**DPS from Smoothed FFP**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_257_1.png)

**Figure 5.33** Ripple voltage: plots of an unsmoothed (top) and smoothed (bottom) DPS data.

you acquire, transform, and evaluate data. Noise is defined as any variability in data not related to a particular mode of failure; it must be sufficiently reduced and/or mitigated to meet the accuracy requirements of a PHM system for each prognostic-enabled device, component, or assembly.

Methods to reduce the effects of noise and nonlinearity in data were presented related to noise margin, measurement errors and uncertainty, operating and ambient environment, nonlinear degradation, and multiple modes of degradation. You also learned a heuristic method for adjusting FFS data and how to evaluate the end result of data conditioning and model adjustment.
