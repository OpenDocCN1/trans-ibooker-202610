
Keep in mind that in the air pollution work, it is typical to define standard temperature as 25°C (298K). Where the volume per mole of an ideal gas has the value 22.4l/mol at T = 0°C (273K) and P = 1 atm.

So the final version is

```
$$ C_{\mathrm{mass}} \left( \mu \mathrm{g} / \mathrm{m}^{3} \right)=40.9 C_{\mathrm{ppm}} \mathrm{MW}_{\mathrm{P}} \text{at } 25^{\circ} \mathrm{C}\left(298 \mathrm{~K}\right) \quad \text{(3.13)} $$
```

where $ C_{\mathrm{mass}} $ = mass concentration (μg/m³) and $ C_{\mathrm{ppm}} $ = volume concentration (ppm).

**Example 3.6** (a) What is the AAAQS for ozone (eight-hour averaging time), expressed in μg/m³? (b) The average daily concentration of sulfur dioxide is observed to be 410 μg/m³ at 25°C and 1 atm in an industrial complex. What is the 24-hour average concentration of SO₂, expressed in ppm?

# Solution

+   (a) The ozone standard is 0.075 ppm. Using Eq. (3.13),

```
$$ C_{\mathrm{mass}}=\left(40.9\right)\left(0.075\right)\left(48.0\right)=147 \mu \mathrm{g} / \mathrm{m}^{3} \text{at } 25^{\circ} \mathrm{C} . $$
```

+   (b) The molecular weight of sulfur dioxide is 64.0. Rearranging Eq. (3.13) to solve for the SO₂ concentration in ppm, we find that

```
$$ \mathrm{SO}_{2} \mathrm{ppm}=\frac{410}{\left(40.9\right)\left(64.0\right)}=0.157 \mathrm{ppm} $$
```

This concentration of SO₂ is above the NAAAQ primary standard of 0.14 ppm (based on a 24-hour average value presented in Table 3.9).

**Example 3.7** The maximum concentration of dust (particulate matter, PM-10) in atmosphere during a 24-hour period was found to be 0.000128 grain per dry standard cubic foot (gr/dscf). What is the equivalent concentration in μg/m³?

# Solution

From Table A.1, it is found that 7000 gr = 454.4 g (1.0 lb) and 1 ft³ = 0.028 33 m³. Hence 1 gr/ft³ equals 2.29 g/m³. Therefore,

```
$$ \begin{aligned} & \text { Concentration }=0.000128 \mathrm{gr} / \mathrm{ft}^{3} \\ & \times\left(2.29 \mathrm{~g} / \mathrm{m}^{3}\right)\left(1 \mathrm{gr} / \mathrm{ft}^{3}\right)\left(10^{6} \mu \mathrm{g} / \mathrm{g}\right)=293 \mu \mathrm{g} / \mathrm{m}^{3} . \end{aligned} $$
```

This value is relatively high and exceeds the 24-hour NAAAQ ambient standard of 150 μg/m³ (based on a 24-hour average value presented in Table 3.9).

## 3.11 Other Applications of the Ideal Gas Law

As mentioned in Section 3.10, the ideal gas law is extremely important to air pollution engineers. Compliance with federal and state laws requires not only proper environmental engineering design and operation of pollution abatement equipment but also careful analysis and accurate measurements of specified pollutants and environmental quality parameters. In the design process and in other types of calculations, engineers who have complete understanding of the ideal gas law will achieve greater accuracy with a smaller expenditure of their time. In this section, through the USE of example problems, we will review various uses of the ideal gas law.

Emission sampling is another important area for which a good understanding of the ideal gas law and the equipment used to measure gas flow is important. Many poorly performing systems are results not of bad design practice but of poor sampling techniques.

Some important considerations in gas sampling are equipment calibration, leaks, sample handling (condensation, adsorption), process operation fluctuations, and representative sampling of the process stream. We will address some of these considerations in the following examples.

When Eq. (3.5) was first presented, one widely used value of R was given for a specific set of units. There are many numerical values of R depending on the units chosen for P, T, V, and n. Table C.3 provides a list of several possible values of R, and the derivation of one such value is illustrated in Example 3.8.

**Example 3.8** Given that the molar volume of an ideal gas is 22.4136 l/gmol at 273.15K and 1.000 atm, calculate the value of R in units of mm Hg-ft³/lbmol-R.

# Solution

