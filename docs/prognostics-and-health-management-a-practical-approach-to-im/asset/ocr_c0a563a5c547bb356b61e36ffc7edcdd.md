**Table 5.4** Example of lookup values for resistance-temperature, platinum RTD (ITS 1990).

| T | −10 |  |  |  |  |  |  |  |  |  |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
|  | 96.09 | 95.69 | 95.30 | 94.91 | 94.52 | 94.12 | 93.73 | 94.34 | 92.95 | 92.55 | 92.16 |
| 0 | 100.0 | 99.61 | 99.22 | 98.83 | 98.44 | 98.04 | 97.65 | 97.26 | 96.87 | 96.48 | 96.09 |
| 0 | 100.00 | 100.39 | 100.78 | 101.17 | 101.56 | 101.95 | 102.34 | 102.73 | 103.12 | 103.51 | 103.90 |
| 10 | 103.90 | 104.29 | 104.68 | 105.07 | 105.46 | 105.85 | 106.24 | 106.63 | 107.02 | 107.40 | 107.79 |

are used to calculate the resistance of the RTD sensing element, *R*<sub>RTD</sub> = *V*<sub>RTD</sub>/*I*<sub>RTD</sub>. Then the calculated resistance is the result of fusing noisy measurements consisting of quantization, nonlinearity, gain, and other errors.

That noisy value of resistance is then used as an input to a table lookup (see Table 5.4) to select a temperature value: each column represents a one-degree change in temperature. The temperature is an indirect measurement involving two analog-to-digital conversions, a data fusion, and then a table lookup that is a form of digitization coupled with a transform from one data type (resistance) to another data type (temperature). The effects of noise can be compounded and become unacceptably high.

In cases where you determine that an active data converter, such as an ADC, is a major contributor to noise issues that require remediation, you could replace that data converter with one that has a higher ENOB and/or less nonlinearity, and/or a faster sampling rate, and/or a higher input-frequency range, and so on. Again, though, you are invariably faced with higher exploitation costs, more weight, and more power input to drive your sensing system.

When noise issues are traced back to quantization errors because of the use of lookup tables, you might consider solving a complex expression—tables often are used to replace such processing. Consider, however, the complexity of such solutions: for example, lookup tables for platinum-wire RTD applications replace solving a complex Callendar-Van Dusen expression (ITS 1990),

$$R/R_0 = 1 + A\,T + B\,T^2 - C\,(T - 100)T^3$$

for

$$-200^\circ\mathrm{C} \leq T \leq 0^\circ\mathrm{C}$$

$$R/R_0 = 1 + A\,T + B\,T^2$$

for

$$-0^\circ\mathrm{C} \leq T \leq 661^\circ\mathrm{C}$$

where

$T$ = temperature in °C, $R$ = resistance at temperature $T$, $R_0$ = resistance at 0 °C
$A = \alpha(1 + \delta/100)$, $B = -(\alpha\,\delta)/(100^2)$, $C = -(\alpha\,\delta)/(100^4)$
$\alpha = (R_{100} - R_0)/(100 + R_0)$
$\delta = (R_0[1 + \alpha\,260] - R_{200})/(4.16\,R_0\,\alpha)$

Solving more complex expressions to improve accuracy and reduce noise usually requires more memory to store variables and parameter values, more processing