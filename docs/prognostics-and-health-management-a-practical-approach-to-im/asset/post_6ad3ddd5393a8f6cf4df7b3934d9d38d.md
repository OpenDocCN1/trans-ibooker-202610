
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_163_0.png)

Figure 3.25 Example plot of FFP-based and DPS-based FNL_i.

where max is a maximum function and abs is an absolute-value function. Example plots on nonlinear errors are shown in Figure 3.25.

## Example 3.15

Previously, you used an FL value of 0.7 and created an FFP-based FFS (in 3.7); you transformed the FFP-based FL value of 0.7 to a DPS-based value of 0.65; then you created a DPS-based FFS (3.9); and you compared the results (Figure 3.15) and qualitatively concluded that the DPS-based FFS was less curvilinear compared to the FFP-based FFS. Now, verify the qualitative conclusion using Eqs. (3.21–3.24) for both sets of FFS data and make a quantitative assessment.

## 3.5 Summary of Data Transforms

Using an SMPS as an example, we described and showed the following signals and the transforms that were performed:

- SMPS output (left side of Figure 3.26). Analog voltage of direct current (DC) and alternating current (AC) component features and noise. One of the features is damped-ringing responses to abrupt changes in load.
- Ringing response (right side of Figure 3.26). An intelligent sensor might do the following: present a series of abrupt, low-power changes in load to the SMPS at timed intervals (sampling periods); capture the ringing responses; convert analog voltage to digital data; calculate an average frequency for each of several sinusoids in the response; calculate an average value of the average resonant frequency of each of the series of ringing responses; calculate the average frequency as a sample value; and output that average as a scalar CBD_i value.
- CBD signature (left side of Figure 3.27). Sensor data is transformed into a CBD signature by subtracting the value of a noise margin from each received CBD_i to

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_165_0.png)

**Figure 3.27** CBD signature and FFP signature.

create a feature point, $FD_i$. The set of $FD_i$ values, $\{FD_i\}$, is a CBD signature, as in Eq. (3.7):

$$FD_i = CBD_i - NM$$

- **FFP signature** (right side of Figure 3.27). A set of $FD_i$ values is transformed into a set of $FFP_i$ values by subtracting from each $FD_i$ value a nominal value of FD and dividing the result by that nominal value. The set of $FFP_i$ values, $\{FFP_i\}$, is an FFP signature, as in Eq. (3.9):

$$FFP_i = (FD_i - FD_0)/FD_0$$

- **FFP-based FFS** (left side of Figure 3.28). An FFP set, $\{FFP_i\}$, is transformed into $\{FFS_i\}$ by dividing each value in the set by a functional-failure level, FL, and multiplying the result by 100, as in Eq. (3.10):

$$FFS_i = 100\ FFP_i/FL$$

- **DPS** (right side of Figure 3.28). An FFP set, $\{FFP_i\}$, is transformed into $\{DPS_i\}$ by transforming the FFP model into a new model that is a function of a parameter, $P_0$, and a change in that parameter, $dP$, and then solving for $dP_i/P_0$ in terms of the feature parameters $FD_i$ and $FD_0$, as in Eq. (3.15):

$$DPS_i = 1 - (FD_0/FD_i)^2$$ for this particular FFP model

- **DPS-based FFS** (Figure 3.29). A set of DPS values, $\{DPS_i\}$, is transformed into a DPS-based FFS, $\{FFS_i\}$ by dividing each $DPS_i$ value in the set by an $FL$ value and multiplying the result by 100, as in Eq. (3.18),

$$FFS_i = 100\ DPS_i/FL$$

- where $FL$ is calculated using Eq. (3.17):

$$FL_{DPS} = 1 - (1/(1 + FL_{FFP}))^2$$

### High-Level Procedure for Producing a DPS-Based FFP

A high-level procedure for producing a DPS-based FFP is the following (refer to Figure 3.30): (i) select one or more output nodes to monitor for prognostic purposes; (ii) develop or acquire sensors to isolate CBD leading indicators of interest and to extract selected FD; (iii) signal condition and transform the collected FD to produce a CBD-based signature – Eq. (3.6); (iv) further condition as necessary and transform CBD signature data into FFP signature data – Eqs. (3.7) and (3.8); (v) transform FFP signature data into DPS data using Eqs. (3.8) and (3.15); and (vi) transform DPS data into an FFS using Eqs. (3.17) and (3.18). You use Eqs. (3.21)–(3.24) to quantitatively assess the nonlinearity of FFS data.

### Qualitative Assessment

The biggest advantage of the DPS approach is that DPS-based FFS data is more linear compared to that based on FFP data. Additional advantages are the following:

