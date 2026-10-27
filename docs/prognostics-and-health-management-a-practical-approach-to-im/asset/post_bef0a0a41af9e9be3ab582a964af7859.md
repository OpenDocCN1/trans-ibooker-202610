
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_156_0.png)

Figure 3.18 Simulated CBD-based signature: FD = CBD – NM.

## Example 3.11

You compare the plots and conclude they are very closely correlated. To complete your verification that Eqs. (3.2) and (3.6) are correct for your application, you examine the design of your experimental test bed and the design of the SMPS you are prognostic enabling and determine the following:

- The SMPS uses pulse-width modulation with voltage feedback, which linearizes the change in frequency: the circled 1 in Figure 3.19, bottom plot.
- Capacitance degradation is an injected fault mode wherein capacitance is discretely removed, which causes a step-like change in the output: see (2) in Figure 3.19, bottom plot.

## 3.3.3 Verifying FFP Modeling

You need to verify your FFP modeling even when you do not intend to use FFP signatures. Your modeling should produce a characteristic FFP curve that is the characteristic CBD curve, except that the curve is shifted downward; it is at or below 0 in the absence of degradation; and the units of measure have changed from physical units such as voltage, amperes, and resistance to a ratio of values such as [voltage/voltage], and so on.

## Example 3.12

An example of verifying FFP modeling is to verify that Eqs. (3.7) and (3.8) are correct for your application:

FFP_i = (FD_i - FD_0)/FD_0
FFP signature = FFP = {FFP_m, FFP_{m+1}, ..., FFP_{m+n-1}, FFP_{m+n}}

- Use your simulated FD data to create and plot a simulated FFP signature such as that shown in the top plot in Figure 3.20.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_157_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_157_1.png)

Figure 3.19 Differences: experimental and simulated signatures.

- Compare the plots of the simulated and experimental-based FFP signatures, such as that shown in the bottom plot in Figure 3.20.

## 3.3.4 Verifying DPS Modeling

Similar to verifying FFP modeling, you need to verify your DPS modeling.

## Example 3.13 Use Eq. (3.16) and plot the DPS data using both the simulated data and experimental data:

- The DPS using simulated data from a square root function is ideal (see Figure 3.21, top plot).

Figure 3.20 Simulated (top) and comparison of experiment (bottom) FFP signatures.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_158_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_158_1.png)

- The DPS using experimental data, while not ideal, is less curvilinear compared to the CBD-based and FFP-based signatures (see Figure 3.21, bottom plot).

## 3.3.5 Verifying DPS-Based FFS Modeling

A final step is to verify the model for a DPS-based FFS.

## Example 3.14

Transform your simulated DPS data into a DPS-based FFS (see the top plot in Figure 3.22, and verify that it is ideal (for a constant rate of degradation). Then compare your simulated result to your experimental result (right-side plot in Figure 3.22).

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_159_0.png)

## 3.4 Evaluation of FFS Curves: Nonlinearity

So far in this chapter, we have successfully developed and verified modeling to produce signatures:

| 公式 | 说明 |
|---|---|
| $FD_i = CBD_i - NM$ | Feature data point – Eq. (3.7) |
| $FFP_i = (FD_i - FD_0)/FD_0$ | FFP signature point – Eq. (3.8) |
| $DPS_i = 1 - (FD_0/FD_i)^2$ | DPS point – Eq. (3.15) |
| $FFS_i = 100\,DPS_i/FL$ | FFS point (using DPS data) – Eq. (3.18) |

The final transform uses Eq. (3.18) to produce a DPS-based FFS to achieve an objective of transforming CBD signature data to FFS data: an ideal DPS-based FFS is a straight line

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_160_0.png)

between the 0% and 100% data points. Prior to that, we developed a model, Eq. (3.10), for transforming FFP signature data into FFP-based FFS data, and we made the following qualitative assessment:

A disadvantage of FFP-based FFS data is that the signature is generally curvilinear: it has the same characteristic curve as the original CBD signature.