```
$$\begin{gathered} R=\frac{P V}{n T}=\left(\frac{V}{n}\right) \frac{P}{T} \\ R=\frac{22.4136 l \quad 1.000 \mathrm{atm} \quad 1 \mathrm{ft}^{3} \quad 453.6 \mathrm{gml}}{\mathrm{gmol} \quad 273.15 \mathrm{~K} \times 28.3161 \quad 11 \mathrm{lbmol}} \\ \times \frac{760.0 \mathrm{mmHg}}{1.000 \mathrm{~K} \times 1.800 \mathrm{R}^{2}} \\ \times \frac{1.000 \mathrm{atm}}{760.0 \mathrm{mmHg}} \times \frac{1.800 \mathrm{R}^{2}}{1.800 \mathrm{R}^{2}} \\ =555.0 \mathrm{mmHg}-\mathrm{ft}^{3} / \mathrm{lbmol}-\mathrm{R} \end{gathered}$$
```

In this example problem, more significant figures were used than are typically used in engineering calculations. Engineers often are satisfied with only three- or four-digit significance. Indeed, the data derived from field measurements are often accurate only to two significant figures. In an age of pocket calculators and personal computers, when it has become routine to obtain numerical answers to eight decimal places, it is important to understand the significance of the numbers we work with. When high accuracy and precision are needed and justified, then many significant figures should be used. However, in many design problems the equations are modified by empirical relationships and must be considered to be approximate.

The weighted-average molecular weight of a mixture of ideal gases is calculated using mole fractions or volume fractions (not mass fractions) as the weighting factors. This is demonstrated in the following example problem.

# Example 3.9 Dry air can be considered to be 78.0% nitrogen (M.W. = 28.0), 21.0% oxygen (M.W. = 32.0), and 1.0% argon (M.W. = 40.0). Calculate the average molecular weight of air.

# Solution

```
M.W._avg = 0.78(28) + 0.21(32) + 0.01(40) = 28.96
```

The density of an ideal gas mixture is a molar average of the densities of the pure components. The density can also be calculated by using the molar-average molecular weight of the gas mixture in Eq. (3.8), as shown in the next example. Note that the density of a gas depends on the pressure and temperature.

# Example 3.10 Calculate the density of air at standard conditions (T = 25°C and P = 1 atm).

# Solution

```
ρ_N₂ = (1.00)(28.0) / [(0.08206)(298)] = 1.145 g/l
ρ_O₂ = (1.00)(32.0) / [(0.08206)(298)] = 1.309 g/l
ρ_Air = (1.00)(40.0) / [(0.08206)(298)] = 1.636 g/l
ρ_Air = 0.78(1.145) + 0.21(1.309) + 0.01(1.636) = 1.184 g/l
```

Alternatively,

```
ρ_Air = (1.00)(28.96) / [(0.08206)(298)] = 1.184 g/l
```

## 3.12 Gas Flow Measurement

A fixed number of moles of gas at a certain temperature and pressure occupies a certain volume. If the pressure and temperature of this fixed number of moles of gas are changed, then the new volume can be easily calculated. Equation (3.5) can be written as follows:

```
PV/T = nR  (3.14)
```

Since n is constant in this case, applying Eq. (3.14) to the two sets of conditions yields

```
P_old V_old / T_old = P_new V_new / T_new  (3.15)
```

or

```
V_new = V_old P_old / P_new = T_new / T_old  (3.16)
```

Equation (3.16) can be applied to a static system or to a flow system as long as the number of moles under consideration remains constant.

# Example 3.11 A sample stream of dry gas is being withdrawn from a stack. The stack gases are at 200°C and 730 mmHg. The stream flows through a heated filter, a set of cooled impingers, a small air pump, and then through a flow meter. The rate of flow is determined to be 30.0 l/min at 20°C and 790 mmHg. (a) Calculate the actual volumetric flow rate through the filter (at T = 200°C and P = 730 mmHg). (b) If 1.42 mg of solid particles are collected on the filter in 30 minutes, calculate the concentration of particles in the stack gas (in µg/m³).

# Solution

(a) Q_filter = (30.0)(790/730)(473/293) = 52.4 l/min

(b) Total volume of gas sampled (at stack conditions): 52.4 1/min × 1m³/1000l × 30 min = 1.57 m³ C_part. = 1.42 mg / 1.57 m³ × 10 µg / 1 mg = 904 µg / m³

In the preceding example problem, if a concentration of, say, 500 ppm of NO₂ were measured in the gases exiting the flow meter, then we could state that the concentration in the stack was also 500 ppm. The reason for this is that ppm is a measure of relative concentration and as the total volume of the gas sample changes, so does the volume of NO₂.
