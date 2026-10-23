relative volume of NO₂ (relative to the total) does not change. However, if some moles of a material that is gaseous in the stack are removed from the gas sample stream (such as water that is condensed in the impingers), we must account for the change in relative gas concentrations at the two places (upstream versus downstream of the impingers). This is shown in the following example problem.

concentrations at other oxygen levels, the following equation is used (Eq. 3.16):

```
Conc[3% O₂] = Conc[m% O₂] [ (20.9 − 3.0) / (20.9 − m% O₂) ] (3.18)
```

where m% O₂ is the nonstandard oxygen concentration. To calculate the concentration at 15% O₂, substitute 15.0 for 3.0 in Eq. (3.18).

# 3.13 Flow at Standard Temperature and Pressure

Flow at standard temperature and pressure (STP) can be determined provided actual temperature and pressure of the flow are known. Using EPA’s standard conditions 20°C (68°F), flow at STP becomes:

```
Flow at STP = (Dry gas flow) ( Pₘ / 14.7 psi ) ( 528°R / Tₘ ) (3.17)
```

where Pₘ is the measured pressure in psi and Tₘ is the measured temperature in °R (°R = 460 + °F).

# 3.14 Gas Flowrate Conversion from SCFM to ACFM

Example 3.12 For a boiler, 200 lb-mol/h of air at STP will be required for complete combustion of methane. What is the actual flow rate per minute at stack condition at 800°F at 1 atm.

# Solution

```
200 lb-mol/h of air = 200 × 392 = 78 400 ft³/h = 78 400 / 60 = 1 306.7 ft³/min (SCFM) (1 lb-mol of air at STP = 392 ft³)
```

```
ACFM at 800°F = (1306.7) ( (460 + 800) / (460 + 77) ) = 3066 ACFM of air (answer)
```

# 3.15 Corrections for Percent O₂

Corrections for percent O₂ are often needed for referencing combustion source emissions. For boilers, comparisons are made at 3% oxygen, consistent with the near stoichiometric conditions in boiler combustors. For combustion turbines, comparisons are made at 15% oxygen, consistent with the high excess air flow in a combustion turbine. To correct for

# 3.16 Boiler Flue Gas Concentrations Are Usually Corrected to 3% Oxygen

Example 3.13 The particulates in a stack gas are measured as 0.045 gr/dry standard cubic foot (dscf) with a stack gas oxygen concentration of 7% by volume. What is the particulate concentration at 3% oxygen?

# Solution

```
Conc₃% = Concₓ% [ (20.7% − 3%) / (20.7% − X%) ] = 0.045 grains/dscf [ (20.7% − 3%) / (20.7% − X%) ] = 0.045 grains/dscf [ 17.7% / 13.7% ] Conc₃% = 0.058 grains/dscf at 3% O₂
```

# 3.17 Air-to-Fuel Ratio and Stoichiometric Ratio

In combustion of a hydrocarbon fuel, it is important to accurately calculate the mass of air-to-fuel ratio (AFR). Then, the stoichiometric ratio (SR) can be defined as the actual AFR divided by the stoichiometric AFR, as illustrated by the following equation:

```
SR = (A / F)ₐcₜᵤₐₗ / (A / F)ₛₜₒᵢcₕ (3.19)
```

The complete combustion of hydrocarbons produces CO₂, the desirable aspect of rapid combustion, although CO₂ is also an agent of global warming. The following equation shows the reaction:

```
CₓHᵧ + (4x + y)/4 O₂ → x CO₂ + y/2 H₂O (3.20)
```