Figure 3.15 contains three plots of example FFS data: (i) a DPS-based FFS, (ii) an ideal FFS transfer curve, and (iii) an FFP-based FFS. A qualitative assessment of those curves might include the following:

- The example DPS-based FFS is more curvilinear compared to the ideal FFS.
- The example FFP-based FFS is also more curvilinear compared to the ideal FFS.
- The example DPS-based FFS is less curvilinear compared to the FFP-based FFS.

But a visual inspection of the plots in Figure 3.15 indicates the latter qualitative assessment is not true for every data-point-by-data-point comparison of the DPS-based FFS

to the FFP-based FFS. What we need is quantitative assessment – and we advocate a quantitative method for assessing the nonlinearity of FFS curves that is based on integral nonlinearity (INL) measurements that are commonly used for assessing the linearity of output-transfer curves of data converters (Texas Instruments 1995; Carr and Brown 2000; Jenq and Qiong 2002).

## 3.4.1 Sensing System

A sensing system comprises everything between a monitored node and the input port of a predication system: the node, the sensor, and all the intervening frameworks. The prediction system acts upon the input from the sensing system to produce prognostic information used to provide a prognosis about a future failure: sensing hardware, data/domain converters, signal conditioning, data fusion, and so on.

In a PHM system, the sensing system provides input to a prediction system that processes the input from the sensing system to produce prognostic information that includes (i) RUL, (ii) SoH, and (iii) PH. An ideal RUL curve is a decreasing straight line between the point in time of the onset of degradation and the point in time of functional failure: the amplitude of that line is the time between functional failure and the onset of degradation. Similarly, an ideal SoH curve is a decreasing straight line between the point in time of the onset of degradation and the point in time of functional failure: an ideal SoH curve decreases linearly from 100% to 0% in amplitude. An ideal PH curve is a horizontal straight line comprising the sum of the current time of a sample and the RUL at that time.

The linearity of a sensing system is an assessment of how the measured curve produced by the sensing system deviates from the ideal curve (TI 1995; Carr and Brown 2000). The output of the sensing system is an FFS, and an ideal FFS input curve to the predication system is a straight line having a value of 0 just before the instant in time that degradation begins and a value of 100 at the instant when the monitored prognostic target is deemed to have functionally failed. For example, the left side of Figure 3.22 is a plot of an ideal FFS curve, and the right side of that figure is a plot of our example DPS-based FFS; as shown, that curve deviates from an ideal FFS curve.

## 3.4.2 FFS Nonlinearity

We define functional-failure signature nonlinearity (FNL) as a measure of the deviation between the actual FFS and an ideal FFS, very much like an INL assessment of an analog-to-digital data converter (ADC) or transducer output transfer curve (Texas Instruments 1995; Carr and Brown 2000; Jenq and Qiong 2002). A point-by-point FNL is a measurement of the error between an actual FFS value and the ideal FFS value at that point in time (see Figure 3.23). A total error (FNLₑ) provides an assessment of the nonlinearity of the FFS curve (see Figure 3.24).

# Calculating FFS Nonlinearity (FNL)

Calculating FNL is a post-processing type of operation using FFS data, because we need to first calculate the total time between the point in time of the onset of degradation and the point in time when functional failure occurs:

$$TTF = t_{FAILURE} - t_{ONSET} \tag{3.21}$$

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_162_0.png)

Figure 3.23 Illustration of point-by-point FNL comparison.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_162_1.png)

Figure 3.24 Illustration of total FNL_E comparison.

Then for each point in the set {FFS_i}, we create an ideal point,
IDEAL_FFS_i = 100(t_i − t_ONSET)/TFF (3.22)
and calculate point-by-point nonlinearity,
FNL_i = FFS_i − IDEAL_FFS_i (3.23)
and calculate total nonlinearity,
FNL_E = max(abs({FNL_i})) (3.24)