- Similar to an FFP signature, the amplitude of $\{DPS_i\}$ data is dimensionless.
- The number of required models is reduced.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_167_0.png)

**Figure 3.28** FFP-based FFS and DPS.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_168_0.png)

**Figure 3.29** DPS-based FFS.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_168_1.png)

**Figure 3.30** Procedural diagram for producing a DPS-based FFS.

Using Eq. (3.14) and the results of the examples used to verify modeling, we make the following inductive observations regarding the set `{DPS_i}`, regardless of the type of an underlying FD model (such as a linear or power function) and regardless of how FD changes with respect to time:

- When a parameter `dPi` changes as a linear function with respect to time, `{DPS_i}` changes linearly with respect to time.
- When a parameter `dPi` changes as a power function with respect to time, `{DPS_i}` changes as a power function with respect to time.
- When a parameter `dPi` changes exponentially with respect to time, `{DPS_i}` changes exponentially with respect to time.

The biggest advantage is the following: `{DPS_i}` is less curvilinear than its underlying `{FFP_i}` signature data, and so `{DPS_i}` data is particularly useful as input to prediction algorithms for processing to predict a future time of failure for a prognostic target.

## 3.6 Degradation Rate

In the previous section of this chapter, we saw that an ideal DPS-based FFS is a linearly increasing line from 0 or less (no degradation) to 100 or more (functionally failed): an ideal DPS-based FFS is a straight line between the 0% and 100% points. Even a non-ideal DPS-based FFS was shown to be less curvilinear than its underlying FFP signature (refer back to Figure 3.29). The linearity of a DPS-based FFS is dependent on the linearity of the rate of change in a degrading parameter, `dD`: if the degradation rate with respect to time is constant, the DPS and the DPS-based FFS will be linear; otherwise the degree of non-linearity of the DPS and the DPS-based FFS is dependent on the degree of nonlinearity in the degradation rate.

### 3.6.1 Constant Degradation Rate: Linear DPS-Based FFS

As we have seen, an idealized DPS for a constant degradation rate results in an idealized linear DPS-based FFS. And even when a DPS is not ideal, the DPS-based FFS is more linear (less curvilinear) compared to the characteristic curve for its DPS. The examples and exemplary data we have been using are a result of assuming a constant degradation rate such as that modeled as follows:

$$dP_i = D_0(t_i)^\delta \tag{3.25}$$

in which `δ = 1.0` for a constant degradation rate and `t_i` equals

$$t_i = (t_S - t_D) \tag{3.26}$$

where `t_S` is the time of a sample and `t_D` is the time of the onset of degradation: `t_S ≥ t_D`.

**Example 3.16** Show that the DPS-based signature for our example is always linear given a constant degradation rate and nonlinear for a nonconstant degradation rate. Using Eqs. (3.25) and (3.12),

$$DPS_i = dP_i/P_0 = D_0/P_0(t_i)^\delta$$

so that for `δ = 1`, `DPS_i` is linear and nonlinear otherwise.

**Example 3.17** Show that the FFP-based signature for our example is always nonlinear, even when $\delta = 1$. Using Eq. (3.15),

$$DPS_i = 1 - (FD_0/FD_i)^2 \to FD_i/FD_0 = \sqrt{1 - (D_0/P_0)(t_i)^\delta}$$

and using Eq. (3.8),

$$FFP_i = (FD_i - FD_0)/FD_0 = FD_i/FD_0 - 1$$

$$= (1/\sqrt{1 - (D_0/P_0)(t_i)^\delta}) - 1$$

which is nonlinear for all nonzero values of $\delta$.

### 3.6.2 Nonlinear Degradation Rate

A non-idealized DPS occurs when the degradation rate is not constant: it is nonlinear with respect to time. However, we will see that even when a DPS is not ideal, a DPS-based curve is still more linear (less curvilinear) compared to its FFP signature.

**Example 3.18** Plot a DPS, $dP/P_0$, using Eq. (3.25), where $D_0 = 0.90/200$ is valid after the onset of degradation, degradation begins at $t = 51$, and $\delta = 1.0$ for a time period of 1 to 250 days. Compare the dotted-line plot in Figure 3.31 to the solid-line plot, in which $dP = (0.75/200)t^{1.05}$.

Notice that the differential nonlinearity (DNL) between the ideal, linear DPS and the non-ideal, curvilinear DPS transfer curves is very small. The nonconstant degradation rate, in this case, increases the nonlinearity of the CBD and FFP signature curves from that seen on the left side of Figure 3.32. The nonlinearity increases even further, as shown

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_170_0.png)

**Figure 3.31** Linear DPS (left side) and nonlinear DPS (right side) data plots.
