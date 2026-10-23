
This equation has been solved for concentrations resulting from emissions into an infinite space:

$$C=\frac{G}{4\pi D r}\exp\left[-\frac{u}{2D}(r-x)\right]\qquad\qquad\qquad\quad(4.15)$$

where G is the contaminant emission rate from the source (mass/time).

The diffusion coefficient (D) can be derived from measurements at the sampling site or estimated from values available in the literature. Measurements of the diffusion coefficient in indoor industrial environments have ranged from 0.05 to 11.5 m²/min, with 0.2 m²/min being a typical value.

### Example 4.12

Freon is emitted from an open-top vapor degreaser at a rate of 0.74 g/min. Estimate the concentration in the air inhaled by a worker 3 m downwind from the degreaser if the air velocity is 0.79 m/min.

### Solution

Since the worker is downwind of the degreaser, x = r in the diffusion-convection equation (Eq. 4.15) and

$$C=\frac{0.74\ \mathrm{g/min}}{(4)(3.1416)(0.2\ \mathrm{m^2/min})(3\ \mathrm{m})}=0.1\ \mathrm{g/m^3}$$

Molecular diffusion theory strictly applies to vapors and gases; however, PM with aerodynamic diameters less than 10 μm are distributed in workplace air in a similar manner. Using the dispersion models to describe the distribution of dusts and fumes is reasonable for small particles.

## 4.16.1 Gaussian Dispersion Model

The Gaussian dispersion model is most often used to characterize the dilution of toxic air pollutants with distance from the source. The model provides reasonable agreement with the experimental data and is, in its simplest form, computationally simple. The mean concentration, C, resulting from emission at a continuous point source of strength Q at a height H above the totally reflecting Earth along the plume centerline is given by

$$C=Q\left(\pi U \sigma_y \sigma_z\right)^{-1}\exp\left(-0.5H^{2}\sigma_z^{-2}\right)\qquad\qquad\quad(4.16)$$

where

- C is the concentration of toxic air pollutant (μg/m³)
- Q is the source release rate (μg/s)
- U is the mean wind speed at the stack height (m/s)
- H is the effective height of release above the Earth (m)
- y is the distance in a direction transverse to the wind (m)
- z is the height at which the observation is made (m)
- \(\sigma_y\) and \(\sigma_z\) are the standard deviations of the concentrations of plume transverse to the wind and perpendicular to the Earth, respectively (m).

Published values of \(\sigma_y\) and \(\sigma_z\) are based on laboratory and field measurements of velocity fluctuations under a variety of atmospheric conditions. Atmospheric stability is used to represent the amount of mixing in the atmosphere and is generally classified as stable, neutral, or unstable. A stable atmosphere is characterized by temperatures that increase with distance from the surface of the Earth and reduced vertical mixing; nighttime atmospheric conditions are generally represented as stable. More vigorous atmospheric mixing is expected as the Sun warms the surface of the Earth and the warmer, less dense air accumulates near the Earth’s surface; eventually, gravity will displace the warm air with cooler air from above. Daytime atmospheric conditions are typically represented as either neutral or unstable.

The product of the standard deviations has been represented by an equation of the form

$$\sigma_y\sigma_z = a x^b\qquad\qquad\qquad\qquad(4.17)$$

where

- a and b are constants (nondimensional)
- x is the distance downwind from the source (length)

Kumar (1998, 1999) performed regression analysis to develop expressions for the constants, a and b, for urban and rural settings and neutral and stable atmospheric conditions. Urban settings are appropriate when there are many obstacles in the immediate area of the release; obstacles include buildings and trees. Rural settings are appropriate when there are no buildings in the immediate area of the release and the terrain is generally flat and unobstructed. Table 4.10 lists the results of the regression when the distance from the source is in meters.

**Table 4.10 Regression equations for dispersion coefficients.**

