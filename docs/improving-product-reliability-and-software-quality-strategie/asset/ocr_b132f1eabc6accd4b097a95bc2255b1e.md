| Requirements/options: | Reliability impact/benefit: |
|---|---|
| 1. Temperature stress (Rule #1):
For every 10 °C increase in temperature the useful life decreases by a factor of 2.

2. Ripple current stress (Rule #2):
Stay below 50% of the maximum ripple allowed.

3. Voltage stress (Rule #3):
For voltage stress derating above 67%, the expected life increases by the fifth power of the rated voltage (V_r)/applied voltage (V_a). | Derating greatly improves the life of the electrolytic capacitor in addition to ensuring greater protection from spikes that can cause shorted capacitors. |
| Detail: | |
| The expected life of a capacitor is described as the maximum expected life (Condition a) at rated temperature times the acceleration factors: temperature (T), voltage (V) and ripple current (I).

L_b = L_a × A_t × A_r × A_v

L_b = L_a × 2^((T_0 - T)/10) × 2 × (1 - (I/I_0)^2) × (V_r/V_a)^5

L_a = Lifetime under condition a, L_b = Lifetime under condition b, A_t = Temperature acceleration, A_v = Voltage acceleration, A_r = Ripple current acceleration. | |

# Figure 20.5 DFR guideline for electrolytic capacitor usage. Source: Courtesy of Teradyne, Inc.

# 20.4.1 Derating Guidelines

Derating guidelines are vital to any product development program and should be a part of the DFR guidelines. If there are no derating guidelines in place, chances are good that some of the customer failures are the result of stress levels that exceeded the component specifications. In addition, having sufficient derating in the design will result in increased design margin. Designs with sufficient design margin have lower test parts per million (PPM) failure rates and higher first-pass yields in production.

It is not necessary to develop derating guidelines from scratch. There are several derating guidelines in the industry that can be purchased. The guidelines cover a broad range of users and so they may not be usable by engineers in this form. However, it is a simple task to tailor derating guidelines for your specific application and user