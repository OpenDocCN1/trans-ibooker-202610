
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_287_0.png)

**Figure 6.30** Diagram of a test bed to inject faults into an EMA.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_288_0.png)

**Figure 6.31** Sampled- and windowed-phase currents: no load (top) and extra load (bottom).

transform, and characterize FD as signatures. Your EMA experiments yield very noisy data that hides current changes related to degradation; so, you employ a windowing technique such as that shown in Figure 6.31. The right side of the right-hand plot exhibits little, if any, increase – this and other experiments confirm that for prognostic purposes, sampling should be done when the EMA is moving a load upward rather than downward.

## 6.4.3 Test Bed: Results

You use the test bed during the initial design and test phase for the following failure modes by Pareto ranking: (i) power supply, loss of filtering capacitance; (ii) EMA, switching power transistor; (iii) EMA, excessive loading on the motor shaft; and (iv) EMA, motor winding.

### Power Supply

The fault-injection experiments using your test bed are successful, as evidenced by Figure 6.32.

### EMA

The EMA experiments yield mixed results: in the absence of any degradation in an EMA, the measured phase currents are, with the exception of noise, unchanging and equal in magnitude in the middle half of actuator movement. Examination of Figure 6.33, and comparing raising a normally loaded surface (top plot) and an excessively loaded surface (bottom plot), exhibit an increase in phase current.

However, despite inserting hundreds of milliohms of resistance into drains of the power-switching transistors, you are unable to detect any change in the measured phase currents. You simulate the power supply with degraded transistors and confirm that despite a half-ohm degradation, there is very little change in current amplitude (see Figure 6.34): less than 1.5% (9 mA) from a nominal root mean square (rms) value of 650 mA. The latter observation leads you to more closely examine the experimental data – especially data for a high level of degradation.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_289_0.png)

**Figure 6.32** FFP signature of fault-injected SMPS.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_290_0.png)
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_290_1.png)

**Figure 6.33** Sampled-phase currents: no load (top) and extra load (bottom).

## 6.5 Special RMS Method for AC Phase Currents

You run an experiment in which you introduce a severe degradation into one of the six power-switching transistors, which results in a level shifting of the reference for the three phase currents (Figure 6.35). You observe the following: (i) a very small change in the measured magnitudes of the phase currents and (ii) significant shifting in the reference levels of the phase currents. PoF analysis and FMEA confirm that because the three windings of the motor are tied to a floating reference point, and due to the design of the H bridge (refer back to Figure 6.30), the observed shifting is an expected result.

**SIMULATED PHASE CURRENTS**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_291_0.png)

**Figure 6.34** Current magnitude: no degradation (high), degraded transistor (reduced amplitude).

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_291_1.png)

**Figure 6.35** Shifted levels due to a degraded power-switching transistor.

Examination of the EMA circuitry (Figure 6.30) and PoF analysis confirms that level shifting of the reference (zero crossover) is real rather than an error in experiment or measurement: the level shift is caused by the necessity to maintain a zero-sum current, even when current imbalances are caused by degradation of the power-switching transistors. You solve the issue of low-amplitude measurement values by splitting the calculation of rms into two: a positive rms and a negative rms. You only use current values above or below a selected threshold: the dark samples seen in Figure 6.36.

**SIMULATED PHASE A CURRENTS - TRUNCATED**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_293_0.png)

**Figure 6.37** Special rms: threshold and truncation.

Let N_S = minimum negative amplitude of the phase current in a burst of 32 000 samples.

Then

p(s) = m(s) when m(s) ≥ 0.80 P_S, else p(s) = 0

n(s) = m(s) when m(s) ≤ 0.80 N_S, else n(s) = 0

P_RMS = (1/P) Σ_{s=1}^{s=n} p(s)  (6.1)

N_RMS = (1/N) Σ_{s=1}^{s=n} n(s)  (6.2)

### 6.5.3 Special Peak-RMS Method: FFP Computational Routine

Let I_120 = nominal value for phase current and AT = 0.80. Then for each of the three phase currents, the sample value for an FFP is calculated using Eq. (6.3) for each data point. The results are plotted in Figure 6.38:

P_PK = (P_RMS − (AT P_RMS)) positive feature data  (6.3)

N_PK = (N_RMS + (AT N_RMS)) negative feature data  (6.4)

**PHASE CURRENTS: SPECIAL RMS**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_294_0.png)

**Figure 6.38** Special rms applied to three phase currents.

### 6.5.4 Peak-RMS Method: EMA

You design and run experiments with and without fault injection for the following failure modes:

- Excessive loading on the motor resulting from binding, friction, or excess weight on whatever surface the EMA is moving.
- Degrading motor winding resulting from, for example, excessive heat, leading to damaged winding insulation with resulting shorting of motor windings.
- Degrading power-switching MOSFETs in the H bridge controller for the EMA, with resulting decreased voltage and power delivered to the motor.

#### Excessive Loading: FFP Signature Computational Routine

You obtain data with and without excessive loading, apply Eq. (6.1)–(6.4), and then fuse and transform the feature data using Eq. (6.5) for each of the three phase currents to obtain and plot FFP data points as shown in Figure 6.39:

FFP = (P_PK − N_PK + 240)/120 positive feature data (6.5)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_295_0.png)

**Figure 6.39** FFP signatures due to loading.

#### Excessive Loading: FFS Computational Routine

You analyze the results of your FFP computational routine and conclude the following:

1. Although there is a slight curvature, the signature is sufficiently linear to meet requirements. FFP-to-DPS transform will not be performed.
2. You will need to apply a noise margin (NM) of about 0.025, because in the absence of degradation, an FFP signature is above the zero threshold.
3. You should apply noise mitigation in the form of data smoothing. You decide to use point averaging.
4. You decide to use a failure threshold of 0.25 – approximately a 25% increase in current due to extra loading due to weight and/or friction.

You apply the NM (Eq. (6.6)); you use data smoothing to create the three FFP signatures shown in Figure 6.40. Next, you select the highest amplitude FFP; you select a failure threshold; and you use Eq. (6.7) to transform that FFP to an FFS (Figure 6.41).

```
FFP = FFP − 0.025   (FFP with applied NM)   (6.6)
FFS = 100 (FFP / 0.25)   (obtain FFS for 0.25 failure threshold)   (6.7)
```

#### Motor Winding: FFP and FFS Signatures

You obtain data with and without injected shorted-winding faults, and you apply the same methods you used for the loading experiments. The FFP signatures are shown in Figure 6.42. You observe the following:

1. Only one FFP signature – that for Phase C – is increasing, instead of all three.
2. You can use the same FFS computational routine as for the EMA loading case (Figure 6.43).
3. The different behavior of the signatures: one signature changes versus three signatures increasing, for diagnostic purposes (fault isolation).
4. There is an unavoidable offset error (see Figure 6.43) due to amplitude differences in the phase currents in the absence of degradation.

**SMOOTHED SIGNATURES - WINDING FAULT STARTS @ 1000**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_297_0.png)

**Figure 6.42** Smoothed FFP signatures due to winding faults.

**SINGLE FFS - WINDING FAULT STARTS @ 1000**

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_297_1.png)

**Figure 6.43** FFS: EMA winding.
