# **Example 4.15**

A sample stream of dry gas is being withdrawn from a stack. The stack gases are 200 °C and 730 mm Hg. The stream flows through a heated filter, a set of cooled impingers, a small air pump, and then through a flow meter, as shown in Figure 4.15. The rate of flow is determined to be 30.0 l/min at 20 °C and 790 mm Hg.

+   1) Calculate the actual volumetric flow rate through the filter (at T = 200 °C and P = 730 mm Hg).
+   2) If 1.32 mg of solid particles is collected on the filter in 30 minutes, calculate the concentration of particles in the stack gas (in μg/m³).

# **Solution**

1) $Q_{\text{filter}}=(30.0)\left(\frac{790}{730}\right)\left(\frac{473}{293}\right)=52.41\ \text{l/min}$

2) Total volume of gas sampled (at stack condition):

$52.41\ \frac{\text{l}}{\text{min}}\times\frac{1\ \text{m}^3}{1000\ \text{l}}\times30\ \text{minutes}=1.572\ \text{m}^3$

$C_{\text{particulate}}=\frac{1.32\ \text{mg}}{1.572\ \text{m}^3}\times\frac{10^3\ \mu\text{g}}{1\ \text{mg}}=841\ \mu\text{g/m}^3$

# **Example 4.16**

A stack (T = 550 °F and P = 750 mm Hg) was sampled using EPA Method 5 (see Figure 4.15). The total gas volume that flowed through the dry gas meter was 2.785 m³ (at T = 60 °F and P = 800 mm Hg). The mass of PM collected was 1.50 g. Also, 72.0 g of H₂O was collected in impingers.

+   a) Calculate the concentration of PM in the stack (μg/m³) at stack conditions.
+   b) The stack gas exits at an average velocity of 20 m/s, and the stack has an inside diameter of 5 m. What is the emission rate of PM in kg/day?
+   c) What is the PM concentration in μg/dscm?

Given:
$T_{\text{stack}}=550^\circ\text{F}=560.85\ \text{K}\qquad P_{\text{stack}}=750\ \text{mm Hg}=0.987\ \text{atm}$
$T_{\text{DG}}=60^\circ\text{F}=288.71\ \text{K}\qquad P_{\text{DG}}=800\ \text{mm Hg}=1.053\ \text{atm}$
$\text{Vol}_{\text{DG}}=2.785\ \text{m}^3=2785\ \text{l}\qquad m_{\text{PM}}=1.50\ \text{gm}$
$m_{\text{H}_2\text{O}}=72.0\ \text{gm}\qquad V_{\text{exit}}=20\ \text{m/s}$
$D_{\text{stock}}=5.0\ \text{m}$

# **Solution**

*Part (a)*
Calculate the moles of dry gas using the ideal gas law (Eq. 3.5)

$R=0.08206\ \frac{\text{atm L}}{\text{gmol K}}$
$n_{\text{DG}}=\frac{P_{\text{DG}}\text{Vol}_{\text{DG}}}{RT_{\text{DG}}}=\frac{(1.053\ \text{atm})(2785\ \text{l})}{0.08206\ \frac{\text{atm l}}{\text{gmol K}}(288.71\ \text{K})}=123.8\ \text{gmol}$

Calculate the moles of water
$\text{MW}_{\text{H}_2\text{O}}=18\ \frac{\text{g}}{\text{gmol}}$
$n_{\text{H}_2\text{O}}=\frac{m_{\text{H}_2\text{O}}}{\text{MW}_{\text{H}_2\text{O}}}=\frac{72.0\ \text{g}}{18\ \frac{\text{g}}{\text{gmol}}}=4.0\ \text{gmol}$

$V_{\text{stack}}=n\cdot R\cdot T\frac{T_{\text{stack}}}{P_{\text{stack}}}$
$=\frac{(123.8\ \text{gmol})(0.08206\ \frac{\text{atm·l}}{\text{gmol K}})(560.85\ \text{K})}{0.987\ \text{atm}}=57701\ \text{l}=5.77\ \text{m}^3$

Calculate the concentration of PM in the stack at stack conditions
$C_{\text{PM}}=\frac{M_{\text{PM}}}{V_{\text{stack}}}=\frac{1.50\ \text{g}}{5.77\ \text{m}^3}\left(\frac{10^6\ \mu\text{g}}{1\ \text{g}}\right)$
$C_{\text{PM}}=\frac{2.6\times10^5\ \mu\text{g}}{\text{m}^3}$

*Part (b)*
Calculate the flow and then the emission rate of PM.
$A_{\text{stack}}=\left[\frac{\pi(D_{\text{stack}})^2}{4}\right]=\frac{\pi(5.0\ \text{m})^2}{4}=19.63\ \text{m}^2$
$Q_{\text{stack}}=A_{\text{stack}}\times V_{\text{exit}}=(19.63\ \text{m}^2)\left(20\ \frac{\text{m}}{\text{s}}\right)=392.6\ \frac{\text{m}^3}{\text{s}}$
$\text{Rate}_{\text{PM}}=Q_{\text{PM}}C_{\text{PM}}$
$=\left(392.6\ \frac{\text{m}^3}{\text{s}}\right)\left(\frac{2.6\times10^5\ \mu\text{g}}{\text{m}^3}\right)\left(\frac{1\ \text{kg}}{10^9\ \mu\text{g}}\right)\left(\frac{3600\ \text{seconds}}{1\ \text{hour}}\right)\left(\frac{24\ \text{hours}}{1\ \text{day}}\right)$
$\text{Rate}_{\text{PM}}=8820\ \frac{\text{kg}}{\text{day}}$

*Part (c)*
Adjust the volume to STP conditions using Eq. (3.5).