| Condition | Regression equation |
| --- | --- |
| a. Rural release, neutral atmosphere, x < 500 m | \(\sigma_y\sigma_z = 0.01082 x^{1.78}\) |
| b. Rural release, neutral atmosphere, x > 500 m | \(\sigma_y\sigma_z = 0.04487 x^{1.56}\) |
| c. Rural release, stable atmosphere, x < 2000 m | \(\sigma_y\sigma_z = 0.0049 x^{1.66}\) |
| d. Rural release, stable atmosphere, x > 2000 m | \(\sigma_y\sigma_z = 0.01901 x^{1.46}\) |
| e. Urban release, neutral atmosphere, x < 500 m | \(\sigma_y\sigma_z = 0.0224 x^{2}\) |
| f. Urban release, neutral atmosphere, x > 500 m | \(\sigma_y\sigma_z = 0.394 x^{1.54}\) |
| g. Urban release, stable atmosphere, x < 500 m | \(\sigma_y\sigma_z = 0.008 x^{2}\) |
| h. Urban release, stable atmosphere, x > 500 m | \(\sigma_y\sigma_z = 0.34 x^{1.37}\) |

### Example 4.13

Hydrogen sulfide is released from a low-level vent in a rural area at a rate of 0.025 kg/s. Calculate the concentration at the plant boundary located 300 m downwind from the vent during daytime conditions when the wind speed is 4 m/s and at night when the wind speed is 2.5 m/s. (a) If the concentration of concern for hydrogen sulfide is 42 mg/m³, will this concentration be exceeded at the plant boundary? (b) Estimate the exposure to hydrogen sulfide of a person living 300 m downwind from the facility described during (i) daytime and (ii) nighttime if the individual at rest breathes 0.9 m³/h of the ambient air.

### Solution

a) Equation (4.16) can be used to calculate the concentration at ground-level by setting the height above the Earth equal to zero. The appropriate correlations for the dispersion coefficients are given by items (a) and (c) in Table 4.10 for daytime and nighttime conditions, respectively. For daytime conditions,

$$C = \frac{0.025\ \mathrm{kg/s}}{\pi(0.01082)(300\ \mathrm{m})^{1.78}(4\ \mathrm{m/s})} = 7.17\times10^{-6}\ \mathrm{kg/m^3} = 7.17\ \mathrm{mg/m^3}$$

For nighttime conditions,

$$C = \frac{0.025\ \mathrm{kg/s}}{\pi(0.0049)(300\ \mathrm{m})^{1.66}(2.5\ \mathrm{m/s})} = 5.02\times10^{-5}\ \mathrm{kg/m^3} = 50.2\ \mathrm{mg/m^3}$$

The concentration of hydrogen sulfide is below the concentration of concern under daytime conditions but exceeds the concentration of concern at night when atmospheric mixing is less.

b) For daytime conditions, (7.17 mg/m³)(0.9 m³/h) = 6.45 mg/h. For nighttime conditions, (50.2 mg/m³)(0.9 m³/h) = 45.2 mg/h.

## 4.16.2 Modeling Protocol

The development of a modeling protocol is the first step in a dispersion modeling analysis performed as part of a regulatory permitting analysis. The modeling protocol describes how the modeling will be carried out. It is usually necessary to submit a modeling protocol to the regulatory agency in order to receive agency approval for the modeling prior to performance of the modeling. The modeling protocol contains the following elements:

- Dispersion model selection
- Modeling methodology and assumptions
- Source input selection and preparation
- Meteorological data selection and processing
- Basis for ambient pollutant concentrations
- Receptor grid development

## 4.16.3 Dispersion Model Selection

AQIAs are carried out using Gaussian dispersion models – models that use the basic assumption of Gaussian dispersion. There are several such models, and different models are applicable to different source types and terrain.

The primary reference for selection of models is EPA’s Guideline on Air Quality Models. It identifies the models that have been developed and validated by EPA and are considered reliable for use in permitting. In particular, there are two models included in the Guideline that are most commonly used. They are

- AERSCREEN
- AERMOD

### 4.16.3.1 AERSCREEN

Rather than writing a separate screening model that would require updating whenever the refined model is updated, AERSCREEN is an interactive wrapper for AERMOD and associated auxiliary programs where the user supplies responses to program prompts. Those responses are used to write an input file for AERMOD and other required programs specific to the problem at hand.

AERSCREEN is the recommended screening model based on AERMOD. The model will produce estimates of “worst-case” one-hour concentrations for a single source, without the need for hourly meteorological data and also includes conversion factors to estimate “worst-case” 3-hour, 8-hour, 24-hour, and annual concentrations. AERSCREEN is intended to produce concentration estimates that are equal to or greater than the estimates produced by AERMOD with a fully developed set of meteorological and terrain data, but the degree of conservatism will vary depending on the application.
